# Skill: Install MCP Server into ContextForge Pipeline

> **Use when:** Adding any new MCP server to Ryan's pipeline.
> **Time:** 5-10 minutes if you follow this exactly. 30+ minutes if you wing it.
> **Last updated:** February 8, 2026

---

## Prerequisites

| Item | Value |
|------|-------|
| VPS2 IP | 72.60.225.81 |
| SSH Tool | `gateway__ssh_execute` and `gateway__ssh_write_file` via Labs connector |
| GitHub PAT | `USE_EXISTING_PAT_FROM_mcp-github_CONTAINER` |
| ContextForge container | `contextforge` |
| ContextForge DB | `/app/app/data/mcp.db` (SQLite) |
| Team ID | `d4cf9c59e66a4d26a64c5f70fb9dfb81` |
| Owner email | `admin@example.com` |
| Docker networks | `internal`, `mcp-network` |
| Stack directory | `/opt/stacks/<server-name>/` |
| Project directory | `/opt/projects/<server-name>/` (for source code) |

## Step 0: Find Next Available Port

```bash
for p in $(seq 9033 9050); do
  if ! docker ps --format '{{.Ports}}' | grep -q ":$p->"; then
    echo "Next available: $p"
    break
  fi
done
```

Current port map (as of Feb 8, 2026):

| Port | Server |
|------|--------|
| 9001 | mcp-filesystem |
| 9004 | mcp-git |
| 9005 | mcp-postgres |
| 9006 | mcp-fetch |
| 9007 | mcp-brave-search |
| 9010 | mcp-slack |
| 9011 | mcp-memory |
| 9012 | mcp-docker |
| 9013 | mcp-github (localhost only) |
| 9014 | mcp-sequential-thinking |
| 9015 | mcp-context7 |
| 9017 | mcp-cloudflare |
| 9020 | mcp-time |
| 9021 | mcp-sqlite |
| 9022 | mcp-everything |
| 9025 | mcp-linkedin (localhost only) |
| 9026 | mcp-facebook (localhost only) |
| 9030 | mcp-filetransfer (localhost only) |
| 9031 | mcp-n8n |
| 9032 | mcp-github-projects |
| **9033** | **Next available** |

---

## Step 1: Clone and Build

Clone to `/opt/projects/<name>`, build in `/opt/stacks/<name>/`.

```bash
cd /opt/projects
git clone <repo-url> <name>
mkdir -p /opt/stacks/<name>
```

---

## Step 2: Dockerfile

Create `/opt/stacks/<name>/Dockerfile`.

### CRITICAL: Use streamableHttp, NOT SSE

Supergateway's SSE mode only supports ONE connection at a time. ContextForge's health checks open connections repeatedly and will crash the server with:
```
Error: Already connected to a transport. Call close() before connecting to a new transport
```

**Always use `--outputTransport streamableHttp`.**

### Template: Node.js server with supergateway

```dockerfile
FROM node:22-alpine

# Install supergateway globally
RUN npm install -g supergateway

WORKDIR /app
COPY . .
RUN npm install && npm run build

EXPOSE 8000

# ALWAYS use streamableHttp + healthEndpoint
ENTRYPOINT ["supergateway", \
  "--stdio", "node /app/build/index.js", \
  "--port", "8000", \
  "--outputTransport", "streamableHttp", \
  "--healthEndpoint", "/healthz"]
```

### Template: Python server with supergateway

```dockerfile
FROM python:3.12-slim

RUN apt-get update && apt-get install -y curl && \
    curl -fsSL https://deb.nodesource.com/setup_22.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g supergateway

WORKDIR /app
COPY . .
RUN pip install --break-system-packages -r requirements.txt

EXPOSE 8000

ENTRYPOINT ["supergateway", \
  "--stdio", "python -m <module_name>", \
  "--port", "8000", \
  "--outputTransport", "streamableHttp", \
  "--healthEndpoint", "/healthz"]
```

