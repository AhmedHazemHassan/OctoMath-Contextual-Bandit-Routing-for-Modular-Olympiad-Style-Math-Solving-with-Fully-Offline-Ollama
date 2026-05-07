# Third-Party Notices

This repository contains original source code and documentation authored by **Ahmed Hazem Hassan** and is licensed under the **MIT License** (see `LICENSE`).

OctoMath is designed to run in a fully offline Kaggle environment. Reproducing the full system may require additional **Kaggle datasets** that bundle third-party software and/or model artifacts (for example: an offline Ollama runtime distribution, runner libraries, and model blobs/manifests, plus an offline embedding model). Those third-party components are **not covered by this repository’s MIT license** and remain subject to their respective upstream licenses and attribution requirements.

## Where to find the applicable third-party licenses

For the canonical reproduction artifacts, please refer to the license/notice files shipped **inside the corresponding Kaggle datasets**, such as (names may vary by version):

- `THIRD_PARTY_NOTICES.txt`
- `LICENSE`
- `NOTICE`
- `QWEN_LICENSE_NOTICE.txt`
- `DEEPSEEK_LICENSE.txt`
- Any additional license files included by upstream projects

## Summary of third-party components used (high level)

Depending on the reproduction path, OctoMath may use:

- **Ollama runtime** (local inference server)
- **Ollama runner libraries** (CPU and/or CUDA/ROCm runners)
- **Open-weight LLM artifacts** referenced by the Ollama model store (e.g., DeepSeek/Qwen-derived model packaging)
- **Sentence-Transformers embedding model** (e.g., `all-MiniLM-L6-v2`) for feature embeddings
- Standard Python scientific stack (e.g., **NumPy**, **SymPy**, etc.)

The exact set of third-party components included in any distribution of OctoMath depends on the specific Kaggle dataset(s) attached and the environment configuration.

## Trademarks

Product names, logos, and brands referenced in this repository (including Kaggle, Ollama, DeepSeek, Qwen, and Sentence-Transformers) are property of their respective owners and are used for identification purposes only.

## Questions / corrections

If you believe a notice is missing or inaccurate, please open an issue or contact the author so it can be corrected.
