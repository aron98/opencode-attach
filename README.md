# opencode-server

An [agent skill](https://skills.sh) for delegating coding tasks to an [OpenCode](https://opencode.ai) server.

## What it does

Teaches AI agents to use OpenCode as a dedicated coding backend. Instead of writing code directly, the agent delegates all coding tasks — writing, debugging, refactoring, reviewing, testing — to an OpenCode server via its CLI.

## Install

```bash
npx skills add aron98/opencode-attach
```

Or install for a specific agent:

```bash
npx skills add aron98/opencode-attach -a claude-code
npx skills add aron98/opencode-attach -a opencode
npx skills add aron98/opencode-attach -a cursor
```

## Prerequisites

- [OpenCode CLI](https://opencode.ai) installed and available on `PATH`
- A running OpenCode server the agent can reach
- Server credentials configured in the environment

## License

MIT
