# mongodb-bootstrap

Get connected and start developing with MongoDB in your IDE in minutes. This repo is a quick-start path for AI-assisted MongoDB development: a local database, MCP connectivity, and official agent skills.

**Quick start checklist**

1. [Prerequisites](#prerequisites)
2. [Start a local MongoDB](#step-1--start-a-local-mongodb)
3. [Connect your IDE via MCP](#step-2--connect-your-ide-via-mcp)
4. [Install MongoDB agent skills](#step-3--install-mongodb-agent-skills)
5. [Verify you're connected](#step-4--verify-youre-connected)

**Related projects**

- [mongodb/agent-skills](https://github.com/mongodb/agent-skills) — official MongoDB agent skills
- [mongodb-js/mongodb-mcp-server](https://github.com/mongodb-js/mongodb-mcp-server) — MCP server for MongoDB and Atlas
- [MongoDB Agent Skills docs](https://www.mongodb.com/docs/agent-skills/)

## Prerequisites

Install these once. The same stack works regardless of IDE; only the config file location differs.

| Tool | Purpose |
|------|---------|
| [Node.js](https://nodejs.org/) 20.19+ | Runs the MongoDB MCP server |
| [Docker Desktop](https://www.docker.com/get-started/) 4.31+ (macOS/Windows) or Docker Engine 27+ (Linux) | Local Atlas deployment |
| [Atlas CLI](https://www.mongodb.com/docs/atlas/cli/current/install-atlas-cli/) | Create and manage local MongoDB |

```bash
brew install mongodb-atlas-cli
```

## Step 1 — Start a local MongoDB

Create a local Atlas deployment and save the connection string for Step 2.

```bash
atlas local setup --force
atlas local connect --connectWith connectionString
```

Optional: load sample datasets during setup:

```bash
atlas local setup --loadSampleData true
```

See [Create a Local Atlas Deployment](https://www.mongodb.com/docs/atlas/cli/current/atlas-cli-deploy-local/) for interactive setup, custom ports, and management commands.

## Step 2 — Connect your IDE via MCP

The [MongoDB MCP Server](https://github.com/mongodb-js/mongodb-mcp-server) lets your IDE agent query and explore MongoDB directly.

### Cursor

1. Copy the example config:

   ```bash
   cp .cursor/mcp.json.example .cursor/mcp.json
   ```

2. Replace `MDB_MCP_CONNECTION_STRING` with the connection string from Step 1.

3. Reload MCP servers in Cursor settings.

Example `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "mongodb": {
      "command": "npx",
      "args": ["-y", "mongodb-mcp-server@latest", "--readOnly"],
      "env": {
        "MDB_MCP_CONNECTION_STRING": "mongodb://localhost:27017/?directConnection=true"
      }
    }
  }
}
```

Replace the connection string with your local deployment URL. `--readOnly` is recommended for safety; remove it when you need write access. For Atlas cloud clusters or service-account auth, see the [mongodb-mcp-server configuration docs](https://github.com/mongodb-js/mongodb-mcp-server#setup).

### Other IDEs

Use the same `npx` command and environment variables. Config file location varies by client:

- **VS Code / Copilot** — see [VS Code MCP docs](https://code.visualstudio.com/docs/copilot/chat/mcp-servers) and the [mongodb-mcp-server README](https://github.com/mongodb-js/mongodb-mcp-server)
- **Claude Desktop, Copilot CLI, OpenCode** — examples in the [mongodb-mcp-server README](https://github.com/mongodb-js/mongodb-mcp-server#setup)

Or run interactive setup for any client:

```bash
npx mongodb-mcp-server@1 setup
```

## Step 3 — Install MongoDB agent skills

Agent skills teach your IDE assistant MongoDB patterns: schema design, query writing, search, MCP setup, and more.

### Cursor

From this repo root:

```bash
./scripts/install-agent-skills.sh
```

This clones [mongodb/agent-skills](https://github.com/mongodb/agent-skills) and copies skills into `.cursor/skills/`. Re-run the script to refresh from upstream.

Alternative: install via the Cursor marketplace with `/add-plugin mongodb`.

### Other IDEs

See [agent-skills installation](https://github.com/mongodb/agent-skills#installation) for Claude, Codex, Gemini, Copilot CLI, and [skills.sh](https://skills.sh/).

## Step 4 — Verify you're connected

In Cursor:

1. Reload MCP servers if you have not already.
2. Ask your agent: **"List my MongoDB databases"** or **"Describe the schema for my collections."**
3. Confirm MongoDB skills are available (e.g. `mongodb-natural-language-querying`, `mongodb-search-and-ai`).

If the agent cannot connect, check that your local deployment is running (`atlas local list`) and that the connection string in `.cursor/mcp.json` matches.

## Optional — Search and Vector Search on local Atlas

Local Atlas deployments support [MongoDB Search](https://www.mongodb.com/docs/atlas/atlas-search/) and [Vector Search](https://www.mongodb.com/docs/atlas/atlas-vector-search/). Vector Search requires MongoDB **7.0.5+**; delete and recreate older local deployments if needed.

Create indexes interactively:

```bash
atlas local search indexes create
```

Manage deployments:

```bash
atlas local list
atlas local start
atlas local stop
atlas local delete
```

For full Search and Vector Search workflows, see:

- [Use MongoDB Search and Vector Search with Atlas CLI](https://www.mongodb.com/docs/atlas/cli/current/atlas-cli-deploy-fts/)
- [Vector Search with a local Atlas deployment](https://www.mongodb.com/docs/atlas/cli/current/atlas-cli-deploy-local/#use-mongodb-vector-search-with-a-local-atlas-deployment)
