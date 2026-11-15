# AI/ML Project Metrics Audit

This report summarizes the AI/ML projects that appear to match the user's work and captures only metrics that are supported by repository code, test reports, or documented project artifacts. It intentionally avoids unsupported accuracy claims.

## Scope reviewed

- Smart Traffic System: https://github.com/moashraf7474/Smart-traffic-system
- Mailmind: https://github.com/moashraf7474/Mailmind
- Swap-AI semantic search: https://github.com/SwapEG/Swap-AI
- Related watermark/logo detection work: https://github.com/SwapEG/swap-app-service

## Important audit rule

I did not find enough evidence to support many of the common performance numbers that are often included on CVs (e.g., accuracy, precision, recall, latency on real datasets, traffic-time reduction). If a metric was not directly measured or documented in the repository, it is marked as unavailable or unverified.

---

## 1) Smart Traffic System

The repository shows vehicle tracking, ROI-based counting, speed estimation, license-plate OCR, and traffic-density logic. I found no training data, evaluation dataset, saved evaluation output, or reproducible model-performance report in the files inspected.

| Metric | Value | Evidence | Status |
|---|---:|---|---|
| Detection/tracking model | YOLO, checkpoint path configured as `best.pt` | `ai/ai_service.py`, lines 21–29 | Configured |
| Tracker | ByteTrack; persistent tracking enabled | `ai/ai_service.py`, lines 189–196 | Configured |
| Detection confidence threshold | `0.25` | `ai/ai_service.py`, lines 189–196 | Configured |
| Road dimensions used for perspective transform | 10 m × 50 m | `ai/ai_service.py`, lines 23–25 | Configured, requires calibration validation |
| Event publishing interval | 2 seconds minimum between sends per vehicle | `ai/ai_service.py`, lines 166–172 and 268–281 | Configured |
| Speed-change send threshold | 2 km/h | `ai/ai_service.py`, lines 166–172 and 274–281 | Configured |
| Segment length | 10 m | `backend/TrafficWorker/TrafficWorker/services/TrafficService.cs`, lines 10–24 | Configured |
| Incident speed-drop threshold | 50% | `backend/TrafficWorker/TrafficWorker/services/TrafficService.cs`, lines 14–16 and 86–100 | Configured |
| Detection mAP, tracking accuracy, counting error, speed error, latency | Not found | No evaluation results found in inspected repository files | Unavailable |
| Dataset size / train-validation-test split | Not found | No dataset or split metadata found in inspected repository files | Unavailable |

### CV-ready achievements using verified facts

- Implemented a vehicle-monitoring pipeline combining YOLO detection, persistent ByteTrack tracking, polygonal ROI counting, and per-vehicle speed estimation.
- Built a perspective-transform speed-estimation pipeline calibrated to a configured 10 m × 50 m road region, with vehicle events published through RabbitMQ.
- Implemented segment-based congestion logic using 10 m segments and a configured 50% speed-drop incident threshold.

These statements describe implementation and configuration, not proven accuracy or traffic improvement.

---

## 2) Mailmind

Mailmind is a local email-analysis app using Qwen2.5-0.5B-Instruct and Pydantic-validated structured output. Its README and pipeline documentation contain performance and accuracy claims, but I found no benchmark dataset, evaluation script, test logs, or saved results to substantiate those claims.

| Metric | Value | Evidence | Status |
|---|---:|---|---|
| Model | `Qwen/Qwen2.5-0.5B-Instruct` | `src/summarizer.py`, lines 18–20 | Configured |
| Parameter count | ~0.5B | `README.md`, lines 3–11 | Documented, not independently verified |
| Structured output fields | 6: summary, important points, action items, deadlines, people, priority | `src/models.py`, lines 18–45 | Confirmed in code |
| Priority classes | 3: Low, Medium, High | `src/models.py`, lines 12–16 | Confirmed in code |
| Maximum generated tokens | 700 | `src/summarizer.py`, lines 27–30 and 68–74 | Configured |
| Fine-tuning / LoRA | None; documented as prompt → generate → validate | `src/summarizer.py`, lines 1–6 | Confirmed by code documentation |
| Generation mode | Greedy/deterministic (`do_sample=False`) | `src/summarizer.py`, lines 68–74 | Configured |
| Extraction accuracy / priority accuracy | README claims ~90% / ~85%; no supporting evaluation evidence found | `README.md`, lines 150–159 | Unverified — do not claim |
| Per-email latency / memory | README documents 10–30 s and 1–2 GB; no benchmark logs or hardware details found | `README.md`, lines 150–159 | Documented estimate, not verified measurement |
| BLEU/ROUGE, field-level precision/recall/F1, JSON-validity rate | Not found | No evaluation artifacts found in inspected repository files | Unavailable |

