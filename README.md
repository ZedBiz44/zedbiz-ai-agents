# zedbiz-ai-agents
How to set up your custom AI agent team for your marketing business.



Recommended Folder Structure
ai-agent-sops/
├── .github/                     # Workflows, issue templates if needed
├── core/                        # Everything that applies to ALL agents
│   ├── templates/
│   ├── cli-cheatsheet.md
│   ├── memory-strategies.md
│   ├── prompt-engineering.md
│   └── best-practices.md
├── zedbiz-main-vps/                    # Your primary 10 agents
│   ├── deployment/
│   ├── sops/
│   └── agent-specific/
├── zedbiz-vps2/                 # Testing / experimental
│   ├── experiments/
│   └── test-results/
├── zedbiz-vps3/                # Domain-specific agent
│   ├── tools/
│   ├── plumbing-workflows.md
│   └── client-handling.md
├── hermes1/                      # Hermes cluster
│   ├── routing-logic.md
│   ├── orchestration/
│   └── swarm-management.md
├── hermes2/                      # Hermes cluster
│   ├── routing-logic.md
│   ├── orchestration/
│   └── swarm-management.md
├── shared-scripts/              # Any actual bash/python scripts used across servers
└── archives/                    # Old versions you want to keep

Key rules to give your agents:

Core folder is read-heavy, edited rarely.
Each server/group mostly stays in its own folder but is allowed to read core/ and other folders.
Use clear naming and date/version stamps in filenames when making big changes.

Practical Deployment Tips

Sparse checkout or simple path-based pulls: Pete’s server can do git sparse-checkout set pete-plumber (or just cd into the folder and work there).
All servers pull from the same repo but only write to their designated folders.
Use GitHub branches only if you need real isolation (e.g., harry-experiment-branch). Most of the time, folders + good communication between agents is enough.
