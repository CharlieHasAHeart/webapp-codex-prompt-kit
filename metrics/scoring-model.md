# Scoring Model

Use this lightweight scoring model to compare prompt versions.

Total: 100 points

## Document Quality Score: 40 points

| Metric | Points |
|---|---:|
| Constraint Coverage | 8 |
| Command Determinism | 8 |
| Traceability Coverage | 8 |
| Acceptance Coverage | 8 |
| Ambiguity Control | 8 |

## Codex Execution Score: 30 points

| Metric | Points |
|---|---:|
| First Run Success | 8 |
| Low Clarification Count | 6 |
| Low Rework Count | 6 |
| Low Command Error Rate | 5 |
| Low Instruction Violations | 5 |

## Delivery Quality Score: 20 points

| Metric | Points |
|---|---:|
| Validation Pass Rate | 8 |
| Acceptance Pass Rate | 5 |
| API Contract Match | 4 |
| DB Schema Match | 3 |

## Human Cost Score: 10 points

| Metric | Points |
|---|---:|
| Low Manual Prompt Count | 5 |
| Low Doc Fix Count | 3 |
| Low Human Code Edit Ratio | 2 |

## Score Bands

| Score | Meaning |
|---:|---|
| 90-100 | Codex-ready |
| 75-89 | Mostly ready |
| 60-74 | Usable but risky |
| < 60 | Not ready |

## Minimum Daily Metrics

For lightweight daily use, track only:

1. Clarification Count
2. Command Error Count
3. Rework Count
4. Validation Pass Rate
5. Manual Prompt Count
