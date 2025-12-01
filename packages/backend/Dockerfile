FROM node:22-bookworm-slim

# Set Python interpreter for `node-gyp` to use
ENV PYTHON=/usr/bin/python3

# Install all required build dependencies for native Node modules
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        python3 g++ build-essential make pkg-config libc6-dev libsqlite3-dev && \
    rm -rf /var/lib/apt/lists/*

# Switch to least-privileged `node` user
USER node

# Set working directory
WORKDIR /app

# Copy Yarn config files
COPY --chown=node:node .yarn ./.yarn
COPY --chown=node:node .yarnrc.yml ./
COPY --chown=node:node backstage.json ./

# Set environment variables
ENV NODE_ENV=production
ENV NODE_OPTIONS="--no-node-snapshot"

# Copy skeleton to reduce cache invalidation
COPY --chown=node:node yarn.lock package.json packages/backend/dist/skeleton.tar.gz ./
RUN tar xzf skeleton.tar.gz && rm skeleton.tar.gz

# Clean Yarn cache before installing
RUN yarn cache clean

# Install production dependencies
RUN --mount=type=cache,target=/home/node/.cache/yarn,sharing=locked,uid=1000,gid=1000 \
    yarn workspaces focus --all --production

# Copy examples if needed
COPY --chown=node:node examples ./examples

# Copy backend bundle and config
COPY --chown=node:node packages/backend/dist/bundle.tar.gz app-config*.yaml ./
RUN tar xzf bundle.tar.gz && rm bundle.tar.gz

# Command to run the backend
CMD ["node", "packages/backend", "--config", "app-config.yaml", "--config", "app-config.production.yaml"]
