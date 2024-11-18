## ToRA: A Tool-Integrated Reasoning Agent for Mathematical Problem Solving - Extensive Summary

This paper introduces ToRA, a series of Tool-integrated Reasoning Agents designed to enhance Large Language Models' (LLMs) mathematical problem-solving capabilities.  ToRA overcomes LLMs' limitations in complex mathematical reasoning by seamlessly integrating natural language reasoning with external tools (computation libraries and symbolic solvers). The core methodology involves three key steps:

1. **Curating Interactive Tool-Use Trajectories:**  The authors leverage GPT-4 to generate high-quality datasets (ToRA-Corpus) containing tool-use trajectories for mathematical problems from GSM8k and MATH datasets.  These trajectories consist of interleaved sequences of natural language rationales (\(r_i\)), program code for tool use (\(a_i\)), and tool execution outputs (\(o_i\)). A trajectory τ is represented as:  \(τ = r_1a_1o_1...r_{n-1}a_{n-1}o_{n-1}r_n\), where \(r_n\) contains the final answer. The generation process, formalized in Algorithm 1, uses GPT-4 to iteratively generate rationales, programs, and execute programs until a stopping condition (answer in a "\boxed{}") is met.  Equations 1, 2, and 3 describe the probabilistic generation of rationales and programs conditioned on previous steps.

   ```latex
   r_{i} \sim \mathbb{P}_{\mathcal{G}}(\cdot|p \oplus q \oplus \tau_{i-1}) \tag{1} \\
   a_{i} \sim \mathbb{P}_{\mathcal{G}}(\cdot|p \oplus q \oplus \tau_{i-1} \oplus r_{i}) \tag{2} \\
   \tau_{i} \leftarrow \tau_{i-1} \oplus r_{i} \oplus a_{i} \oplus o_{i} \tag{3}
   ```
   where:
    * \(r_i\): Rationale at step \(i\)
    * \(a_i\): Program at step \(i\)
    * \(o_i\): Output from tool execution at step \(i\)
    * \(p\): Prompt
    * \(q\): Question
    * \(\tau_i\): Trajectory up to step \(i\)
    * \(\mathcal{G}\): GPT-4 model
    * \(\mathcal{E}\): External tool execution function
    * \(\oplus\): Concatenation

   Algorithm 1 outlines this iterative process:

   ```
   Algorithm 1: Inference of Tool-Integrated Reasoning
   1: problem q, model G, prompt p, external tools E, stop condition Stop(⋅), maximum iteration rounds n
   2: τ0 ← ∅  Trajectory Initialization
   3: for i ← 1 to n do
   4:   ri ∼ PG(⋅|p ⊕ q ⊕ τi−1) Rationale Generation (Eq. 1)
   5:   if Stop(ri) then Stopping Criteria
   6:     return τi−1 ⊕ ri
   7:   end if
   8:   ai ∼ PG(⋅|p ⊕ q ⊕ τi−1 ⊕ ri) Program Generation (Eq. 2)
   9:   oi ← E(ai) Tool Execution
   10:  τi ← τi−1 ⊕ ri ⊕ ai ⊕ oi Trajectory Update (Eq. 3)
   11: end for
   12: return τn
   ```

2. **Imitation Learning:**  The curated ToRA-Corpus is used to fine-tune various LLaMA-2 and CodeLLaMA models (7B to 70B parameters) through imitation learning.  Equation 4 shows the loss function minimized during training:

   ```latex
   \mathcal{M} = \arg\min_{\mathcal{M}} \sum_{q, \tau} \sum_{i=1}^{n-1} -\log \mathbb{P}_{\mathcal{M}}(r_{i+1}a_{i+1}|q, r_1...o_i) \tag{4}
   ```
   where \(\mathcal{M}\) represents the trained ToRA model.

3. **Output Space Shaping:** To enhance the diversity and robustness of the model's reasoning, output space shaping is applied.  This involves sampling multiple trajectories using nucleus sampling, retaining valid ones, and correcting invalid trajectories using a larger teacher model (CodeLLaMA-34B).  The model is then retrained on the combined dataset of original ToRA-Corpus, valid samples, and corrected trajectories.


**Experiments and Results:**

ToRA models were evaluated on 10 mathematical reasoning datasets (including GSM8k, MATH, and various out-of-distribution datasets).  The results (Table 2 in the paper) consistently show significant improvements over various baselines (LLaMA-2, CodeLLaMA, WizardMath, and even GPT-4's Chain-of-Thought (CoT) prompting in some cases).  Specifically:

* ToRA models outperformed open-source baselines by 13-19% absolute improvement on average.
* ToRA-7B surpassed WizardMath-70B by 22% on the MATH dataset.
* ToRA-Code-34B achieved >50% accuracy on MATH, exceeding GPT-4's CoT result and showing competitiveness with GPT-4 using code.

**Ablation Studies:** Ablation studies (Figure 4 and 5, Table 3) demonstrated the effectiveness of both the tool-integrated reasoning format and the output space shaping technique.  The interleaved format consistently outperformed rationale-only and program-only approaches. Output space shaping significantly improved performance, especially for smaller models and more challenging problems.

**Analysis:** The authors analyzed the failure modes of ToRA on the MATH dataset (Table 4), identifying key challenges:  incorrect reasoning steps, diagram misinterpretations, tool usage errors, and limitations in formalizing abstract reasoning tasks as programs.

**Conclusion:**  The paper successfully demonstrates the effectiveness of ToRA in significantly improving LLMs' mathematical reasoning abilities through tool integration and output space shaping. The analysis of failure modes provides valuable insights for future research directions.  The code and models are publicly available on GitHub ([https://github.com/microsoft/ToRA](https://github.com/microsoft/ToRA)).  The appendix contains further details on related work, datasets, additional experiments, and examples.
