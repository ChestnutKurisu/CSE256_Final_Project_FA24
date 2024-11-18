## Plan-and-Solve Prompting: Improving Zero-Shot Chain-of-Thought Reasoning by Large Language Models - An Extensive Summary

This paper addresses the limitations of Zero-shot Chain-of-Thought (Zero-shot-CoT) prompting for large language models (LLMs) in multi-step reasoning tasks.  Zero-shot-CoT, which simply adds the instruction "_Let's think step by step_" to the problem prompt,  suffers from calculation errors, missing steps, and semantic misunderstandings.  The authors propose *Plan-and-Solve (PS)* prompting to mitigate these issues, particularly focusing on missing steps.

**1. Methodology:**

The core of the proposed methodology is a two-step prompting strategy:

**Step 1: Reasoning Generation:**  This step aims to elicit a plan from the LLM before executing it. The authors introduce two prompting variations:

* **PS Prompting:** Replaces the "_Let's think step by step_" instruction with "_Let's first understand the problem and devise a plan to solve the problem. Then, let's carry out the plan and solve the problem step by step_". This encourages the LLM to break down the problem into smaller, manageable subtasks.

* **PS+ Prompting:** Extends PS prompting with more detailed instructions: "_extract relevant variables and their corresponding numerals_" and "_calculate intermediate results (pay attention to calculation and commonsense)_. "  This aims to improve the accuracy of calculations and reduce missing steps by explicitly guiding the LLM's attention to crucial information.

**Step 2: Answer Extraction:**  A secondary prompt is used to extract the final numerical answer from the LLM's generated reasoning text. A simple template like "Therefore, the answer (arabic numerals) is" is appended to the generated text to facilitate this extraction.


**2. Algorithms and Formulas:**

The paper doesn't introduce novel algorithms or complex mathematical formulas. The core idea relies on carefully crafted prompts to guide the LLM's reasoning process. The only implicit formula used is in the calculation of accuracy:

```
Accuracy = (Number of Correct Answers) / (Total Number of Questions) * 100%
```


**3. Notation:**

The paper uses standard notation.  No specific mathematical notation is introduced beyond the accuracy calculation mentioned above.  Key terms include:

* **LLM (Large Language Model):** A deep learning model trained on a massive dataset of text and code, capable of generating human-quality text and performing various NLP tasks.
* **CoT (Chain-of-Thought):** A prompting technique that encourages LLMs to generate intermediate reasoning steps before providing a final answer.
* **Zero-shot:**  A learning paradigm where the model solves new problems without any prior training examples or fine-tuning.
* **Few-shot:** A learning paradigm where the model is given a small number of examples before encountering a new problem.
* **PS Prompting (Plan-and-Solve Prompting):** The proposed prompting method that encourages LLMs to create a plan before solving the problem.
* **PS+ Prompting:**  The enhanced version of PS prompting with more detailed instructions.


**4. Datasets and Experimental Setup:**

The evaluation uses ten datasets across three reasoning categories:

* **Arithmetic Reasoning:** GSM8K, SVAMP, MultiArith, AddSub, AQuA, SingleEq
* **Commonsense Reasoning:** CommonsenseQA, StrategyQA
* **Symbolic Reasoning:** Last Letter, Coin Flip


The authors compare PS and PS+ prompting against several baselines:

* **Zero-shot baselines:** Zero-shot-CoT, Zero-shot-PoT (Program-of-Thought)
* **Few-shot baselines:** Manual-CoT (with manually crafted examples), Auto-CoT (with automatically selected examples)


The experiments use GPT-3 (text-davinci-003) with a temperature of 0 (greedy decoding) for the zero-shot methods and 8 (or fewer, depending on the dataset) demonstration examples for the few-shot methods.  Accuracy is the primary evaluation metric.


**5. Results and Outcomes:**

The results, summarized in Tables 2, 4, and additional tables in the appendix, consistently show that:

* **PS+ prompting significantly outperforms Zero-shot-CoT** across all datasets, demonstrating the effectiveness of the plan-and-solve strategy and detailed instructions.
* **PS+ is competitive with, and sometimes surpasses, Zero-shot-PoT.**
* **PS+ achieves performance comparable to 8-shot Manual-CoT** on arithmetic reasoning tasks, indicating that zero-shot prompting can achieve results similar to few-shot prompting.  This is a significant finding, as it reduces the need for manual example creation.
* **Self-consistency improves the performance** of PS+ prompting further, especially for challenging datasets like GSM8K.
* Error analysis reveals that PS+ reduces both calculation errors and missing-step errors compared to Zero-shot-CoT.


**6. Literature Review:**

The paper reviews existing work on reasoning in NLP, focusing on methods to improve LLMs' reasoning abilities. It highlights the limitations of previous approaches and positions PS prompting as a novel contribution to the field of prompt engineering.

**7. Discussion and Conclusion:**

The authors conclude that PS+ prompting offers a superior approach to zero-shot multi-step reasoning by providing more structured guidance to the LLM.  The results suggest a potential to reduce manual effort in prompting while achieving high accuracy, opening new avenues for prompting research.  They acknowledge limitations, such as the effort involved in crafting effective prompts and the persistence of semantic misunderstanding errors.

**8. Code and Data:**

The authors provide a link to their code on GitHub: [https://github.com/AGI-Edgerunners/Plan-and-Solve-Prompting](https://github.com/AGI-Edgerunners/Plan-and-Solve-Prompting)  The paper mentions the licensing of the datasets used.

In summary, the paper presents a valuable contribution to the field of prompt engineering for LLMs, showing how carefully designed prompts can significantly enhance their performance in complex reasoning tasks. The proposed Plan-and-Solve prompting strategy, particularly PS+, offers a promising path toward more efficient and effective utilization of LLMs for reasoning problems.
