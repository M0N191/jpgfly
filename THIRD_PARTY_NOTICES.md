# Third-party notices and research attribution

Audited 7 October 2026 against the public JPGFLY source. This document records external inputs, their terms, and the boundary between those inputs and JPGFLY's implementation. The [MIT license](LICENSE) covers original project source; external software, datasets, model weights and artwork retain their applicable terms. Implementation statuses follow [Features](docs/FEATURES.md).

## 1. Scientific datasets and research

### MaleCNS — optional external-service reference

[MaleCNS](https://male-cns.janelia.org/) is the Drosophila male central nervous system connectome produced by FlyEM at HHMI Janelia, the University of Cambridge Department of Zoology, MRC Laboratory of Molecular Biology and Google Research. The official [`male-cns:v1.0` dataset](https://male-cns.janelia.org/download/) is licensed [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

JPGFLY's [client](brain_provider.py) requests optional action bias from an external service; [telemetry](app.py) uses the label `MaleCNS v1.0`. The public repository contains neither the simulator nor connectome data, and does not pin or verify the dataset/build loaded by a configured service. That service's code license must be checked separately. Numerical dynamics, action-bias mappings and display layouts are engineered interpretations, not biological claims by the MaleCNS researchers.

Research citation: Berg et al. (2026), *Sexual dimorphism in the complete Drosophila male central nervous system connectome*, Cell 189(18), 5504–5526.e15. [Publication and authors](https://www.janelia.org/publication/sexual-dimorphism-in-the-complete-drosophila-male-central-nervous-system-connectome); DOI `10.1016/j.cell.2026.08.015`.

### ZebraCNS / ZAPBench — optional recorded-data integration

The [ZebraCNS local service](zebracns_local.py) accesses ZAPBench's released trace volume at `gs://zapbench-release/volumes/20240930/traces/`. The [official dataset documentation](https://zapbench-release.storage.googleapis.com/volumes/README.html) licenses the recordings **CC BY 4.0** and credits:

- Alex Bo-Yuan Chen in the Ahrens lab at HHMI Janelia for functional recordings;
- the CellMap Project Team at HHMI Janelia for segmentation annotations;
- Google Research for processed datasets, including alignment, segmentation and trace extraction.

JPGFLY downloads and caches a distributed subset locally, normalizes recorded activity, and maps it into bounded critic/behavior signals. These mappings and visual layouts are JPGFLY engineering; they are not biological art judgments, anatomical motor labels or validated accounts of intent. Recordings and cache files are not bundled in the tracked source.

Research citation: Lückmann et al. (2025), *ZAPBench: A Benchmark for Whole-Brain Activity Prediction in Zebrafish*, ICLR. [Official publication and authors](https://research.google/pubs/zapbench-a-benchmark-for-whole-brain-activity-prediction-in-zebrafish-2/).

The separate [ZAPBench benchmark code](https://github.com/google-research/zapbench) is [Apache-2.0 licensed](https://github.com/google-research/zapbench/blob/7347b3b21a3f332e281b774c945f1505dd966abf/LICENSE). JPGFLY supplies its own loader/adapter and does not vendor that benchmark package. Its code license and the recording license are distinct.

### MICrONS / Mouse Vision — research direction

JPGFLY names [MICrONS Explorer](https://www.microns-explorer.org/) as a visual-perception research reference. Credit belongs to the MICrONS Consortium, including the Allen Institute for Brain Science, Princeton's Seung Lab and Baylor's Tolias Lab, with IARPA support. **Mouse Vision remains RESEARCH DIRECTION:** no MICrONS data loader, digital-twin inference, CAVEclient integration, model weights or dataset are included in this public runtime.

Research citation: MICrONS Consortium et al. (2025), *Functional connectomics spanning multiple areas of mouse visual cortex*, Nature 640, 435–447; DOI `10.1038/s41586-025-08790-w`. The [citation policy](https://www.microns-explorer.org/citation-policy) identifies the appropriate publications and website attribution. Explorer material is **CC BY 4.0** under its [terms](https://www.microns-explorer.org/terms-and-conditions); this does not establish a license for separately released model weights. This entry is research acknowledgement, not a claim of implemented data use.

## 2. Language and model stack

### FLM — Fly Language Model

[FLM](https://github.com/nftechie/flm), by **Alex Wormuth / nftechie**, supplies the optional written Fly voice and Room-writing runtime. Its original code is [MIT licensed](https://github.com/nftechie/flm/blob/main/LICENSE), copyright 2026 Alex Wormuth. FLM is external work; JPGFLY's [bridge](flm_bridge.py) and [provider](flm_text_provider.py) interface with a separately installed checkout and completed conversational adapter run. The public repository does not vendor FLM, its trainer, trained adapters or model weights.

The current upstream conversation recipe uses Liquid AI's [LFM2.5-1.2B-Instruct](https://huggingface.co/LiquidAI/LFM2.5-1.2B-Instruct), governed by the **[LFM Open License v1.0](https://huggingface.co/LiquidAI/LFM2.5-1.2B-Instruct/blob/main/LICENSE)**. This is a separate model license with commercial-use conditions tied to its defined US$10 million revenue threshold; FLM's MIT license does not replace it.

The upstream recipe also uses Hugging FaceTB's [Everyday Conversations](https://huggingface.co/datasets/HuggingFaceTB/everyday-conversations-llama3.1-2k) subset, declared **Apache-2.0**, through [SmolTalk](https://huggingface.co/datasets/HuggingFaceTB/smoltalk). SmolTalk's existing component datasets retain their original licenses; the collection is not uniformly covered by one license. See [FLM's upstream notices](https://github.com/nftechie/flm/blob/main/THIRD_PARTY_NOTICES.md) for its additional inputs.

These statements describe the audited upstream recipe. Public JPGFLY does not pin an FLM revision or include an adapter training manifest; an operator's actual run, base model and training data require independent verification.

### Qwen

Credit: **Qwen Team / Alibaba Cloud**.

| Public configuration/reference | Official source | License |
| --- | --- | --- |
| `qwen3-vl:8b-instruct-q4_K_M` in [.env.example](.env.example) and the [visual composition client](composition_vision.py) | [Qwen3-VL](https://github.com/QwenLM/Qwen3-VL), [Qwen3-VL-8B-Instruct model card](https://huggingface.co/Qwen/Qwen3-VL-8B-Instruct), [exact Ollama tag](https://ollama.com/library/qwen3-vl:8b-instruct-q4_K_M) | Apache-2.0 |
| `qwen3:8b` fallback when the [narrative provider](narrative_provider.py)'s model setting is unset | [Qwen3](https://github.com/QwenLM/Qwen3), [Qwen3-8B model card](https://huggingface.co/Qwen/Qwen3-8B) | Apache-2.0 |

Qwen provides advisory composition/context and writing support. Candidate Fly Brain retains art-decision authority. Model identifiers are configurable defaults, not evidence that a model is installed or running. Weights are obtained separately, and an installed model's license/NOTICE files remain authoritative for that artifact.

## 3. Model/runtime software

[Ollama](https://github.com/ollama/ollama) is the separately installed local model server used by JPGFLY's HTTP clients. The open-source CLI/server is [MIT licensed](https://github.com/ollama/ollama/blob/main/LICENSE), copyright Ollama. Its software license does not replace the licenses of models it serves. This credit covers the open-source runtime, not every separately distributed Ollama product.

FLM's separate environment includes its own model/runtime dependencies and notices. This repository does not bundle that environment; preserve the notices supplied by its actual installed packages.

## 4. Direct application dependencies

These package-manager-installed libraries are credited for the public [requirements](requirements.txt). They retain their own licenses and any additional notices shipped in binary distributions.

| Project / maintainers | Pinned version | Verified upstream license |
| --- | --- | --- |
| [FastAPI / Sebastián Ramírez and contributors](https://github.com/fastapi/fastapi) | 0.141.1 | [MIT](https://github.com/fastapi/fastapi/blob/0.141.1/LICENSE) |
| [Starlette / contributors](https://github.com/Kludex/starlette) | 1.6.0 | [BSD-3-Clause](https://github.com/Kludex/starlette/blob/1.6.0/LICENSE.md) |
| [Uvicorn / contributors](https://github.com/Kludex/uvicorn) | 0.35.0 | [BSD-3-Clause](https://github.com/encode/uvicorn/blob/0.35.0/LICENSE.md) |
| [Pydantic / Samuel Colvin and contributors](https://github.com/pydantic/pydantic) | 2.11.7 | [MIT](https://github.com/pydantic/pydantic/blob/v2.11.7/LICENSE) |
| [NumPy / NumPy Developers](https://github.com/numpy/numpy) | 2.1.3 | [BSD-3-Clause](https://github.com/numpy/numpy/blob/v2.1.3/LICENSE.txt) |
| [Pillow / Alex Clark and contributors; PIL upstream](https://github.com/python-pillow/Pillow) | 12.3.0 | [MIT-CMU / PIL-style license](https://github.com/python-pillow/Pillow/blob/12.3.0/LICENSE) |

[TensorStore / Google and contributors](https://github.com/google/tensorstore) is the optional, unpinned data-access library imported by `zebracns_local.py`. It is **[Apache-2.0 licensed](https://github.com/google/tensorstore/blob/v0.1.85/LICENSE)**; that link identifies a verified upstream release, not a version installed by JPGFLY. TensorStore is not in the base requirements. Its bundled components retain their own notices.

## 5. Artwork, sprites, icons and fonts

The maintainer reports **ChatGPT / OpenAI** as the source used to generate the project's fly, logo and banner artwork, including the supplied README banner. This is maintainer-provided provenance; per-image generation records, exact model versions and complete edit histories are not recorded in the public repository.

| Tracked asset group | Provenance / modification record |
| --- | --- |
| [README banner](assets/jpgfly-readme-banner.jpg) and [profile banner](assets/jpgfly-profile-banner-1500x500.jpg) | Project-supplied artwork; reported ChatGPT origin. Exact generation/export history is unverified. |
| [Fly image](web/images/fly.png), [site icon](web/images/jpgfly-icon-white.png), [favicon](web/favicon.png) | Project artwork; reported ChatGPT origin. Per-file source and edit lineage are unverified. |
| Root `github-banner-*` and `jpgfly-banner*` images/SVGs | Project banner variants. Individual derivation/export records are unverified. |
| [Image-directory JPEG](web/images/github-banner.jpg) and [SVG](web/images/github-banner.svg) | The SVG embeds the same JPEG bytes; this establishes wrapping, not independent authorship or permission. |

[OpenAI's Terms of Use](https://openai.com/policies/terms-of-use/) assign output rights to the user as between the user and OpenAI, to the extent permitted by law. They do not grant an open-source artwork license to downstream JPGFLY users, establish exclusive copyright, or resolve rights in third-party input/output. **Manual review remains needed:** record a per-asset origin/edit record and an explicit artwork reuse grant. This notice does not assign a new artwork license. Consult the existing [asset documentation](web/images/README.md) and [trademark notice](TRADEMARK.md).

The tracked source contains no identifiable external sprite/game asset pack or vendored font files. UI and SVG font declarations use system font families; no external font download is configured. Five tracked image files could not be fully decoded during the metadata audit (`github-banner-requested.webp`, `jpgfly-banner-requested.webp`, `web/favicon.png`, `web/images/github-banner.jpg`, `web/images/jpgfly-icon-white.png`); their visual contents remain unverified. No assets were changed.

## 6. Original JPGFLY implementation

The source audit identifies Candidate Fly Brain / FlyBrain, server-authoritative painting mechanics, Experience Memory, Learned Art Policy, Backrooms archive, artist profiles, application UI/visualization and the application-side ZebraCNS critic mappings as JPGFLY-specific implementation. It found no evidence of substantial upstream Candidate Fly Brain source being copied; no external FlyBrain attribution is inferred.

FLM, Qwen and the scientific datasets remain external inputs. Using their outputs or data does not make them the authors of JPGFLY's decision engine, memory, policy or artistic mappings. Conversely, JPGFLY's source license does not relicense those inputs. Future adapted or vendored code needs its source, exact license, modification history and required notices recorded here.

## 7. Redistribution and notice requirements

The citations and integration credits above are informational/research acknowledgements where only an external interface or research reference is present. License conditions apply when licensed material is used or redistributed as specified by its own terms:

- Keep JPGFLY's root `LICENSE` with its original source. Preserve this attribution record with distributions.
- For redistributed MIT code, retain its copyright and permission notice. BSD distributions require their applicable copyright, conditions and disclaimer; retain Pillow's supplied notice and terms as well.
- For [Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0) code/models/corpus material, include the license, retain applicable attribution and NOTICE content, and identify modifications as required. Do not replace these with JPGFLY's MIT notice.
- When sharing [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/legalcode.en) data or adaptations, retain supplied creator, copyright, license and disclaimer information, link the source/license, and indicate modifications. The ZAPBench sampling/normalization and JPGFLY engineering are described above. Follow the MICrONS citation policy if its material is later used.
- Separately downloaded model weights and adapters retain their own terms, including the LFM Open License's conditions. Verify the actual artifact and training-data provenance before redistributing it.
- Keep notices supplied with installed wheels, runtimes and bundled subcomponents. This document covers substantive public inputs and direct dependencies; it is not a replacement for distribution-specific license files.
- Resolve the artwork reuse grant and any external-service provenance before treating those artifacts as freely redistributable. The project does not bundle scientific datasets, upstream model weights or external service environments in this source tree.

## 8. No endorsement

Third-party names and marks identify their respective projects and owners. These credits do not imply endorsement, sponsorship, partnership or affiliation with JPGFLY. Recorded biological data, simulated dynamics, engineered artistic signals and character voice are separate concepts; the upstream researchers make no JPGFLY-specific claims of biological intent, consciousness or artistic agency.
