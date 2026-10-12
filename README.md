# zafarr-infra

Kubernetes manifests for everything on **zafarr.info**. One folder per app.

| App        | Folder            | Address                          |
|------------|-------------------|----------------------------------|
| portfolio  | `apps/portfolio/` | `portfolio.localhost:8080` (local), `k8s.zafarr.info` (Phase 1) |

Deploy an app: `kubectl apply -f apps/portfolio/`
Always check first: `kubectl config current-context` (should be `k3d-zafar`)

Learning plan + progress log: `~/dev/kubernetes-boost/ZAFARR_INFO_PLAN.md`
