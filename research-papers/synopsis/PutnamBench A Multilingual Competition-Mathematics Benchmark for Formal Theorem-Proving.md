## Extensive Summary of "PutnamBench: A Multilingual Competition-Mathematics Benchmark for Formal Theorem-Proving"

This paper introduces PutnamBench, a new multilingual benchmark for evaluating formal theorem-proving algorithms.  It leverages problems from the William Lowell Putnam Mathematical Competition, a prestigious undergraduate-level mathematics competition. The benchmark aims to push the boundaries of automated theorem proving by providing challenging problems requiring diverse mathematical knowledge and skills.

**1. Introduction:**

The paper highlights the growing need for robust benchmarks in automated mathematical reasoning, particularly with the rise of neural theorem provers. Existing benchmarks like MiniF2F (high school level) and Fimo (IMO shortlist problems) have limitations: MiniF2F contains easily solvable problems, and Fimo only supports the now-deprecated Lean 3.  PutnamBench addresses these limitations by offering a collection of formally verified problems from the Putnam competition, known for its challenging problems spanning various undergraduate mathematics topics.  The authors also emphasize the importance of preventing data leakage between training and evaluation sets in the age of LLMs.

**2. Background:**

* **Formal Theorem Proving:** The paper explains the core concepts of interactive theorem provers (ITPs) like Lean 4, Coq, and Isabelle. These systems allow users to formally state theorems and construct machine-verifiable proofs through a sequence of proof steps, transforming the initial proof state to a final "QED" state.  Figure 1 in the paper illustrates a Lean 4 theorem and its proof.
* **The Putnam Competition:** The paper describes the Putnam competition, emphasizing its breadth of mathematical topics (analysis, linear algebra, abstract algebra, number theory, geometry, set theory, and combinatorics) and its difficulty, making it a suitable source for a challenging benchmark.

**3. PutnamBench:**

PutnamBench contains formalizations of 514 Putnam problems in Lean 4 and Isabelle, with an additional 309 in Coq, totaling 1337 formalizations. Key features:

* **Diversity and Breadth:** Unlike previous benchmarks focusing primarily on high school mathematics, PutnamBench covers a wider range of undergraduate-level mathematical concepts.
* **Multilinguality:**  It's the first benchmark to offer formalizations across Lean 4, Isabelle, and Coq, enabling cross-ITP comparisons and facilitating research in multilingual theorem proving.  The formalizations are structurally aligned but may differ due to the underlying foundations of each language.  The use of different mathematical libraries (Mathlib for Lean, HOL for Isabelle, Coquelicot and others for Coq) also impacts the formalizations.
* **Factored Solutions:**  Approximately 60% of Putnam problems require finding a closed-form solution before proving its correctness.  PutnamBench addresses this by offering two tasks: (1) finding the solution and then proving it, and (2) proving a given solution. This better reflects the true difficulty of the original problems.
* **Licensing:** The benchmark is released under Apache 2.0 (Lean 4 and Isabelle) and MIT (Coq) licenses.

**4. Experimental Evaluation:**

The authors evaluate several neural and symbolic theorem provers on PutnamBench:

* **Models:** GPT-4 (used across all languages), Copra (Lean 4 and Coq), Draft-Sketch-Prove (DSP) (Isabelle), Sledgehammer (Isabelle), and CoqHammer (Coq).
* **Metrics:** The `pass@n` metric is used, measuring the success rate within `n` proof attempts.
* **Results:** The overall results are quite poor, demonstrating the significant challenge PutnamBench presents.  Only a handful of problems were solved across all languages and methods:
    * **Lean 4:** GPT-4 solved only one problem (Putnam 1988 B1); Copra, with modifications for Lean 4, also solved only one (Putnam 1988 B1).
    * **Isabelle:** GPT-4 failed to solve any problems; DSP solved two; Sledgehammer solved three (all involving sets with binary operations).
    * **Coq:** GPT-4 solved one problem (Putnam 1988 B1); Copra solved one (Putnam 1988 B1); CoqHammer solved none.  Figure 2, 3, and 13 show examples of formalizations and proofs.
* **General Analysis:** The successfully solved problems were generally among the easiest in the benchmark, highlighting the current limitations of automated theorem provers in handling complex, multi-concept problems.  The authors point to two main reasons for the failures: (i) Difficulty in synthesizing new lemmas and complex proof strategies, and (ii) Inefficient leveraging of knowledge within existing mathematical repositories.  Figures 15-21 illustrate various aspects of the evaluation, including prompt engineering, error messages, and successful/failed proof attempts.

**5. Related Works:**

The paper provides a comprehensive review of related work, including existing formal benchmarks (MiniF2F, Fimo, ProofNet, Compfiles, LeanDojo, ProverBot9001, PISA), informal benchmarks (MATH, GSM8K, NaturalProofs), and methods for formal theorem proving (GPT-f, PACT, FMSCL, HTPS, COPRA, LLEMMA, DeepSeek-Prover, AlphaGeometry,  Isabelle-related methods like DSP, Sledgehammer, Lyra, POETRY, LEGO-Prover, Baldur, and Coq-related methods like ASTactic and Proverbot9001).

**6. Conclusion:**

PutnamBench is presented as a challenging benchmark for future research in neural theorem proving, highlighting the need for advancements in lemma synthesis, proof strategy generation, and efficient utilization of mathematical repositories.

**7. Impact Statement:**

The paper concludes with a brief impact statement acknowledging the potential societal consequences of advancements in machine learning without specific elaboration.


**Mathematical Formulas and Notation:**

The paper uses standard mathematical notation, including set notation (e.g., \(X \subseteq \mathbb{R}\), \(|X|\)), summation notation (e.g., \(\sum_{s\in S} s\)), group theory notation (e.g., \(g_1 g_2 g_3 = e\)), and limits.  Specific formulas are present within the problem statements, but there aren't any overarching mathematical formulas used for the methodology itself beyond the `pass@n` metric.


**Code/LaTeX Snippets:**

Several snippets of Lean 4, Isabelle, and Coq code are included in the paper to illustrate formalizations of Putnam problems.  These snippets are too numerous to reproduce here but are integral to understanding the specific formalizations used in the benchmark.  The paper also shows examples of GPT-4 generated proofs, highlighting both successful and unsuccessful attempts.


**Tables:**

The paper does not contain explicit tables summarizing the results, but the results are presented in a textual format in Section 4.3 and discussed in the analysis.  The performance of each method on each ITP is reported individually.


Overall, the paper presents a significant contribution to the field by introducing a challenging, multilingual benchmark that will likely drive further innovation in automated theorem proving. The detailed analysis of the experimental results and the comprehensive literature review provide valuable insights into the current limitations and future directions of the field.
