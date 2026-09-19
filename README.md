# IPN-101 v2 — privacy policies of consumer IoT devices, annotated for data practices

101 privacy policies for consumer IoT devices sold into the EU, segmented into passages and
annotated with the data practices each passage describes. **Every claim carries a verbatim quote
from the passage it came from**, with character offsets, so any claim can be checked against the
text without leaving the dataset.

| | |
|---|---|
| policies | **101** |
| passages (segments) | **10,627** |
| annotated claims | **55,350** |
| evidence spans | **55,350** (every claim has one) |
| policy text | 4,994,828 characters |
| device categories | 35 across 12 domains |
| median policy | 78 passages, 422 claims |

## Layout

```
scheme.json            6 categories, 19 attributes, 18 closed vocabularies, with definitions
corpus.json            one row per policy: device, domain, category, URL, provenance, counts
policies/<slug>.json   that policy's passages, each with its claims and evidence
STATS.json             the figures above, recomputed at build time
checksums.sha256       every file
```

A policy file is self-contained. One passage looks like:

```json
{"segment_id": 89,
 "text": "| Google | Ads, workspace and analytics |",
 "section_title": "DISCLOSURES",
 "block_type": "table_row",
 "claims": [
   {"category": "Other-party data collection and use",
    "attribute": "other_party_type",
    "value": "Analytics provider",
    "evidence": {"text": "Google", "start": 2, "end": 8}}
 ]}
```

`evidence.start`/`end` index into that passage's `text`. `text[start:end] == evidence.text` holds
for all 55,350 spans — `build_dataset_release.py --verify-only` re-checks it.

## Domains

| domain | policies |
|---|---|
| Mobility | 18 |
| Health | 17 |
| Smart home | 13 |
| AR/VR/XR headsets + glasses | 11 |
| Domestic robots | 11 |
| Smart Speaker and Voice assistants | 6 |
| Baby/Kids | 6 |
| TV | 6 |
| Smart toys | 4 |
| HVAC | 4 |
| Drones | 3 |
| Smart Kitchen | 2 |

## How it was made

Policies were fetched as HTML, extracted to markdown by a best-of-N extractor with a quality gate,
segmented into passages by an anchored LLM segmenter, and annotated passage by passage against the
scheme. The annotator was `openai/gpt-oss-120b`.

Evidence is mandatory by construction: a claim without a verbatim quote is dropped, so the corpus
contains no uncited claims. That is a guarantee about *form*, not about correctness — a claim can
be wrong while its quote is real.



## Provenance

Each policy's `provenance` field says whether it is unchanged since v1, re-extracted for v2, or
added in v2. Five were re-extracted after a bug was found in which the extractor dropped table
cells. one (Dexcom) was added after the same bug turned out to be the reason it had been excluded.

