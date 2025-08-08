# AI‑Driven Heart Health Monitoring System *Custom‑Built Digital Stethoscope*

<img width="1379" height="626" alt="frontend-interface" src="https://github.com/user-attachments/assets/e6a75deb-2224-4f01-b88d-4843ed3b1ae5" />


A **full‑stack, open‑hardware** toolkit for **non‑invasive cardiac auscultation and AI‑assisted disease screening**.  The project spans **3D‑printed hardware**, a **Residual‑CRNN** built in TensorFlow, a **FastAPI** inference service, and a **Next.js** web client capable of real‑time waveform visualisation.  Target latency: **< 150 ms**; Macro‑F1 ≥ **0.90** across *normal*, *murmur*, and *artifact* PCG classes.

---

## Key Features

* **Custom Digital Stethoscope** — PLA probe, MEMS mic, MAX9814 pre‑amp; parts < USD 29.
* **Residual‑CRNN** — 3‑block architecture with squeeze‑excite + spectral aug augmentation.
* **FastAPI + ONNX Runtime** — CPU inference ≈ 60 ms (Raspberry Pi 4).
* **Next.js Dashboard** — Live waveform, log‑Mel spectrogram, class probability bar.
* **Reproducibility** — ML pipeline tracked with **DVC**, metrics via **MLflow**.
* **DevOps** — Docker Compose, GitHub Actions (lint, pytest, docs, push → ECR), Terraform AWS stack.
* **Academic Artifacts** — MkDocs site mirrors thesis background/methodology; `papers/` holds pre‑print & poster.

---

## Tech Stack

| Layer       | Technology                                  |
| ----------- | ------------------------------------------- |
| Hardware    | 3D‑printed probe                            |
| ML Training | TensorFlow                                  |
| Backend     | FastAPI                                     |
| Frontend    | React 18 + Next.js 14, Tailwind CSS,        |
| DevOps      | Docker, GitHub Actions, Nginx               |

<img width="494" height="290" alt="end-to-end-deployment-pipeline" src="https://github.com/user-attachments/assets/6e33620e-97d7-441f-bab1-1de0a1972266" />

---

## Directory Layout

```text
heart-health-stethoscope/
├── docs/               ← MkDocs site (results)
├── data/               ← DVC pointers; ≤10 anonymised WAVs only
├── notebooks/          ← Jupyter exploration (ordered 00-, 01-, …)
├── models/             ← Versioned .md5, .keras
├── hardware/
│   ├── cad/            ← .stl / .step
│   └── renders/        ← PNGs
├── webapp/
│   ├── backend/        ← FastAPI service + Dockerfile
│   └── frontend/       ← Next.js client + Tailwind config
└── papers/             ← Thesis pre‑print
```

---

## REST API

| Method | Endpoint   | Payload (JSON / multipart)      | Returns                                |
| ------ | ---------- | ------------------------------- | -------------------------------------- |
| POST   | `/predict` | `{ "file": <WAV> }`             | `{ "class":"murmur", "proba":0.91 }`   |
| GET    | `/health`  | –                               | `200 OK`                               |

Live Swagger docs on `http://localhost:8080/docs`.

### Example curl

```bash
curl -X POST -F file=@demo.wav \
     http://localhost:8080/predict | jq
```

---

## Dataset & Weights

Large artefacts are stored with **DVC** + **Git LFS**.  After cloning:

```bash
$ dvc pull data models     # requires DVC ≥3.0 & access token
```

Dataset summary:

| Split | WAV files | Duration (min) | Pathologies              |
| ----- | --------- | -------------- | ------------------------ |
| Train | 3 210     | 174            | normal, murmur, artifact |
| Val   | 429       | 23             | —                        |
| Test  | 648       | 34             | —                        |

---

## Train & Evaluate

<img width="518" height="219" alt="confusion-matrix" src="https://github.com/user-attachments/assets/d659e2ca-91ea-4734-b81f-26f7f0481724" />

<img width="432" height="202" alt="lurning-curve" src="https://github.com/user-attachments/assets/d21148e4-65f2-43bd-84d1-171d04527615" />

---

## Benchmark Results

| Metric            | Value     |
| ----------------- | --------- |
| F1‑macro          | **0.89** |
| Model size        | 11.4 MB    |

---

## Hardware Build

![parts-of prob](https://github.com/user-attachments/assets/849b6f79-ae34-47cb-bf91-731eabc0b71e)

