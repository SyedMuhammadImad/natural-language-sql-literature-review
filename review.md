# Natural language interfaces to SQL: a focused literature review

project team recorded in the original documents: Syed Muhammad Imad (Syed Muhammad Imad), Abdul Momin (F2023376153), Fayz Liaqat (F2023376090), and Talha Kamran (F2023376156). This public edition was revised on 7 October 2026 from the original project topic, with checked primary references. It is a completed literature review; it contains no implementation, fine-tuned model, deployment, or newly measured benchmark result.

## Scope and method

The review examines four foundational contributions from 2018–2023: cross-schema evaluation, schema representation, constrained decoding, and database-grounded benchmarking. Sources are the authors' conference papers or paper records. This is a focused comparison, not a systematic review of every method or a current leaderboard survey. Historical numerical results from separate papers are not compared as if measured under one protocol.

## Evidence from the literature

**Spider (Yu et al., 2018).** Spider evaluates SQL generation across databases with different schemas in training and testing. This makes schema generalization a central challenge, rather than letting a parser memorize a single database. The relevant lesson for a warehouse interface is that a good score on familiar tables does not establish performance on a new schema. [Primary paper](https://aclanthology.org/D18-1425/).

**RAT-SQL (Wang et al., 2020).** RAT-SQL represents relations among schema elements and links question language to columns and tables through relation-aware attention. Its contribution is a way to expose schema structure and question-to-schema alignment to the parser. This addresses an ambiguity such as which of several similarly named columns a question means. It does not establish that every valid query matches the user's intention. [Primary paper](https://aclanthology.org/2020.acl-main.677/).

**PICARD (Scholak et al., 2021).** PICARD constrains autoregressive generation through incremental parsing and rejects tokens incompatible with the target formal language. For SQL generation, this reduces invalid output paths. Syntactic admissibility is useful, but a syntactically valid query can still select the wrong table, aggregation or filter. This semantic limitation is an inference from the distinction between valid syntax and the requested result, not a new experimental result. [Primary paper](https://aclanthology.org/2021.emnlp-main.779/).

**BIRD (Li et al., 2023).** BIRD expands evaluation toward larger databases and database-content grounding. Its paper highlights challenges involving noisy values, external knowledge and SQL efficiency. The implication for a practical warehouse interface is that schema names alone do not resolve every question; relevant data values and execution behavior also matter. [Primary paper](https://arxiv.org/abs/2305.03111).

## Comparison

| Contribution | Main question addressed | What it does not prove |
|---|---|---|
| Spider | Does a parser generalize across database schemas? | Correct operation on an arbitrary production warehouse |
| RAT-SQL | How can the model represent schema relations and language alignment? | Correct user intent in every ambiguous question |
| PICARD | Can generation reject inadmissible SQL continuations? | Semantic correctness of every valid query |
| BIRD | Can systems handle database-grounded questions at a larger scale? | Universal safety, access control or reliability |

## Design implications

The following are recommendations derived from the comparison, not claims that the project implemented them. A future interface should provide explicit schema context, evaluate on held-out databases, separate syntactic validity from answer correctness, and report execution behavior alongside accuracy. Ambiguous questions need clarification or abstention rather than forced SQL. Authorization and database access controls belong outside the model. Evaluation should declare dataset version, split, prompt/model configuration, execution limits and the definitions of each metric.

## Limitations and conclusion

This review supports a clear division of concerns: benchmark design measures generalization, schema linking helps identify relevant database elements, constrained decoding helps form valid SQL, and content-grounded evaluation tests additional practical demands. These contributions complement one another. The project does not establish a working Text-to-SQL product or a Gemma/Phi/Qwen fine-tuning result. Building such a system requires source code, reproducible training or inference configuration, execution tests and independent held-out evaluation.

## References

1. Yu et al. (2018). *Spider: A Large-Scale Human-Labeled Dataset for Complex and Cross-Domain Semantic Parsing and Text-to-SQL Task*. EMNLP. DOI: 10.18653/v1/D18-1425.
2. Wang et al. (2020). *RAT-SQL: Relation-Aware Schema Encoding and Linking for Text-to-SQL Parsers*. ACL. DOI: 10.18653/v1/2020.acl-main.677.
3. Scholak, Schucher and Bahdanau (2021). *PICARD: Parsing Incrementally for Constrained Auto-Regressive Decoding from Language Models*. EMNLP. DOI: 10.18653/v1/2021.emnlp-main.779.
4. Li et al. (2023). *Can LLM Already Serve as A Database Interface? A BIg Bench for Large-Scale Database Grounded Text-to-SQLs*. arXiv:2305.03111.
