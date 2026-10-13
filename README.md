# zafarr-infra

Kubernetes manifests for everything on **zafarr.info**. One folder per app.

| App        | Folder            | Address                          |
|------------|-------------------|----------------------------------|
| portfolio  | `apps/portfolio/` | `portfolio.localhost:8080` (local), `k8s.zafarr.info` (Phase 1) |

Deploy an app: `kubectl apply -f apps/portfolio/`
Always check first: `kubectl config current-context` (should be `k3d-zafar`)

Learning plan + progress log: `~/dev/kubernetes-boost/ZAFARR_INFO_PLAN.md`

## Cloudflare Tunnel (`apps/cloudflared/`)
Tunnel `zafarr` → Traefik → Ingress. The credentials Secret is **not in git**; recreate it with:
`kubectl create ns cloudflared && kubectl -n cloudflared create secret generic tunnel-credentials --from-file=credentials.json=$HOME/.cloudflared/4c1942ee-0b4b-4dea-bc87-6f03cac9872c.json`