### Template: Using supercorp/supergateway image (simplest)

If the MCP server is available as a pip/npm package:

```dockerfile
FROM supercorp/supergateway:latest

ENTRYPOINT ["supergateway", \
  "--stdio", "uvx <package-name>", \
  "--port", "8000", \
  "--outputTransport", "streamableHttp", \
  "--healthEndpoint", "/healthz"]
```

### If the server needs bun:

```dockerfile
FROM node:22-alpine

RUN apk add --no-cache curl bash && \
    curl -fsSL https://bun.sh/install | bash
ENV BUN_INSTALL=/root/.bun
ENV PATH=/root/.bun/bin:$PATH

RUN npm install -g supergateway

WORKDIR /app
COPY . .
RUN bun install

# Use npx instead of bunx for build tools (bunx has PATH issues in Docker)
RUN npx <build-command>

EXPOSE 8000

ENTRYPOINT ["supergateway", \
  "--stdio", "node /app/build/index.js", \
  "--port", "8000", \
  "--outputTransport", "streamableHttp", \
  "--healthEndpoint", "/healthz"]
```

---

## Step 3: docker-compose.yml

Create `/opt/stacks/<name>/docker-compose.yml`.

```yaml
services:
  <container-name>:
    image: <image-name>:latest
    container_name: <container-name>
    build:
      context: /opt/projects/<source-dir>
      dockerfile: /opt/stacks/<name>/Dockerfile
    ports:
      - "<HOST_PORT>:8000"
    environment:
      - ENV_VAR_1=value1
      - ENV_VAR_2=value2
    networks:
      - internal
      - mcp-network
    deploy:
      resources:
        limits:
          memory: 256M
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "wget", "--spider", "-q", "http://localhost:8000/healthz"]
      interval: 30s
      timeout: 10s
      retries: 3

networks:
  internal:
    external: true
  mcp-network:
    external: true
```

Replace:
- `<container-name>`: e.g. `mcp-github-projects`
- `<image-name>`: e.g. `mcp-github-projects`
- `<HOST_PORT>`: from Step 0 (e.g. `9033`)
- Environment variables: whatever the server needs (API keys, etc.)

---

## Step 4: Build and Start

```bash
cd /opt/stacks/<name>
docker compose up -d --build
```

Verify it's healthy:
```bash
docker ps --filter name=<container-name>
docker logs <container-name> --tail 20
curl -s http://localhost:<HOST_PORT>/healthz
```

You should see supergateway output like:
```
[supergateway] Running stateless server
[supergateway] StreamableHttp endpoint: http://localhost:8000/mcp
[supergateway] Listening on port 8000
```

---

## Step 5: Discover Tools

Before registering, find out what tools the server exposes:

```bash
curl -s -X POST http://localhost:<HOST_PORT>/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"test","version":"1.0"}}}'
```

Then list tools:
```bash
curl -s -X POST http://localhost:<HOST_PORT>/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -H 'Mcp-Session-Id: <session-id-from-init-response>' \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/list","params":{}}'
```

Note: For streamableHttp, the session ID comes in the response header `mcp-session-id` from the initialize call. Some servers return it in the body instead.

**Save the full tool list output** — you need each tool's name, description, and inputSchema for Step 6.

---

## Step 6: Register Gateway in ContextForge

Run inside the contextforge container:

