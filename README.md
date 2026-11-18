# RegAssist

An AI assistant that answers questions about UK financial regulation (the FCA Handbook) and **cites the exact rule** behind each answer.

**Status:** early stage. The source document and an evaluation test set are ready; the pipeline is not built yet. See the roadmap below.

## The problem

The FCA Handbook runs to thousands of pages, and finding which rule applies to a question is slow. A general-purpose chatbot can answer confidently but invent rules or cite the wrong section, which is unacceptable in compliance work.

RegAssist uses **retrieval-augmented generation (RAG)**: it first finds the relevant Handbook text, then answers only from that text, with a reference such as `PRIN 2A.2.1 R`. If the Handbook does not cover the question, it should say so instead of guessing.

## Scope (v0.1)

Version 0.1 covers one sourcebook: **PRIN (Principles for Businesses)**, including the Consumer Duty (PRIN 2A), as published in June 2026.

## Planned approach

1. **Ingest:** extract the PRIN text and split it into rule-level chunks, keeping each rule's reference (e.g. `PRIN 2.1.1 R`) as metadata.
2. **Retrieve:** embed the chunks and return the most relevant ones for a question.
3. **Generate:** an LLM answers using only the retrieved rules and cites them; it declines when nothing relevant is found.
4. **Evaluate:** measure answer correctness, citation accuracy, and whether it correctly declines unanswerable questions.

## Evaluation test set

A 20-question test set (`notes/prin-testset.md`) in five categories:

| Category | What it tests |
|---|---|
| Direct lookup | Finding a single rule |
| Definition / meaning | Explaining defined terms |
| Multi-rule | Combining several rules |
| Who / what it applies to | Scope and applicability |
| Unanswerable (traps) | Declining questions PRIN does not cover |

The questions were drafted with an LLM from the PDF text and are being checked by hand against the source. To avoid same-model bias, the system under test will not use the model that drafted the questions.

## Roadmap

- [x] Project structure and Python environment
- [x] Source document chosen (PRIN, June 2026)
- [x] Draft 20-question test set
- [ ] Verify every test question by hand
- [ ] Ingestion and rule-level chunking
- [ ] Retrieval
- [ ] Answer generation with citations
- [ ] Evaluation results
- [ ] Extend to further sourcebooks (e.g. COBS, DISP)

## Repository structure

```
src/     source code
tests/   automated tests
notes/   reading notes and the evaluation test set
data/    source documents (not committed)
```

## Data

FCA Handbook content is published by the Financial Conduct Authority. It is downloaded locally and not redistributed in this repository.
