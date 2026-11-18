# Google ADK — Installation

## Overview
Google Agent Development Kit (ADK) is a framework for creating agents with instructions, models, tools, orchestration, sessions, and workflow patterns.

## Prerequisites
- Python (check the current ADK compatibility matrix)
- pip and virtual environments
- Gemini API key or Google Cloud authentication, depending on the backend

## Ubuntu setup

```bash
sudo apt update
sudo apt install -y python3 python3-venv python3-pip git
mkdir adk-lab && cd adk-lab
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install google-adk
```

Check installation:

```bash
python --version
pip show google-adk
```

## Local API key setup

Create a key in Google AI Studio, then set it in your shell:

```bash
export GOOGLE_API_KEY="YOUR_API_KEY"
```

For local projects, use an untracked `.env` file and `.gitignore`. Never commit keys.

```gitignore
.env
.venv/
__pycache__/
```

## Scaffold and run

Common ADK CLI workflow:

```bash
adk create my_agent
cd my_agent
adk run my_agent
adk web
```

CLI commands can evolve. Check the installed version's help:

```bash
adk --help
adk run --help
adk web --help
```

## Troubleshooting

| Problem | Check |
|---|---|
| `adk: command not found` | Venv active? ADK installed in that environment? |
| Auth error | Key variable, provider, model access |
| Import error | Project layout and working directory |
| API limit | Quota, access, request rate |
| CLI options differ | Use installed `adk --help` and official docs |

## Lab checklist
- [ ] Create venv and install ADK
- [ ] Configure authentication securely
- [ ] Scaffold an agent project
- [ ] Run it locally
- [ ] Confirm `.env` is not tracked

## References
- Official docs: https://google.github.io/adk-docs/
- Python repository: https://github.com/google/adk-python
