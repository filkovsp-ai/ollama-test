## build custom image:
```bash
docker build -t anythingllm .
```

## How to Launch and Configure

1. Start the stack: 
    - Run in your terminal: 
    ```bash
    docker-compose up -d
    docker compose --file path/to/docker-compose.yml up -d
    ```
2. Download the "Brain": Ollama starts empty. You need to pull a model.
    - Run: 
    ```bash
    docker exec -it ollama ollama pull llama3.3
    ```
3. Access the UI: Open your browser to http://localhost:3001.
4. Initial Setup: * Follow the setup wizard. Choose Multi-user mode.
    - When asked for the LLM Provider, select Ollama.
    - For the "Ollama URL", use http://ollama:11434 (this is the internal Docker name).

## Handling Your Specific Data Sources
- For the Shared Drive:
    * In the docker-compose add volumes mound under /app/server/storage at the container
    AnythingLLM will then see that drive as a local folder. 
    You can go to the Workspace -> Documents and move the files from that folder into your workspace with one click.

- For Postgres:
    * Inside the AnythingLLM settings, go to Tools / Agents.
    * You can enable the SQL Agent. Provide your Postgres connection string (e.g., postgresql://user:pass@192.168.1.x:5432/dbname).
    * The AI will now be able to generate SQL queries to answer questions about the data in your tables.


install or update container:
1. connect to terminal as root:
```bash
docker exec -it --user root  CONTAINER-ID /bin/bash
```
    see https://www.linkedin.com/pulse/installing-python-modules-docker-container-rajesh-tandukar-9fdvc/
2. install python: https://askubuntu.com/a/1195153

Troublshooting:
---

https://hub.docker.com/r/mintplexlabs/anythingllm
https://docs.anythingllm.com/installation-docker/local-docker
https://docs.anythingllm.com/installation-docker/localhost


## run posygres db container:
```bash
docker run -d -p 5432:5432 \
--name postgres \
-e POSTGRES_PASSWORD=postgres \
-e POSTGRES_HOST_AUTH_METHOD=trust \
-v /Users/pfilkovskyi/.postgres:/var/lib/postgresql/data \
postgres:16.4-alpine3.20
```

## run ollama container:
```bash
docker run -d -p 11434:11434 \
--name ollama \
-v /Users/pfilkovskyi/.ollama:/root/.ollama \
ollama/ollama:latest
```

## run anythingllm container:
```bash
docker run -d -p 3001:3001 \
--name anythingllm --cap-add SYS_ADMIN \
-v /Users/pfilkovskyi/.anythingllm:/app/server/storage \
-v /Users/pfilkovskyi/.anythingllm/.env:/app/server/.env \
-e STORAGE_DIR="/app/server/storage" \
-e DATABASE_URL="postgresql://postgres:postgres@172.17.0.2:5432/anythingllm" \
mintplexlabs/anythingllm:master
```

```bash
docker run -d -p 3001:3001 \
--name anythingllm --cap-add SYS_ADMIN \
-v /Users/pfilkovskyi/.anythingllm:/app/server/storage \
-v /Users/pfilkovskyi/.anythingllm/.env:/app/server/.env \
-e STORAGE_DIR="/app/server/storage" \
-e DATABASE_URL="postgresql://postgres:postgres@172.17.0.2:5432/anythingllm" \
mintplexlabs/anythingllm:latest
```

```bash
docker run -d -p 3001:3001 \
--name anythingllm-pg --cap-add SYS_ADMIN \
-v /Users/pfilkovskyi/.anythingllm:/app/server/storage \
-v /Users/pfilkovskyi/.anythingllm/.env:/app/server/.env \
-e STORAGE_DIR="/app/server/storage" \
-e DATABASE_URL="postgresql://postgres:postgres@172.17.0.2:5432/anythingllm" \
mintplexlabs/anythingllm:pg
```

## apply mcp config:
path: `/app/server/storage/plugins/`
filename: `anythingllm_mcp_servers.json`

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/app/server/storage"
      ]
    }
  }
}
```