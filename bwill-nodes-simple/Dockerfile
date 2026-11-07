# Multi-stage: bake community nodes outside /home/node/.n8n (volume-safe).
# Pattern: N8N_CUSTOM_EXTENSIONS=/opt/n8n-custom-nodes (n8n 2.x official).
# Mirror for GitHub push: dev\n8n-custom-images\bwill-nodes-simple\Dockerfile
# Maintainer: bwilliamson5509
ARG N8N_VERSION=2.46.6

FROM node:22-alpine AS nodes
RUN apk add --no-cache python3 make g++
WORKDIR /opt/n8n-custom-nodes
RUN npm init -y \
 && npm install --omit=dev --no-fund --no-audit \
      n8n-nodes-meilisearch@0.2.0 \
      n8n-nodes-clickuplookup@1.2.0 \
      n8n-nodes-magento-rest@2.0.2 \
      n8n-nodes-suitetalk-tba@0.1.1 \
      n8n-nodes-mishimoto@0.1.2 \
      n8n-nodes-attentive-api@0.2.0 \
      n8n-nodes-clicksend@0.1.0

FROM n8nio/n8n:${N8N_VERSION}
USER root
COPY --from=nodes /opt/n8n-custom-nodes /opt/n8n-custom-nodes
RUN chown -R node:node /opt/n8n-custom-nodes
ENV N8N_CUSTOM_EXTENSIONS=/opt/n8n-custom-nodes
USER node