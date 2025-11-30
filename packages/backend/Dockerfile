FROM node:18-bullseye as builder

WORKDIR /app

RUN apt-get update && \
    apt-get install -y \
      python3 \
      build-essential \
      libcairo2-dev \
      libpango1.0-dev \
      libjpeg-dev \
      libgif-dev \
      librsvg2-dev \
      libsqlite3-dev \
      pkg-config \
      git && \
    rm -rf /var/lib/apt/lists/*

COPY . .

RUN yarn install --immutable

WORKDIR /app/packages/backend

RUN yarn build

FROM node:18-bullseye-slim

WORKDIR /app

RUN apt-get update && \
    apt-get install -y \
      libcairo2 \
      libpango-1.0-0 \
      libjpeg62-turbo \
      libgif7 \
      librsvg2-2 \
      libsqlite3-0 \
      ca-certificates && \
    rm -rf /var/lib/apt/lists/*

COPY --from=builder /app/packages/backend/dist/skeleton.tar.gz /tmp/
RUN tar xzf /tmp/skeleton.tar.gz -C /app && rm /tmp/skeleton.tar.gz

COPY --from=builder /app/packages/backend/dist/bundle.tar.gz /tmp/
RUN tar xzf /tmp/bundle.tar.gz -C /app && rm /tmp/bundle.tar.gz

RUN yarn install --production --frozen-lockfile

COPY --from=builder /app/app-config*.yaml /app/

ENV NODE_ENV=production
EXPOSE 7007

CMD ["node", "packages/backend", "--config", "app-config.yaml"]
