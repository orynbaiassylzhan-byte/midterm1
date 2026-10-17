GitHub username: orynbaiassylzhan-byte
Session code: zebra-28
Fingerprint: a09860a8

AI-use declaration
For my first draft I used AI assistants and Python scripts to calculate the retrieval metrics, the Wilson intervals and the answers to the theory questions. After that I used Claude (Anthropic) to review my ANSWERS.md against my data sheet. Claude found errors in my first draft: the retrieval metrics in 1a and 1b did not match the data, the yearly model costs in 1c used a wrong token count per request, Pipeline L needs one server and not two, the overall accuracies and some interval bounds in 2a were wrong, and the answer to Q4 should only cover the account-number and birthday sets. I re-ran the retrieval metrics in Google Colab with code suggested by Claude, and the 95% Wilson intervals, the costs and the Part 3 values were recalculated with Claude's help. I replaced my values with the corrected ones and also adopted the corrected wording of the decision in 1d, the memo in 2b and the explanations in Part 3 that Claude suggested. These parts are therefore AI-assisted work, and I am declaring them as such.

Part 1: Retrieval and cost decision
1a. Metrics (all 60 questions)
| config | recall@1 | recall@3 | recall@5 | MRR@10 |
| ------ | -------- | -------- | -------- | ------ |
| A      | 0.550    | 0.667    | 0.783    | 0.629  |
| B      | 0.433    | 0.683    | 0.767    | 0.551  |

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

Decision: Both A candidates are over budget: A, k=3 costs 9199 and A, k=5 costs 12170 per year, against 8000. Only B fits. B, k=5 costs 6732 (1268 under budget) and finds the correct chunk for 0.767 of questions (95% CI 0.646-0.856), against 0.683 (0.558-0.787) for B, k=3 at 5937. The extra 795 per year buys 5 more correct retrievals out of 60. The intervals overlap, so this gain is suggestive, not proven, but the cost fits the budget, so I take the higher recall. By language, recall of B, k=5: EN = 0.90, RU = 0.70, KK = 0.70. With 20 questions per language these differences are noisy; Russian and Kazakh may still be weaker.

1e. Next measurement (at most 60 words)
Measure recall@5 of config B on at least 100 labelled questions per language (300 in total), then recompute the Wilson intervals. With 20 questions per language the intervals are very wide, too wide to tell whether Russian and Kazakh retrieve worse than English, or whether k=5 really beats k=3.

Part 2: A document pipeline you can defend
2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
| -------- | ---------------- | ---------------------------------- | -------------- | ----- | ----- | ----------- |
| H        | 0.878            | KK, degraded                       | 0.667          | 0.488 | 0.808 | 20732       |
| L        | 0.867            | KK, degraded                       | 0.767          | 0.591 | 0.882 | 9000        |

Personal-data pages per year sent abroad under H: 233600

2b. Memo (at most 300 words)
To: Head of Operations
Recommendation: use Pipeline L (own server in Kazakhstan).

Accuracy. H is correct on 158 of 180 test invoices (0.878), L on 156 (0.867). This one-point gap is well within noise at n=180. The weakest cell for both is Kazakh degraded scans: H 0.667 (95% CI 0.488-0.808), L 0.767 (0.591-0.882). The intervals overlap, so L's edge there is not proven, but H is certainly not better.

Cost. L needs one server, since 4000 pages per day is below its 6000 limit: 9000 USD per year. H costs 20732 USD per year (5040 input and 412 output tokens per page, 1.46 million pages), more than double.

Data. 16% of the pages carry a name and ИИН. Under H that is 233600 pages per year sent to a data centre abroad. Law No. 94-V on personal data requires a legal basis and adequate protection for cross-border transfers; a vendor's statement does not show this. L keeps these pages in Kazakhstan.

Risks and next steps. Kazakh degraded scans remain the weakest cell (0.77 at best), so route them to human review. One L server has room for 6000 pages per day, so 50% volume growth still fits, but a second server would be needed as a backup. Legal counsel should confirm the transfer rules before any move to H.

Part 3: Explain your own numbers
Q1
Number: 2.13
Why: The bill weights output tokens 6x more than input tokens. Kazakh output grows only 1.74x versus 2.37x for input, pulling the bill ratio to 2.13, below the 2.31 token ratio.

Q2
Number: 592
Why: No. 640 cl100k tokens is about 657 e5 tokens; the sentence starts at 592, past the 512 limit, so it is truncated and never embedded. The chunk cannot be retrieved by it. No error appears: truncation is silent.

Q3
Number: 80.2
Why: Softmax at T=1 gives Monday 0.741, Friday 0.183. Top-p 0.8 keeps both (cumulative 0.924), renormalised Monday is 80.2%. Top-p cuts the unlikely tail but keeps the ratios; lower temperature reshapes all probabilities.

Q4
Number: 21.7
Why: Excluded multiplication and arithmetic sequences: rule-based tasks, not memorised facts. For account numbers and birthdays the singleton rate bounds errors: (146x0.28 + 336x0.19) / 482 = 21.7%. A "never invent" prompt adds no knowledge, so the bound stays.

Q5
Number: 2.75
Why: 34B params on 1.87T tokens is 55 tokens per parameter; Chinchilla used 20, so the ratio is 2.75. By that criterion the model is over-trained. Chinchilla said models like GPT-3 were under-trained: too large for their data.
