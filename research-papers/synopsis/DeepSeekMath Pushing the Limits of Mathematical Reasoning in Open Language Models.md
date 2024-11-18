## DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models - An Extensive Summary

This paper introduces DeepSeekMath 7B, an open-source large language model (LLM) designed for superior mathematical reasoning capabilities.  It surpasses existing open-source models and approaches the performance of closed-source models like GPT-4 and Gemini-Ultra on various benchmarks.  The paper details its methodology, focusing on two key innovations: a high-quality, large-scale pre-training dataset and a novel reinforcement learning algorithm.

**1. Literature Review:**

The paper reviews the advancements in LLMs for mathematical reasoning, highlighting the performance gap between closed-source (GPT-4, Gemini-Ultra) and open-source models.  It cites previous work on quantitative and geometric reasoning benchmarks and the use of LLMs for assisting in complex mathematical problem-solving.  The authors note the lack of publicly available, high-performing open-source models in this domain.


**2. Methodology:**

The DeepSeekMath approach involves three main stages:

**2.1 Math Pre-training at Scale:**

* **Data Collection:** The core of the approach is the creation of the DeepSeekMath Corpus, a massive dataset of 120 billion math-related tokens.  This dataset is mined from Common Crawl using an iterative process:
    * **Iteration 1:** A fastText classifier is trained using a seed corpus (OpenWebMath) for positive examples and randomly selected Common Crawl pages for negative examples. This classifier is used to filter Common Crawl for relevant pages.
    * **Subsequent Iterations:**  The classifier is iteratively refined by identifying and manually annotating additional high-quality mathematical domains within Common Crawl, adding these to the seed corpus.  This iterative process continues until diminishing returns are observed (nearly 98% data collected in the 3rd iteration).  The final corpus consists of 35.5 million mathematical web pages.
    * **Benchmark Decontamination:**  A crucial step involves removing any text segments from the corpus that overlap with established mathematical benchmarks (GSM8K, MATH, CMATH, AGIEval) to prevent contamination.

* **Model Initialization and Pre-training:** DeepSeekMath-Base 7B is initialized using DeepSeek-Coder-Base-v1.5 7B (a pre-trained code model). The pre-training process involves continual learning on the DeepSeekMath Corpus, along with data from AlgebraicStack, arXiv, GitHub code, and general natural language data (English and Chinese). The rationale behind using a code-trained model is that it provides a better foundation for mathematical reasoning.

**2.2 Supervised Fine-Tuning (SFT):**

DeepSeekMath-Base 7B undergoes SFT using a dataset of 776,000 examples, including English and Chinese problems with solutions in chain-of-thought (CoT), program-of-thought (PoT), and tool-integrated reasoning formats.  The English data comprises annotated GSM8K and MATH problems, a subset of MathInstruct, and Lila-OOD training data. The Chinese data includes K-12 problems across numerous sub-topics.  This stage results in DeepSeekMath-Instruct 7B.


**2.3 Reinforcement Learning (RL) with Group Relative Policy Optimization (GRPO):**

To further enhance performance, DeepSeekMath-Instruct 7B undergoes RL using GRPO.  GRPO is a variant of Proximal Policy Optimization (PPO) that improves efficiency by eliminating the critic model.  Instead, it estimates the baseline from group scores (average reward of multiple sampled outputs for the same question).  The paper details two types of supervision:

* **Outcome Supervision:** The reward is assigned only at the end of the generated solution.
* **Process Supervision:** Rewards are assigned at the end of each reasoning step.

The RL process uses a subset of the English instruction tuning data (GSM8K and MATH). The GRPO objective function is presented as:

```latex
\begin{split}\mathcal{J}_{GRPO}(\theta)&=\mathbb{E} [q\sim P(Q),\{o_{i}\}_{i=1}^{G}\sim\pi_{\theta_{old}}(Q|q)]\\ &\frac{1}{G}\sum_{i=1}^{G}\frac{1}{|o_{i}|}\sum_{t=1}^{|\alpha|} \left\{\min\left[\frac{\pi_{\theta}(o_{i,t}|q,o_{i<t})}{\pi_{\theta_{old}}(o_ {i}|q,o_{i<t})}\hat{A}_{i,t},\text{clip}\left(\frac{\pi_{\theta}(o_{i,t}|q,o_{ i<t})}{\pi_{\theta_{old}}(o_{i,t}|q,o_{i<t})},1-\varepsilon,1+\varepsilon \right)\hat{A}_{i,t}\right]-\beta\text{D}_{KL}\left[\pi_{\theta}||\pi_{ref} \right]\right\},\end{split} 
```

where:

*  \( \pi_{\theta} \) and \( \pi_{\theta_{old}} \) are the current and old policy models.
* \( q \) is a question, and \( o_i \) are outputs.
* \( G \) is the number of samples per question.
* \( \hat{A}_{i,t} \) is the advantage (normalized reward in outcome supervision or sum of future normalized rewards in process supervision).
* \( \varepsilon \) is a clipping hyperparameter.
* \( \beta \) controls the KL divergence penalty between the current policy and a reference policy (usually the SFT model).
* \( \text{D}_{KL} \) is the Kullback-Leibler divergence.

The paper also introduces an iterative RL approach where the reward model is retrained after each policy update. This final stage produces DeepSeekMath-RL 7B.


**3. Results and Evaluation:**

The models were evaluated on several English and Chinese mathematical reasoning benchmarks:

* **English:** GSM8K, MATH, SAT, OCW Courses, MMLU-STEM
* **Chinese:** MSSM-zh, CMATH, Gaokao-MathCloze, Gaokao-MathQA

Evaluation included both chain-of-thought (CoT) reasoning and tool-integrated reasoning (using Python). DeepSeekMath-Base 7B significantly outperformed other open-source base models, even exceeding Minerva 540B (a much larger closed-source model) on some metrics. DeepSeekMath-Instruct 7B further improved performance, surpassing most open-source instruction-tuned models. Finally, DeepSeekMath-RL 7B achieved the highest scores, reaching over 50% accuracy on the challenging MATH benchmark for the first time in the open-source community, approaching the performance of GPT-4 and Gemini-Ultra.  The model was also evaluated on general language understanding, reasoning, and code generation benchmarks (MMLU, BBH, HumanEval, MBPP), showing improvements resulting from the math pre-training.


**4. Discussion and Analysis:**

The paper includes an extensive discussion of the pre-training and RL experiments. Key findings include:

* **Code training benefits mathematical reasoning:**  Pre-training on code improves performance on mathematical tasks, both with and without tool use.
* **ArXiv papers show limited effectiveness:**  Contrary to expectations, using only arXiv papers for pre-training did not significantly improve performance.
* **Unified paradigm for RL methods:**  The authors propose a unified framework to analyze different RL methods (SFT, RFT, DPO, PPO, GRPO), highlighting the roles of data source, reward function, and algorithm (gradient coefficient).
* **RL improves robustness:** RL primarily enhances the model's ability to select the correct answer from the top K predictions (Maj@K) rather than fundamentally improving its reasoning capabilities.
* **Future directions for RL:** The authors discuss potential improvements focusing on data sampling strategies, more robust algorithms, and improved reward models (generalization, uncertainty handling, process supervision).


**5. Conclusion:**

DeepSeekMath demonstrates that large-scale, high-quality pre-training data and efficient RL algorithms can significantly improve the mathematical reasoning capabilities of open-source LLMs. The paper provides valuable insights into data selection, algorithm design, and the potential limitations of current RL approaches. The authors highlight several promising avenues for future research to further enhance the capabilities of LLMs in mathematical reasoning.
