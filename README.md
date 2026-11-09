# Reading notes

Papers I am reading, grouped by what I am trying to understand. Notes are short on purpose. For each paper I
write down the problem, the core idea, and the part I would actually reuse.

Some of the longer notes are interactive pages, written in Chinese with the original English terms kept.

| Page | What it covers |
|---|---|
| [World models](https://etherimirr.github.io/reading-notes/world-models/) | Foundations, 2023 to 2026 papers, and medical world models. Each entry records what the state is and how the transition is modeled. |
| [Running large models locally](https://etherimirr.github.io/reading-notes/local-inference/) | Why decoding is bound by memory, five lines of work that attack it, and a tokens-per-second calculator. |

## What I care about

**Agent evaluation and user simulation.** A tool-calling agent can pass a benchmark and still fail real users.
I work on measuring that gap: turn-level failure analysis, simulated users, and checking that an LLM judge
agrees with people before trusting its scores. Benchmarks I work with: [tau-bench](https://arxiv.org/abs/2406.12045)
and [tau2-bench](https://arxiv.org/abs/2506.07982).

**Parameter-efficient and multi-task adaptation.** One model, many tasks, and the tasks interfere. My paper
TaskMap (WSDM 2027 Findings, second author) studies where that interference comes from. Its baselines are
[LoRA](https://arxiv.org/abs/2106.09685), [VeRA](https://arxiv.org/abs/2310.11454), and a full-factor hypernetwork.

**World models.** What should the state be, and how do you predict it without generating every pixel.
I am reading this with medical imaging in mind, where visits are irregular and most of the patient is unobserved.

**On-device inference.** Generating a token means reading the active weights from memory once, so capacity
and bandwidth decide what runs locally. Quantization, KV cache compression, and MoE all follow from that.

**Vision on the edge and in 3D.** Detection under a 5W budget on a Jetson Nano, reconstruction, diffusion.

## World models

| Paper | Venue | One line |
|---|---|---|
| [World Models](https://arxiv.org/abs/1803.10122) | Ha & Schmidhuber · 2018 · NeurIPS 2018 | V (encode), M (dynamics), C (control). First policy trained inside its own dream. |
| [PlaNet → DreamerV3](https://arxiv.org/abs/1811.04551) | Hafner et al. · PlaNet ICML 2019 · DreamerV3 Nature 2025 | RSSM: a deterministic memory plus a stochastic latent, the standard latent state for model-based RL. |
| [LeCun: A Path Towards AMI + I-JEPA](https://arxiv.org/abs/2301.08243) | LeCun 2022 (position paper) · Assran et al. ICCV 2023 | Predict in representation space instead of pixels, with a latent for what cannot be predicted. |
| [Genie: Generative Interactive Environments](https://arxiv.org/abs/2402.15391) | Google DeepMind · ICML 2024 Best Paper | Learns latent actions from unlabeled video, so a world model does not need action labels. |
| [DIAMOND: Diffusion for World Modeling](https://arxiv.org/abs/2405.12399) | Alonso et al. · NeurIPS 2024 Spotlight | Diffusion in pixel space as the world model, visual detail matters for the agent. |
| [UniSim: Learning Interactive Real-World Simulators](https://arxiv.org/abs/2310.06114) | Yang, Du et al. · ICLR 2024 | One video model conditioned on many kinds of actions, trained across datasets. |
| [Navigation World Models](https://arxiv.org/abs/2412.03572) | Bar, LeCun et al. · CVPR 2025 Best Paper Honorable Mention | Conditional diffusion transformer that plans navigation by imagining future views. |
| [DINO-WM](https://arxiv.org/abs/2411.04983) | Zhou, LeCun, Pinto · ICML 2025 | World model on frozen DINOv2 patch features, zero-shot planning with no reconstruction. |
| [V-JEPA 2](https://arxiv.org/abs/2506.09985) | Meta FAIR · 2025 | Video JEPA pretrained at scale, then an action-conditioned predictor for robot planning. |
| [Ctrl-World](https://arxiv.org/abs/2510.10125) | Guo, Finn et al. · ICLR 2026 | Controllable multi-view world model for robot manipulation. Reproduced. |
| [SD-JEPA](https://arxiv.org/abs/2605.31111) | 2026 · arXiv | Separates a progression subspace from a content subspace. |
| [LeWorldModel](https://arxiv.org/abs/2603.19312) | 2026 · arXiv | End-to-end JEPA that trains stably without the usual collapse tricks. |
| [Dreamer 4](https://arxiv.org/abs/2509.24527) | Hafner et al. · 2025 | Trains an agent from offline data only, inside the world model. |
| [Understanding World or Predicting Future?](https://arxiv.org/abs/2411.14499) | Ding et al. · ACM Computing Surveys | Survey that splits the field into understanding the world and predicting the future. |
| [EHRXDiff](https://arxiv.org/abs/2409.07012) | Kyung et al. KAIST · CHIL 2025 | Predicts the next chest X-ray from the previous one plus EHR events. |
| [CLARITY](https://arxiv.org/abs/2512.08029) | UCF CRCV · 2025 | Simulates context-aware disease trajectories in latent space to guide treatment decisions. |
| [MeWM](https://arxiv.org/abs/2506.02327) | ICCV 2025 | Medical world model that simulates tumor evolution under treatment. |
| [BrLP](https://arxiv.org/abs/2405.03328) | MICCAI 2024 Oral | Latent diffusion for individual brain MRI progression. |

## Running large models locally

The [page](https://etherimirr.github.io/reading-notes/local-inference/) follows five lines of work:
compressing weights, KV cache and attention, parallel decoding, tiering and offloading, and model
architectures that are friendlier to local hardware.
