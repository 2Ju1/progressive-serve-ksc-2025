# Reported results

[`paper-results.csv`](paper-results.csv) transcribes Table 1 from the KSC 2025 paper.

## Column definitions

- `system`: evaluated serving system.
- `stage`: baseline or ProgressiveServe recovery stage.
- `em_percent`: exact-match score on TriviaQA validation.
- `f1_percent`: F1 score on TriviaQA validation.
- `stage_wall_clock_seconds`: wall-clock duration reported for the baseline or stage.
- `ttft_seconds`: time to first token; only applicable to the baseline and Stage 1.

The values are publication-level aggregates. Raw trial logs are not included. The three ProgressiveServe stage durations sum to 128 seconds, while the first response is available after Stage 1 at 90 seconds.
