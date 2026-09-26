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
