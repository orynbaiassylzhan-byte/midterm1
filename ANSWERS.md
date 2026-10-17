GitHub username: orynbaiassylzhan-byte
Session code: zebra-28
Fingerprint: a09860a8

AI-use declaration
For my first draft I used AI assistants and Python scripts to calculate the retrieval metrics, the Wilson intervals and the answers to the theory questions. After that I used Claude (Anthropic) to review my ANSWERS.md against my data sheet. Claude found errors in my first draft: the retrieval metrics in 1a and 1b did not match the data, the yearly model costs in 1c used a wrong token count per request, Pipeline L needs one server and not two, the overall accuracies and some interval bounds in 2a were wrong, and the answer to Q4 should only cover the account-number and birthday sets. I re-ran the retrieval metrics in Google Colab with code suggested by Claude, and the 95% Wilson intervals, the costs and the Part 3 values were recalculated with Claude's help. I replaced my values with the corrected ones and also adopted the corrected wording of the decision in 1d, the memo in 2b and the explanations in Part 3 that Claude suggested. These parts are therefore AI-assisted work, and I am declaring them as such.

Part 1: Retrieval and cost decision
1a. Metrics (all 60 questions)
config | recall@1 | recall@3 | recall@5 | MRR@10
A | 0.550 | 0.667 | 0.783 | 0.629
B | 0.433 | 0.683 | 0.767 | 0.551

1b. Recall and 95 % Wilson interval of each candidate

| candidate | recall@k | lower | upper |
| --------- | -------- | ----- | ----- |
| A, k=3    | 0.667    | 0.541 | 0.773 |
| A, k=5    | 0.783    | 0.664 | 0.869 |
| B, k=3    | 0.683    | 0.558 | 0.787 |
| B, k=5    | 0.767    | 0.646 | 0.856 |

1c. Annual model cost in USD

| candidate | annual cost |
| --------- | ----------- |
| A, k=3    | 9199        |
| A, k=5    | 12170       |
| B, k=3    | 5937        |
| B, k=5    | 6732        |

1d. Decision (at most 150 words)
Chosen candidate: B, k=5

Decision: A, k=3 and A, k=5 cost 9199 and 12170 per year, both above the 8000 budget, so A is excluded despite its recall. Within budget remain B, k=3 (5937; recall 0.683, 95% CI 0.558-0.787) and B, k=5 (6732; recall 0.767, CI 0.646-0.856). B, k=5 has the higher recall and still fits the budget, so I choose it. The intervals overlap, so the gain over k=3 is suggestive, not proven. Recall of B, k=5 by language: EN = 0.90, RU = 0.70, KK = 0.70. With only 20 questions per language these values are noisy, so the differences between languages should not be over-interpreted.

1e. Next measurement (at most 60 words)
Label more test questions, for example 100 per language, and recompute recall of B, k=5 per language. With 20 questions per language the intervals are too wide to tell whether Russian and Kazakh really retrieve worse than English.

Part 2: A document pipeline you can defend
2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
| -------- | ---------------- | ---------------------------------- | -------------- | ----- | ----- | ----------- |
| H        | 0.878            | KK, degraded                       | 0.667          | 0.488 | 0.808 | 20732       |
| L        | 0.867            | KK, degraded                       | 0.767          | 0.591 | 0.882 | 9000        |

Personal-data pages per year sent abroad under H: 233600

2b. Memo (at most 300 words)
Recommendation: Pipeline L (own server in Kazakhstan).

Accuracy: H reaches 0.878 overall (158/180), L 0.867 (156/180); a one-point gap is within noise at n=180. In the weakest cell, Kazakh degraded scans, L reaches 0.767 (95% CI 0.591-0.882) against 0.667 (0.488-0.808) for H.

Cost: L needs one server (4000 pages per day is below its 6000 limit): 9000 USD per year. H costs 20732 USD per year (5040 input and 412 output tokens per page, 1.46 million pages).

Data: 16% of the 4000 daily pages carry a name and ИИН. Under H that is 233600 pages per year sent to a data centre abroad. Law No. 94-V requires a legal basis and adequate protection for such transfers, which we cannot verify for the vendor. L removes the issue.

Risk: Kazakh degraded scans remain the weakest cell in both pipelines (0.77 at best), so I suggest human review for them.

Part 3: Explain your own numbers
Q1
Number: 2.13
Why: Output tokens cost 6x input. Kazakh needs 2.37x more input but only 1.74x more output tokens, so the bill grows 2.13x, less than the 2.31x token count.

Q2
Number: 592
Why: No: the sentence starts at token 592, beyond e5's 512 limit, so it is truncated and never embedded. There is no error; the text is silently cut and retrieval just fails.

Q3
Number: 80.2
Why: Top-p keeps the smallest set of tokens with cumulative probability at least 0.8 (Monday, Friday) and renormalises, dropping the tail. Lower temperature instead sharpens the whole distribution and keeps every token possible.

Q4
Number: 21.7
Why: Left out multiplication and arithmetic sequences: they follow rules, not memorised one-off facts. The weighted singleton rate of account numbers and birthdays gives 21.7%. A "never invent" instruction adds no knowledge, so the bound stays.

Q5
Number: 2.75
Why: 55 tokens per parameter against Chinchilla's 20: the model is over-trained relative to the compute-optimal ratio. Chinchilla found models like GPT-3 under-trained: too large for their data.
