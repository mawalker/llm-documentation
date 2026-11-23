# Structured and Reliable Output
- Requesting JSON, tables, machine-readable formats
- Native schema-enforced modes: OpenAI Structured Outputs (`strict: true`), Anthropic JSON outputs and strict tool use — these guarantee the output matches your schema, unlike just asking for JSON
- Validation and error handling
- Determinism strategies — note that temperature 0 is still not fully deterministic in practice ([Thinking Machines, 2025](https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/))
- Post-processing
