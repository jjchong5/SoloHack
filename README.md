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
---

# Model Card for act

<!-- Provide a quick summary of what the model is/does. -->


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

## PhysAI Hackathon 2026 — Reproduction Notes

This model was trained at the **Physical AI Hackathon (Feb 2026, SoloTech)** using an SO101 robot arm
performing a water-pouring task.

### Repos

| Purpose | Repo |
|---|---|
| Model weights & artifacts (this repo) | `https://github.com/jjchong5/SoloHack` |
| Training code (lerobot fork + SO101 camera fixes) | `https://github.com/jjchong5/lerobot-SO101-PhysAIHackathon-SoloTech` |
| Training dataset (HuggingFace Hub) | `https://huggingface.co/datasets/SoloHack/pouring-merged` |

### Setup from scratch

```bash
# 1. Clone the lerobot fork (includes Windows MSMF camera backend fixes for SO101)
git clone https://github.com/jjchong5/lerobot-SO101-PhysAIHackathon-SoloTech
cd lerobot-SO101-PhysAIHackathon-SoloTech

# 2. Install (Ubuntu recommended; see requirements-ubuntu.txt for Windows notes)
pip install -e .

# 3. Re-train on the pouring dataset
lerobot-train \
  --dataset.repo_id=SoloHack/pouring-merged \
  --policy.type=act \
  --output_dir=outputs/train/act-pouring \
  --job_name=act_pouring \
  --policy.device=cuda \
  --policy.repo_id=jjchong5/SoloHack \
  --wandb.enable=true
```

### Run inference on SO101

```bash
lerobot-record \
  --robot.type=so100_follower \
  --dataset.repo_id=jjchong5/eval_pouring \
  --policy.path=jjchong5/SoloHack \
  --episodes=10
```

### Hardware used

- SO101 robot arm (~$350 + printed parts + clamps + webcam, ~$450 total)
- 2 cameras: one on claw, one on improvised tripod
- Training data: ~25 teleoperated leader-follower episodes of water pouring
- Model: ACT (Action Chunking with Transformers)

> **Note:** The upstream lerobot library is at https://github.com/huggingface/lerobot —
> no need to keep a local clone; install via `pip install lerobot` or clone fresh as needed.

---

## Model Details

- **License:** apache-2.0