---
name: infra-agent-skill
description: Transversal Infrastructure, CI/CD, Containerization, IaC, and Deployment Skill for AI Agents.
---

# 🏗️ Infra Agent Skill Directive

## Bootstrap de la Habilidad

Al detectar `$infra` o tareas relacionadas con Docker, CI/CD, Terraform, Kubernetes, VPS o despliegues, cargar y ejecutar:

1. `.skill/infra-agent-skill/SKILL.md` ← **Directiva principal de la habilidad**
2. `.skill/infra-agent-skill/core/commands.md`
3. `.skill/infra-agent-skill/core/brain.md`
4. `.skill/infra-agent-skill/core/path_map.md`

## Regla Canónica Agnóstica

Esta habilidad opera transversalmente sobre cualquier lenguaje o plataforma (**Flutter, Node/TypeScript, Python, Go, Rust, C++, C# Game Engine**):
- **Aislamiento de Frontera Transversal (Domain Shield)**: La habilidad orquesta pipelines, empaquetado, secretos, optimización de release e IaC. **NUNCA** audita ni acopla lógica de UI, insets/lifecycles de SO, layouts ni reglas de negocio. Esas validaciones pertenecen exclusivamente a las habilidades de plataforma/aplicación.
- **Cero Acoplamiento Rígido**: Usar abstracciones genéricas de compilación, containerización y distribución sin sesgos monomodales.
- **Contrato de Compuertas de Calidad (Quality Gates)**: Todo pipeline debe consumir hooks estándar provistos por la aplicación (`pre-flight:lint`, `pre-flight:test`, `pre-flight:security`) antes de empaquetar. Si una compuerta falla, el pipeline aborta.
- **Release Hardening & Optimización Obligatoria**: Bloquear la distribución a producción si las banderas de optimización de release (minificación, dead code elimination, tree-shaking, stripping) están desactivadas, garantizando además la extracción y archivo de símbolos de depuración/mapeo para observabilidad.
- **Verificación de Salud Agnóstica al Arquetipo**: Ningún despliegue se considera exitoso sin validación post-release adaptada a su naturaleza (HTTP `/health` para servicios, telemetría/crash rate y staged rollout para clientes/mobile, smoke test para CLIs/binarios).
- **Economía de Tokens**: Manifiestos precisos y modulares, sin comentarios redundantes ni bloques vacíos.
