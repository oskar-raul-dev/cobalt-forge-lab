# 04 · BMAD Method

> **Estado:** 🟡 sketch · **Última verificación:** 2026-11-25 · **Versión documentada:** v6.12 estable · V7 preview alpha [VERIFICAR si la V7 ya es estable]
> **Nivel SDD:** 2 · Spec-first (con tendencia a anchored dentro del árbol de tickets)
> **Relacionado:** [01 · Brief](01-brief-frameworks-dev-ia.md)

Convenciones: `TODO:` falta escribir · `[VERIFICAR]` confirmar con fuente primaria · `[OPINIÓN]` valoración propia.

---

## 1. Resumen (TL;DR)
*Breakthrough Method of Agile AI-Driven Development.* Simula un equipo ágil completo con personas-agente (analista, PM, PO, UX, arquitecto, dev) y lleva el trabajo de la idea al código. La profundidad de planeación escala con el tamaño y el riesgo. Su patrón estrella: un chat nuevo por historia, con el archivo de la historia como paquete de traspaso.

## 2. Origen y contexto
- **Autor:** Brian (BMad), BMad Code LLC (BMad™ y BMAD-METHOD™ son marcas registradas) [VERIFICAR nombre completo].
- **Versiones:** v4 (estable histórica) → v6 alpha (sharding, step-files) → v6.3–v6.8 (abr–may 2026) → **v6.12 estable** → **V7 preview** (todo sobre skills; ETA anunciada 16-oct-2026).
- **Métricas:** ~49k estrellas (jun-2026) [VERIFICAR actual].
- **Licencia:** MIT.
- **Problema:** pérdida de contexto entre agentes y ausencia de disciplina de producto en el desarrollo con IA.

## 3. Filosofía y principios
- Modelar toda la organización de entrega (producto, arquitectura, desarrollo y QA) como roles.
- Ceremonia proporcional: según tamaño (sesiones de build) y riesgo (de hobby a regulado).
- Las etapas no son una secuencia: se entra donde está tu input.
- Cada sesión lee el ticket, no el historial del chat.
- Un build termina en "built"; "done" lo marca un humano.

## 4. Nivel SDD y ubicación en el espectro
- **Nivel:** 2 · Spec-first.
- **Justificación:** PRD, arquitectura y specs se escriben antes; las historias son el paquete de implementación.
- **Tendencia anchored:** requirements con IDs estables y `tickets.toml` persistentes, pero sin fusión automática en una spec del sistema [OPINIÓN].

## 5. Conceptos y glosario propio
| Término (EN) | Español | Definición |
|---|---|---|
| Persona / agent | Persona | Rol con nombre: Mary, John, Sofia, Sally, Winston, Amelia |
| Initiative | Iniciativa | Spec del resultado completo con sus épicas en orden |
| Epic | Épica | Capacidad que alguien podría ver funcionando sola |
| Story | Historia | Capacidad de punta a punta; su build plan registra el progreso |
| Sharding | Fragmentación | Partir PRD y arquitectura en archivos autocontenidos |
| Step-file architecture | Arquitectura de pasos | Control fino de pasos con estado guardable (v6) |
| Party mode | Modo fiesta | Discusión entre varios agentes |
| Advanced elicitation | Elicitación avanzada | Cuestionar un borrador |
| Quick Flow / Planning path | Ruta de planeación | Trivial · una sesión · épica · proyecto |
| Deferred work | Trabajo diferido | `deferred-work.md` con pendientes de sesión |

## 6. Instalación y requisitos
```bash
# v6 estable (Node 20.12+, Python 3.10+, uv)
npx bmad-method install
# V7 preview como skills
npx skills add bmad-code-org/BMAD-METHOD --skill '*'
# Claude Code plugin
/plugin marketplace add bmad-code-org/bmad-plugins
```
- TODO: instalación en Codex (marketplace) y en otros agentes.
- Histórico v6 alpha: `npx bmad-method@alpha install` y `*workflow-init`.

