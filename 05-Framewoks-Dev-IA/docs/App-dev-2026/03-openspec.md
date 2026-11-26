# 03 · OpenSpec (Fission AI)

> **Estado:** 🟡 sketch · **Última verificación:** 2026-11-25 · **Versión documentada:** TODO [VERIFICAR en releases / npm `@fission-ai/openspec`]
> **Nivel SDD:** 3 · Spec-anchored
> **Relacionado:** [01 · Brief](01-brief-frameworks-dev-ia.md) · [02 · Spec Kit](02-spec-kit.md)

Convenciones: `TODO:` falta escribir · `[VERIFICAR]` confirmar con fuente primaria · `[OPINIÓN]` valoración propia.

---

## 1. Resumen (TL;DR)
Framework SDD ligero que trabaja **por cambio**: mantiene una spec viva del sistema (`openspec/specs/`) y cada cambio se describe como un **delta** (ADDED / MODIFIED / REMOVED / RENAMED) que se fusiona al archivarlo. Es la opción natural para código existente.

## 2. Origen y contexto
- **Organización:** Fission AI (actualizaciones en X por @0xTab [VERIFICAR autor]).
- **Licencia:** MIT. ~73k estrellas (nov-2026), 30+ agentes soportados.
- **Thoughtworks Radar:** Assess (abr-2026) → **Trial (nov-2026)**.
- **Problema:** las specs se pierden en el chat y el SDD pesado no encaja en brownfield.
- TODO: fecha del primer release e historia de versiones.

## 3. Filosofía y principios
- "Fluid not rigid · iterative not waterfall · easy not complex".
- Describir el **diff**, no el destino.
- No documentar todo el sistema de entrada: la spec crece cambio a cambio.
- Funciona sin API keys propias: usa el agente que ya tienes.

## 4. Nivel SDD y ubicación en el espectro
- **Nivel:** 3 · Spec-anchored.
- **Justificación:** la spec persistente es la fuente de verdad y los deltas la actualizan al archivar. Thoughtworks lo distingue explícitamente de Spec Kit y Kiro por esto.
- **Riesgo:** si no se archiva con disciplina, retrocede a spec-first.

## 5. Conceptos y glosario propio
| Término (EN) | Español | Definición |
|---|---|---|
| Spec (main) | Spec principal | `openspec/specs/<capability>/spec.md`: verdad actual |
| Capability | Capacidad | Área funcional que agrupa requirements (auth, ui, export…) |
| Change | Cambio | Carpeta en `openspec/changes/<nombre>/` con los artefactos de una modificación |
| Requirement | Requisito | Bloque `### Requirement:` con al menos un `#### Scenario:` |
| Scenario | Escenario | Formato WHEN/THEN (herencia de BDD) |
| Archive | Archivar | Fusiona deltas en la spec principal y mueve el cambio a `archive/` |
| Profile | Perfil | Conjunto de comandos activos (default o expanded) |
| Store | Store | Repo de planeación compartido para multi-repo (beta) |
| Schema | Esquema | Personalización del formato de artefactos [VERIFICAR] |

