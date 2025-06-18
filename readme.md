https://openreview.net/forum?id=b0jNBAVfdk#discussion

# Response to Reviewer FiGH
Thanks for your valuable comments.

## Q1:Ethics, Misuse, Disclosure 
The "Incremental Exploitation Prompts" aren't a static dataset that's easily misused. Instead, they're a mechanism generated in real-time, under a specific strategy. This dynamic nature helps us counteract fine-tuning defenses against malicious prompts and prevents easy misuse. For an everyday user to achieve an attack effect comparable to MIEJ, they'd need a deep understanding and the ability to adjust prompts in real-time. This is beyond the technical scope of an average user.

## Q2:LLM Judge Bias
GPT-4o's ability to assess harmful content has been validated by its high correlation with human judgments. As we state in the paper (e.g., Section 4.1 "Experimental Settings"), comparative experiments with 500 human-annotated samples showed a Pearson correlation coefficient between GPT-4o and human judgments as high as 0.96. We chose GPT-4o for evaluation precisely because it has more advanced language understanding and judgment capabilities than all the LLM models we tested. By using a higher-tier model to scrutinize the behavior of lower-tier target models, we can mitigate potential inherent biases or limitations that might arise during the evaluation process.

## Q3:Response Inertia Mechanism
We plan to investigate this further in future work. This includes using explainable AI techniques (like attention mechanism analysis and activation pattern visualization) to explore the specific LLM mechanisms within the model that cause the gradual weakening of safety alignment during MIEJ attacks. Understanding these micro-mechanisms will help us describe and exploit 'response inertia' more precisely, providing critical insights for developing more robust defense measures.

## Q4:MIEJ Future Defenses
We anticipate that MIEJ will likely remain effective against future LLMs with more sophisticated defense mechanisms. The core of MIEJ lies in exploiting the LLM's response inertia, which is inherently linked to the model's in-context learning ability and the mechanisms of autoregressive generation. We believe that completely eliminating this inertia without compromising the LLM's core performance might represent a fundamental trade-off. If future defense mechanisms interfere too much with the contextual information flow, the model could become incoherent or "forgetful" in normal dialogue, which would impact its usability.

# Response to Reviewer ZTs9
Thanks for your valuable comments.
## Q1, L1 & L6:Notations & Format problems
We humbly clarify that T was explicitly introduced in Section 3.1, representing the target LLM. Thank you for your careful review. We identified two instances where definitions could be clearer, which will be addressed if given revision opportunity.
## Q2:Methodology Clarity
In our methodology, the concatenation operator (⊕) serves to sequentially combine prior prompts and model responses, as detailed in Equation (2).The "increment" concept denotes our strategy of gradually escalating conversational malice. This concept is elaborated in the Introduction and the "Attack Preparation" section.
## Q3 & L4:Harm Threshold Analysis
We have already validated harm threshold 2 (Pearson correlation 0.96) against human expert judgments in the Experimental Settings section. For further detail, we are pleased to provide a comprehensive evaluation of Recall, Precision, and F1-score across different harm thresholds, based on 500 human-annotated samples.
|Harm Threshold|Recall|Precision|F1-score|
|--|--|--|--|
|2|0.92|0.94|0.93|
|3|0.85|0.95|0.90|
|4|0.77|0.98|0.87|
|5|0.65|0.99|0.78|
## Q4 & L5:Prompt Selection Criteria
Our "Incremental exploitation prompts construction" employs a random, dynamic prompt generation strategy to counter fine-tuning defenses. Heuristic selection (distant embedding clusters post-refusal) was explored but offered no significant gain (reducing average query attempts by 0.1), added complexity, and contradicted our dynamic generation goal.
## Q5 & L3:Malice Level Definition
Given the page limit for ACM Multimedia papers, we have placed detailed definitions of some secondary but easily understandable concepts, including the "malice level," in Supplementary Material Section A.2. We kindly request that you refer to that section for more comprehensive information.
## L2:Response Inertia Exlpanation
To support "response inertia," we conducted an analytical experiment on token probability distribution. We used refusal-answer prefixes from a control model as baselines. Experimental Results: As MIEJ rounds increased, the average ranking of refusal keywords significantly decreased, empirically confirming "response inertia."
|Round/Token|"cannot"|"sorry"|"don"|"but"|
|--|--|--|--|--|
|0|1|1|1|1|
|1|2.4|2.6|2.1|3.8|
|2|3.1|4.8|4.9|4.2|
|3|4.7|5.3|5.5|4.1|
|4|4.3|5.0|6.9|5.8|


