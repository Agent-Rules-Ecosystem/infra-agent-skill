# 🔄 Best Practices: CI/CD Pipelines Agnósticos

## 1. Estructura Estándar de Pipeline

Un pipeline de CI/CD transversal y agnóstico debe estructurarse en 5 etapas secuenciales:

1. **Quality Gates & Static Analysis (Pre-flight Contract)**:
   - Invocación de hooks de la aplicación (`lint`, `test`, `security-audit`).
   - Bloqueo inmediato si las compuertas de la plataforma fallan.
2. **Automated Test Matrix**:
   - Ejecución concurrente de pruebas unitarias y de integración bajo entornos reproducibles.
3. **Release Hardening & Audit**:
   - Verificación de banderas de optimización (minificación, dead code elimination, stripping).
   - Generación, extracción y archivo de símbolos de depuración/mapeo (`mapping.txt`, `dSYMs`).
4. **Artifact Packaging**:
   - Empaquetado según arquetipo (Docker multi-stage para servicios, AAB/APK/IPA para clientes, binarios limpios para CLIs).
   - Inyección de certificados y llaves de firma desde secretos cifrados de CI.
5. **Deploy & Post-flight Health Verification**:
   - Promoción según estrategia (Zero-downtime, Staged Rollout o publicación atómica).
   - Monitoreo post-despliegue adaptado al arquetipo (`/health`, telemetría de fallos o smoke test).

## 2. Optimización de Rendimiento
- Usar caché de dependencias (npm/cargo/gradle/pub/pip) indexado por hash del lockfile.
- Ejecutar jobs independientes en paralelo (ej. lint y security audit).
- Runners efímeros y con permisos mínimos (`permissions: read-all`).