## 7. Flujo de trabajo paso a paso
### Modelo V7: ciclo de 4 etapas
| Etapa | Personas | Skills | Salida |
|---|---|---|---|
| Clarify | Mary | `bmad-brainstorming`, `bmad-deep-recon` | Por qué y si vale la pena |
| Plan | John, Sofia, Sally, Winston | TODO | Requisitos, decisiones, historias listas |
| Build & verify | Amelia | `bmad-build`, `bmad-build-auto`, `bmad-code-review`, `bmad-walkthrough` | Código "built" |
| Learn & adjust | — | `bmad-retrospective` | Veredicto por épica; cambios vuelven a Plan |

Helpers: `bmad` (qué sigue), `bmad-help`, `bmad-advanced-elicitation`, `bmad-party-mode`, `bmad-review` (lentes de revisión unificadas).

### Rutas de planeación
| Ruta | Flujo |
|---|---|
| Trivial | cambiar → editar → verificar |
| Una sesión | intención → build → resultado |
| Épica | spec → historias → un build por historia → integración → retro |
| Proyecto | contratos compartidos → ruta épica por cada épica |

### v6 clásico (estable)
- Corta: `/quick-spec` → `/dev-story` → `/code-review`.
- Completa: `/create-architecture` → `/create-epics-and-stories` → `/sprint-planning` → por historia: `/create-story` → `/dev-story` → `/code-review`.
- Slash commands v6: `/bmad:bmm:workflows:prd`, `/bmad:bmm:workflows:dev-story` [VERIFICAR].

## 8. Artefactos y estructura de archivos
```
# V7 (árbol de tickets) [VERIFICAR rutas]
initiative.md, epics/, stories/, tickets.toml, deferred-work.md
# requirements con IDs estables, Constraints, Done when, Boundaries, References
# builds grandes: PRD, UX, architecture

# v6
{bmad_folder}/
├── core/   # BMad Master
├── bmm/    # BMad Method (12 agentes, 34 workflows)
├── bmb/ cis/
└── _cfg/   # personalización que sobrevive a updates
```

## 9. Personalización y extensiones
- **Módulos:** BMad Method · BMad Builder (crear skills, workflows y agentes) · Creative Intelligence Suite · Test Architect · BMad Loop (épica sin supervisión) · Game Dev Studio (Unity, Unreal, Godot, Phaser).
- Personalizar personas (nombre, rol, estilo) en `_cfg/agents/` (v6).
- Capas de revisión configurables (`[[workflow.review_layers]]`), incluido un revisor externo vía bash.
- Web Bundles (Gemini Gems / ChatGPT Custom GPTs) para planear fuera del IDE.

## 10. Uso en brownfield
- Ruta oficial: *Start in an Existing Codebase* / *Change an Existing Codebase*.
- TODO: documentar el contexto del proyecto (`bmad-document-project` → [VERIFICAR nombre en V7]).

## 11. Compatibilidad con agentes
| Agente | Soporte | Cómo se invoca | Notas |
|---|---|---|---|
| Claude Code | ✅ | skills / plugin | |
| Codex | ✅ | plugin marketplace | |
| Cursor | ✅ (v6) [VERIFICAR V7] | | |
| Otros con skills | ✅ (V7) | | La V7 requiere soporte de skills |

## 12. Ejemplo práctico
- TODO: ruta "proyecto" para el sistema de inventario y pedidos (brief → PRD → arquitectura → épicas).
- TODO: ruta "una sesión" para un cambio pequeño; comparar el overhead.
- TODO: medir tokens con y sin sharding.

## 13. Ventajas
- Único con capa de producto completa (análisis, PRD, UX, arquitectura).
- Ceremonia escalable según tamaño y riesgo.
- Chat nuevo por historia + sharding → buen manejo del contexto.
- Muy personalizable (Builder); agnóstico desde la V7.

