# OctoMath — Contextual Bandit Routing for Modular Olympiad-Style Math Solving (Fully Offline Ollama, CPU/GPU-capable)

OctoMath is a modular math-solving system built for **AIMO 3 (AI Mathematical Olympiad – Progress Prize 3)**-style problems. It routes each problem to one of several solver “arms” using a **contextual multi-armed bandit (LinUCB)**, combining deterministic math modules with **LLM-assisted** reasoning and **LLM→SymPy code execution** under strict **offline** constraints.

This repository is a lightweight project page and documentation entrypoint. The canonical reproduction artifacts are the **public Kaggle notebook and datasets** linked below.

---

## Highlights

- **Contextual bandit routing (LinUCB):** chooses an appropriate solver arm based on cheap features (regex + MiniLM embeddings).
- **Hybrid solver arms:** deterministic number theory / combinatorics / DP / geometry heuristics + LLM direct reasoning + LLM→SymPy codegen+execution.
- **Verification-first design:** strict integer extraction and SymPy-based checks where applicable.
- **Fully offline deployment:** packaged **Ollama runtime + runners + model blobs/manifests** to run with **Kaggle Internet OFF**, CPU-only or GPU-backed.
- **Reproducibility assets:** public Kaggle datasets + notebook, plus a small **synthetic** evaluation set (no competition leakage).

---

## Architecture (high level)

1. **Feature extraction**
   - Regex detectors (e.g., modular arithmetic, geometry, recurrence hints)
   - Embeddings from **all-MiniLM-L6-v2** (stored offline as a Kaggle dataset)

2. **Routing (LinUCB)**
   - Disjoint LinUCB ranks candidate solver arms by estimated reward + exploration bonus
   - Mild reward shaping encourages verifiable arms

3. **Solver arm execution**
   - Deterministic modules where possible
   - Otherwise LLM direct reasoning or LLM→SymPy codegen

4. **Verification / extraction**
   - Enforce integer output constraints (AIMO expects integer answers in a bounded range)
   - Verify with symbolic execution when feasible; fallback to alternatives on failure

---

## Reward signal (used for LinUCB)

| Outcome | Reward | Notes |
|---|---:|---|
| Verified answer (deterministic module or SymPy-verified) | **1.0** | Highest-confidence signal; encourages selection of verifiable arms |
| Unverified LLM-direct answer accepted as fallback | **0.5** | Partial credit when verification is not applicable/available |
| Invalid output / execution error / failed verification / no answer | **0.0** | No learning signal |

This shaping biases routing toward deterministic/SymPy-verifiable arms while still allowing partial credit when direct LLM reasoning produces a usable answer but cannot be symbolically verified.

---

## Offline Ollama packaging notes (CPU/GPU)

Offline inference uses three components together:

- **Ollama runtime:** `ollama` executable and support files
- **Runners:** CPU/CUDA runner libraries used at serve time
- **Model store:** `blobs/` + `manifests/` in Ollama’s expected layout

Typical environment variables:

- `OLLAMA_RUNNERS_DIR` — points to packaged runners directory
- `OLLAMA_MODELS` — points to the local model store (blobs/manifests)
- `LD_LIBRARY_PATH` — include CUDA/runtime library paths as needed

**Operational note:** Ollama may generate an ephemeral SSH keypair on first run if `/root/.ollama/id_ed25519` is missing; logs may print the public key. No prompts/completions are logged by default in the provided setup.

---

## Model reference (Ollama)

- Tag: `erwan2/DeepSeek-R1-Distill-Qwen-7B:latest`  
- ID: `f1a7a890c7a1`

---

## Synthetic evaluation dataset (no leakage)

A small dataset (**Test_95_Problems**) is used for regression testing and plumbing validation.

**Provenance:** generated with ChatGPT; **not derived from AIMO** public or private test content.

---

## Links (canonical reproduction)

- **Kaggle write-up:** https://kaggle.com/writeups/ahmedhazemhassan/octomath-contextual-bandit-routing-for-Math
- **Kaggle submission notebook (public):** https://www.kaggle.com/code/ahmedhazemhassan/octomath
- **Kaggle dataset — Offline Ollama runtime/models:** https://www.kaggle.com/datasets/ahmedhazemhassan/octomath-ollama-utils
- **Kaggle dataset — all-MiniLM-L6-v2 embeddings:** https://www.kaggle.com/datasets/ahmedhazemhassan/embedding
- **Kaggle dataset — Test_95_Problems (synthetic):** https://www.kaggle.com/datasets/ahmedhazemhassan/test-95-problems

---

## License

- Repository code: **MIT** (see `LICENSE`)
- Packaged datasets/assets include third-party components distributed under their original licenses and notices (see dataset-specific `THIRD_PARTY_NOTICES` / license
