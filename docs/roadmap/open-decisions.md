# Jarvis open design decisions

Unresolved product and structure choices. When a decision closes, record the outcome in [`platform-spec.md`](./platform-spec.md) or [`README.md`](./README.md), then remove or archive the row here. See [`MAINTENANCE.md`](./MAINTENANCE.md).

- Which universal scaffolds are mandatory for every target project?
- Which scaffolds should be optional based on project size and risk?
- How much should Jarvis infer from repository files before asking the user? *(Partial: language/framework detection uses high/medium/low/conflict rules in [`stack-scaffolding/detection.md`](../stack-scaffolding/detection.md); other fields still open.)*
- How should Jarvis verify generated files without depending on target-project tooling that may not exist yet?
