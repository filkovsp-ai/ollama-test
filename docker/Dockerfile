# custom Dockerfile for AnythingLLM with updated Python and Node

FROM mintplexlabs/anythingllm:master

# Switch to root to install system tools
USER root

COPY --from=ghcr.io/astral-sh/uv:0.5.20 /uv /bin/uv

RUN uv python install 3.13.1 && \
    ln -sf /root/.local/share/uv/python/cpython-3.13.1-linux-x86_64-gnu/bin/python3 /usr/local/bin/python3

# install `n` manager for Node and `Node.js v22`
RUN npm install -g n && n 22.12.0

# Ensure the app directory is still owned by the default user
RUN chown -R 1000:1000 /app

# Everyone else can read/write/execute
RUN chmod -R 755 /app

# Switch back to the default user
USER 1000

# Metadata/Env
ENV NODE_ENV=production
ENV PATH="/root/.local/bin/:$PATH"

# No need for CMD/Entrypoint; it inherits from the base image