# Response to Reviewer Gm4v
Thanks for your valuable comments.

## Q1: How would MIEJ perform against state-of-the-art multimodal models like Gemini? Or any strategy differences for the closed-source models
 MIEJ Multimodal/Closed-source
MIEJ, as a text-based jailbreak method, performs well on the text modality of multimodal models such as Gemini, achieving comparable performance to GPT-4. This effectiveness stems from MIEJ's clever exploitation of the LLM's core in-context learning ability and response inertia, characteristics commonly found across various advanced models. 

As emphasized in the paper, MIEJ is an effective "black-box" method. Consequently, MIEJ is also effective on closed-source models like GPT-4, achieving Jailbreak Success Rates comparable to state-of-the-art methods.

## Q2: The "response inertia" hypothesis is based on empirical observations. Can you supplement with more empirical analyses or gradient-based explanations of why LLMs lower safety thresholds in multi-round interactions? Response Inertia Explanation
To strengthen our findings on "response inertia", we conducted an analytical experiment, observing its impact from the perspective of token probability distribution. For each malicious question, we extracted the prefix text sequence generated by the control group model when it produced a refusal answer. We used these prefixes as a baseline to observe the model's propensity to generate subsequent refusal-prone tokens under the influence of different MIEJ contexts. 
Experimental Results: We observed that the average ranking of keywords originally prominent in refusal answers significantly decreased as the number of MIEJ attack rounds increased. This empirically supports the "response inertia" concept.

| Round / Token | "cannot" | "sorry" | "don" | "but" | "safety" |
|---------------|----------|---------|-------|-------|----------|
| 0 (Control)   | 1.0      | 1.0     | 1.0   | 1.0   | 1.0      |
| 1             | 2.42     | 2.67    | 2.15  | 3.89  | 2.03     |
| 2             | 3.18     | 4.82    | 4.97  | 4.21  | 2.65     |
| 3             | 4.75     | 5.34    | 5.51  | 4.13  | 2.12     |
| 4             | 4.31     | 5.05    | 6.98  | 5.87  | 2.56     |

## Q3: In scenarios with strict token limits (e.g., mobile assistants), how does MIEJ adapt when conversation history exceeds context windows? Are there optimisation strategies Token Limit Adaptation
MIEJ adapts to strict token limits by leveraging two strategies:
Weaker Alignment in Small Models: Smaller models, due to scaling laws, often have weaker safety alignment. This means less malicious context is needed for jailbreaking, allowing MIEJ to be efficient even with limited context windows.
Dynamic Context Summarization: MIEJ could adapt by implementing dynamic context summarization. This strategy would involve intelligently pruning older, less critical dialogue while prioritizing key malicious segments to maintain response inertia within the given context window. 

# Response to Reviewer PSSg
Thanks for your valuable comments.

## Q1: The high success rates are compelling, but were tests conducted on non-English-dominated models (e.g., Alibaba’s Qwen or Baidu’s ERNIE)? How does MIEJ perform on models with stricter regional safety alignments? on-English Models
In Section 4.1, we mentioned testing on Chinese models (ChatGLM3-6B, Qwen2-7B-Instruct), which showed over 97% Jailbreak Success Rates (Table 2). This demonstrates that the in-context learning and response inertia exploited by MIEJ are universal weaknesses.

## Q2: The paper critiques single-turn defenses but does not propose countermeasures. Could techniques like conversation history sanitization or dynamic harm-scoring thresholds mitigate MIEJ? Countermeasures Proposal
We believe that countermeasures like conversation history sanitization or dynamic harm-scoring thresholds would come at the cost of model performance (inference time, token count). Moreover, they might disrupt contextual coherence due to false positives, impacting user experience and usability.

## Q3: The attack assumes fully automated interactions. How does MIEJ’s success rate change if human moderators intervene during suspicious multi-turn escalations? Human Intervention Impact
MIEJ relies on automated interactions, and human intervention could indeed disrupt its effectiveness. However, real-time "human-in-the-loop" moderation at the scale of large LLM systems is impractical due to cost, efficiency, and responsiveness limitations.

