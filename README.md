# Copilot analytics guidelines

A short guide that GitHub Copilot follows while doing data analysis. It keeps inputs and outputs tidy, makes every number traceable to a saved DuckDB query, and checks in with you on what the final story should say.

Status: draft. [docs/design.md](docs/design.md) has the reasoning, open questions, and validation plan.

## Use

Copy [`guide/AGENTS.md`](guide/AGENTS.md) to the root of your analysis project. Copilot reads a root `AGENTS.md` as project instructions; see [Use custom instructions in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-instructions).

The project needs DuckDB, and Git for history.

## Relationship to analytics-workbench

This project is a separate, much smaller take on [analytics-workbench](https://github.com/quentin-hickam/analytics-workbench). That project remains available as a fuller skill suite with helpers and an audit trail.

## License

MIT. See [LICENSE](LICENSE).
