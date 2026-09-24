# Benchmarks

How Traxlate's quality numbers were measured, and every number behind the charts on the
[project page](README.md#measured-quality).

## Method

| | |
| --- | --- |
| **Test set** | [FLORES-200](https://github.com/facebookresearch/flores/tree/main/flores200) devtest — a public benchmark of sentences with professional human translations |
| **Sample** | 200 sentences per direction (fixed seed 42), 27 directions, 5,395 sentences in total |
| **Metric** | COMET-22 ([Unbabel/wmt22-comet-da](https://huggingface.co/Unbabel/wmt22-comet-da)), shown × 100; chrF++ and spBLEU from sacrebleu alongside |
| **System** | The Traxlate pipeline as it ships: the on-device translator plus its quality check, one sentence per request, on the device |
| **Date** | Recorded 2026-08-05, scored with COMET on CPU |

**What these numbers are — and are not.** Each score measures how close Traxlate's output is to a
professional human translation of the same sentence. It is **not** a comparison with any other
translation service: none was called, and a score here should not be read as "better or worse
than" another product.

## Results

Mean COMET-22 across all 27 directions: **87.0**.

| Direction | Sentences | COMET-22 | chrF++ | spBLEU | Rewritten by the quality check | Rewrites that scored higher |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| English → Arabic | 200 | 85.6 | 52.5 | 36.4 | 25 | 24/25 |
| English → Chinese (Simplified) | 200 | 86.3 | 27.6 | 33.7 | 4 | 3/4 |
| English → French | 195 | 87.2 | 69.2 | 53.5 | 3 | 3/3 |
| English → German | 200 | 86.0 | 63.5 | 44.1 | 5 | 4/5 |
| English → Hebrew | 200 | 86.9 | 56.0 | 41.0 | 4 | 4/4 |
| English → Hindi | 200 | 79.2 | 51.9 | 33.5 | 3 | 3/3 |
| English → Indonesian | 200 | 91.0 | 65.5 | 43.6 | 9 | 9/9 |
| English → Italian | 200 | 87.0 | 57.6 | 37.1 | 4 | 4/4 |
| English → Japanese | 200 | 89.9 | 26.8 | 26.2 | 3 | 3/3 |
| English → Korean | 200 | 89.4 | 36.6 | 30.6 | 9 | 8/9 |
| English → Persian | 200 | 87.3 | 49.6 | 32.8 | 3 | 3/3 |
| English → Portuguese | 200 | 88.5 | 68.9 | 50.9 | 3 | 3/3 |
| English → Russian | 200 | 89.1 | 56.0 | 40.5 | 4 | 4/4 |
| English → Spanish | 200 | 84.6 | 54.0 | 33.0 | 3 | 3/3 |
| English → Thai | 200 | 85.1 | 42.2 | 34.0 | 3 | 2/3 |
| English → Turkish | 200 | 88.6 | 54.4 | 32.9 | 17 | 17/17 |
| English → Vietnamese | 200 | 88.9 | 59.0 | 42.5 | 4 | 4/4 |
| Arabic → English | 200 | 86.4 | 64.6 | 40.7 | 38 | 38/38 |
| Chinese (Simplified) → English | 200 | 85.0 | 55.0 | 29.2 | 36 | 13/36 |
| French → English | 200 | 88.1 | 66.3 | 45.4 | 3 | 2/3 |
| German → English | 200 | 84.5 | 64.8 | 37.4 | 3 | 2/3 |
| Japanese → English | 200 | 87.7 | 55.5 | 31.3 | 28 | 15/28 |
| Korean → English | 200 | 87.8 | 57.1 | 34.1 | 20 | 19/20 |
| Persian → English | 200 | 87.9 | 62.0 | 40.2 | 1 | 1/1 |
| Portuguese → English | 200 | 88.6 | 69.9 | 49.6 | 4 | 3/4 |
| Russian → English | 200 | 86.6 | 61.3 | 40.7 | 2 | 2/2 |
| Spanish → English | 200 | 86.6 | 58.9 | 33.9 | 2 | 2/2 |

## The quality check

Traxlate checks every translation before showing it, and rewrites the ones that fail. Across the
5,395 sentences it rewrote **243** (4.5%) — the rest were already right and passed through
unchanged. Of those rewrites, **198 scored higher than the first draft (81%)** under COMET-22.

Because the check only acts on the few sentences that need it, its effect on a whole-corpus
average is small by construction; the right question is what happens to the sentences it
touches, which is what the last two columns answer.

## Speed

| Device | Result | How |
| --- | --- | --- |
| Desktop, NVIDIA RTX 3070 | 0.36 s per sentence (median) | 195 short sentences through the local API, 2026-09-23 |
| Samsung Galaxy Z Fold5 | 2.0 s for two sentences | The Android app, fully on the phone, 2026-09-23 |

Speed depends on the device, the length of the text and whether the engine is already warm; the
first translation after start-up takes longer while the language pack loads.
