# SmolLM2-135M Training (ERA HW13)

## Overview
Train the [SmolLM2-135M](https://huggingface.co/HuggingFaceTB/SmolLM2-135M) language model **from scratch** on the provided `input.txt` corpus using Google Colab. The workflow is captured in `smollm2_colab.ipynb` and mirrors the official configuration from the SmolLM repository while adapting it to Colab constraints.

## Contents
- `smollm2_colab.ipynb` – Colab-ready notebook that drives the entire pretraining run.
- `input.txt` – Training corpus (upload this when prompted).
- `stage-1.log`, `stage-2.log` – Example training logs (optional).

## Prerequisites
1. Google account with Colab access (GPU runtime recommended; A100/L4 preferred, T4 works).
2. `input.txt` dataset available locally for upload.
3. Optional: Google Drive for storing checkpoints.

## Running the Notebook
1. Open `smollm2_colab.ipynb` in Google Colab.
2. Switch the runtime to GPU (`Runtime → Change runtime type → GPU`).
3. Execute cells top-to-bottom:
   - Dependency installation pinning NumPy 1.26.4 to avoid binary incompatibilities.
   - Automatic download of `config_smollm2_135M.yaml`.
   - Upload `input.txt` when prompted (uses `files.upload()`).
   - Tokenization into 2,048-token blocks with a dedicated pad token.
4. Launch Stage 1 training (5,000 steps). A sample generation callback prints outputs every 500 steps.
5. Run the “Sync trainer state” cell to copy `trainer_state.json`, optimizer, and scheduler artifacts into `smollm2_135m_runs/stage1_5000_steps/checkpoint-final`.
6. Launch Stage 2 to resume training for 50 additional steps (total 5,050). The notebook automatically reloads weights and optimizer state.

## Outputs
- Checkpoints under `smollm2_135m_runs/`:
  - `stage1_5000_steps/checkpoint-final/` – Model weights after Stage 1 plus trainer state for resumption.
  - `stage2_5050_steps/checkpoint-final/` – Model weights after Stage 2.
- TensorBoard logs located inside each stage directory (`logs/`).

## Continuing Training
- To extend beyond 5,050 steps, adjust `max_steps`, `save_steps`, and resume from the latest `checkpoint-final` directory using the same pattern as Stage 2.
- The notebook detects whether bfloat16 is available; otherwise it defaults to float32 to avoid GradScaler issues on older GPUs.

## Troubleshooting
- **NumPy binary incompatibility:** Restart runtime and rerun the install cell to pin NumPy 1.26.4 before importing `datasets`.
- **FlashAttention missing:** The notebook falls back to standard attention; optionally install `flash-attn` (`pip install flash-attn --no-build-isolation`) before loading the model.
- **Missing `trainer_state.json`:** Always run the sync cell after Stage 1 so Stage 2 can resume optimizers and scheduler state.

## References
- SmolLM configuration: https://raw.githubusercontent.com/huggingface/smollm/main/text/pretraining/smollm2/config_smollm2_135M.yaml
