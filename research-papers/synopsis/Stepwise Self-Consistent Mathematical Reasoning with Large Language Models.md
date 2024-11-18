This paper introduces Stepwise Self-Consistent Chain-of-Thought (SSC-CoT), a novel algorithm designed to enhance Large Language Models' (LLMs) ability to perform complex mathematical reasoning.  The core issue addressed is the difficulty LLMs face in handling multi-step reasoning problems, specifically in selecting crucial intermediate steps and exploring solution paths effectively.

**1. Methodology:**

SSC-CoT employs a multi-step approach inspired by human problem-solving strategies:

* **Step 1: Information Extraction:**  The LLM extracts key information (trigonometric functions and angles) from the input question Q using an extraction function  `E(p<sub>θ</sub>, Q)`, where `p<sub>θ</sub>` represents the LLM with parameters θ.

* **Step 2: Knowledge Graph Query:** This extracted information is used to query a specifically designed Knowledge Graph (KG)  `r<sub>k</sub> = S(G, V)`, where `S` is the search function, and `G` represents the KG containing relevant trigonometric identities and relationships. The result `r<sub>k</sub>` serves as a "hint" for the next step.

* **Step 3: Reasoning Chain Generation:** The LLM generates N reasoning chains (`C<sub>i</sub> = G<sub>k</sub>(p<sub>θ</sub>, Q, r<sub>k</sub>)` for round k, where `G<sub>k</sub>` is the chain generation function) based on the question and the hint from the KG. Each chain consists of intermediate results (states) `x<sub>i</sub><sup>j</sup>`.

* **Step 4: Overlapping State Identification:**  The algorithm identifies intermediate results that appear across multiple reasoning chains.  This is done by converting intermediate results into TF-IDF vectors and computing cosine similarity.  Results with similarity above a threshold (T=0.999) are considered overlapping. A human-in-the-loop (HITL) option allows human experts to select overlapping states.

* **Step 5: Verification:**  A verification function `V(Q, x<sub>i</sub><sup>1</sup>x<sub>i</sub><sup>2</sup>...x<sub>i</sub><sup>j</sup>)` (implemented using the LLM) checks the correctness of the overlapping states. Only verified states (`S<sub>v</sub>`) are retained.

* **Step 6: Iteration:** Steps 2-5 are iterated for a predefined number of rounds, using verified states from previous rounds to refine KG queries and guide further reasoning.

* **Step 7: Final Result:** The final answer is determined by a majority vote among conclusion statements from the various reasoning chains.

**2. Knowledge Graph (KG):**

The KG is a directed graph with two types of nodes: "Conceptual Nodes" (e.g., sin³x, cos x) and "Theorem Nodes" (e.g., cos(π/2) = 0).  Four types of edges represent relationships: Dependency, Derivation, Application, and Identity links.  The KG is queried based on extracted trigonometric functions and angles from the question.  The authors provide a mechanism to expand the KG with new lemmas derived from solved problems.

**3. Intermediate Result Selection:**

The algorithm prioritizes deeper states within reasoning chains.  When multiple overlapping groups of intermediate results exist, the selection process, formalized in Algorithm 1, utilizes a pairwise comparison based on the depth of states within chains (Equation 1).  If group A can be inferred from group B, B is selected; otherwise, the union of A and B is considered. Algorithm 1 iteratively applies pairwise comparisons to select the optimal group. If no overlap occurs, the final state from each chain is verified.


**4. Datasets:**

* **TriMaster100:** A new dataset of 100 complex trigonometry problems (senior high school to Mathematical Olympiad level), with solutions broken down into scored intermediate steps.  This allows for a more nuanced evaluation than focusing solely on final answer accuracy. Human performance on a subset of TriMaster100 was also evaluated.

* **MATH Level 5:** A subset of the MATH dataset containing the most difficult 1324 questions, used for benchmarking against other methods.


**5. Baselines:**

The paper compares SSC-CoT against several state-of-the-art methods:

* Tree-of-Thought (ToT)
* Chain-of-Thought with Self-Consistency (CoT-SC)
* LLEMMA (7B and 34B versions)

**6. Results:**

* **TriMaster100:** SSC-CoT significantly outperforms baselines, achieving a score 34% higher than CoT-SC (the second-best method).  Even without the KG, SSC-CoT's performance is 29% higher than CoT-SC.

* **MATH Level 5:** SSC-CoT surpasses the second-best method by 7.2% in accuracy.  Ablation studies demonstrate the effectiveness of both the KG and the intermediate result selection mechanism.  Qualitative analysis showcases SSC-CoT's ability to identify critical intermediate steps more efficiently than ToT, often leading to correct solutions. However, the paper also highlights cases where SSC-CoT makes errors, primarily due to limitations in the LLM's arithmetic capabilities and the verification process.

**7. Code and Data:**

The code and the TriMaster100 dataset are available at [https://github.com/zhao-zilong/ssc-cot](https://github.com/zhao-zilong/ssc-cot).

**8. Conclusion:**

SSC-CoT presents a promising approach to improve LLMs' complex mathematical reasoning capabilities.  The use of a KG and a self-consistent chain-of-thought strategy with a verification mechanism leads to superior performance compared to existing methods.  Future work focuses on improving the automatic selection of overlapping intermediate results and the verification process.


**Equation 1 (LaTeX):**

```latex
\begin{cases}
B, & \text{if } \forall m \in M, b_j|_{b_i=m} > a_j|_{a_i=m}, \\
A, & \text{if } \forall m \in M, a_j|_{a_i=m} > b_j|_{b_i=m}, \\
A \cup B, & \text{otherwise}.
\end{cases}
```

This equation describes the logic for selecting between two overlapping groups of intermediate results (A and B) based on their positions within reasoning chains.


The paper provides extensive experimental results and qualitative analysis to support its claims.  However, the limitations concerning the LLM's arithmetic capabilities and the potential for improvement in the verification process are acknowledged.
