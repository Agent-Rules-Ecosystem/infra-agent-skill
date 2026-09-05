# 🚀 Estrategias de Despliegue y Rollback por Arquetipo

## 1. Despliegue Blue/Green (Servicios / APIs)
- Se mantienen dos entornos idénticos (Azul = Activo, Verde = Nuevo).
- Se despliega la nueva versión en Verde y se ejecutan pruebas de humo.
- Se cambia el enrutador/load balancer de Azul a Verde instantáneamente con zero downtime.

## 2. Despliegue Canary (Microservicios / Tráfico Web)
- Se direcciona un porcentaje pequeño de tráfico (ej. 5%) a la nueva versión.
- Se monitorean métricas de error y latencia durante una ventana de observación.
- Si no hay incremento en el error rate, se promueve progresivamente al 100%.

## 3. Despliegue Staged Rollout (Clientes / Mobile / Tiendas de Aplicaciones / Desktop)
- Distribución escalonada por fases porcentuales (ej. 5% -> 10% -> 25% -> 50% -> 100%).
- Monitoreo activo de métricas de telemetría y tasa de crashes (Crashlytics, Sentry).
- Si la tasa de sesiones libres de fallos cae por debajo del umbral (ej. < 99.5%), se detiene o congela la distribución de inmediato.

## 4. Publicación Atómica SemVer (Librerías / CLIs / Paquetes)
- Publicación inmutable en registry correspondiente (npm, crates.io, PyPI, GH Releases).
- Validación de integridad mediante checksums y firmas criptográficas.
- Smoke-test de instalación y ejecución en runner limpio post-publicación.

## 5. Protocolos de Rollback y Detención Preventiva
- **Servicios**: Reversión automática de tráfico en router/load balancer al detectar fallos repetidos en healthcheck.
- **Clientes / Mobile**: Pausa o detención del rollout en consola/API de distribución, permitiendo revertir la versión sugerida o acelerar un hotfix.
- **Web Estática**: Reasignación atómica del alias/puntero CDN al snapshot anterior en Object Storage.
