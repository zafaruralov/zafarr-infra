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

## Rebuild the whole cluster from zero
Images, Secrets and database data are NOT in git. Data is lost on `k3d cluster delete` unless you copy a backup out first.
```bash
k3d cluster create zafar -p "8080:80@loadbalancer"
# images: build in each repo (docker build -t backend:v3 . / portfolio:v2), then:
k3d image import portfolio:v2 backend:v3 -c zafar
kubectl create secret generic postgres-secret --from-literal=POSTGRES_PASSWORD=<password>
kubectl create ns cloudflared
kubectl -n cloudflared create secret generic tunnel-credentials \
  --from-file=credentials.json=$HOME/.cloudflared/4c1942ee-0b4b-4dea-bc87-6f03cac9872c.json
kubectl apply -R -f apps/
```
