# FuBanking-gitops — repo GitOps (ArgoCD)

Repo separado del código. Jenkins (CI) actualiza los `newTag` en
`overlays/dev/kustomization.yaml` en cada build. ArgoCD (CD) sincroniza
automáticamente el namespace `fubanking-dev` en minikube.

```
apps/                  Applications de ArgoCD (apuntan a overlays/dev)
base/                  Deployments + Services + Namespace (Kustomize base)
overlays/dev/          Overlay dev: fija namespace e image tags
```

## Secret real (NO commitear valores)

El backend exige (ver `backend/src/shared/config/env.ts`):
`JWT_SECRET, SUPABASE_URL, SUPABASE_ANON_KEY, SUPABASE_SERVICE_ROLE_KEY,
GMAIL_USSER, GMAIL_PASS, CLIENT_URL`. Crear una sola vez:

```bash
kubectl create secret generic fubanking-backend-env -n fubanking-dev \
  --from-literal=PORT=3001 \
  --from-literal=NODE_ENV=production \
  --from-literal=JWT_SECRET='cambia-esto-min-16-chars' \
  --from-literal=JWT_EXPIRES_IN=7d \
  --from-literal=SUPABASE_URL='https://TU-PROYECTO.supabase.co' \
  --from-literal=SUPABASE_ANON_KEY='...' \
  --from-literal=SUPABASE_SERVICE_ROLE_KEY='...' \
  --from-literal=CLIENT_URL='http://localhost:3000' \
  --from-literal=GMAIL_USSER='tu@correo.com' \
  --from-literal=GMAIL_PASS='...'
```

Ver plantilla en `base/backend-secret.example.yaml`.
