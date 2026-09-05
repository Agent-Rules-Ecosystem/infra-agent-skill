# 🗺️ Mapa de Rutas de Infraestructura y Release Engineering

## 1. Pipelines de CI/CD y Automatización
- `.github/workflows/`: Workflows de GitHub Actions (build, quality gates, release).
- `.gitlab-ci.yml`: Definición de pipeline para GitLab CI.
- `azure-pipelines.yml` / `bitbucket-pipelines.yml`: Pipelines alternativos de CI/CD.

## 2. Contratos de Quality Gates & Tooling de Release
- `scripts/ci/`: Scripts transversales de verificación pre-flight y validación de compuertas.
- `Makefile` / `Taskfile.yml`: Tareas unificadas para testing, linting y build agnóstico.
- `fastlane/`: Automatización de distribución y release para tiendas y canales móviles.

## 3. Release Hardening & Símbolos de Depuración
- `build/outputs/mapping/` o `dist/symbols/`: Archivos de mapeo (`mapping.txt`, `.map`) para deofuscación.
- `dSYMs/` / `symbols/`: Símbolos de depuración nativos extraídos para plataformas de telemetría.
- Archivos de configuración de optimización (consumidos por el pipeline para auditoría de release).

## 4. Containerización y Orquestación
- `Dockerfile`: Definición de construcción multi-stage para servicios y aplicaciones empaquetadas.
- `docker-compose.yml`: Orquestación de servicios en entornos locales, desarrollo y VPS.
- `infra/k8s/` o `helm/`: Manifiestos de Kubernetes y paquetes Helm.

## 5. Infraestructura como Código (IaC)
- `infra/terraform/` o `terraform/`: Manifiestos idempotentes de provisionamiento cloud.
- `ansible/`: Playbooks de automatización y configuración de servidores.

## 6. Persistencia y Seguimiento de Tareas
- `overview/work/skill/infra.md`: Registro activo de auditoría y tareas pendientes de infraestructura.
