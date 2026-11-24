# Quality, Safety, and Evaluation
- Evaluating agent/skill quality (task success rate, step efficiency, hallucination rate)
- Verify, don't trust: check the actual result rather than the model's report that it succeeded
- Failure modes: loop, tool misuse, context overflow, prompt injection via retrieved content
- The **lethal trifecta**: an agent that has access to private data, is exposed to untrusted content, and can communicate externally can be tricked into leaking data ([Simon Willison, 2025](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/))
- Guardrails: iteration caps, tool allow-lists, output filters, human approval gates
- Supply chain: third-party skills, plugins and MCP servers run with the agent's permissions
- PII, confidentiality, compliance
- Logging, monitoring, audit trails