Términos generales usados en este capítulo (definidos en el [glosario general](00-glosario.md)): [spec delta](00-glosario.md#spec-driven-development) · [requisito](00-glosario.md#spec-driven-development) · [escenario WHEN/THEN](00-glosario.md#spec-driven-development) · [spec viva](00-glosario.md#spec-driven-development).

## 6. Instalación y requisitos
```bash
npm install -g @fission-ai/openspec@latest   # Node 20.19.0+
# o: brew install openspec
# o: curl -fsSL https://raw.githubusercontent.com/Fission-AI/OpenSpec/main/scripts/binary/install.sh | sh
cd tu-proyecto && openspec init
openspec update   # tras actualizar la herramienta, en cada proyecto
```
- En Alpine: `apk add libstdc++ libgcc`.
- Windows: `irm .../install.ps1 | iex`.

## 7. Flujo de trabajo paso a paso
| Paso | Comando | Qué hace | Artefacto | ¿Control humano? |
|---|---|---|---|---|
| 0 | `/opsx:explore` | Planeación abierta sin objetivo fijo | Notas | Sí |
| 1 | `/opsx:propose <nombre>` | Crea el cambio completo | proposal, specs delta, design, tasks | **Sí (revisión clave)** |
| 2 | `openspec validate <id> --strict` | Valida formato y escenarios | — | — |
| 3 | `/opsx:apply` | Implementa las tareas | Código + `tasks.md` marcado | Sí |
| 4 | `/opsx:archive` | Fusiona los deltas en la spec principal | `specs/` actualizado + `archive/` | Sí |

**Perfil expandido** (`openspec config profile` + `openspec update`): `/opsx:new`, `/opsx:continue` (un artefacto a la vez), `/opsx:ff` (fast-forward [VERIFICAR]), `/opsx:verify`, `/opsx:sync` (fusionar sin archivar), `/opsx:bulk-archive`, `/opsx:onboard` (tutorial sobre tu código).

**Sintaxis por agente:** `/opsx-propose` (Cursor, Copilot) · `@opsx-propose` (Amazon Q) · `$openspec-propose` (Codex).

**Cuándo NO crear un cambio:** un bugfix que restaura comportamiento ya especificado, o un typo o formato → se arregla directo.

## 8. Artefactos y estructura de archivos
```
openspec/
├── specs/<capability>/spec.md          # verdad actual
└── changes/
    ├── <change>/
    │   ├── proposal.md                 # por qué, alcance (in/out)
    │   ├── specs/<capability>/spec.md  # DELTA
    │   ├── design.md                   # cómo (decisiones técnicas)
    │   └── tasks.md                    # checklist - [ ] / - [x]
    └── archive/AAAA-MM-DD-<change>/    # historial (≈ ADRs)
```
**Reglas del delta:**
- ADDED → se agrega a la spec.
- MODIFIED → reemplaza el bloque completo (copiar el requirement entero y editarlo).
- REMOVED → se borra (incluir Reason y Migration).

## 9. Personalización y extensiones
- Perfiles de comandos.
- Esquemas de la comunidad y Community Showcase en `docs/` del repo [VERIFICAR].
- TODO: documentar `openspec/config` y la personalización de plantillas.

## 10. Uso en brownfield
- Es su punto fuerte: las specs se agregan sobre código existente.
- `/opsx:onboard` ofrece un recorrido guiado sobre tu propio código.
- Estrategia: empezar por la capability que más se toca y dejar crecer `specs/`.
- Cambios en paralelo sin conflicto si tocan requirements distintas.

## 11. Compatibilidad con agentes
| Agente | Soporte | Cómo se invoca | Notas |
|---|---|---|---|
| Claude Code | ✅ | `/opsx:*` | |
| Cursor | ✅ | `/opsx-*` | |
| GitHub Copilot | ✅ | `/opsx-*` | |
| Codex | ✅ | `$openspec-*` | |
| Amazon Q | ✅ | `@opsx-*` | |
| OpenCode | ✅ [VERIFICAR] | | Tutorial OpenCode + OpenSpec |
| Otros (30+) | | | TODO: lista *Supported Tools* |

## 12. Ejemplo práctico
- TODO greenfield: capability `orders` desde cero con 3 requirements.
- TODO brownfield: cambio `add-volume-discount` con ADDED (descuento) + MODIFIED (cálculo de total).
- TODO: mostrar el antes y después de `openspec/specs/orders/spec.md` tras archivar.

## 13. Ventajas
- El mejor de la lista para brownfield; más predecible que el prompting suelto (Thoughtworks).
- Ceremonia mínima: propose → apply → archive.
- Spec viva legible por humanos y agentes; el historial funciona como ADR.
- Poco costo en tokens; instalación simple (Node, brew o binario).

## 14. Desventajas y críticas
- Los agentes generan Markdown en exceso (*documentation sprawl*).
- Mantener la spec al día exige disciplina.
- Sin capa de producto (PRD, UX); sin TDD ni roles por diseño.
- Stores multi-repo en beta, sin comparaciones independientes.
- Video crítico: *I Tried OpenSpec and Others* cuestiona si estas herramientas complican de más.

## 15. Dónde brilla y dónde evitarlo
| Brilla en | Evitar en |
|---|---|
| Apps en producción con cambios frecuentes | Producto nuevo sin visión (mejor BMAD) |
| Dev solo / equipo pequeño | Hotfixes triviales |
| Mantenimiento y documentación viva | Exploración pura (usar solo `/opsx:explore`) |

## 16. Combinaciones con otros frameworks
- OpenSpec (qué) + Superpowers (TDD, revisión).
- RPI/QRSPI como research previo a `/opsx:propose`.
- AI-DLC como proceso + OpenSpec como formato.
- Ralph para ejecutar `tasks.md` de un cambio ya aprobado [OPINIÓN].

## 17. Preguntas abiertas
- ¿Cómo resuelve conflictos `bulk-archive` cuando dos cambios modifican el mismo requirement?
- Estado real de Stores (multi-repo).
- ¿Cómo evitar que `openspec/specs/` se vuelva inmanejable en sistemas grandes?
- Comparación de tokens contra Spec Kit en el mismo cambio.

## 18. Referencias
### Oficiales
- Repositorio — https://github.com/Fission-AI/OpenSpec
- Documentación — https://openspec.dev/docs
- Getting started — https://openspec.dev/docs/getting-started
- Conceptos — https://github.com/Fission-AI/OpenSpec/blob/main/docs/concepts.md
- Overview — https://github.com/Fission-AI/OpenSpec/blob/main/docs/overview.md
- Ejemplos — https://github.com/Fission-AI/OpenSpec/blob/main/docs/examples.md
- Discord — https://discord.gg/YctCnvvshC
### Artículos y guías
- Thoughtworks Radar, OpenSpec (Trial, nov-2026) — https://www.thoughtworks.com/radar/tools/openspec
- *OpenSpec Tutorial (2026): SDD in Claude Code, Step by Step* — https://pub.towardsai.net/openspec-tutorial-2026-spec-driven-development-in-claude-code-step-by-step-2901236c21a8
- *Getting Started with OpenSpec: A Beginner's Guide* — https://levelup.gitconnected.com/getting-started-with-openspec-a-beginners-guide-to-spec-driven-development-9696ece3fa5e
- Christopher Sidebottom, *Spec-Driven Development with OpenSpec* — https://dev.to/mousius/spec-driven-development-with-openspec-1fmb
- *OpenSpec Quickstart: Install, Workflow, and Common Pitfalls* — https://glukhov.org/ai-devtools/openspec/
- *OpenSpec: Make AI Coding Assistants Follow a Spec* — https://recca0120.github.io/en/2026/03/08/openspec-sdd
- Serie de tutoriales Conduction (parte 2) — https://conduction.nl/academy/openspec-tutorial-2-first-change
- *SDD with OpenCode and OpenSpec* — https://nsclass.github.io/2026/05/24/spec-driven-development-openspec-opencode
- Notas de Ted Neward — https://research.tedneward.com/ai/llm/spec-driven/openspec.html
- intent-driven.dev (guías y plantillas) [VERIFICAR URL]
### Comparativas
- Ver la sección de comparativas en [02 · Spec Kit](02-spec-kit.md)
- *Reviving Brownfield Projects with AI: Spec Kit, OpenSpec, BMAD* — https://medium.com/@mathivananmani/reviving-brownfield-projects-with-ai-a-comparative-look-at-github-spec-kit-openspec-and-bmad-a7bc5116dd9a
### YouTube / charlas
- *OpenSpec is Insane: SDD is 100X Better Than Vibe Coding* (AI Stack Studio) — https://www.youtube.com/watch?v=wZFOW89Lsc0
- *Beginner's Guide to SDD With OpenSpec* — https://www.youtube.com/watch?v=9MImID9o8MQ
- *Spec-Driven Development for AI Agents: I Tried OpenSpec and Others* (AI Coding Daily) [VERIFICAR URL]
- *I Found the Simplest AI Dev Tool Ever* (Better Stack) [VERIFICAR URL]
- *Spec-Driven Development with OpenCode and OpenSpec* [VERIFICAR URL]
- *GitHub Just Launched Spec Kit — But Fission AI's OpenSpec Fights Back!* [VERIFICAR URL]
- Maven, *Spec-Driven Prototyping with OpenSpec and Claude Code* — https://maven.com/p/7b4261/spec-driven-prototyping-with-open-spec-and-claude-code
### Libros (base conceptual)
- Gojko Adzic, *Specification by Example*
- Seb Rose & Gáspár Nagy, *The BDD Books: Formulation*

---

## Checklist de completitud
- [ ] Versión actual confirmada
- [ ] Perfil expandido documentado con un ejemplo por comando
- [ ] Ejemplo brownfield con antes/después de la spec
- [ ] URLs de YouTube confirmadas
- [ ] Coherente con el brief (01)
