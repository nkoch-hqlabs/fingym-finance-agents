# fingym-finance-agents
Where finance workflows meet agentic AI. An exploratory lab for institutional finance agents

# FinGym: Reinforcement Learning Environments & Deterministic Benchmarks for Finance Agents

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Code Style: Black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

`FinGym` is an open-source evaluation harness and simulated reinforcement learning (RL) sandbox designed to benchmark autonomous AI agents on high-stakes enterprise financial workflows.

---

## 1. Problem Statement & Research Motivation

Frontier Large Language Models (LLMs) demonstrate strong qualitative reasoning on conversational tasks but frequently fail in enterprise financial workflows. In commercial lending, underwriting, and compliance, models suffer from three systemic failure modes:
1. **Context Fragmentation across Multi-Page Filings:** Standard models lose fidelity when extracting unstructured tabular metrics across 80+ page filings, leading to unverified hallucinations.
2. **Omission of Disclosed Adjustments:** Models fail to reconcile main statement line items with footnote disclosures (e.g., non-recurring restructuring charges, contingent liabilities).
3. **Cross-Schema Discrepancies:** Models fail to cross-reference stated company liabilities against external commercial credit bureau reports and active lien registries.

`FinGym` bridges this gap by providing high-fidelity, deterministic environments where agents interact via structured tool-calling APIs and are evaluated against zero-tolerance, programmatic ground truth.

---

## 2. Architecture Overview

                  ┌────────────────────────────────────────┐
                  │            Task Directive              │
                  │  "Underwrite $15M Facility for Acme;   │
                  │   Reconcile Liens & Evaluate Covenants"│
                  └───────────────────┬────────────────────┘
                                      │
                                      ▼
┌────────────────────────────────────────────────────────────────────────┐
│                       Agent Execution Sandbox                          │
│                                                                        │
│   Observation Space (State):                                           │
│   ├── /data_room/financials.json    (Audited 3-Statement Statements)   │
│   ├── /data_room/footnotes.json     (Restructuring & Legal Notes)      │
│   └── /data_room/bureau_report.json (UCC-1 Filings & Drawn Debt Lines) │
│                                                                        │
│   Action Space (Tools):                                                │
│   ├── extract_statement(statement_name)                                │
│   ├── query_footnote(keyword)                                          │
│   ├── query_bureau(field)                                              │
│   └── submit_credit_decision(memo_payload)                             │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                     Deterministic Evaluation Harness                   │
│                                                                        │
│   ├── Level 1: Mathematical Invariants (Assets == Liab + Equity)       │
│   ├── Level 2: Factual Extraction (Footnote 4 Add-Back Match)          │
│   ├── Level 3: Cross-Schema Reconciliation (Bureau Debt Capture)       │
│   └── Trajectory Scoring: Step efficiency & error penalties            │
└────────────────────────────────────────────────────────────────────────┘

---

## 3. Environment Specifications

### Observation Space ($S$)
Agents are provided a sandboxed enterprise data room containing:
* `financials.json`: Audited Balance Sheet, Income Statement, and Cash Flow statement lines.
* `footnotes.json`: Disclosure notes containing non-recurring items and contingent litigation reserves.
* `bureau_report.json`: Third-party corporate registry and commercial credit bureau records.

### Action Space ($A$)
Interaction is governed by typed JSON tool calls:
* `extract_statement(statement_name: str)`: Fetches normalized statement tables.
* `query_footnote(search_term: str)`: Conducts semantic search across disclosure items.
* `query_bureau(field_name: str)`: Queries corporate registries, UCC filings, and active credit facilities.
* `submit_credit_decision(memo: CreditDecisionMemo)`: Terminal action submitting structured decision criteria.

---

## 4. Multi-Tier Evaluation Harness

Unlike subjective LLM-as-a-judge approaches, `FinGym` enforces strict deterministic validation:

| Grading Tier | Target Criterion | Verification Method | Max Points |
| :--- | :--- | :--- | :--- |
| **Tier 1: Accounting Invariants** | Balance sheet reconciliation & cash flow tie-out | Deterministic programmatic assertion (`Assert Assets == Liab + Eq`) | 20 pts |
| **Tier 2: Footnote Normalization** | Did the Agent capture details from footnote? | Ground-truth delta check | 30 pts |
| **Tier 3: Bureau Reconciliation** | Did the Agent verify from credit bureaus? | Bureau delta assertion | 30 pts |
| **Tier 4: Covenant Verification** | Downside stress-test DSCR and leverage evaluation | Policy rule gate assertion | 20 pts |
| **Trajectory Penalty** | Excessive API iterations or circular loops (Efficiency) | Step penalty ($-2\text{ pts}$ per redundant step $>4$) | Variable |

---

## 5. Benchmark Results: Naive LLM vs. FinGym-Guided Agent

Standard zero-shot evaluation on an unstandardized commercial loan dataset:
(Why giving an AI agents an environment matters)

| Agent / Model Variant | Statement Math | Footnote Catch Rate | Undisclosed Debt Detection | Benchmark Score |
| :--- | :---: | :---: | :---: | :---: |
| **Direct Prompting (Zero-Shot)** | 72.0% | 18.0% | 0.0% | **30.0 / 100** |
| **Chain-of-Thought (No Tools)** | 84.0% | 34.0% | 12.0% | **43.3 / 100** |
| **FinGym ReAct Agent (Structured Tools)** | **100.0%** | **94.0%** | **100.0%** | **98.0 / 100** |

---

## 2. Architecture Overview