## 14. Desventajas y críticas
- Curva de aprendizaje alta: personas, fases, sharding.
- Consumo alto de tokens (históricamente llenaba la ventana).
- Exagerado para tareas simples ("necesitaba una app de tareas, no una ceremonia de sprint").
- Checklists de 500+ líneas por historia.
- API inestable en la transición v6 → V7.
- No reemplaza a un orquestador multiagente real.

## 15. Dónde brilla y dónde evitarlo
| Brilla en | Evitar en |
|---|---|
| Producto nuevo que necesita brief, PRD y arquitectura | Prototipos, scripts, bugfixes sueltos |
| Dev que hace de PM | Proyectos solo técnicos sin dimensión de producto |
| MVP serio / startup | Equipos que no quieren aprender la jerga |

## 16. Combinaciones con otros frameworks
- BMAD (Clarify + Plan) → GSD Core o Compound Engineering (Build).
- BMad Loop ≈ Ralph loop por épica.
- OpenSpec después del lanzamiento para el mantenimiento [OPINIÓN].

## 17. Preguntas abiertas
- Estado real de la V7 a la fecha.
- Diferencias concretas entre la ruta épica de V7 y el flujo completo de v6.
- Costo en tokens por historia.
- ¿Cómo convive con un AGENTS.md propio?

## 18. Referencias
### Oficiales
- Repositorio — https://github.com/bmad-code-org/BMAD-METHOD
- Releases — https://github.com/bmad-code-org/BMAD-METHOD/releases
- Docs — https://docs.bmad-method.org
- How BMad Works — https://docs.bmad-method.org/start/how-bmad-works/
- Build Your First Change — https://docs.bmad-method.org/start/build-your-first-change/
- Start in an Existing Codebase — https://docs.bmad-method.org/existing-codebases/start-in-an-existing-codebase/
- Discord — https://discord.gg/gk8jAdXWmj
### Artículos y guías
- Augment Code, *What Is the BMAD Method?* (jul-2026) — https://www.augmentcode.com/guides/bmad-method-ai-development
- CodeMySpec, *The BMAD Method Explained* — https://codemyspec.com/blog/bmad-method-explained
- Ry Walker, investigación BMAD (jun-2026) — https://rywalker.com/research/bmad-method
- *BMAD-METHOD Guide* — https://redreamality.com/garden/notes/bmad-method-guide/
- *Mastering the BMAD Method* — https://medium.com/accelerated-analyst/mastering-the-bmad-method-a-better-approach-to-agile-ai-driven-development-for-modern-software-a77a6808a9a0
- *Learning how to BMAD* — https://dev.to/dmitryame/learning-how-to-bmad-307j
- asantos2000, *In-Depth Overview, Evaluation, and Comparison* — https://asantos2000.github.io/blog/post-2/
### Críticas
- asantos2000, *Limitations of BMAD Method* — https://asantos2000.github.io/blog/post-3/
- *BMAD method disappointment* — https://guillim.github.io/ai/development/2026/02/08/bmad-method-disappointment.html
- *I stopped using BMAD, here's what replaced it* — https://dev.to/ndabene/i-stopped-using-bmad-heres-what-replaced-it-540e
### Comparativas
- Ver la sección de comparativas en [02 · Spec Kit](02-spec-kit.md)
### YouTube / charlas
- Canal oficial BMad Code — https://youtube.com/@BMadCode
- TODO: walkthrough V6 con patrón de chat nuevo por historia [VERIFICAR URL]
- TODO: anuncio o walkthrough de la V7
### Libros (base conceptual)
- Jeff Patton, *User Story Mapping*
- Marty Cagan, *Inspired*
- Mike Cohn, *User Stories Applied* [OPINIÓN: útil para la calidad de las historias]

---

## Checklist de completitud
- [ ] Estado V7 confirmado
- [ ] Rutas de archivos V7 verificadas
- [ ] Ejemplo práctico con dos rutas comparadas
- [ ] Coherente con el brief (01)
