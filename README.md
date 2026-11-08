# congo-imf-llm

A Google Colab notebook that reads one IMF staff report with Gemini models and checks what the models say against the report itself.

**Source document:** IMF Country Report No. 23/271, *Republic of Congo: Third Review under the Three-Year Arrangement under the Extended Credit Facility* (July 2023, 130 pages). The PDF is in `data/`.

## What is in the repository

| Path | Contents |
|---|---|
| `Congo_IMF_LLM_Pipeline.ipynb` | the notebook, saved with the outputs of its Colab run |
| `data/` | the IMF report (PDF) |
| `cache/replies/` | every model reply of the recorded run (a `diskcache` store), so a rerun needs no API key |
| `cache/vectors_*.npy` | the chunk embeddings |
| `results/` | JSON outputs and the two 300 dpi figures |
| `requirements.txt` | package versions of the Colab runtime that produced the outputs |

The `results/` folder holds:
- `indicators.json`, in the six-field schema of the brief, with `indicators_provenance.json` beside it;
- `section_summaries.json` and `executive_summary.json`;
- `theme_counts.json` and `strengths_vulnerabilities.json`;
- `time_series.json` and `rag_answers.json`;
- `extension_multiquery_hybrid.json`;
- `call_log.json`, `prompt_book.json` and `hand_checks.json`;
- `dashboard.png` and `extension.png`.

## Running it

1. Open the notebook in Colab, with `data/` and `cache/` in the working directory (`/content`). Either clone the repository there, or upload and unzip the two folders.
2. Choose *Runtime > Run all*.

All model calls are answered from `cache/replies/`, so no key is needed. The replies were recorded on Colab with the versions in `requirements.txt`. One detail matters outside Colab: in the extension, chunks C100 (p. 43) and C170 (p. 81) hold the same text and get the same score for Q02, and a different numpy version can order the tie the other way. That changes the prompt, so that single question then needs a key. To make fresh calls, add a Gemini API key as a Colab secret. The secret name is the `key_secret` setting in the first settings cell; the default is `GEMINI_FREE_KEY`. Then delete the cache folder.

Models used in the recorded run: `gemini-3.8-flash` as the writer and `gemini-3.5-flash-lite` as the judge, both through the Gemini API. Embeddings: `sentence-transformers/all-distilroberta-v1` with a FAISS `IndexFlatL2`.

## Licence

MIT, see `LICENSE`. The IMF report remains the property of the International Monetary Fund and is included only so the notebook can run.