### CV-ready achievements using verified facts

- Built a local email-analysis app using Qwen2.5-0.5B-Instruct to generate structured summaries and extract action items, deadlines, people, and priority.
- Added Pydantic schema validation for 6 output fields and a 3-class priority enum, with deterministic generation and JSON extraction.
- Implemented a local inference pipeline with configurable generation up to 700 new tokens; no external LLM API is required by the described design.

---

## 3) Swap Egypt AI — Semantic Search

The Swap-AI repository includes a hybrid search implementation and local acceptance tests. It documents a corpus of 10,519 items, 376 enabled synonym groups, and test-suite results. The test count is inconsistent across documentation, so I would not use that count on a CV until it is reconciled.

| Metric | Value | Evidence | Status |
|---|---:|---|---|
| Search corpus | 10,519 items | `Swap_AI/AUDIT_REPORT.md`, lines 36–42 | Reported corpus count |
| Enabled synonym groups | 376 | `Swap_AI/docs/semantic-search.md`, lines 25–36 | Configured/documented |
| Embedding model | `intfloat/multilingual-e5-small`, 384 dimensions | `Swap_AI/docs/semantic-search.md`, lines 38–45 | Configured/documented |
| Retrieval design | BM25 + E5 k-NN, Reciprocal Rank Fusion with `k=60`, brand/model boost | `Swap_AI/docs/semantic-search.md`, lines 3–15 | Implemented |
| Semantic timeout | 300 ms, with BM25 fallback | `Swap_AI/search_pipeline.py`, lines 420–445 | Configured and acceptance-tested in code |
| Normalizer tests | 55/55 reported passing | `Swap_AI/AUDIT_REPORT.md`, lines 24–32 | Reported test result |
| Real-query groups | 25/25 reported passing | `Swap_AI/AUDIT_REPORT.md`, lines 24–32 | Reported test result |
| E2E tests | README says 39/39; audit report says 41/41 | `Swap_AI/README.md`, lines 5–12; `Swap_AI/AUDIT_REPORT.md`, lines 24–32 | Conflicting documented counts; verify before use |
| Recall@10 improvement | Technical spec claims +5.72 points over BM25, with 95% CI [+2.41, +9.34] | `Swap_AI/docs/semantic-search.md`, lines 38–45 | Not sufficiently substantiated in available artifacts |
| Evaluation backing for Recall@10 claim | Audit says the 332-query comparison was not addressed / unavailable in the audit | `Swap_AI/AUDIT_REPORT.md`, lines 70–74 | Do not use the +5.72 figure as a verified achievement yet |
| Latency and model size | Spec documents 7.3 ms p50 and 112.6 MB; README also lists a 604 MB model folder | `Swap_AI/docs/semantic-search.md`, lines 38–45; `Swap_AI/README.md`, lines 26–37 | Conflicting/unverified; not a measured result I can confirm |
| Deployment status | Repository says local Python pipeline verified, not live / awaiting backend integration | `Swap_AI/README.md`, lines 5–12 | Documented status |

### CV-ready achievements using supported evidence

- Implemented a bilingual marketplace hybrid-search pipeline combining BM25, multilingual E5 embeddings, RRF ranking, and brand/model boosting over a 10,519-item corpus.
- Built text normalization for Arabic, English, and Franco-Arabic queries, with 376 synonym groups and shared test vectors; the audit reports 55/55 normalizer tests passing.
- Added a 300 ms semantic-search timeout with BM25 fallback and acceptance tests for query variants, mixed scripts, brand boosting, and zero-result behavior.

These bullets use corpus size, design, configured timeout, and reported test outcomes—not the unsupported recall-improvement claim.

---

## 4) Swap Egypt AI — Image/Logo Detection

The image/logo work appears in the related SwapEG application-service repository, not the Swap-AI search repository. The matcher uses normalized cross-correlation (NCC) with templates; code contains thresholds and synthetic unit tests, but I found no labeled real-image benchmark or measured precision/recall. The production-service README and current test evidence should be checked before representing this as a personal contribution.

