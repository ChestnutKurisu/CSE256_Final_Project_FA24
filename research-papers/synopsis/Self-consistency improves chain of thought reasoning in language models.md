This paper, "Self-Consistency Improves Chain of Thought Reasoning in Language Models," introduces a novel decoding strategy called self-consistency to enhance the reasoning capabilities of large language models (LLMs).  The core idea builds upon chain-of-thought (CoT) prompting, a technique that encourages LLMs to generate intermediate reasoning steps before arriving at a final answer.  The paper argues that while CoT prompting improves reasoning, it can be further improved by leveraging the inherent diversity of reasoning paths leading to a correct solution.

**Literature Review:**

The paper reviews existing literature on reasoning in LLMs, highlighting the limitations of solely increasing model scale to improve reasoning abilities. It mentions previous work on chain-of-thought prompting (Wei et al., 2022), which demonstrated significant improvements in multi-step reasoning tasks.  The authors also discuss existing decoding strategies like greedy decoding, temperature sampling, top-k sampling, and nucleus sampling, as well as re-ranking methods that use additional verifiers or human annotations to improve generation quality (Cobbe et al., 2021; Thoppilan et al., 2022).  The paper contrasts self-consistency with these methods, emphasizing its unsupervised nature and lack of need for additional training or data.

**Methodology:**

The core of the methodology is the self-consistency decoding strategy.  It consists of three steps:

1. **CoT Prompting:**  The LLM is prompted with a question and a few manually written examples demonstrating chain-of-thought reasoning.

2. **Diverse Path Sampling:** Instead of using greedy decoding (selecting the single most likely next token at each step), the model samples multiple reasoning paths from its decoder.  This leverages various sampling techniques like temperature sampling, top-k sampling, and nucleus sampling, with parameters tuned for each model.

3. **Answer Marginalization:** The sampled reasoning paths, each potentially leading to a different answer (denoted as  $\mathbf{a}_{i} \in \mathbb{A}$, where  $i = 1, \dots, m$ indexes the  $m$ samples), are analyzed. The final answer is chosen by marginalizing out the reasoning paths and selecting the most frequent answer (a majority vote).  The paper also explores weighted averaging and weighted summation using the unnormalized or normalized probabilities of generating each path and answer pair:

   $P(\mathbf{r}_{i},\mathbf{a}_{i}\mid\text{prompt},\text{question})=\exp^{\frac{1 }{R}\sum_{k=1}^{K}\log P(t_{k}|\text{prompt},\text{question},t_{1},\ldots,t_{ k-1})}$ (Equation 1)

   where  $\mathbf{r}_{i}$ represents the reasoning path,  $t_{k}$ is the  $k$-th token,  $K$ is the total number of tokens, and  $R$ is a normalization factor (used for the normalized version).  The paper finds that the unweighted sum (majority vote) performs comparably to the normalized weighted sum, suggesting the model doesn't reliably distinguish between correct and incorrect reasoning paths.


**Experiments and Results:**

The paper evaluates self-consistency on several arithmetic and commonsense reasoning benchmarks, including:

* **Arithmetic Reasoning:** GSM8K, SVAMP, AQuA, AddSub, MultiArith, ASDiv
* **Commonsense Reasoning:** CommonsenseQA, StrategyQA, ARC-challenge
* **Symbolic Reasoning:** Last letter concatenation, Coinflip

Four LLMs of varying scales are used: UL2-20B, GPT-3-175B, LaMDA-137B, and PaLM-540B.  The results (Tables 2 and 3) consistently show that self-consistency significantly improves accuracy compared to CoT prompting with greedy decoding across all models and tasks.  The improvement is more pronounced for larger models.  Self-consistency achieves state-of-the-art results on many benchmarks.  Figure 2 shows that accuracy generally increases with the number of sampled paths. Table 4 provides illustrative examples where self-consistency corrects errors made by greedy decoding.

Additional experiments demonstrate that self-consistency:

* **Improves robustness to imperfect prompts:** Even with noisy prompts, self-consistency maintains performance better than greedy decoding.
* **Works with different sampling strategies:** The improvement is relatively consistent across different sampling parameters.
* **Outperforms other approaches:** Self-consistency surpasses sample-and-rank, beam search, and ensemble methods.  Table 7 shows its superiority over prompt-order and multi-prompt ensemble techniques.


**Discussion and Conclusion:**

The paper concludes that self-consistency is a simple yet effective method for boosting the reasoning capabilities of LLMs.  It highlights the benefits beyond improved accuracy, such as providing rationales and uncertainty estimates. The authors acknowledge the increased computational cost but suggest using a smaller number of sampling paths to mitigate this.  They also mention the need for future work to address issues like generating nonsensical reasoning paths and improving model calibration and factuality.  The reproducibility statement notes that two of the four models used (UL2 and GPT-3) are publicly available.

**Code Snippets (Conceptual):**

The paper doesn't provide specific code, but the core self-consistency algorithm can be conceptually represented as follows (pseudocode):

```python
def self_consistency(model, prompt, question, num_samples):
  answers = []
  for _ in range(num_samples):
    reasoning_path, answer = model.generate_with_cot(prompt, question) # Assumes a function to generate with CoT
    answers.append(answer)
  return max(set(answers), key=answers.count) # Majority vote

```

This summary provides a comprehensive overview of the paper, including all the requested elements.  Note that some tables and figures are only described qualitatively due to the limitations of reproducing them in this text format.  The complete paper should be consulted for the detailed numerical results.
