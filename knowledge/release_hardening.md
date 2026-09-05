# 🔒 Guía Transversal: Release Hardening, Optimización y Gestión de Símbolos

## 📌 Principio Rector

Ningún artefacto destinado a distribución pública o producción debe generarse con perfiles de desarrollo o banderas de optimización desactivadas. La infraestructura de CI/CD actúa como compuerta de bloqueo (Release Gate).

---

## 1. Verificación Pre-Build de Optimización y Minificación

Antes de compilar el paquete de distribución final, el pipeline debe verificar:

| Arquetipo / Stack | Bandera / Configuración Requerida | Efecto |
|---|---|---|
| **Android / Gradle** | `isMinifyEnabled = true`, `isShrinkResources = true` | R8 / ProGuard eliminan código y recursos muertos; ofuscan bytecode. |
| **iOS / Xcode / Swift** | `SWIFT_COMPILATION_MODE = wholemodule`, `DEBUG_INFORMATION_FORMAT = dwarf-with-dsym` | Optimización completa y generación de dSYM separados. |
| **Flutter** | `--release`, `--obfuscate`, `--split-debug-info=<path>` | Minificación de Dart, tree-shaking y separación de símbolos. |
| **Rust / Go / C++** | Profile release (`--release`, `-O3`), stripping (`strip`, `LTO`) | Eliminación de símbolos de depuración en binario final. |
| **Node.js / Web / Frontend** | Minificación JS/CSS, Dead code elimination / Tree-shaking | Reducción de bundle y omisión de sourcemaps públicos. |

> **Regla de Bloqueo**: Si una compilación de producción tiene `debuggable = true` o la minificación explícitamente desactivada sin excepción documentada, el pipeline **aborta la ejecución**.

---

## 2. Gestión y Archivo de Símbolos de Depuración

La ofuscación y minificación son incompatibles con la observabilidad a menos que se archiven los símbolos correspondientes:

1. **Extracción y Aislamiento**:
   - Archivos de mapeo de ofuscación (`mapping.txt`).
   - Archivos de símbolos de depuración nativos (`dSYM`, debug symbols de C++/Rust).
   - Sourcemaps de frontend (`.map`).
2. **Cero Exposición Pública**:
   - Los símbolos y sourcemaps de aplicaciones privadas o comerciales no deben empaquetarse en bundles públicos ni servirse en servidores web abiertos.
3. **Carga Segura a Observabilidad**:
   - El pipeline debe subir automáticamente estos archivos a la plataforma de monitoreo de fallos (Crashlytics, Sentry, Datadog) indexados con el commit SHA y versión del build.

---

## 3. Contrato de Preservación de Símbolos (Keep Rules & Reflection)

Cuando se activa minificación/ofuscación, el compilador puede renombrar o eliminar clases que se instancian dinámicamente:

- **Responsabilidad de la Plataforma/App**: Proveer las reglas de exclusión/preservación (`proguard-rules.pro`, `@Keep`, configuraciones de serialización JSON/Protobuf/GraphQL).
- **Responsabilidad de la Infraestructura**:
  * Auditar que el compilador referencie el archivo de reglas de preservación.
  * Exigir que las pruebas de integración o smoke tests se ejecuten contra el binario **minificado/compilado en modo release**, no solo sobre el código en desarrollo.
