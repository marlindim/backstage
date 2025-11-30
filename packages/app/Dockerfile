FROM node:18-bullseye as builder

WORKDIR /app

RUN apt-get update && \
    apt-get install -y \
      python3 \
      build-essential \
      pkg-config \
      git && \
    rm -rf /var/lib/apt/lists/*

COPY . .

RUN yarn install --immutable

WORKDIR /app/packages/app

RUN yarn build

FROM nginx:alpine

COPY --from=builder /app/packages/app/dist /usr/share/nginx/html

RUN echo 'server { \n\
    listen 80; \n\
    server_name localhost; \n\
    root /usr/share/nginx/html; \n\
    index index.html; \n\
    location / { \n\
        try_files $uri $uri/ /index.html; \n\
    } \n\
    location /api { \n\
        proxy_pass http://backend:7007; \n\
        proxy_http_version 1.1; \n\
        proxy_set_header Upgrade $http_upgrade; \n\
        proxy_set_header Connection "upgrade"; \n\
        proxy_set_header Host $host; \n\
        proxy_cache_bypass $http_upgrade; \n\
    } \n\
}' > /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
