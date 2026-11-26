# 06 · Research → Plan → Implement (HumanLayer) / QRSPI

> **Estado:** 🟡 sketch · **Última verificación:** 2026-11-25 · **Versión documentada:** RPI (2025) → QRSPI / "CRISPY" (2026)
> **Nivel SDD:** 1 · Plan-first (artefactos por ticket, sin spec persistente)
> **Relacionado:** [01 · Brief](01-brief-frameworks-dev-ia.md) · [07 · Ralph](07-ralph-loop.md)

Convenciones: `TODO:` falta escribir · `[VERIFICAR]` confirmar con fuente primaria · `[OPINIÓN]` valoración propia.

---

## 1. Resumen (TL;DR)
Disciplina de *context engineering* para trabajar con agentes en codebases grandes y complejos. No se instala: son prompts, fases y artefactos Markdown. La versión original (Research → Plan → Implement) evolucionó en 2026 a **QRSPI / CRISPY**: Questions → Research → Design → Structure → Plan → Implement → PR, con prompts más cortos y lectura obligatoria del código.

## 2. Origen y contexto
- **Autor:** Dex (Dexter) Horthy, CEO de HumanLayer (YC F24); antes en NASA JPL y Replicated. Autor de *12-Factor Agents* (abr-2025); se le atribuye haber acuñado "context engineering".
- **Hitos:** ACE / ace-fca.md (2025) · charla *No Vibes Allowed* (AI Engineer Code, nov/dic-2025) · *Everything We Got Wrong About RPI* (2026) · *Why Software Factories Fail* (AI Engineer World's Fair 2026).
- **Alcance:** miles de ingenieros, desde startups hasta Fortune 500; ~10.000 descargas de los prompts originales.
- **Problema:** los agentes fallan en codebases brownfield grandes por saturación de contexto y desalineación de intención.

## 3. Filosofía y principios
- La ventana de contexto es un recurso de ingeniería limitado: mantenerla en 40–60 %.
- **Compactación intencional:** destilar el estado en archivos y reiniciar con contexto limpio.
- Subagentes para controlar el contexto, no para juego de roles.
- **Apalancamiento humano:** una línea mala de research produce miles de líneas malas de código; revisar temprano.
- Corrección de 2026: también hay que **leer el código** ("probamos no leerlo seis meses y no terminó bien").
- Expectativas realistas: 2–3× de productividad, no 10×.

## 4. Nivel SDD y ubicación en el espectro
- **Nivel:** 1 · Plan-first.
- **Justificación:** los artefactos (research, design, outline, plan) son por ticket y no forman una spec del sistema.
- Puede alimentar a un framework de nivel 2–3 (Spec Kit / OpenSpec) [OPINIÓN].

## 5. Conceptos y glosario propio
| Término (EN) | Español | Definición |
|---|---|---|
| Blind research | Research ciego | Investigar sin conocer el objetivo para evitar sesgo |
| Design discussion | Discusión de diseño | Documento de ~200 líneas: estado actual, estado deseado, patrones, decisiones, preguntas |
| Structure outline | Esquema de estructura | ~2 páginas, fases en rebanadas verticales con checkpoints |
| Vertical vs horizontal plan | Plan vertical vs horizontal | Por funcionalidad completa vs por capa |
| 12-Factor Agents | 12 factores de agentes | Manifiesto para agentes fiables en producción |

Términos generales usados en este capítulo (definidos en el [glosario general](00-glosario.md)): [ingeniería de contexto](00-glosario.md#contexto-y-prompts) · [compactación](00-glosario.md#contexto-y-prompts) · [zona tonta](00-glosario.md#contexto-y-prompts) · [presupuesto de instrucciones](00-glosario.md#contexto-y-prompts) · [rebanada vertical](00-glosario.md#procesos-roles-y-rituales).

## 6. Instalación y requisitos
- Sin instalación. Prompts de referencia en `humanlayer/humanlayer/.claude/commands/`: `research_codebase.md`, `create_plan.md`, `implement_plan.md`.
- Implementación portable sugerida (skills):
```
skills/
├── rpi-questions/SKILL.md
├── rpi-research/SKILL.md
├── rpi-design/SKILL.md
├── rpi-outline/SKILL.md
└── rpi-implement/SKILL.md
```
- TODO: revisar si HumanLayer publicó prompts QRSPI oficiales [VERIFICAR].

## 7. Flujo de trabajo paso a paso
### RPI original
| Fase | Qué hace | Artefacto | Dónde |
|---|---|---|---|
| Research | Código relevante, flujo de información, causas | `research.md` | main |
| Plan | Pasos exactos, archivos, verificación por fase | `plan.md` | main |
| Implement | Fase por fase; compacta el estado en el plan | Código | git worktree |

### QRSPI / CRISPY (2026)
| Etapa | Propósito | Salida | Clave |
|---|---|---|---|
| Questions | Preguntas de research desde el ticket | Preguntas | Aislada; no ve los objetivos |
| Research | Hechos objetivos del código | Hallazgos | Contexto limpio, research ciego |
| Design | Alinear humano y agente | ~200 líneas | Revisable por el equipo |
| Structure | Fases y checkpoints de tests | ~2 páginas | Rebanadas verticales |
| Plan | Plan táctico | Plan detallado | Ya no es la herramienta principal de dirección |
| Implement | Código | Código | — |
| PR | Revisión del equipo | Pull request | Revisión de código obligatoria |

- TODO: confirmar el nombre oficial (QRSPI vs CRISPY) y la expansión del acrónimo [VERIFICAR].

## 8. Artefactos y estructura de archivos
```
docs/agent/<ticket>/       # o thoughts/ [VERIFICAR convención de HumanLayer]
├── questions.md
├── research.md
├── design.md
├── outline.md
├── plan.md
└── progress.md
```

## 9. Personalización y extensiones
- Adaptar los prompts a skills portables.
- Subagentes de solo lectura (locator, analyzer, pattern-finder) [VERIFICAR nombres en HumanLayer].
- TODO: producto comercial de HumanLayer (CodeLayer u otro) [VERIFICAR].

## 10. Uso en brownfield
- Es su caso principal. Ejemplos: BAML (300k LOC en Rust): un bugfix en ~1 h por alguien nuevo en el código; cancelación + WASM: 35k LOC en ~7 h entre dos personas.
- Caso fallido: quitar Hadoop de parquet-java, por research superficial del árbol de dependencias.

## 11. Compatibilidad con agentes
| Agente | Soporte | Notas |
|---|---|---|
| Cualquiera con subagentes y archivos | ✅ | Es disciplina, no herramienta |
| Claude Code | ✅ | Prompts originales en `.claude/commands` |
| Codex / Gemini / Cursor / OpenCode | ✅ | Adaptar a skills |

## 12. Ejemplo práctico
- TODO brownfield: "descuento por volumen" con QRSPI completo; guardar todos los artefactos.
- TODO: medir el % de contexto por etapa y comparar con hacerlo en un solo chat.
- TODO: hacer una variante con research no ciego y comparar el sesgo.

## 13. Ventajas
- Cero dependencias; agnóstico.
- Lo mejor para brownfield grande y legacy.
- Artefactos cortos y revisables; mantiene a los humanos pensando.
- Probado a escala.

## 14. Desventajas y críticas
- Resultados inconsistentes fuera de manos expertas (~50 % con malos resultados en RPI original).
- Sin estructura de equipo, producto ni operaciones.
- Sin tooling oficial estable; las técnicas cambian rápido.
- Sin spec viva; costo de tokens premium en modelos frontera (~12k USD/mes para un equipo de 3 en Opus).

## 15. Dónde brilla y dónde evitarlo
| Brilla en | Evitar en |
|---|---|
| Monorepos, legacy, >100k LOC | Greenfield trivial |
| Bugs difíciles, refactors delicados | Autonomía nocturna sin supervisión |
| Devs senior, equipos chicos | Equipos que buscan un proceso "llave en mano" |

## 16. Combinaciones con otros frameworks
- Motor interno de Spec Kit, OpenSpec o AI-DLC (research antes de la spec).
- Superpowers para Implement con TDD.
- GSD Core comparte la filosofía (contexto limpio por fase).
- Ralph solo en fases bien acotadas del outline.

## 17. Preguntas abiertas
- ¿Prompts oficiales de QRSPI publicados?
- ¿Cómo medir el % de contexto en cada agente?
- Relación con "software factories" y los límites de los loops.

## 18. Referencias
### Oficiales / fundacionales
- ACE (ace-fca.md) — https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md
- Why Software Factories Fail (wsff.md) — https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/wsff.md
- 12-Factor Agents — https://github.com/humanlayer/12-factor-agents (atajo https://hlyr.dev/12fa)
- Prompts — https://github.com/humanlayer/humanlayer (`.claude/commands`)
- HumanLayer — https://humanlayer.dev
- PRs de ejemplo en BAML: https://github.com/BoundaryML/baml/pull/2259 · /2357 · /2330
### Artículos y guías
- ZenML, *Evolution from RPI to CRISPY* — https://www.zenml.io/llmops-database/evolution-from-rpi-to-crispy-multi-stage-workflow-for-production-coding-agents
- Heavybit, *What's Missing to Make AI Agents Mainstream?* — https://www.heavybit.com/library/article/whats-missing-to-make-ai-agents-mainstream
- AAIF, *Dex still thinks long-running coding agents stink* (sep-2026) — https://aaif.io/blog/dex-still-thinks-long-running-coding-agents-stink
- AI Engineer, resumen *Context Engineering for Complex Codebases* — https://ai.engineer/talks/context-engineering-for-complex-codebases
- Anthropic, *Effective context engineering for AI agents* — https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
### YouTube / charlas
- *Advanced Context Engineering for Coding Agents* — https://www.youtube.com/watch?v=IS_y40zY-hc (https://hlyr.dev/ace)
- *No Vibes Allowed: Solving Hard Problems in Complex Codebases* — https://www.youtube.com/watch?v=rmvDxxNubIg (https://hlyr.dev/nva)
- *Everything We Got Wrong About RPI* — https://www.youtube.com/watch?v=YwZR6tc7qYg (https://hlyr.dev/qrspi-mlops)
- *Why Software Factories Fail* (AI Engineer World's Fair 2026) — https://ai.engineer/talks/harness-engineering-is-not-enough-why-software-factories-fail
- *The Great Loops Debate* (con Huntley y otros) [VERIFICAR URL]
- Sean Grove, *Specs are the new code* — https://www.youtube.com/watch?v=8rABwKRsec4
- Estudio de Stanford sobre IA y productividad — https://www.youtube.com/watch?v=tbDDYKRFjhk
### Podcasts
- The Pragmatic Engineer, *Context engineering with Dex Horthy* — https://newsletter.pragmaticengineer.com/p/context-engineering-with-dex-horthy
- Dev Interrupted, *Dex Horthy on Ralph, RPI, and escaping the "Dumb Zone"* — https://linearb.io/dev-interrupted/podcast/dex-horthy-humanlayer-rpi-methodology-ralph-loop
- Chroma, *Dex Horthy on Agents and Context Engineering* — https://trychroma.com/interviews/dex-horthy-context-engineering-ep-1
- Heavybit High Leverage Ep. 12, *The Limits of Lights-Out Coding* — https://www.heavybit.com/library/podcasts/high-leverage/ep-12-the-limits-of-lights-out-coding-with-dexter-horthy
- Screaming in the Cloud, *Coding Agents, Chaos, and the Future of Dev Work* — https://sitc.transistor.fm/episodes/coding-agents-chaos-and-the-future-of-dev-work-with-dexter-horthy
- Qodo, *Closing the gap: AI agent hype and production reality* (may-2026) [VERIFICAR URL]
### Libros (base conceptual)
- Michael Feathers, *Working Effectively with Legacy Code*
- John Ousterhout, *A Philosophy of Software Design*

---

## Checklist de completitud
- [ ] Nombre y etapas oficiales de QRSPI confirmados
- [ ] Skills portables escritas y probadas
- [ ] Ejemplo brownfield con métricas de contexto
- [ ] Coherente con el brief (01)
