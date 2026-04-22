# OctoMath: Contextual Bandit Routing for Modular Olympiad-Style Math Solving with Fully Offline Ollama Inference (CPU/GPU-capable)

OctoMath is a modular math-solving system built for **AIMO 3 (AI Mathematical Olympiad)** style problems. It combines deterministic symbolic routines with LLM-based reasoning and uses a contextual bandit router to decide which solver arm to call per problem under strict runtime and offline constraints.

The project focus is practical deployment: run end-to-end in a Kaggle offline environment, package Ollama inference assets locally, and keep experiments reproducible.

## Contribution Summary

- **LinUCB routing:** A contextual bandit (LinUCB-style) policy that routes each item to solver arms based on lightweight features and online reward signals.
- **Hybrid solver arms:** Multiple specialized arms (deterministic + LLM-assisted) for different math regimes instead of a single monolithic prompt path.
- **Offline Ollama packaging:** Local model/runtime packaging for no-internet execution (including model blobs/manifests and Ollama runners/runtime assets).
- **Reproducibility assets:** Packaged datasets/notebooks and fixed execution steps intended for repeatable offline runs.

## High-Level Architecture

At a high level, OctoMath follows this flow:

1. **Input normalization & feature extraction**  
   Parse problem text and derive routing features.
2. **Contextual bandit router (LinUCB)**  
   Select the solver arm with the best expected utility under compute/time constraints.
3. **Solver arm execution**  
   Execute one of several modular arms, then standardize outputs.
4. **Aggregation/post-processing**  
   Normalize final numeric/string format and prepare submission records.

### Solver Arms (Overview)

- **Deterministic algebra/number-theory style routines** (e.g., CRT-style and formula-driven paths when applicable).
- **Geometry/structured symbolic paths** for pattern-specific problem families.
- **DP/combinatorial routines** for finite-state/counting style prompts.
- **LLM-assisted arm(s)** via offline Ollama for problems that benefit from broader reasoning/search.

> Exact module boundaries may evolve as cleanup/reproducibility notebooks are finalized.

## Offline Ollama Packaging Notes

To support fully offline inference, OctoMath uses three components together:

1. **Model data:** `blobs/` + `manifests/` (Ollama model store layout)
2. **Ollama runtime:** `ollama` executable and supporting runtime files
3. **Runners:** CPU/CUDA runner libraries used by Ollama at serving time

Required environment variables in offline runtime setup:

- `OLLAMA_RUNNERS_DIR` — path to packaged runners
- `OLLAMA_MODELS` — path to local model store containing blobs/manifests
- `LD_LIBRARY_PATH` — include runner/runtime CUDA-related library paths as needed

Model reference used in this project:

- **Tag:** `erwan2/DeepSeek-R1-Distill-Qwen-7B:latest`
- **ID:** `f1a7a890c7a1`

### Hardware Variability (Conservative Note)

OctoMath has been tested in different Kaggle hardware modes (e.g., GPU-backed sessions such as H100 and CPU-only sessions). Behavior and runtime naturally vary by hardware availability, and hardware-specific runtime observations should **not** be interpreted as official competition score validation.

## Reproducibility

### Kaggle Offline Reproduction (Planned Canonical Path)

1. Attach required Kaggle datasets (Ollama runtime/model assets, embeddings/features, and evaluation assets).
2. Set environment variables (`OLLAMA_RUNNERS_DIR`, `OLLAMA_MODELS`, `LD_LIBRARY_PATH`).
3. Start local server:
   ```bash
   ollama serve
   ```
4. Run the OctoMath pipeline notebook/script.
5. Export final predictions to:
   - `submission.parquet`

A cleaned reproducibility notebook is planned to make these steps deterministic and audit-friendly. An optional Colab variant may be provided for demonstration if platform rules allow it.

## Evaluation

- **OctoMath-95** is a synthetic dataset used for regression testing and sanity-check evaluation, hosted on Kaggle (link placeholder in the **Links** section).
- Quantitative leaderboard-style result tables are intentionally deferred; consolidated numbers and ablations are planned for a later update during the competition rebuttal phase.

## Licensing and Third-Party Notices

- Repository code license: **MIT** (see `LICENSE`).
- Packaged datasets/assets may include third-party components distributed under their original notices (including **MIT** and **Apache-2.0** where applicable). Please review dataset-specific notices before redistribution.

## Links (Placeholders)

- Kaggle submission notebook: **[TBD]**
- Kaggle dataset — Ollama utils/runtime assets: **[TBD]**
- Kaggle dataset — embeddings/features: **[TBD]**
- Kaggle dataset — OctoMath-95: **[TBD]**
- Write-up / technical report: **[TBD]**

---

If you are a reviewer reproducing runs, please prioritize the upcoming clean reproducibility notebook instructions once published.
