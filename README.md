# Hands-On Labs for Microsoft Foundry Agents

A series of progressive, **notebook-based** labs for building agents on
[Microsoft Foundry](https://learn.microsoft.com/azure/ai-foundry/), inspired by
[Azure/azure-ai-agents-labs](https://github.com/Azure/azure-ai-agents-labs) but
rewritten to use the **latest Agent Framework + Foundry hosting SDKs** and the
same patterns used in the parent repository.

Two agent styles are covered:

| Style | What it is | Where it runs |
|---|---|---|
| **Prompt agent** | A declarative agent (model + instructions) created via the Foundry SDK | Managed by Foundry; invoke via the OpenAI-compatible API |
| **Hosted agent** | Your own container (Agent Framework) deployed to Foundry | Runs as a container with its own dedicated agent identity |

## Labs

| Lab | Notebook | Focus |
|---|---|---|
| 1 | [labs/Lab 1 - Project Setup.ipynb](labs/Lab%201%20-%20Project%20Setup.ipynb) | Install libraries, connect to your project, test a chat completion |
| 2 | [labs/Lab 2 - Prompt Agent.ipynb](labs/Lab%202%20-%20Prompt%20Agent.ipynb) | Create + invoke a **prompt agent**; add a new version |
| 3 | [labs/Lab 3 - Hosted Agent.ipynb](labs/Lab%203%20-%20Hosted%20Agent.ipynb) | Scaffold, deploy (azd) and invoke a **hosted agent** (Responses protocol) |
| 4 | [labs/Lab 4 - Hosted Agent RAG.ipynb](labs/Lab%204%20-%20Hosted%20Agent%20RAG.ipynb) | Add **RAG** over Azure AI Search + the agent-identity RBAC gotcha |
| 5 | [labs/Lab 5 - Evaluating a RAG Agent.ipynb](labs/Lab%205%20-%20Evaluating%20a%20RAG%20Agent.ipynb) | **Evaluate** the RAG agent: groundedness, citation, out-of-scope refusal + LLM-as-judge |

Run the notebooks in order — each builds on the previous.

## Prerequisites

- An **Azure AI Foundry** project with a deployed chat model (e.g. `gpt-4.1` or `gpt-5-mini`)
- **Python 3.10+**
- **Azure CLI** signed in: `az login`
- For hosted agents (Labs 3–4): **Docker** running locally and the **azd AI agent extension**
  ```bash
  azd extension install azure.ai.agents
  azd auth login
  ```
- For Lab 4: an **Azure AI Search** service

## Quick start

```bash
# from this folder
python -m venv .venv
# Windows
.venv\Scripts\Activate.ps1
# macOS/Linux
source .venv/bin/activate

pip install -r requirements.txt
cp .env.example .env   # then fill in your project endpoint + model
```

Open **Lab 1** and select the `.venv` as the notebook kernel.

## Required RBAC

| Identity | Role | Needed for |
|---|---|---|
| Your signed-in user | **Azure AI User** (or higher) on the Foundry project | All labs |
| Your signed-in user | **Search Service Contributor** + **Search Index Data Contributor** on the search service | Lab 4 (provision the index) |
| **Hosted agent's own identity** | **Search Index Data Reader** on the search service | Lab 4 (the agent queries as its *own* identity, not yours or the project MI) |

> The agent-identity RBAC step is the most common hosted-agent RAG gotcha — Lab 4
> automates it by reading the agent's identities from its version definition.

## License

MIT — see [LICENSE](LICENSE).
