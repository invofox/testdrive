# Invofox Testdrive

A public, browsable collection of the **document types Invofox can process** — sample
PDFs grouped by type, each paired with a ground-truth file.

The goal is to let anyone *take Invofox for a test drive*: inspect the kinds of
documents the platform handles, and (soon) run them locally through the Invofox SDK —
and optionally against generic LLM SDKs such as OpenAI and Gemini — to evaluate
extraction quality on your own machine. It is **not a formal benchmark**, but it is
built with benchmarking in mind.

---

## What's inside

- **31 document types** (invoices, receipts, IDs, tax forms, mortgage docs, …)
- **5 sample documents per type** → **155 documents** total
- Every document ships with a matching **ground-truth JSON**
- ~34 MB, all samples are small PDFs

## Repository structure

```
testdrive/
├── README.md            ← you are here
├── .gitattributes       ← keeps PDFs binary, text as LF
├── .gitignore
└── samples/
    ├── README.md        ← dataset layout & conventions
    ├── Invoice/
    │   ├── originals/        # the source documents (PDF)
    │   │   ├── document_01_tech_consultancy.pdf
    │   │   └── …
    │   └── ground_truth/     # one JSON per original, same basename
    │       ├── document_01_tech_consultancy.json
    │       └── …
    ├── Passport/
    │   ├── originals/
    │   └── ground_truth/
    └── …                 # one folder per document type (31 total)
```

**Conventions**

- One folder per document type, named in `PascalCase` (e.g. `Invoice`, `BankStatement`, `FormW2`).
- Inside each type: `originals/` holds the source files, `ground_truth/` holds the labels.
- A document and its ground truth share the **same basename** — `originals/foo.pdf` ↔ `ground_truth/foo.json`.

See [`samples/README.md`](samples/README.md) for the full layout contract.

## Document types

| Category | Types |
| --- | --- |
| **Accounts payable / receivable** | `Invoice`, `CreditNote`, `Receipt`, `PurchaseOrder`, `Quote`, `DeliveryNote`, `ExpenseReport` |
| **Banking & credit** | `BankStatement`, `CreditReport` |
| **Identity** | `Passport`, `IDCard`, `DriversLicense` |
| **Payroll & tax (US)** | `Payslip`, `FormW2`, `FormW4`, `FormW9`, `Form1040`, `Form1099` |
| **Mortgage & real estate (US)** | `Form1004`, `Form1008`, `ClosingDisclosure`, `MortgageDeed`, `LoanApplication`, `LeaseAgreement`, `PurchaseSaleAgreement` |
| **Insurance** | `InsurancePolicy`, `AcordCertificate` |
| **Other** | `Contract`, `Resume`, `UtilityBill`, `CustomsDeclaration` |

Samples are intentionally diverse within a type — different countries, layouts,
languages and edge cases (e.g. `Invoice` includes a German supplier, a UK freelancer
and a Spanish SaaS bill).

## Ground truth

Each original has a sibling JSON in `ground_truth/` with the same basename. The current
schema is a minimal stub that will be expanded with the expected extracted fields per
document type:

```json
{
  "documentType": "Invoice",
  "sourceFile": "document_01_tech_consultancy.pdf"
}
```

| Field | Meaning |
| --- | --- |
| `documentType` | The document type — always matches the parent folder name. |
| `sourceFile` | The original file name (matches the file in `originals/`). |

> **Status:** ground truth is currently a stub. Per-field expected values (the data an
> extraction pipeline should return) will be added incrementally.

## How to use it

**Today** — browse the samples to see the document types Invofox covers, and use them as
inputs for your own experiments.

**Coming soon** — a thin local harness to:

1. Run the samples through the **Invofox SDK** and compare against `ground_truth/`.
2. Optionally run the same documents through generic LLM SDKs (**OpenAI**, **Gemini**)
   for a side-by-side, evaluation-style comparison.

## Roadmap

- [ ] Expand ground truth with expected extracted fields per type.
- [ ] Add a local runner for the Invofox SDK.
- [ ] Add optional OpenAI / Gemini runners for side-by-side comparison.
- [ ] Publish periodic comparison results (e.g. a GitHub Pages dashboard refreshed every few months).

## Data & privacy

These are **sample documents intended for evaluation**. They should not contain real
personal data. If you spot anything that looks like genuine PII, please open an issue so
we can remove it.

## About Invofox

[Invofox](https://invofox.com) automates document processing — turning unstructured
documents like the ones in this repository into structured, validated data.
