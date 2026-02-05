---
datasets: SoloHack/pouring-merged
library_name: lerobot
license: apache-2.0
model_name: act
pipeline_tag: robotics
tags:
- act
- robotics
- lerobot
- physical-ai-hack-2026
---

# SO-101 Water Pouring - Physical AI Hack 2026

This model was created at **Physical AI Hack 2026** (January 31 – February 1, 2026) at Founders Inc, San Francisco.

## Event Info

| | |
|---|---|
| **Event** | [Physical AI Hack 2026](https://physicalaihack.com/) |
| **Track** | Pouring Liquid (Track 03) |
| **Robot** | SO-101 |
| **Framework** | LeRobot + ACT |
| **Team** | Jon Chong, Sota Miyajima, Siddha Kanthi, Udit Karthikeyan |

## Links

| Resource | URL |
|----------|-----|
| Project Submission | https://devspot.app/projects/884 |
| Training Repo | https://huggingface.co/SoloHack/pouring |
| Base Dataset | https://huggingface.co/datasets/HuggingFaceVLA/community_dataset_v3/tree/main/LeRobot-worldwide-hackathon/91-AM-PM-pouring-liquid |
| Presentation | https://docs.google.com/presentation/d/1aEaip9ZFcIVnPLBpbkDN_S2j6nFPJh4cIznNbAK1mvI/edit |
| Event Archive | See `docs/physical_ai_hack_2026_raw_archive.md` |

## Results

- ~25 training episodes recorded (piggybacked on existing community dataset)
- Peak performance: 70-80% success rate
- Models very sensitive to camera positioning

---

# Model Card for act

[Action Chunking with Transformers (ACT)](https://huggingface.co/papers/2304.13705) is an imitation-learning method that predicts short action chunks instead of single steps. It learns from teleoperated data and often achieves high success rates.


This policy has been trained and pushed to the Hub using [LeRobot](https://github.com/huggingface/lerobot).
See the full documentation at [LeRobot Docs](https://huggingface.co/docs/lerobot/index).

---

## How to Get Started with the Model

For a complete walkthrough, see the [training guide](https://huggingface.co/docs/lerobot/il_robots#train-a-policy).
Below is the short version on how to train and run inference/eval:

### Train from scratch

```bash
lerobot-train \
  --dataset.repo_id=${HF_USER}/<dataset> \
  --policy.type=act \
  --output_dir=outputs/train/<desired_policy_repo_id> \
  --job_name=lerobot_training \
  --policy.device=cuda \
  --policy.repo_id=${HF_USER}/<desired_policy_repo_id>
  --wandb.enable=true
```

_Writes checkpoints to `outputs/train/<desired_policy_repo_id>/checkpoints/`._

### Evaluate the policy/run inference

```bash
lerobot-record \
  --robot.type=so100_follower \
  --dataset.repo_id=<hf_user>/eval_<dataset> \
  --policy.path=<hf_user>/<desired_policy_repo_id> \
  --episodes=10
```

Prefix the dataset repo with **eval\_** and supply `--policy.path` pointing to a local or hub checkpoint.

---

## Model Details

- **License:** apache-2.0
