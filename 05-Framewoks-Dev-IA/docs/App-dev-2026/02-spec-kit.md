# 02 · Spec Kit (GitHub)

> **Estado:** 🟡 sketch · **Última verificación:** 2026-11-25 · **Versión documentada:** 1.4.5 (25-nov-2026); serie 1.x desde 1.0.0 (21-ago-2026) ([CHANGELOG](https://github.com/github/spec-kit/blob/main/CHANGELOG.md))
> **Nivel SDD:** 2 · Spec-first (por feature; `/speckit-converge` lo acerca a anchored dentro del feature)
> **Relacionado:** [01 · Brief](01-brief-frameworks-dev-ia.md) · [03 · OpenSpec](03-openspec.md)

Convenciones: `TODO:` falta escribir · `[VERIFICAR]` confirmar con fuente primaria · `[OPINIÓN]` valoración propia.

---

## 1. Resumen (TL;DR)
Kit open source de GitHub para Spec-Driven Development con cualquier agente de código. Una CLI (`specify`), plantillas y slash commands llevan cada feature por constitución → spec → aclaración → plan → tareas → implementación → convergencia. Es la implementación de referencia del SDD "completo".

## 2. Origen y contexto
- **Organización:** GitHub. Lo idearon Den Delimarsky y John Lam; Den hizo el primer commit el 21-ago-2025. Desde el 22-ene-2026 el mantenedor principal es Manfred Riem (`@mnriem`), en la etapa que el proyecto llama *community stewardship* ([historia oficial](https://github.github.io/spec-kit/overview/history.html)).
- **Hitos:** anuncio en septiembre de 2025 (blog de Microsoft Dev) → serie 0.x → **1.0.0 (21-ago-2026)** con 5 primitivas: integraciones, extensiones, presets, workflows y pasos de workflow → serie 1.1–1.4 (oct–nov 2026; la 1.4.5 salió el 25-nov-2026 según el [CHANGELOG](https://github.com/github/spec-kit/blob/main/CHANGELOG.md)).
- **Problema:** "vibe coding" sin intención persistente; los agentes producen código plausible pero sin alinear con requisitos.
- **Licencia:** MIT.
- **Métricas (25-nov-2026, [portada de la documentación](https://github.github.io/spec-kit/)):** 147K+ estrellas, 320+ contribuidores, 43 opciones de integración (41 agentes + `generic` + `none`) y 186 extensiones de comunidad. El catálogo de presets de comunidad tiene 44 entradas (`presets/catalog.community.json`). Como referencia: en el lanzamiento de 1.0.0 eran 38 integraciones, 157 extensiones, 33 presets y 270+ contribuidores ([historia](https://github.github.io/spec-kit/overview/history.html)).
- TODO: historia completa desde `docs/history.md` del repo.

## 3. Filosofía y principios
- La spec es la fuente de verdad; el código es el resultado.
- Separar el **qué/por qué** (`specify`) del **cómo** (`plan`).
- Constitución = reglas no negociables que gobiernan todas las specs y planes.
- Tres modos de uso desde el inicio: 0-to-1 (greenfield), exploración paralela y mejora de brownfield.
- Postura del mantenedor sobre la 1.0: no congela el modelo ni promete estabilidad de API. Según Manfred Riem, 1.0.0 "es solo un número": como los agentes abaratan adaptarse al cambio, el valor pasa de la estabilidad a la adaptabilidad ([*Spec Kit Turns One — and Ships 1.0.0*](https://www.manorrock.com/blog/2026/08/21/spec_kit_turns_one.html), 21-ago-2026). La historia oficial agrega que los procesos y convenciones pueden evolucionar mientras los agentes ayudan a aplicar esos cambios en cada proyecto.
- TODO: resumir `spec-driven.md` (documento de metodología del repo).

## 4. Nivel SDD y ubicación en el espectro
- **Nivel:** 2 · Spec-first (Böckeler lo clasifica así).
- **Justificación:** la spec se escribe antes y guía la implementación, pero no se mantiene sincronizada con el sistema después del merge.
- **Movimiento:** `/speckit-converge` compara código ↔ spec/plan/tasks y agrega tareas hasta "Converged": anchored *dentro del feature*, no a nivel sistema.
- TODO: analizar si existen extensiones de la comunidad que mantengan una spec viva (p. ej. `spec-kit-refine`).

## 5. Conceptos y glosario propio
Términos generales usados en este capítulo (definidos en el [glosario general](00-glosario.md)): [especificación (spec)](00-glosario.md#spec-driven-development) · [nivel 2 · spec-first](00-glosario.md#spec-driven-development) · [nivel 3 · spec-anchored](00-glosario.md#spec-driven-development) · [spec viva](00-glosario.md#spec-driven-development) · [trazabilidad](00-glosario.md#spec-driven-development) · [criterio de aceptación](00-glosario.md#spec-driven-development) · [cascada disfrazada](00-glosario.md#spec-driven-development) · [comando de barra](00-glosario.md#estandares-y-protocolos) · [agnóstico](00-glosario.md#agentes-y-arnes) · [subagente](00-glosario.md#agentes-y-arnes) · [funcionalidad (feature)](00-glosario.md#procesos-roles-y-rituales) · [puerta](00-glosario.md#procesos-roles-y-rituales) · [greenfield / brownfield](00-glosario.md#procesos-roles-y-rituales) · [TDD](00-glosario.md#git-testing-e-infraestructura) · [worktree](00-glosario.md#git-testing-e-infraestructura).

| Término (EN) | Español | Definición | Mini-ejemplo |
|---|---|---|---|
| Constitution | Constitución | Principios rectores del proyecto; se crea una vez y se puede apilar en capas con presets | Regla "test-first: ningún endpoint sin un test que falle antes", válida para todos los features |
| `spec.md` | Spec del feature | Artefacto de Spec Kit con requisitos, historias y criterios de aceptación de un feature (concepto general: [spec](00-glosario.md#spec-driven-development)) | `spec.md` del módulo de pedidos: "crear pedido valida stock antes de confirmar" |
| `plan.md` | Plan técnico | Stack, arquitectura y diseño del feature; responde al *cómo* | `plan.md`: "API REST, sin dependencias nuevas sin justificar" |
| `tasks.md` | Tareas | Tareas ordenadas por dependencias, derivadas de spec y plan | T1 modelo `Pedido` → T2 endpoint `POST /pedidos` → T3 tests |
| Converge | Convergencia | Ciclo implement ↔ converge que compara código con spec/plan/tasks y agrega tareas hasta "Converged" | Converge detecta que falta la validación de stock y agrega una tarea a `tasks.md` |
| Integration | Integración | Adaptador para un agente concreto (41 agentes, más `generic` y `none`) | `specify init mi-proyecto --integration claude` |
| Extension | Extensión | Agrega comandos, puertas de calidad o procesos completos | La extensión `bug` agrega `/speckit-bug-assess`, `/speckit-bug-fix` y `/speckit-bug-test` |
| Preset | Preset | Cambia plantillas, terminología o estándares sin agregar capacidades | Preset del equipo que exige criterios de aceptación en todas las specs (ejemplo ilustrativo) |
| Bundle | Paquete | Combina extensiones, presets y workflows | Bundle que instala la extensión `bug` y el preset del equipo (ejemplo ilustrativo) |
| Workflow | Flujo automatizado | Automatiza procesos repetibles | Encadenar specify → plan → tasks en cada feature nuevo (ejemplo ilustrativo) |
| Feature state | Estado del feature | `.specify/feature.json`: feature activo (no depende de la rama git) | Sigues en `main`, pero `feature.json` indica que el feature activo es el de pedidos |

## 6. Instalación y requisitos
- Python 3.11+ y `uv`.
```bash
uv tool install specify-cli
# o fijando versión:
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z
specify init mi-proyecto --integration copilot   # claude, codex, gemini, cursor-agent...
```
- **Claves de `--integration`** (43, según el [índice oficial](https://github.github.io/spec-kit/integrations/index.html), 25-nov-2026): `alquimia`, `amp`, `agy` (Antigravity), `auggie`, `bob` (IBM Bob), `claude`, `cline`, `codebuddy`, `codex`, `command-code`, `copilot`, `cursor-agent`, `devin`, `docker-agent`, `droid` (Factory Droid), `dsh` (DeepSeek Harness), `firebender`, `forge`, `gemini`, `goose`, `grok`, `hermes`, `junie`, `kilocode`, `kimi`, `kiro-cli`, `lingma`, `mcode` (MiniMax Code), `muse`, `omp` (Oh My Pi), `opencode`, `pi`, `qodercli`, `qwen`, `rovodev`, `shai` (OVHcloud), `tabnine`, `trae`, `vibe` (Mistral Vibe), `zcode`, `zed`, más `generic` (otro agente) y `none` (solo el andamiaje compartido, sin agente).
- Ojo: la clave de Cursor es `cursor-agent`, no `cursor`.
- TODO: instalación en entornos offline y empresariales.

## 7. Flujo de trabajo paso a paso
| Paso | Comando | Qué hace | Artefacto | ¿Control humano? |
|---|---|---|---|---|
| 1 | `/speckit-constitution` | Principios del proyecto (una vez) | Constitución | Sí |
| 2 | `/speckit-specify` | Spec desde lenguaje natural | `spec.md` | Sí |
| 3 | `/speckit-clarify` | Preguntas sobre huecos; las integra | `spec.md` refinado | Sí |
| 4 | `/speckit-plan` | Diseño técnico, stack y arquitectura | `plan.md` | Sí |
| 5 | `/speckit-checklist` | Checklist de calidad de requisitos | `checklists/*.md` | Sí (revisor) |
| 6 | `/speckit-tasks` | Tareas ordenadas por dependencias | `tasks.md` | Opcional |
| 7 | `/speckit-analyze` | Consistencia spec/plan/tasks (solo lectura) | Reporte | Sí |
| 8 | `/speckit-implement` | Ejecuta las tareas; revisa el checklist antes | Código | Si hay ítems sin marcar |
| 9 | `/speckit-converge` | Compara código y spec; agrega tareas | `tasks.md` + | Hasta "Converged" |

- **Ruta corta:** specify → plan → tasks → implement → converge.
- **Sintaxis:** la forma de invocar depende de la integración y del modo, no de la versión de Spec Kit ([invocación por agente](https://github.github.io/spec-kit/integrations/cli.html#command-invocation)):

  | Integración / modo | Ejemplo |
  |---|---|
  | Copilot (skills, por defecto), Claude Code, Cursor | `/speckit-specify` |
  | Notación de la referencia oficial; agentes con comandos (p. ej. Gemini, TOML) | `/speckit.specify` |
  | Codex, Command Code, ZCode | `$speckit-specify` |
  | Kimi | `/skill:speckit-specify` |
- TODO: transcribir un ejemplo real de cada paso.

## 8. Artefactos y estructura de archivos
```
.specify/
├── .gitignore               # gestionado por init; excluye feature.json y estado local
├── memory/constitution.md   # constitución
├── scripts/                 # bash/, powershell/ o python/ según --script
├── templates/
│   └── overrides/           # personalización local (prioridad 1)
├── presets/<id>/            # presets instalados (prioridad 2)
├── extensions/<id>/         # extensiones instaladas (prioridad 3); config: <id>-config.yml
├── workflows/               # workflows, runs/, overlays/, steps/
├── integration.json         # integraciones instaladas y la predeterminada
├── feature.json             # feature activo (no depende de la rama git)
├── bugs/<slug>/             # extensión bug
└── assessments/<slug>/      # extensión assess
specs/NNN-nombre/            # p. ej. specs/003-user-auth
├── spec.md
├── checklists/requirements.md   # lo crea /speckit-specify; /speckit-checklist agrega otros
├── plan.md
├── research.md              # fase 0 de plan
├── data-model.md            # fase 1 de plan
├── contracts/               # fase 1, si hay interfaces externas
├── quickstart.md            # fase 1: guía de validación
└── tasks.md
```
- Archivos del agente (ejemplos): `.claude/skills/speckit-<cmd>/SKILL.md` · `.github/skills/…` (Copilot) · `.agents/skills/…` (Codex) · `.cursor/skills/…` · `.gemini/commands/speckit.<cmd>.toml`.
- Fuentes: plantillas de comandos del repo (`templates/commands/constitution.md`, `plan.md`), [referencia SDD](https://github.github.io/spec-kit/reference/agentic-sdd.html), [instalación](https://github.github.io/spec-kit/installation.html), guías de [extensiones](https://github.github.io/spec-kit/extensions/overview.html) y [artefactos](https://github.github.io/spec-kit/overview/artifacts.html).

## 9. Personalización y extensiones
- **Orden de prioridad:** overrides locales → presets → extensiones → core.
- **Extensiones oficiales:** `bug` (`/speckit-bug-assess|fix|test`) y `assess` (intake, research, define, shape, decide).
- **Comunidad** (todas siguen en el catálogo de comunidad al 25-nov-2026): `tdd` (red-green-refactor con evidencia), `refine` (actualiza la spec y propaga a plan y tasks), `figma`, `tasks-to-project`, `confluence`, `blueprint`, `conduct` (ejecuta una fase delegando en un subagente), `docguard`, `atlas`. Se buscan con `specify extension search`.
- **Constitución por capas:** base de la organización → reglas de dominio → equipo (vía presets).
- **CLI:** `specify extension|preset|workflow|bundle search/add`.
- ⚠️ Estar en el catálogo no significa estar auditado: revisar el código antes de instalar.
- TODO: tutorial para crear un preset propio.

## 10. Uso en brownfield
- Guía oficial: [*How to adopt Spec Kit in an existing project*](https://github.github.io/spec-kit/how-to/existing-projects.html) (antes titulada *Adopting Spec Kit in an Existing Project*; se movió de `guides/` a `how-to/` el 03-nov-2026). Arranque: `specify init --here --force --integration <key>` sobre una rama con el trabajo previo ya commiteado.
- Práctica recomendada: inferir la constitución del código real y escribir specs solo para cambios nuevos.
- Thoughtworks reporta uso mayoritario en brownfield con ciclo spec → plan → tasks → code → review.
- Walkthroughs de la comunidad: proyecto Java multi-módulo sin specs previas; dashboard Go/React con Copilot CLI.
- TODO: documentar un caso propio.

## 11. Compatibilidad con agentes
| Agente | Soporte | Cómo se invoca | Notas |
|---|---|---|---|
| GitHub Copilot | ✅ | `/speckit-*` | Ejemplo por defecto en la documentación; skills en `.github/skills/` |
| Claude Code | ✅ | `/speckit-*` | Skills en `.claude/skills/`; admite `--no-model-invocation` |
| Codex | ✅ | `$speckit-*` | Skills en `.agents/skills/` (directorio compartido con otras integraciones) |
| Gemini CLI | ✅ | `/speckit.*` | Comandos TOML en `.gemini/commands/` |
| Cursor | ✅ | `/speckit-*` | Clave `cursor-agent`; skills en `.cursor/skills/` |
| Otros (36 agentes más, más `generic` y `none`) | ✅ | Según la integración | Ver el [índice de integraciones](https://github.github.io/spec-kit/integrations/index.html) |

## 12. Ejemplo práctico
- TODO greenfield: módulo de pedidos con constitución "test-first + API REST + sin dependencias nuevas sin justificar".
- TODO brownfield: "descuento por volumen" sobre el sistema de inventario existente.
- TODO: medir tiempo, tokens, archivos generados y líneas de Markdown vs líneas de código.

## 13. Ventajas
- Realmente agnóstico (30+ agentes); la spec viaja entre herramientas.
- Trazabilidad requisito → tarea → código.
- `clarify` y `analyze` sacan la ambigüedad antes de programar.
- Ecosistema grande y respaldado por GitHub; constitución por capas para empresas.

## 14. Desventajas y críticas
- Markdown verboso y repetitivo, a veces más pesado de revisar que el código (Böckeler/Fowler).
- Verificaciones defensivas innecesarias y salidas que aumentan la carga cognitiva (Thoughtworks Radar).
- La spec se desactualiza tras el merge (crítica general a SDD).
- Riesgo de "cascada disfrazada" (Marmelab).
- Excesivo para bugfixes; API todavía en movimiento.
- Mitigación: personalizar plantillas para limitar archivos y verbosidad.

## 15. Dónde brilla y dónde evitarlo
| Brilla en | Evitar en |
|---|---|
| Features nuevos medianos/grandes | Prototipos desechables |
| Equipos de 3+ que necesitan acuerdos | Bugfixes pequeños (usar la extensión `bug` o nada) |
| Contextos regulados / auditoría | Exploración sin objetivo claro |
| Organizaciones con varios agentes | Dev solo con cambios mínimos frecuentes (mejor OpenSpec) |

## 16. Combinaciones con otros frameworks
- Spec Kit (qué) + Superpowers (cómo, con TDD): la extensión de comunidad [`speckit-superpowers-bridge`](https://github.com/lihan3238/speckit-superpowers-bridge) (v1.3.0 en el catálogo al 25-nov-2026) se describe como un orquestador delgado entre los artefactos de diseño de Spec Kit y la disciplina de implementación de Superpowers, verificado en Codex y Claude Code. Su lema: "Spec Kit writes WHAT. Superpowers enforces HOW."
- AI-DLC como proceso de equipo + Spec Kit como formato de artefactos.
- RPI como research previo a `/speckit-specify` en brownfield.

## 17. Preguntas abiertas
- ¿Cómo se comporta `converge` en features grandes? ¿Cuántas iteraciones hacen falta?
- ¿Existe una forma oficial de pasar de spec-first a spec-anchored?
- Costo real en tokens por feature, comparado con OpenSpec.
- Diferencias de comportamiento entre integraciones (Claude vs Copilot vs Codex).

## 18. Referencias
> Última revisión de enlaces: 2026-11-25. Dominio canónico de la documentación: `https://github.github.io/spec-kit/`.

### Oficiales
- Repositorio — https://github.com/github/spec-kit
- Documentación — https://github.github.io/spec-kit/
- CHANGELOG: fuente de la versión vigente (releases cada 2–3 días) — https://github.com/github/spec-kit/blob/main/CHANGELOG.md
- Releases — https://github.com/github/spec-kit/releases
- Instalación — https://github.github.io/spec-kit/installation.html
- Actualización (`specify self upgrade`, refresco del proyecto) — https://github.github.io/spec-kit/upgrade.html
- Quickstart (recorrido SDD completo) — https://github.github.io/spec-kit/quickstart.html
- Referencia de comandos SDD: qué hace cada `/speckit.*` y qué escribe — https://github.github.io/spec-kit/reference/agentic-sdd.html
- Índice de integraciones (las 43 claves de `--integration`) — https://github.github.io/spec-kit/integrations/index.html
- Invocación por agente (`/speckit-*`, `/speckit.*`, `$speckit-*`, `/skill:`) — https://github.github.io/spec-kit/integrations/cli.html#command-invocation
- Adopción en un proyecto existente (brownfield) — https://github.github.io/spec-kit/how-to/existing-projects.html
- Persistencia de specs tras la entrega (4 modelos; clave para el nivel SDD) — https://github.github.io/spec-kit/how-to/persisting-specs.html
- Personalización (overrides, presets, extensiones) — https://github.github.io/spec-kit/how-to/customization.html
- Comunidad (extensiones, presets, walkthroughs) — https://github.github.io/spec-kit/community/overview.html
- Historia (fundadores, mantenedores, hitos) — https://github.github.io/spec-kit/overview/history.html
- Metodología (`spec-driven.md`) — https://github.com/github/spec-kit/blob/main/spec-driven.md
- Newsletters mensuales (cambios con enlaces a PRs; útiles para P10) — https://github.com/github/spec-kit/tree/main/newsletters

### Autores y mantenedores
- Den Delimarsky, *Spec-driven development with AI: Get started with a new open source toolkit* (GitHub Blog, 02-sep-2025). Anuncio original — https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/
- Den Delimarsky, *Diving Into Spec-Driven Development With GitHub Spec Kit* (Microsoft for Developers, 15-sep-2025) — https://developer.microsoft.com/blog/spec-driven-development-spec-kit/
- Manfred Riem, *Six months leading Spec Kit* (22-jul-2026). Cambio de mantenedor y enfoque — https://www.manorrock.com/blog/2026/07/22/six_months_leading_spec_kit.html
- Manfred Riem, *Spec Kit Turns One — and Ships 1.0.0* (21-ago-2026). Postura sobre "1.0 no es estabilidad" — https://www.manorrock.com/blog/2026/08/21/spec_kit_turns_one.html

### Artículos y guías
- Apoorv Gupta, *From Spec-First to Enterprise-Ready: Extending GitHub Spec Kit* (Microsoft for Developers, 07-oct-2026). Presets, extensiones, bundles y catálogos gobernados — https://developer.microsoft.com/blog/from-spec-first-to-enterprise-ready-extending-github-spec-kit/
- Thoughtworks Technology Radar, *GitHub Spec Kit*: Assess, Vol. 34 (15-abr-2026). Uso en brownfield, verbosidad y *context rot*; ya no figura en la edición vigente — https://www.thoughtworks.com/radar/languages-and-frameworks/github-spec-kit
- Martin Heller, *4 cutting-edge tools for spec-driven development* (InfoWorld, 15-may-2026). Kiro, Spec Kit, Tessl y Zenflow — https://www.infoworld.com/article/4171332/4-cutting-edge-tools-for-spec-driven-development.html
- David Ramel, *GitHub Spec Kit Takes Off as Antidote to Piecemeal Vibe Coding* (Visual Studio Magazine, 12-may-2026). Crónica de la demo de Riem — https://visualstudiomagazine.com/articles/2026/05/12/github-spec-kit-takes-off-as-antidote-to-piecemeal-vibe-coding.aspx
- Theo van Kraay, *Spec-Driven Development comes to Azure Cosmos DB: The First Database Extension for GitHub Spec Kit* (Microsoft DevBlogs, 21-sep-2026). Ejemplo de extensión de un equipo de producto — https://devblogs.microsoft.com/cosmosdb/spec-driven-development-comes-to-azure-cosmos-db-the-first-database-extension-for-github-spec-kit/
- Alfredo Pérez, *Living Specs in SpecKit* (15-oct-2026). Specs a nivel de capacidad que se mantienen vivas; apoyo para §4 — https://www.alfredo-perez.dev/blog/2026-10-15-living-specs
- Jesse Liberty, serie de personalización de Spec Kit (08–10-oct-2026). Cómo crear, probar y publicar presets y extensiones — https://jesseliberty.com/2026/10/08/4-solutions-to-spec-kit-issues/

### Comparativas
- Birgitta Böckeler, *Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl* (martinfowler.com, 15-oct-2025). Origen de los niveles 2–4. ⚠️ Fuente de más de 6 meses: la crítica sobre la rama por spec y la falta de guía brownfield ya no aplica a la 1.4 — https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html
- Sanderson Oliveira de Macedo, *From Prompt to Process: a Process Taxonomy and Comparative Assessment of Frameworks Supporting AI Software Development Agents* (arXiv, 03-jun-2026). Taxonomía de 6 dimensiones aplicada a 6 frameworks — https://arxiv.org/abs/2606.04967
- cameronsjo, *spec-compare* (GitHub, actualizado el 28-ago-2026). 20 herramientas SDD, soporte de worktrees y niveles de madurez — https://github.com/cameronsjo/spec-compare
- William Torres, *Spec Kit vs BMAD vs OpenSpec: Choosing an SDD Framework in 2026* (DEV, 23-abr-2026) — https://dev.to/willtorber/spec-kit-vs-bmad-vs-openspec-choosing-an-sdd-framework-in-2026-d3j
- Rost Glukhov, *GitHub Spec Kit vs Kiro vs Claude Code SDD Workflows* (DEV, 12-jul-2026; publicado originalmente en glukhov.org) — https://dev.to/rosgluk/github-spec-kit-vs-kiro-vs-claude-code-sdd-workflows-1fi1
- *Spec-Driven Development Frameworks a Year Later: Pattern Families, Measurements and a Choice Matrix* (Level Up Coding, nov-2026; artículo solo para miembros, autor sin confirmar). Datos hasta fines de octubre de 2026 — https://levelup.gitconnected.com/spec-driven-development-frameworks-a-year-later-pattern-families-measurements-and-a-choice-matrix-125bcddf1e64
- Sirajuddin Khan, *Spec-Driven Development Tools Compared: Spec Kit vs OpenSpec vs Kiro vs BMAD* (TestMu AI, 07-nov-2026). ⚠️ Contenido de proveedor: promociona sus propias herramientas de testing — https://www.testmuai.com/blog/spec-driven-development-tools/

### Críticas y evidencia empírica
- Brenn Hill, *Specification Artifacts in Open-Source Pull Request Workflows Do Not Reduce Defects: An Empirical Test of Spec-Driven Development Claims* (ESEM 2026, LIPIcs). 88.052 PRs en 119 repos; sin efecto de la calidad de la spec en defectos ni retrabajo — https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.ESEM.2026.84
- Shyam Agarwal y Bogdan Vasilescu, *SpecMine: A Large-Scale Corpus of Spec-Driven Development Artifacts* (arXiv, 25-ago-2026). Corpus de artefactos de 11 herramientas, incluido Spec Kit — https://arxiv.org/abs/2608.25202
- François Zaninotto, *Spec-Driven Development: The Waterfall Strikes Back* (Marmelab, 12-nov-2025). Ejemplo con Spec Kit: 8 archivos y 1.300 líneas para una feature mínima — https://marmelab.com/blog/2025/11/12/spec-driven-development-waterfall-strikes-back
- Sam K., *Spec-Driven Development Renamed an Old Problem; It Didn't Solve It* (DZone, 21-jul-2026; también en DEV). La sincronía de la spec como problema no resuelto — https://dzone.com/articles/spec-driven-development
- Capítulo 9 del curso *AI Coding Agents & SDD*, "The honest critique: maintenance tax, MDD and the illusion of control" (sin autor ni fecha visibles). Seis objeciones; resume a Isoform y a Böckeler. Solo como apoyo — https://jmlopezdona.github.io/ai-coding-agents-sdd/09-honest-critique/

### YouTube / charlas
- Den Delimarsky, *The ONLY guide you'll need for GitHub Spec Kit* (canal de Den, 13-sep-2025, ~40 min). Guía del autor original; corresponde a la versión 0.x — https://www.youtube.com/watch?v=a9eR1xsfvHg
- *Open Source Friday with Spec Kit* (canal de GitHub, 08-may-2026). Introducción y demo SDD con Manfred Riem — https://www.youtube.com/watch?v=2IArMAhkJcE
- *Open Source Friday: Spec Kit Part 2* (canal de GitHub, 16-oct-2026). Manfred Riem y Nicole Haugen sobre 1.0 y el toolkit ampliado — https://www.youtube.com/watch?v=8hHCpNVLhmI

### Podcasts
- *.NET Rocks!*, episodio "GitHub Spec Kit with Den Delimarsky" (30-oct-2025, 1 h 05 min). Carl Franklin y Richard Campbell con el autor — https://pocketcasts.com/podcast/net-rocks/3a3d5940-36f9-012e-0fd1-00163e1b201c/github-spec-kit-with-den-delimarsky/8a46f218-7ce4-4e90-ac6b-982216e117ec

### Libros (base conceptual)
- Gojko Adzic, *Specification by Example* (Manning, 2011). Especificaciones como ejemplos ejecutables: el antecedente directo de la spec como contrato
- Addy Osmani, *Beyond Vibe Coding: From Coder to AI-Era Developer* (O'Reilly, 18-ago-2025; 254 páginas). Del vibe coding a la ingeniería asistida por IA; contexto para los niveles 0–1 — https://www.oreilly.com/library/view/beyond-vibe-coding/9798341634749/

---

## Checklist de completitud
- [ ] Secciones 6, 7, 8 y 11 verificadas contra la documentación oficial
- [ ] Ejemplo práctico ejecutado (greenfield y brownfield)
- [ ] URLs de YouTube confirmadas
- [ ] Métricas actualizadas
- [ ] Coherente con el brief (01)
