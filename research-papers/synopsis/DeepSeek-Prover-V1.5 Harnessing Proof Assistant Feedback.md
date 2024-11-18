This paper introduces DeepSeek-Prover-V1.5, an open-source language model for theorem proving in Lean 4.  It builds upon DeepSeek-Prover-V1 by improving both training and inference. The core methodology combines whole-proof generation with a novel Monte-Carlo Tree Search (MCTS) variant to leverage proof assistant feedback effectively.

**1. Literature Review:**

The paper reviews existing approaches to language model-based theorem proving, categorizing them into two main strategies:

* **Proof-step generation:**  Generates and verifies tactics sequentially, often using tree search. Examples include GPT-f, Thor, ReProver, Hypertree Proof Search, and InternLM2-StepProver.
* **Whole-proof generation:** Generates the entire proof at once. Examples include DSP, Subgoal-Prover, LEGO-Prover, Lyra, and miniCTX.

DeepSeek-Prover-V1, the predecessor, used whole-proof generation but suffered from the compounding error problem in long proofs.

**2. Methodology:**

DeepSeek-Prover-V1.5 addresses the limitations of its predecessor through a three-stage training process and an enhanced inference method:

**2.1. Model Training:**

* **Pre-training:** The base model (DeepSeek-Prover-V1.5-Base) is pre-trained on a large dataset of mathematical text and code, focusing on formal languages like Lean, Isabelle, and Metamath.
* **Supervised Fine-tuning (SFT):**  The pre-trained model is fine-tuned on an enhanced dataset derived from DeepSeek-Prover-V1.  This dataset is augmented in two ways:
    * **Thought-augmented Proof Generation:**  Natural language chain-of-thought (CoT) comments are added to the Lean 4 code using DeepSeek-Coder V2 236B, aligning natural language reasoning with formal proof steps.
    * **Prompt Augmentation with Tactic State Information:** Intermediate tactic states from the Lean 4 prover are inserted as comments within the code, allowing the model to utilize compiler feedback.  This is crucial for the truncate-and-resume mechanism.
* **Reinforcement Learning from Proof Assistant Feedback (RLPAF):** The supervised fine-tuned model (DeepSeek-Prover-V1.5-SFT) is further improved using the GRPO algorithm. The Lean prover provides binary rewards (1 for correct proofs, 0 for incorrect).  The authors address reward sparsity by focusing on challenging yet solvable problems for the SFT model.

**2.2. Model Inference:**

DeepSeek-Prover-V1.5 offers two inference methods:

* **Single-pass sampling:** Generates a whole proof and verifies it.  If incorrect, it repeats the process.
* **RMaxTS (Reward-Max Tree Search):** A novel MCTS variant incorporating a truncate-and-resume mechanism and an intrinsic reward system.

**2.2.1. RMaxTS Algorithm:**

RMaxTS uses a tactic-level tree abstraction where each node represents a tactic state transition. The algorithm consists of four steps:

* **Selection:** Uses a UCB1-based tree policy (Equation 1 and 2) modified for non-stationary rewards via a discounted UCB (DUCB, Equation 7-9).  The tree policy balances exploration and exploitation using a virtual node technique.
* **Expansion:** The model generates a proof segment from the selected node. If an error occurs, it truncates the generated code at the error and adds the successful portion as new nodes to the tree.
* **Simulation:** Integrated into expansion; the whole-proof generation acts as the simulation step.
* **Backpropagation:** Updates the Q-values along the selected trajectory using extrinsic rewards (1 for correct proofs, 0 otherwise) and intrinsic rewards (1 if a new node is added to the tree, 0 otherwise, Equation 3).  DUCB is employed to handle non-stationary intrinsic rewards.

The algorithm is parallelized using root parallelization (multiple MCTS runners), tree parallelization (multiple thread workers), and a virtual loss technique to encourage exploration.

**3. Results and Evaluation:**

The model is evaluated on two benchmarks:

* **miniF2F:** High school level mathematics problems.
* **ProofNet:** Undergraduate level mathematics problems.

The metric used is pass@K (the percentage of problems solved correctly within K attempts).

DeepSeek-Prover-V1.5 significantly outperforms DeepSeek-Prover-V1 and achieves state-of-the-art results on both benchmarks, especially when using RMaxTS.  The results show the effectiveness of each training stage (pre-training, SFT, RLPAF) and the benefits of the CoT prompting and RMaxTS.  Ablation studies confirm the importance of intrinsic rewards, discounted UCB, and tactic state information in RMaxTS.

**4. Conclusion and Future Work:**

The paper concludes that DeepSeek-Prover-V1.5 establishes a strong baseline for LLM-based theorem provers. Future work includes developing a critic model to improve exploitation in the MCTS algorithm and extending the model to handle complex, multi-theorem Lean files.


**Key Formulas and Notations:**

* **Equation 1:** `TreePolicy(s) = argmax_{a∈Children(s)∪{∅}} Q_{UCB}(s,a)`  (Tree policy selection)
* **Equation 2:** `∀ a∈Children(s)∪{∅}, Q_{UCB}(s,a) = Q(s,a) + UCB(s,a)` (UCB value estimation)
* **Equation 3:** `R_{intrinsic}(τ) = mathbb{I}[at least one new node is added to the search tree]` (Intrinsic reward)
* **Equation 4:** `Q_{UCB1}(s,a) = W(s,a)/N(s,a) + sqrt(2ln(Σ_{a'}N(s,a'))/N(s,a))` (UCB1)
* **Equation 5:** `W(s,a) = Σ_{τ∈Γ(s,a)}R(τ)` (Sum of rewards)
* **Equation 6:** `N(s,a) = |Γ(s,a)|` (Number of visits)
* **Equation 7:** `Q_{DUCB}(s,a) = Wγ(s,a)/Nγ(s,a) + sqrt(2ln(Σ_{a'}Nγ(s,a'))/Nγ(s,a))` (Discounted UCB)
* **Equation 8:** `Wγ(s,a) = Σ_{t=1}^{N(s,a)}γ^{N(s,a)-t}R(τ_t)` (Discounted sum of rewards)
* **Equation 9:** `Nγ(s,a) = Σ_{t=0}^{N(s,a)-1}γ^t` (Discounted number of visits)

Where:

* `s`:  Node in the search tree
* `a`: Action (tactic)
* `Q(s,a)`: Estimated value of action `a` in state `s`
* `UCB(s,a)`: Upper Confidence Bound for exploration
* `τ`: Trajectory (sequence of states and actions)
* `R(τ)`: Reward for trajectory `τ`
* `Γ(s,a)`: Set of trajectories passing through (s,a)
* `γ`: Discount factor


The paper provides extensive experimental results in tables comparing DeepSeek-Prover-V1.5 to other state-of-the-art models, showcasing its superior performance.  The appendices offer detailed examples illustrating the CoT and non-CoT prompting methods.
