ARG NODE_VERSION=18
FROM n8nio/base:${NODE_VERSION}

ARG N8N_VERSION

ENV N8N_VERSION=latest
ENV NODE_ENV=production
USER root
RUN set -eux; \
	apkArch="$(apk --print-arch)"; \
	case "$apkArch" in \
	'armv7') apk --no-cache add --virtual build-dependencies python3 build-base;; \
	esac && \
	npm install -g --omit=dev n8n@latest && \
	case "$apkArch" in \
	'armv7') apk del build-dependencies;; \
	esac && \
	find /usr/local/lib/node_modules/n8n -type f -name "*.ts" -o -name "*.js.map" -o -name "*.vue" | xargs rm && \
	rm -rf /root/.npm

# Install the browserless package
RUN mkdir /home/node/.n8n/nodes && cd /home/node/.n8n/nodes && npm install n8n-nodes-clicksend && npm install n8n-nodes-clickuplookup && npm install n8n-nodes-browserless

# Set a custom user to not have n8n run as root
WORKDIR /data
RUN apk --no-cache add su-exec
COPY docker-entrypoint.sh /docker-entrypoint.sh
RUN chmod +x /docker-entrypoint.sh
ENTRYPOINT ["tini", "--", "/docker-entrypoint.sh"]