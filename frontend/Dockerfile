# Build stage
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci --only=production --no-audit --no-fund && \
    npm cache clean --force

COPY . .

RUN NODE_OPTIONS=--openssl-legacy-provider npm run build

# Final stage (copy data)
FROM alpine:3.19

WORKDIR /app

COPY --from=builder /app/dist /app/static
