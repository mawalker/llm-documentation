# Local Models

## Why Run Locally
- Privacy, offline use, no per-token cost
- Trade-offs: hardware requirements, lower quality than frontier models, slower

## Running Them
- Ollama, LM Studio, vLLM: serve an OpenAI-compatible endpoint
- Pointing a harness at it: Pi, opencode and Hermes Agent all support custom / OpenAI-compatible endpoints

## What Changes
- **Quantization**: smaller, faster models at some cost in quality
- Smaller usable context window
- Weaker and less reliable tool calling
- Simpler, shorter instructions work better (see the example in [Prompting](../prompting/prompting.md), recorded with opencode running `qwen3-coder:30b` on Ollama)
