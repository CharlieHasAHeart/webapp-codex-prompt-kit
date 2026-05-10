# Scoring Model

Total: 100 points

## Document Quality Score: 40

- Constraint Coverage: 8
- Command Determinism: 8
- Traceability Coverage: 8
- Acceptance Coverage: 8
- Length Budget Compliance: 8

## Codex Execution Score: 30

- First run success: 8
- Low clarification count: 6
- Low rework count: 6
- Low command error count: 5
- Low instruction violations: 5

## Delivery Quality Score: 20

- Validation pass rate: 8
- Acceptance pass rate: 5
- API contract match: 4
- DB schema match: 3

## Human Cost Score: 10

- Low manual prompt count: 5
- Low document fix count: 3
- Low human code edit ratio: 2

## Rating

| Score | Rating |
|---:|---|
| 90-100 | Codex-ready |
| 75-89 | Mostly ready |
| 60-74 | Usable but risky |
| <60 | Not ready |
