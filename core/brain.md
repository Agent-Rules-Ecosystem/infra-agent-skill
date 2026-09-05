# 🧠 Engine de Infraestructura (Decision Brain)

## 🛡️ Principio de Aislamiento de Dominio (Domain Shield)

1. **Delimitación Estricta de Responsabilidad**:
   - Si una tarea o requerimiento involucra layouts, insets del SO, widgets de UI, temas visuales o reglas de negocio (ej. Edge-to-Edge de Android 15, `SafeArea`, `enableEdgeToEdge()`, Dynamic Island):
     * **La infraestructura NO implementa ni audita directamente el código de UI**.
     * **Acción del Brain**: Delegar la responsabilidad a la habilidad de la plataforma (Flutter, Android, React, etc.).
     * **Rol de Infra**: Proveer y exigir el contrato de compuerta (`pre-flight:lint` / `verify`) en el pipeline de CI/CD. Si el hook de plataforma falla, el pipeline detiene el despliegue.

---

## 🏗️ Matriz de Decisión por Arquetipo de Entrega

| Arquetipo | Estrategia de Build | Release Hardening | Despliegue | Verificación Post-Deploy |
|---|---|---|---|---|
| **Servicios / APIs** | Multi-stage container (Builder -> Runner no-root) | Stripping de símbolos en binarios, omisión de devDependencies | Rolling / Blue-Green / Canary | Sondeo `/health` HTTP durante 120s |
| **Clientes / Mobile / Games** | Runner aislado, SDK reproducible, llaves inyectadas | Minificación, tree-shaking, preservación de keep rules, archivo de mapping/dSYM | Staged Rollout en tiendas/MDM (5% -> 100%) | Telemetría de Crashlytics / Error-rate |
| **CLIs / Librerías** | Compilación estática, LTO (Link-Time Optimization) | Stripping de símbolos de debug, empaquetado mínimo | Publicación atómica semver en registry | Smoke test de ejecución en entorno limpio |
| **Web Estática / JAM** | Build optimizado con content hashing | Minificación de assets, sourcemaps privados | Sincronización inmutable a CDN/Object Storage | Verificación de HTTP 200 y cache-control |

---

## 🔒 Matriz de Decisión de Release Hardening

1. **Si se detecta intento de release a producción**:
   - **Auditoría de Perfil**: Prohibido compilar para distribución final con banderas de depuración (`debuggable = true` o equivalentes).
   - **Auditoría de Optimización**: Minificación y reducción de recursos (R8, ProGuard, tree-shaking, binary strip) deben estar estrictamente activas.
   - **Auditoría de Símbolos**: Los archivos de símbolos y mapeo (mapping.txt, dSYM, sourcemaps) deben ser extraídos, versionados y enviados a la plataforma de monitoreo antes de cerrar el release.
   - **Auditoría de Keep Rules / Reflexión**: Si el compilador aplica minificación/ofuscación, el pipeline debe verificar que la plataforma haya definido sus reglas de preservación para serializadores y modelos de reflexión.

---

## ⚠️ Clasificación Universal de Riesgos de Despliegue

1. **Crítico / Bloqueante (Exige abortar pipeline o staging estricto)**:
   - Banderas de optimización o minificación desactivadas en perfiles de producción.
   - Elevación de versiones de SO / Target API sin compuertas pre-flight aprobadas.
   - Cambios destructivos o no reversibles en esquemas de base de datos o APIs públicas.
   - Ausencia de archivo de símbolos de deofuscación para el reporte de fallos.

2. **Medio (Exige validación post-release y rollout escalonado)**:
   - Nuevos microservicios, cambios en variables de entorno o migración de dependencias principales.
   - Despliegue a producción mediante Staged Rollout con monitor de telemetría activo.

3. **Bajo (Despliegue directo estándar)**:
   - Actualización de documentación, assets estáticos independientes o metadatos de configuración no sensibles.
