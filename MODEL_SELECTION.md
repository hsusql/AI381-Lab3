# Model selection

## The question

The model predicts whether a customer is likely to churn (leave the service). A false negative may cause the company to miss a customer who is likely to leave, while a false positive may lead to unnecessary customer retention efforts.

## Candidates

| Candidate | Why I tried it | AUC | F1 | F1 per slice (lowest) | p95 latency (ms) | Size (KB) | MLflow run ID |
|---|---|---:|---:|---:|---:|---:|---|
| Logistic Regression | A simple and interpretable baseline for comparison. | 0.7982 | 0.4948 | 0.2500 | 25.6788 | 3.5479 | 77350a65889043cbbac61f38814915df |
| Random Forest | To test a tree-based ensemble that can capture nonlinear relationships. | 0.7589 | 0.4536 | 0.2222 | 19.6285 | 3160.127 | b4cb789b2dbe4377b54c5c3649d66796 |
| Gradient Boosting | To compare boosting with the other model families. | 0.7476 | 0.4167 | 0.2222 | 4.8857 | 141.6758 | a0f2fb04f6c9487dabfc74c28cf094b3 |

## The champion

I selected Logistic Regression as the champion because it achieved the highest overall F1 (0.4948) and AUC (0.7982). It also had the best lowest-slice F1 (0.2500) and the smallest model size (3.55 KB).

I accepted its slightly higher inference latency (25.68 ms) compared with Random Forest and Gradient Boosting, because it remains within my proposed 30 ms latency budget. Gradient Boosting was the fastest model, but its overall F1 was lower (0.4167).

## My promotion rules

- Latency budget (p95): **30 ms**, because this allows the best-performing candidate to be selected while keeping prediction latency relatively low.
- Minimum gain to replace the champion (F1 points): **0.01**, because a new model should provide at least a one-percentage-point improvement in F1 to justify replacing the current champion.

## What I would check next

I would investigate the F1 scores for each contract type, especially the one-year and two-year groups, and check the number of customers in each group. I would also monitor changes in customer data over time to detect data drift.