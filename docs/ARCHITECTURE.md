# Architecture

## Offline preparation

ProgressiveServe prepares three layer groups from Llama 2 7B before deployment.

1. Compute input-output cosine similarity for each layer.
2. Prune consecutive layers 21-28 using the angle-based layer-pruning strategy.
3. Store layers 1-20 and 29-32 as Group A, layers 21-24 as Group B, and layers 25-28 as Group C.
4. Train an A adapter for Stage 1 and an AB adapter for Stage 2 using SQuAD.

## Online recovery

### Stage 1: respond early

- Fetch the model configuration, Group A, and the A adapter.
- Build the full architecture with placeholders for the missing layers.
- Load Group A and replace missing positions with PassLayer modules.
- Activate the A adapter and begin serving.

### Stage 2: recover Group B

- Fetch and load Group B in the background.
- Replace the corresponding PassLayer modules with the recovered layers.
- Disable the A adapter and activate the AB adapter.

### Stage 3: recover Group C

- Fetch and load Group C.
- Replace the remaining PassLayer modules.
- Disable the AB adapter after the original architecture is restored.

## Evaluation boundary

The reported TTFT is wall-clock time from Ray Serve actor creation to the first generated token. Each trial starts after clearing OS page cache and GPU cache. The baseline uses ServerlessLLM with `sllm-store`; ProgressiveServe reads its prepared artifacts directly from the same NFS path.

The published evaluation used:

- Ubuntu 24.04
- NVIDIA driver 580.65.06
- CUDA 13.0
- NVIDIA RTX 5090
- Llama 2 7B in FP16 Safetensors format
- NFSv4 storage over 16 Gbps Ethernet
- TriviaQA validation with zero-shot greedy decoding and `max_new_tokens=10`

These notes summarize Sections 2 and 3 of the paper. They are not a substitute for the full methodology in the PDF.
