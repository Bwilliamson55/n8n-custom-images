# Custom images for n8n

Thin Dockerfiles: `FROM n8nio/n8n:<version>` then bake community `n8n-nodes-*` packages.

## Current pattern (n8n 2.x / 2.46+)

Do **not** install into `~/.n8n/nodes` in the image. A volume mount on `/home/node/.n8n` hides that path at runtime.

Official approach: install under a path outside the data volume and set `N8N_CUSTOM_EXTENSIONS` (see [Install private nodes](https://docs.n8n.io/connect/create-nodes/deploy-your-node/install-private-nodes/) and [Specify custom nodes location](https://docs.n8n.io/deploy/host-n8n/configure-n8n/basic-configuration/configuration-examples/specify-custom-nodes-location/)).

Preferred folder: **`bwill-nodes-simple/`** (also used by DO App Platform `goldfish-app`).

```dockerfile
ARG N8N_VERSION=2.46.6

FROM node:22-alpine AS nodes
WORKDIR /opt/n8n-custom-nodes
RUN npm init -y && npm install --omit=dev <n8n-nodes-…@pins>

FROM n8nio/n8n:${N8N_VERSION}
USER root
COPY --from=nodes /opt/n8n-custom-nodes /opt/n8n-custom-nodes
RUN chown -R node:node /opt/n8n-custom-nodes
ENV N8N_CUSTOM_EXTENSIONS=/opt/n8n-custom-nodes
USER node
```

Compose/runtime should also set `N8N_CUSTOM_EXTENSIONS=/opt/n8n-custom-nodes` on main + workers.

### Alternate (stock image, n8n ≥ 2.21)

`N8N_COMMUNITY_PACKAGES_MANAGED_BY_ENV=true` + `N8N_COMMUNITY_PACKAGES='[…]'` installs into the persisted `~/.n8n/nodes` at startup. Fine for simple single-instance deploys; bake + `N8N_CUSTOM_EXTENSIONS` is better for queue mode and reproducible images.

Older blog notes (post-1.0 `node` user / `~/.n8n/nodes` bake): [Persist n8n community nodes on docker through updates](https://www.weeumson.com/posts/Persist-n8n-community-nodes-on-docker-through-updates/) — superseded for volume-backed Docker by `N8N_CUSTOM_EXTENSIONS`.
