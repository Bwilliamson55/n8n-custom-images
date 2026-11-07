# Custom community nodes baked into Dockerfile

Source: npm maintainer **bwilliamson5509** (author Ben Williamson).
n8n-01 (192.168.1.151) had empty `~/.n8n/nodes` — packages from npm, not the LAN volume.

| Package | Pin |
|---------|-----|
| n8n-nodes-meilisearch | 0.2.0 |
| n8n-nodes-clickuplookup | 1.2.0 |
| n8n-nodes-magento-rest | 2.0.2 |
| n8n-nodes-suitetalk-tba | 0.1.1 |
| n8n-nodes-mishimoto | 0.1.2 |
| n8n-nodes-attentive-api | 0.2.0 |
| n8n-nodes-clicksend | 0.1.0 |

Base image: `n8nio/n8n:2.46.6` → `local/n8n-custom:2.46.6`