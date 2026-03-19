# Data Inventory

## Purpose

This document explains how raw datasets are organized for FinPersoAI, why each file is placed in its category, and where the team should drop data after downloading it from the shared Drive.

## Classification principle

Datasets are grouped by business domain first, then by sub-domain:

- `transactions/` for payment, wallet, retail banking and fraud transaction streams
- `credit_risk/` for credit scoring and lending datasets
- `household_consumption/` for household expenditure and consumption surveys

This choice is based on direct inspection of each dataset schema and sample rows.

## Expected raw data structure

```text
data/raw/
  transactions/
    mobile_money/
    retail_banking/
    fraud_detection/
  credit_risk/
    credit_cards/
      duplicates/
    micro_loans/
    personal_loans/
    sme_loans/
  household_consumption/
```

## Dataset mapping

| Dataset | Category | Why it belongs there | Expected path |
| --- | --- | --- | --- |
| `nigerian_mobile_money_clean.parquet` | `transactions/mobile_money` | Contains wallet transactions with `transaction_type`, `amount_ngn`, `fee_ngn`, wallet identifiers and timestamps. | `data/raw/transactions/mobile_money/` |
| `nigerian_mobile_money_full.parquet` | `transactions/mobile_money` | Same business domain as above, broader variant of mobile money transaction history. | `data/raw/transactions/mobile_money/` |
| `nigerian_retail_transactions_clean.parquet` | `transactions/retail_banking` | Contains bank account transactions with balances before/after transaction, customer and account identifiers. | `data/raw/transactions/retail_banking/` |
| `nigerian_retail_transactions_full.parquet` | `transactions/retail_banking` | Same retail banking transaction domain as the clean variant. | `data/raw/transactions/retail_banking/` |
| `paysim_transactions.csv` | `transactions/fraud_detection` | Transaction stream with fraud labels such as `isFraud` and `isFlaggedFraud`. | `data/raw/transactions/fraud_detection/` |
| `default_credit_card_clients_clean.csv` | `credit_risk/credit_cards` | Credit card default risk dataset with repayment history, limit and demographic features. | `data/raw/credit_risk/credit_cards/` |
| `default_credit_card_clients_clean (1).csv` | `credit_risk/credit_cards/duplicates` | Exact duplicate copy of the main credit card dataset, isolated to avoid confusion. | `data/raw/credit_risk/credit_cards/duplicates/` |
| `nigerian_micro_loans_clean.parquet` | `credit_risk/micro_loans` | Micro-loan records with loan purpose, borrower information and maturity dates. | `data/raw/credit_risk/micro_loans/` |
| `nigerian_personal_loans_clean.parquet` | `credit_risk/personal_loans` | Personal loan applications and disbursement outcomes. | `data/raw/credit_risk/personal_loans/` |
| `nigerian_sme_loans_clean.parquet` | `credit_risk/sme_loans` | SME lending dataset with business identifiers, sector and loan details. | `data/raw/credit_risk/sme_loans/` |
| `conso_household_unified.csv` | `household_consumption` | Household expenditure / survey data with household identifiers, region and spending fields. | `data/raw/household_consumption/` |

## Team rules

- Put datasets downloaded from Drive into the exact category folders above.
- Keep source files as-is in `data/raw/`.
- If the team generates a cleaned or enriched version internally, save the derived output in `data/processed/`.
- Keep duplicates isolated and documented.
- If a new dataset does not clearly fit an existing category, document the decision here before adding a new folder.

## Notes

- Several source files contain `_clean` in their filename but are still treated as source assets because they were received as-is by the project.
- The raw data files themselves are intentionally ignored by Git. Only the folder structure and documentation are versioned.