| Metric | Value | Evidence | Status |
|---|---:|---|---|
| Matching approach | Multi-scale normalized cross-correlation | `DubizzleTemplateMatcherService.java`, lines 15–36 | Implemented |
| Corner regions checked | 4 | `DubizzleTemplateMatcherService.java`, lines 157–175 | Implemented |
| Templates used for corner matching | 2 of 3 loaded templates | `DubizzleTemplateMatcherService.java`, lines 18–23 and 62–68 | Configured |
| Working image maximum width | 400 px | `DubizzleTemplateMatcherService.java`, lines 70–91 | Configured |
| NCC decision threshold | 0.55 | `DubizzleTemplateMatcherService.java`, lines 52–58 and 173–183 | Configured |
| Unit-test coverage | Synthetic template placements and edge cases exist | `DubizzleTemplateMatcherServiceTest.java` and `WatermarkDetectionServiceTest.java` | Tests present; no real-image benchmark result found |
| Precision, recall, F1, false-positive/negative rate, latency | Not measured in the evidence inspected | No labeled evaluation result found | Unavailable |

### CV-ready achievement using supported evidence

- Implemented a Dubizzle watermark matcher using multi-scale NCC, checking 4 image corners against logo templates after resizing large inputs to a configured maximum width of 400 px.

Do not claim an accuracy percentage until it has been tested on labeled real images.

---

## Top 5 strongest verified numbers

1. 10,519 items in the Swap-AI corpus.
2. 376 synonym groups in the Swap-AI normalization pipeline.
3. 55/55 normalizer tests passing (reported in the audit).
4. 25/25 real-query groups passing (reported in the audit).
5. 300 ms semantic timeout with BM25 fallback.

These are the best-supported numbers available. They describe corpus scale, test outcomes, and system configuration—not production impact or model accuracy.

---

## Interview talking points

- Swap-AI: Explain why BM25 and vector retrieval complement each other, how RRF combines ranked lists, and why language-aware normalization and exact brand/model boosts matter for Egyptian marketplace search. Be transparent that the claimed Recall@10 lift needs reproducible evaluation evidence.
- Mailmind: Discuss the tradeoff of a small local model: privacy and no API dependency versus the need to measure extraction quality. Explain the JSON extraction, sanitization, and Pydantic validation path.
- Smart Traffic: Explain how ROI masking, ByteTrack IDs, perspective transformation, and speed smoothing connect the vision pipeline to downstream traffic events. Clarify that the configured speed and congestion rules have not been validated against ground truth.
- Logo detection: Describe NCC template matching, corner crops, multi-scale scanning, and the threshold. Distinguish synthetic unit tests from real-world accuracy evaluation.

---

## Missing evaluations and recommended experiments

- Swap-AI retrieval: Build a judged set of queries with relevant item IDs. Compare BM25-only, semantic-only, and hybrid retrieval on the same held-out queries; calculate Recall@K, Precision@K, MRR, and NDCG. Include a reproducible script, query set, and result file. Then benchmark warm search latency at p50/p95 on documented hardware.
- Mailmind: Create a labeled email test set and score each field separately: precision, recall, and F1 for action items, deadlines, people, and priority; ROUGE or human evaluation for summaries; JSON/schema-validity rate; and per-email latency/memory with hardware and email lengths recorded.
- Smart Traffic: Annotate representative videos with vehicle boxes, identities, counts, and speed ground truth. Report detection mAP, tracking metrics such as IDF1/HOTA, counting error, and speed MAE. Compare traffic-control outcomes against a defined baseline before claiming reduced waiting time.
- Logo detection: Create a labeled image set covering corner/center logos, clean images, compression, transparency, and difficult backgrounds. Report a confusion matrix, precision/recall/F1 by scenario, and latency. Keep a separate holdout set for the final numbers.

---

## Consolidated comparison

| Project | Strongest supported numbers | Measured performance available? | Main CV-safe angle |
|---|---|---|---|
| Smart Traffic System | 10 m × 50 m configured ROI; 10 m segments; 50% incident threshold | No model or system evaluation results found | YOLO + ByteTrack pipeline, ROI counting, calibrated speed-estimation design |
| Mailmind | 6 output fields; 3 priority classes; 700-token generation limit | No quality benchmark found; README accuracy/timing claims are unverified | Local LLM summarization and schema-validated structured extraction |
| Swap-AI Semantic Search | 10,519-item corpus; 376 synonym groups; 55/55 normalizer tests; 25/25 query groups; 300 ms timeout | Some local test results are documented; Recall@10 claim is not sufficiently substantiated | Bilingual BM25 + E5 hybrid search, RRF, normalization, fallback |
| Swap Logo Detection | 4 corners; 2 matching templates; 400 px working-width limit; 0.55 NCC threshold | No real-image accuracy benchmark found | Multi-scale NCC watermark detection |

## Final recommendation

Do not include numbers like “90% accuracy,” “25% less waiting time,” or “95% logo detection accuracy” unless you run and save the corresponding evaluations. The most defensible CV metrics from the inspected work are corpus sizes, test counts, model configuration details, and exact algorithmic thresholds.
