## Putnam-AXIOM: An Extensive Summary

This paper introduces Putnam-AXIOM, a benchmark designed to evaluate the higher-level mathematical reasoning capabilities of Large Language Models (LLMs).  It addresses the saturation of existing benchmarks and the problem of data contamination, where LLMs might achieve high scores by memorizing problems from their training data.

**1. Literature Review:**

The paper reviews existing mathematical reasoning benchmarks like MATH and GSM8K, highlighting their limitations due to LLM performance saturation.  It discusses the growing concern of data contamination, where models memorize answers from publicly available datasets.  Related work on combating this issue, such as the creation of functional variations (Srivastava et al., 2024), and other contemporary datasets like ARB, OlympiadBench, and SciBench are also discussed, highlighting their limitations in terms of automatic evaluation and scalability.  PutnamBench, focusing on formal theorem proving, is also mentioned as a related, but distinct, approach.

**2. Methodology:**

Putnam-AXIOM comprises two datasets:

* **Putnam-AXIOM Original:** This dataset consists of 236 problems from the William Lowell Putnam Mathematical Competition (1985-2023), selected for their suitability for automated evaluation.  Problems are categorized into 11 domains (Geometry, Algebra, Trigonometry, Calculus, Linear Algebra, Combinatorics, Probability, Number Theory, Complex Numbers, Differential Equations, and Analysis) and by difficulty level (A/B for sitting, 1-6 for increasing complexity within a sitting).  Solutions are provided with boxed final answers (`\boxed{}`) for automated evaluation.  Some problems were modified to ensure a single, easily extractable boxed answer, preserving the core difficulty while simplifying evaluation.

* **Putnam-AXIOM Variation:** This dataset contains functional variations of 52 problems from the original dataset.  Variations are generated programmatically by:
    * **Variable Change:** Altering variable names.
    * **Constant Change:** Modifying numerical constants in the problem statement and solution.

These variations create an effectively infinite supply of novel, equally challenging problems, mitigating data contamination.

**3. Algorithms and Formulas:**

The paper doesn't present novel algorithms. The core methodology relies on:

* **Automated Evaluation:**  LLM responses are evaluated by extracting the boxed answer and comparing it to the ground truth using an equivalence function. This function handles variations in answer representation (e.g., 0.5, 1/2, ½).
* **Functional Variation Generation:** A simple algorithmic process to modify variables and constants in the original problems to produce variations.  No specific formulas are presented for this, as the changes are problem-specific.

**4. Notation:**

* `\boxed{}`:  Indicates the boxed final answer in the problem solutions.
* \(F_m\): The m-th Fibonacci number.
* \(p(x)\): A polynomial.
* \(\Gamma(p(x))\): The sum of squares of the coefficients of polynomial \(p(x)\).
* \(i\): The imaginary unit (\(i^2 = -1\)).
* \(\lfloor a \rfloor\): The floor function (largest integer less than or equal to \(a\)).
* \(|z|\): The magnitude (absolute value) of a complex number \(z\).

**5. Results and Analysis:**

The paper evaluates several LLMs (OpenAI's o1-preview, GPT-4, GPT-4o, Claude-3.5 Sonnet, Qwen2-Math-7B, Qwen2-Math-7B-Instruct, NuminaMath, NuminaMath-7B-TIR, and DeepSeek-Math-7B-RL) on both datasets.

* **Putnam-AXIOM Original:** OpenAI's o1-preview achieved the highest accuracy (41.95%), while other models scored significantly lower (mostly below 10%).

* **Putnam-AXIOM Variation:**  All models showed a significant drop in accuracy compared to their performance on the corresponding original problems (20-44% reduction). This demonstrates the effectiveness of the variations in revealing the models' reliance on memorization.  The confidence intervals for many models indicated statistically significant differences between original and variation performance.

Error analysis focused on OpenAI o1-preview and GPT-4o, revealing a common weakness: lack of mathematical rigor in their solutions.  While often reaching the correct final answer, these models frequently lacked justification for intermediate steps.  Open-source models exhibited additional errors like calculation mistakes, hallucinations, and misunderstandings of the problem statement.

A further analysis on binary questions (questions with only two possible answers) showed that their inclusion inflated the accuracy scores of some models, especially less-advanced ones, but this effect was less prominent for the top-performing models.

**6. Conclusion:**

Putnam-AXIOM provides a challenging benchmark for evaluating advanced mathematical reasoning in LLMs. The use of functional variations effectively mitigates data contamination issues. The results highlight the limitations of current LLMs in tackling complex, high-level mathematical problems and underscore the need for further research in artificial reasoning.  The data and evaluation code are publicly available.


**7. Tables and Figures:**

The summary mentions several tables and figures from the paper which illustrate the performance of different LLMs on the original and variation datasets, as well as examples of model errors and the analysis of binary questions versus complex questions.  These visuals are crucial for a full understanding of the results but cannot be reproduced here without access to the original paper's figures.  The paper mentions Figure 1 contrasting original and variation accuracies with confidence intervals, Table 1 showing Putnam-AXIOM Original dataset accuracies, and Table 2 presenting mean accuracies and confidence intervals for the Putnam-AXIOM Variation dataset.  Figures 3, 4, 7, 8, 9, 10, 11, and 12 illustrate examples of modified questions and model responses.  Figure 6 compares overall accuracies on Putnam-AXIOM with and without binary questions.

**8. Legal Compliance:**

The paper includes an appendix discussing the legal compliance of using Putnam problems, arguing that their use falls under fair use due to transformative nature of the dataset, non-commercial purpose, and the negligible effect on the market for Putnam problems.
