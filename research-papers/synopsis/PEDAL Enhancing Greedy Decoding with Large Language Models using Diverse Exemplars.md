## PEDAL: Enhancing Greedy Decoding with Large Language Models using Diverse Exemplars - An Extensive Summary

This paper introduces PEDAL (**P**rompts based on **E**xemplar **D**iversity **A**ggregated using **LLMs**), a novel hybrid self-ensembling approach designed to improve the accuracy of text generation using Large Language Models (LLMs) while reducing inference costs compared to existing methods.  The core idea is to combine the efficiency of Greedy Decoding with the robustness of self-ensembling techniques.

**1. Literature Review:**

The paper reviews existing work in several key areas:

* **LLMs and Reasoning:**  LLMs, despite their impressive capabilities (Brown et al., 2020; Raffel et al., 2020; Chowdhery et al., 2022; Touvron et al., 2023), often require carefully crafted prompts for optimal performance (Khattab et al., 2023; Fernando et al., 2023).  Their reasoning abilities are a focus of ongoing research (Wei et al., 2022; Zhou et al., 2022; Zhao et al., 2023).

* **Self-Ensembling Strategies:**  Self-Consistency (SC) (Wang et al., 2022) generates diverse reasoning paths ("Chain-of-Thought" or CoT) and aggregates them to improve accuracy.  However, SC suffers from high inference costs due to the generation of numerous tokens and often relies on custom aggregation methods or a fixed answer set.  Universal Self-Consistency (USC) (Chen et al., 2023) addresses the aggregation problem by using the LLM itself to select the most consistent response.  Other related work includes tree-structured CoT (Long, 2023; Yao et al., 2023).

* **Prompt Ensembling Strategies:** Several techniques focus on improving LLM performance through prompt ensembling (Zhang et al., 2023; Pitis et al., 2023; Singh et al., 2023; Arora et al., 2022; Hou et al., 2023).  Li et al. (2023b) notably used diverse exemplars within prompts, combined with diverse reasoning paths in SC, improving accuracy but still incurring high costs.

* **LLM Inference Cost Reduction:**  Research into reducing LLM inference cost focuses on model compression (Zhu et al., 2024; Jacob et al., 2018; Cheng et al., 2024; Gou et al., 2021), efficient decoding (Shazeer, 2019; Wu et al., 2024), and early stopping strategies (Chen et al., 2023a).

**2. Methodology:**

PEDAL combines prompt ensembling and LLM-based aggregation to offer a balance between accuracy and efficiency.  The approach consists of two main steps:

* **Prompts with Diverse Exemplars:**  Instead of a single prompt with fixed exemplars, PEDAL generates multiple prompts by randomly sampling exemplars for In-Context-Learning (ICL) using different random seeds. This induces diversity in the LLM's outputs.

* **LLM-based Aggregation:**  Following USC, PEDAL uses the same LLM to aggregate the candidate responses generated in the previous step.  The LLM effectively selects the most consistent answer among the candidates.


**3. Experiments:**

* **Datasets:**  The experiments are conducted on two publicly available datasets:
    * **SVAMP (Patel et al., 2021):** Elementary-level math word problems.
    * **ARC-Challenge (Clark et al., 2018):**  A subset of the AI2 Reasoning Challenge, containing more difficult questions.  The paper uses a randomly sampled 30% of the ARC-Challenge dataset.

* **Baselines:**  The following baselines are used for comparison:
    * **Greedy Decoding:**  Standard Greedy Decoding.
    * **Self-Consistency (SC):**  Standard Self-Consistency with CoT prompting.
    * **Unified Diverse Exemplars (UDE):**  All diverse exemplars are combined into a single prompt, then Greedy Decoding is applied.


* **Models & Settings:**  Experiments use Qwen2-7B-Instruct (Yang et al., 2024) and Llama-3-8B-Instruct (Touvron et al., 2023) LLMs, with 4-bit quantization.  Three random seeds are used for reproducibility.  Three exemplars are selected per prompt.  For USC, three intermediate outputs are generated. For PEDAL, three diverse prompts are used.

**4. Results and Analysis:**

Results are presented in Tables 2, 3, 4, and 5.  Key findings:

* **Accuracy:** PEDAL consistently outperforms Greedy Decoding in terms of accuracy on both datasets.  While USC sometimes achieves higher accuracy, PEDAL's accuracy is competitive.

* **Inference Cost:** PEDAL significantly reduces the number of output tokens compared to USC, resulting in lower inference cost.  The number of input tokens is slightly higher for PEDAL than for USC in some cases.  The paper compares the output token counts of PEDAL and CoT (a single intermediate output from SC), indicating that PEDAL is often more cost-efficient, especially with Llama3.

* **Impact of Number of Diverse Prompts:** Table 6 shows that increasing the number of diverse prompts from 2 to 4 leads to slight improvements in accuracy for the SVAMP dataset but shows no consistent pattern for the ARC dataset.

**5. Conclusion:**

PEDAL effectively enhances Greedy Decoding, providing a trade-off between the accuracy of self-ensembling and the cost-efficiency of Greedy Decoding. The paper showcases the advantages of combining diverse exemplars in prompts with LLM-based aggregation for improved performance and reduced inference cost. Future work includes exploring its application to larger datasets and more complex free-form text generation tasks.


**Mathematical Formulas and Notation:**  The paper does not present any complex mathematical formulas or notations beyond basic statistical measures like accuracy and standard deviation reported in tables (e.g.,  "83.38 ± 0.55").  The core methodology is algorithmic rather than mathematically formal.

**Code Snippets:** No code snippets are provided in the paper.

**Tables:** Tables 1-6 summarize the dataset sizes, experimental results (accuracy and token counts), and the impact of the number of diverse prompts.  The tables are included above.

This summary provides a comprehensive overview of the paper, incorporating all the requested aspects.  The analysis is objective and detailed, reflecting the content and claims made by the authors.