```python
import sqlite3, uuid
from datetime import datetime, timezone

conn = sqlite3.connect('/app/app/data/mcp.db')
now = datetime.now(timezone.utc).strftime('%Y-%m-%d %H:%M:%S.%f')
gw_id = uuid.uuid4().hex

conn.execute(
    '''INSERT INTO gateways
    (id, name, slug, url, description, transport, capabilities,
     created_at, updated_at, enabled, reachable, tags, version,
     signing_algorithm, team_id, owner_email, visibility, created_by, created_via)
    VALUES (?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?)''',
    (gw_id,
     '<DisplayName>',           # e.g. 'GitHubProjects'
     '<slug>',                  # e.g. 'githubprojects' (lowercase, no spaces)
     'http://<container-name>:8000/mcp',  # internal Docker URL
     '<description>',
     'STREAMABLE_HTTP',         # ALWAYS this for streamableHttp
     '{"tools": {}}',
     now, now,
     1,    # enabled
     1,    # reachable
     '[]', # tags
     1,    # version
     'ed25519',
     'd4cf9c59e66a4d26a64c5f70fb9dfb81',  # team_id
     'admin@example.com',
     'public',
     'admin@example.com',
     'script'))

conn.commit()
print(f'Gateway registered: {gw_id}')
```

**Note the transport value:** Use `STREAMABLE_HTTP` (with underscore) — that's what ContextForge expects. NOT `streamableHttp`.

---

## Step 7: Register Tools in ContextForge

For each tool discovered in Step 5, insert into the `tools` table:

```python
import sqlite3, json, uuid
from datetime import datetime, timezone

conn = sqlite3.connect('/app/app/data/mcp.db')
now = datetime.now(timezone.utc).strftime('%Y-%m-%d %H:%M:%S.%f')

# Get gateway ID from Step 6
gw_id = conn.execute('SELECT id FROM gateways WHERE slug=?', ('<slug>',)).fetchone()[0]
team_id = 'd4cf9c59e66a4d26a64c5f70fb9dfb81'

# For each tool from Step 5:
tools = [
    {"name": "tool-name", "description": "Tool description", "inputSchema": {"type": "object", "properties": {}, "required": []}},
    # ... more tools
]

for tool in tools:
    tool_id = uuid.uuid4().hex
    orig = tool['name']
    prefixed = f'<slug>-{orig}'          # e.g. 'githubprojects-list-projects'
    display = orig.replace('-', ' ').title()

    conn.execute(
        '''INSERT INTO tools
        (id, original_name, url, description, integration_type, request_type,
         headers, input_schema, output_schema, annotations,
         created_at, updated_at, enabled, reachable, jsonpath_filter, tags,
         created_by, created_via, federation_source, version,
         custom_name, custom_name_slug, display_name,
         expose_passthrough, gateway_id, team_id, owner_email, visibility, name)
        VALUES (?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?,?)''',
        (tool_id, orig, 'http://<container-name>:8000/mcp',
         tool['description'], 'MCP', 'STREAMABLE_HTTP',
         'null', json.dumps(tool['inputSchema']), 'null', '{}',
         now, now, 1, 1, '', '[]',
         'admin@example.com', 'script', '<DisplayName>', 1,
         orig, orig, display,
         1, gw_id, team_id, 'admin@example.com', 'public', prefixed))

conn.commit()
print(f'Registered {len(tools)} tools')
```

### Required tool columns (all NOT NULL):

| Column | Value | Notes |
|--------|-------|-------|
| id | `uuid.uuid4().hex` | 32-char hex |
| original_name | tool name from server | e.g. `list-projects` |
| integration_type | `MCP` | always |
| request_type | `STREAMABLE_HTTP` | match gateway transport |
| input_schema | JSON string | from tools/list response |
| created_at | ISO datetime | |
| updated_at | ISO datetime | |
| enabled | `1` | |
| reachable | `1` | |
| jsonpath_filter | `''` | empty string, not null |
| tags | `'[]'` | JSON array |
| version | `1` | |
| custom_name | tool name | |
| custom_name_slug | tool name | |
| expose_passthrough | `1` | |
| visibility | `public` | |
| name | `<slug>-<tool-name>` | prefixed name, this becomes the callable name |

---

## Step 8: Restart Services

```bash
# Restart ContextForge to pick up new gateway
docker restart contextforge

# Wait for it to be healthy (takes ~15-20 seconds)
sleep 20
docker ps --filter name=contextforge --format '{{.Names}} {{.Status}}'

# Restart tool filter to rebuild tool index
docker restart mcp-tool-filter-cf mcp-name-bridge

# Wait for tool filter to initialize (~10 seconds)
sleep 10
```

