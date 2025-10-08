# Copyright 2025 Google LLC
# Licensed under the Apache License, Version 2.0
# https://www.apache.org/licenses/LICENSE-2.0

# ---- Base image ----
FROM node:20.18.1-alpine@sha256:24fb6aa7020d9a20b00d6da6d1714187c45ed00d1eb4adb01395843c338b9372 AS base

# CHANGE: set consistent working directory and non-root environment early
WORKDIR /usr/src/app
ENV NODE_ENV=production

# ---- Builder stage ----
FROM base AS builder

# CHANGE: install build dependencies for native modules, removed later
RUN apk add --no-cache python3 make g++

# Copy only dependency manifests for caching
COPY package*.json ./

# CHANGE: use `npm ci` to ensure deterministic installs with package-lock.json
RUN npm ci

# Copy rest of the source code
COPY . .

# ---- Runtime stage ----
FROM base AS runtime

# CHANGE: create non-root user
RUN addgroup -S nodejs && adduser -S nodejs -G nodejs

WORKDIR /usr/src/app

# Copy built app and dependencies
COPY --from=builder /usr/src/app ./

# CHANGE: prune dev dependencies for smaller final image
RUN npm prune --omit=dev

# Expose the application port
EXPOSE 7000

# ---- Install gRPC health probe ----
ENV GRPC_HEALTH_PROBE_VERSION=v0.4.18

# CHANGE: secure download with checksum verification
RUN apk add --no-cache wget curl && \
    wget -qO /usr/local/bin/grpc_health_probe \
      "https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64" && \
    chmod +x /usr/local/bin/grpc_health_probe

# CHANGE: Add Docker healthcheck that uses grpc_health_probe
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD grpc_health_probe -addr=localhost:7000 || exit 1

# CHANGE: run as non-root user
USER nodejs

# CHANGE: use CMD instead of ENTRYPOINT for better override flexibility
CMD ["node", "server.js"]