## Q4: The Fleiss’ Kappa score (0.85) suggests strong inter-rater agreement, but how were edge cases (e.g., "grey area" prompts) resolved? Were adversarial examples tested for annotation robustness? Edge Case Resolution
The Fleiss' Kappa score (0.85) primarily validated the high consistency among human experts in providing few-shot examples, ensuring their meticulousness. For expanded malicious prompts, we utilized a fine-tuned XLM-RoBERTa model to assess alignment with expert judgments, as detailed in Section 3.2, "Attack Preparation." 

## Q5: The experiments use curated datasets (AdvBench). How scalable is MIEJ in noisy, real-world chat environments where users mix benign and malicious queries? Real-World Scalability
We conducted supplementary experiments confirming MIEJ's effectiveness in noisy, real-world chat (e.g., GPT-4). Its core mechanisms persist despite benign query interference, only slightly increasing interaction rounds (15-20%) while maintaining successful harmful output.

## Q6: While MIEJ shows cross-model transferability, did the authors evaluate its effectiveness against models with explicit multi-turn safety fine-tuning? Multi-turn Fine-tuning
Our tested models, including GPT-3.5/4 and the Mistral/Qwen/Llama series, already incorporate extensive multi-turn safety fine-tuning, as detailed in their technical reports and community introductions. MIEJ's effectiveness against these models has been demonstrated.

## Q7: Low-resource languages exhibit higher vulnerability. Does this correlate with training data volume, or are safety filters inherently weaker for these languages? Low-Resource Vulnerability
We believe that higher vulnerability in low-resource languages stems primarily from the scarcity of training data. Due to limited safety alignment data, LLMs struggle to generalize harmful semantics and cultural nuances, thus undermining their safety mechanisms, as shown in Figure 5 in the paper.

## Q8: The method relies on long-context windows. How does performance degrade with models enforcing sh Short Context Window
For models with strict token limits, typically smaller parameter models, their safety alignment mechanisms are generally weaker due to the scaling-law. These models require less malicious context for optimal jailbreaking, a phenomenon observed in our experiments where Mistral, for instance, required fewer rounds than Qwen for successful jailbreaking (Figure 6).




# Response to Reviewer 6ArY
Thanks for your valuable comments.

## Q1: Could the authors provide a more detailed explanation of how the prompt set Q is constructed? Q2:What specific LLMs were used for prompt augmentation, and what prompts or templates guided the augmentation?   Q1:Prompt Set Construction & Q2: Prompt Augmentation Details
Due to main paper's page limits, detailed prompt set Q construction and augmentation specifics are in Supplementary Material A.2, "Malicious Question Generation." Briefly, human experts manually created initial malicious questions across topics/malice levels. These were then fed to GPT-4o API for few-shot prompt augmentation. Experts subsequently reviewed augmented questions for accuracy and malice-level alignment, resolving discrepancies by consensus. Finally, as noted in Section 3.2 "Attack Preparation," a fine-tuned XLM-RoBERTa model performed a two-stage validation for objective malice-level alignment. We direct you to Supplementary Material A.2 for the complete system prompts and full details.


## Q3: Have the authors conducted any experiments to assess the success rate of MIEJ under the presence of defense mechanisms, such as safety filters or behavioral detection systems? Defense Mechanism Assessment
We conducted supplementary experiments on MIEJ's effectiveness against defense mechanisms.

1. Blacklist Filter Defense Experiment:
We evaluated MIEJ against a keyword blacklist filter using Qwen2-7B-Instruct and GPT-4. The filter blocked responses containing malicious keywords.

| Model                     | Max Malicious Conversation Turns (Turns) |
|---------------------------|------------------------------------------|
| Qwen2-7B-Instruct (w/o filter) | 48                                       |
| Qwen2-7B-Instruct (w/ filter)  | 18                                       |
| GPT-4 (w/o filter)        | 48                                       |
| GPT-4 (w/ filter)         | 16                                       |

2. Optimization Strategy Against Blacklists:
To counter blacklists, we used a prompt optimization strategy: a named entity substitution LLM replaced sensitive keywords (e.g., "bomb" to "explosive device"). This restored MIEJ's attack effectiveness by circumventing literal keyword matching.

3. LLM-based (Non-Rule) Filters:
While LLM-based judgment filters significantly hinder MIEJ by recognizing subtle malicious intent, their practical deployment faces substantial challenges. Such defenses incur nearly double the time and token costs due to cascaded LLM inference, rendering them unfeasible for large-scale, real-time applications. Our work highlights existing LLM defense vulnerabilities under real-world cost and efficiency constraints, pushing for more efficient and intelligent defense paradigms.

















