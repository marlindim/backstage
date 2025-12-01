FROM node:18-bullseye as builder

WORKDIR /app

RUN apt-get update && \
    apt-get install -y \
      python3 \
      build-essential \
      pkg-config \
      git && \
    rm -rf /var/lib/apt/lists/*

COPY package.json yarn.lock .yarnrc.yml ./
COPY .yarn ./.yarn
COPY packages packages
COPY app-config.yaml ./

RUN mkdir -p plugins

RUN yarn config set httpTimeout 600000

RUN yarn install --immutable || \
    yarn install --immutable || \
    yarn install

WORKDIR /app/packages/app

RUN yarn build

FROM nginx:alpine

COPY --from=builder /app/packages/app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