**Important:** The SSH gateway tool will lose connection during restarts because it routes through the MCP pipeline. Wait and retry.

---

## Step 9: Verify

### Check gateway is reachable:
```bash
docker exec contextforge python3 -c "
import sqlite3
conn = sqlite3.connect('/app/app/data/mcp.db')
r = conn.execute('SELECT reachable, last_seen FROM gateways WHERE slug=?', ('<slug>',)).fetchone()
print(f'reachable={r[0]}, last_seen={r[1]}')
"
```

### Check tools appear in search:
Use `Labs:search_tools` with a query matching the new tools.

### Test a tool call:
Use `Labs:execute_tool` to call one of the new tools.

---

## Gotchas (Things That Have Broken Before)

### 1. Supergateway SSE crashes on multiple connections
**Symptom:** `Error: Already connected to a transport`
**Cause:** SSE mode = single connection. ContextForge health checks open new ones.
**Fix:** ALWAYS use `--outputTransport streamableHttp`. Never SSE for new servers.

### 2. ContextForge deactivates gateway after 3 health check failures
**Symptom:** Gateway shows `reachable=0`, tools show "enabled but inaccessible"
**Cause:** Server was down or unhealthy during CF's check cycle (~1 min intervals, 3 strikes)
**Fix:** Re-enable in DB and restart CF:
```bash
docker exec contextforge python3 -c "
import sqlite3
conn = sqlite3.connect('/app/app/data/mcp.db')
conn.execute('UPDATE gateways SET reachable=1 WHERE slug=?', ('<slug>',))
conn.execute('UPDATE tools SET reachable=1 WHERE gateway_id=(SELECT id FROM gateways WHERE slug=?)', ('<slug>',))
conn.commit()
print('re-enabled')
"
docker restart contextforge
```

### 3. Tool filter shows old count after adding tools
**Symptom:** `total_available` doesn't change in search results
**Fix:** Restart `mcp-tool-filter-cf` and `mcp-name-bridge`. They cache on startup.

### 4. bunx breaks in Docker builds
**Symptom:** Build fails with `command not found` during `bunx tsx` or similar
**Cause:** bun's PATH doesn't propagate to subshells in Docker RUN
**Fix:** Use `npx` instead of `bunx` for build tools.

### 5. Tools register but calls return empty
**Symptom:** Tool call succeeds (no error) but response text is empty
**Cause:** This is normal for some MCP servers on mutation operations. The action succeeded.
**Workaround:** Verify via a read operation or direct API check.

### 6. SSH tool loses connection during restarts
**Symptom:** `fetch failed` errors from `gateway__ssh_execute`
**Cause:** SSH gateway routes through the MCP pipeline. Restarting CF or tool-filter drops the connection.
**Fix:** Just wait 15-20 seconds and retry.

### 7. Transport value format
**Symptom:** Gateway registers but tools show offline
**Cause:** Used `streamableHttp` instead of `STREAMABLE_HTTP` (with underscore, uppercase)
**Fix:** Use `STREAMABLE_HTTP` for both gateway transport and tool request_type columns.

---

## Quick Reference: ContextForge DB Connection

```bash
# Run Python inside contextforge container
docker exec contextforge python3 -c "<python-code>"

# Or copy a script in and run it
docker cp /tmp/script.py contextforge:/tmp/script.py
docker exec contextforge python3 /tmp/script.py
```

## Quick Reference: Check Current Gateways

```bash
docker exec contextforge python3 -c "
import sqlite3
conn = sqlite3.connect('/app/app/data/mcp.db')
for r in conn.execute('SELECT name, slug, transport, reachable FROM gateways ORDER BY name').fetchall():
    print(f'{r[0]:25s} slug={r[1]:20s} {r[2]:15s} reachable={r[3]}')
"
```
