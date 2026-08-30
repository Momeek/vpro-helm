# vpro-helm

GitOps repository for deploying the vProfile application stack on Kubernetes using Helm.

## Project Structure

```
vpro-helm/
├── helm/vprofile/          # Helm chart for the full vProfile stack
│   ├── templates/          # Kubernetes manifest templates
│   └── values.yaml         # Default configuration values
└── kubedefs/               # Raw Kubernetes manifests (non-Helm)
```

## Stack Components

| Service    | Image                          | Port  |
|------------|--------------------------------|-------|
| App        | ECR (vproappimg)               | 8080  |
| Database   | vprocontainers/vprofiledb      | 3306  |
| Memcached  | memcached                      | 11211 |
| RabbitMQ   | rabbitmq                       | 5672  |

## Prerequisites

- Kubernetes cluster
- Helm 3+
- AWS ECR access (for the app image)
- `gp2` StorageClass available (for DB PVC)

## Deploy with Helm

```bash
helm upgrade --install vprofile ./helm/vprofile \
  --namespace vprofile --create-namespace \
  -f helm/vprofile/values.yaml
```

## Key Configuration (`values.yaml`)

| Key | Description |
|-----|-------------|
| `app.tag` | App image tag (commit SHA) |
| `ingress.host` | Ingress hostname |
| `secrets.dbPass` | MySQL root password |
| `secrets.rmqPass` | RabbitMQ password |
| `dockerregistry.enabled` | Enable private Docker registry secret |

## Ingress

The app is exposed via ingress at `vprofile.meeklab.site` on port `8080`.
