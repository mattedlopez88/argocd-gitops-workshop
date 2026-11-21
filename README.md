# argocd-gitops-workshop

Repositorio del taller **"Despliega una app en Kubernetes con Argo CD (GitOps)"**.
La app es un juego estático (Whack-a-mole) servido con nginx.

## Estructura

```
app/                        # archivos del juego (HTML, CSS, imagen)
argocd/application.yml      # Application de Argo CD que sincroniza k8s/
Dockerfile                  # imagen nginx con el juego
.github/workflows/build.yml # construye y publica la imagen en GHCR
k8s/                        # manifiestos que Argo CD sincroniza
  deployment.yml            # 2 réplicas del juego
  service.yml               # NodePort 30080
  hpa.yml                   # autoescala de 2 a 5 réplicas por CPU
```

## Cómo funciona

1. Cada push a `main` que toca `app/` o el `Dockerfile` publica
   `ghcr.io/mattedlopez88/argocd-gitops-workshop` con las etiquetas `latest` y el SHA corto.
2. Argo CD apunta a este repositorio, carpeta `k8s/`, rama `main`.
   Lo que está en Git es el estado deseado: Argo CD lo compara con el clúster y lo sincroniza.
3. Para cambiar el despliegue (réplicas, recursos, imagen), edita `k8s/` y haz push. No uses `kubectl apply` a mano.

## Acceso

Con Docker Desktop, abre <http://localhost:30080>.
