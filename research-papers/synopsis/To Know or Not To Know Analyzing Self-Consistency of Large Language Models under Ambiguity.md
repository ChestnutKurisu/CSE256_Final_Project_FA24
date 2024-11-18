This paper investigates the self-consistency of large language models (LLMs) when dealing with entity ambiguity.  The authors argue that while LLMs demonstrate impressive performance due to their vast factual knowledge, inconsistencies in their responses, particularly under ambiguity, raise concerns about their trustworthiness.  The core focus is on how LLMs handle ambiguous entities (entities with multiple meanings, e.g., "Apple" as a fruit or company).

**Methodology:**

The study employs a four-part experimental protocol designed to disentangle an LLM's "knowing" (awareness of multiple entity readings) from its "applying knowledge" (correctly selecting the appropriate reading based on the context).  The methodology uses 49 ambiguous entities, each with at least two interpretations: a specific entity type (animal, fruit, myth, person, location, abstract concept) and a company name (Table 1).  The research questions are:

1. **RQ1:** How well do LLMs resolve entity ambiguity?
2. **RQ2:** Is the ability to infer the correct entity type biased towards "preferred readings"?
3. **RQ3:** Can LLMs self-verify their answers?

Four studies comprise the evaluation:

* **Study 1 (K): Knowledge Verification:**  Assesses whether the LLMs possess knowledge of both interpretations of each ambiguous entity. Prompts like `"Tell me about <entity type> <entity>"` are used.  A secondary study (1a) directly asks about ambiguity awareness using prompts like `"Can <entity> mean anything other than <entity type>? Answer only with Yes or No."`

* **Study 2 (K + A): Eliciting Preferences:** Determines each LLM's preferred reading for each entity type by giving an underspecified grouping task: `"Group the following entities according to what they all have in common: <entities>"`. The model's choice reveals its preferred interpretation.

* **Study 3 (K → A): Knowledge to Application:** Evaluates the ability to apply knowledge.  LLMs are prompted with questions requiring the correct entity reading, e.g., `"Provide the <entity property> for <entity>"` (ambiguous) and compared to a non-ambiguous baseline with explicit entity type hints, e.g., `"Provide the <entity property> for <entity type> <entity>"`.

* **Study 4 (A → K): Applying to Knowing:** Tests self-consistency.  Factual information extracted from Study 3 responses is used to create verification prompts, e.g., `"Does an animal X have <info> speed?"`.  The focus is on consistency within the model's own responses, not factual accuracy.

**Results and Discussion:**

* **RQ1:**  While Study 1 confirmed that LLMs "know" about multiple readings, Study 3 showed that they struggle to apply this knowledge correctly when given ambiguous prompts, achieving only 85.3% accuracy on average.  Non-ambiguous prompts (with entity type hints) yielded 90.5% accuracy, indicating a significant ambiguity-related performance drop.

* **RQ2:**  A strong bias towards preferred readings was observed (Table 2).  Accuracy was 85.4% for preferred readings and only 74.5% for alternative readings in ambiguous prompts. This bias correlates with entity popularity on Wikipedia, suggesting that frequent exposure to a specific meaning in the training data influences the LLM's preference.

* **RQ3:**  LLMs demonstrated poor self-verification capabilities (Figure 3).  Even when given the same information shortly after providing it, models frequently failed to confirm the information's correctness.  Further probing with explanatory prompts revealed that models sometimes contradict themselves in the first answer but offer correct information in the subsequent explanation.

**Overall, the paper's findings highlight a critical gap in current LLMs:**  They may possess factual knowledge, but they struggle with consistent and reliable application of that knowledge under ambiguous situations, exhibiting bias towards preferred readings and a lack of self-consistency.  The authors conclude that addressing entity ambiguity is crucial for building more trustworthy LLMs.

**Limitations:**

The study uses a simplified definition of ambiguity (company vs. non-company).  Further research into the varying degrees of polysemy and potential ambiguity in entity properties is suggested.


**Code and Data:**

The authors provide code and model outputs on GitHub: [https://github.com/anasedova/ToKnow_or_NotToKnow](https://github.com/anasedova/ToKnow_or_NotToKnow)

The Appendices provide further detail on entity popularity analysis, annotation procedures, prompt variations, and additional experimental results, including tables showing detailed accuracy breakdown and examples of LLM responses across all studies.  These details are too extensive to fully reproduce here.
