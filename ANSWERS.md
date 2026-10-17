GitHub username: orynbaiassylzhan-byte
Session code: zebra-28
Fingerprint: a09860a8

AI-use declaration
Used AI assistants and Python scripts to calculate metrics, Wilson intervals, and answer theoretical questions.

Part 1: Retrieval and cost decision
1a. Metrics (all 60 questions)
config | recall@1 | recall@3 | recall@5 | MRR@10
A | 0.533 | 0.717 | 0.817 | 0.638
B | 0.383 | 0.617 | 0.733 | 0.505

1b. Recall and 95 % Wilson interval of each candidate
candidate | recall@k | lower | upper
---|---|---|---
A, k=3 | 0.717 | 0.593 | 0.816
A, k=5 | 0.817 | 0.701 | 0.894
B, k=3 | 0.617 | 0.490 | 0.729
B, k=5 | 0.733 | 0.610 | 0.828

1c. Annual model cost in USD
candidate | annual cost
---|---
A, k=3 | 6825
A, k=5 | 8150
B, k=3 | 3110
B, k=5 | 3745

1d. Decision (at most 150 words)
Chosen candidate: (A, k=3)
Decision: Config (A, k=3) is chosen because its annual cost ($6,825) is well within the $8,000 budget, unlike (A, k=5) which exceeds it. Although (A, k=5) has slightly higher recall, the Wilson intervals overlap, showing no statistically significant difference between k=3 and k=5 for config A. Per-language recall for the choice (A, k=3): EN = 0.80, RU = 0.70, KK = 0.65. Kazakh (KK) is served worst due to higher token length and difficulty.

1e. Next measurement (at most 60 words)
A human evaluation on a sample of low-confidence Kazakh queries to determine if semantic misses are due to chunk boundary fragmentation or translation quality issues.

Part 2: A document pipeline you can defend
2a. Numbers
pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost
---|---|---|---|---|---|---
H | 0.933 | KK, degraded | 0.667 | 0.490 | 0.814 | $28,950
L | 0.883 | KK, degraded | 0.767 | 0.593 | 0.882 | $18,000

Personal-data pages per year sent abroad under H: 233,600

2b. Memo (at most 300 words)
Memorandum to Head of Operations:
We evaluated Pipeline H (hosted API abroad) and Pipeline L (local server in Kazakhstan). Pipeline H serves Kazakh degraded scans worst (accuracy 0.67). Legally, under Kazakhstan's Personal Data Law (No. 94-V), 233,600 pages containing personal data (ИИН) would cross borders under H, which violates compliance since vendor statements are insufficient. Pipeline L keeps data locally at an annual cost of $18,000. We recommend Pipeline L to ensure full legal compliance, despite slightly lower overall accuracy, mitigating severe regulatory risks.

Part 3: Explain your own numbers
Q1
Number: 2.13
Why: Output tokens cost more than input tokens, and Kazakh text uses more tokens per request, driving up the bill disproportionately.

Q2
Number: 592
Why: The answer starts beyond the 512 e5-small token limit, so it cannot be retrieved, without throwing an error.

Q3
Number: 80.2
Why: Top-p cuts off the tail of the probability distribution at cumulative 80% and renormalizes, unlike temperature scaling which alters all relative ranks.

Q4
Number: 28.0
Why: Facts seen only once cannot be memorized reliably by base models. System prompts cannot inject missing knowledge.

Q5
Number: 2.75
Why: The model is over-trained compared to Chinchilla's compute-optimal ratio of 20 tokens per parameter.
