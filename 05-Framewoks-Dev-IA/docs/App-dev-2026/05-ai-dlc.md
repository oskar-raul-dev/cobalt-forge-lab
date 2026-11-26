# 05 · AI-DLC — AI-Driven Development Life Cycle (AWS)

> **Estado:** 🟡 sketch · **Última verificación:** 2026-11-25 · **Versión documentada:** aidlc-workflows 2.11.x (v2) [VERIFICAR última]; v1 = reglas Markdown (v1.0.1)
> **Nivel SDD:** 2 · Spec-first (a nivel de equipo)
> **Relacionado:** [01 · Brief](01-brief-frameworks-dev-ia.md)

Convenciones: `TODO:` falta escribir · `[VERIFICAR]` confirmar con fuente primaria · `[OPINIÓN]` valoración propia.

---

## 1. Resumen (TL;DR)
Metodología organizacional de AWS que pone a la IA en el centro del ciclo de vida, con los humanos a cargo de las decisiones críticas. Reorganiza el ágil: *bolts* en lugar de sprints, *Units of Work* en lugar de épicas y rituales grupales (*Mob Elaboration*, *Mob Construction*). El arnés open source `aidlc-workflows` corre en varios agentes.

## 2. Origen y contexto
- **Autor:** Raja SP (Principal Solutions Architect, Developer Transformation, AWS) y su equipo.
- **Fechas:** blog original 31-jul-2025 · arnés open source (reglas para Q/Kiro) · re:Invent 2025 (DVT214, con Anupam Mishra) · v2 con CLI `aidlc` (2026).
- **Base:** más de 100 experimentos con clientes (según re:Invent).
- **Problema:** las dos formas dominantes de usar IA (*AI-assisted* puntual y *AI-autonomous* total) dan resultados mediocres en velocidad y calidad.
- **Licencia:** MIT-0.

## 3. Filosofía y principios
- **AI-powered execution with human oversight:** la IA planea, pregunta y deja las decisiones críticas a los humanos.
- **Dynamic team collaboration:** equipos en espacios compartidos para decidir rápido.
- **Ciclo central:** la IA propone un plan → pregunta → el humano valida → la IA implementa (en cada actividad del ciclo).
- **Contexto persistente:** planes, requisitos y diseño se guardan en el repo.

## 4. Nivel SDD y ubicación en el espectro
- **Nivel:** 2 · Spec-first a nivel de equipo.
- **Justificación:** Inception produce requisitos, historias y unidades antes de construir; los artefactos persisten entre fases.
- **No define** una fusión de deltas en una spec viva [VERIFICAR en la especificación v2].

## 5. Conceptos y glosario propio
| Término (EN) | Español | Definición |
|---|---|---|
| Intent | Intención | Objetivo de negocio que inicia Inception |
| Unit of Work | Unidad de trabajo | Reemplaza a la épica |
| Bolt | Bolt | Ciclo de horas o días; reemplaza al sprint |
| Mob Elaboration | Elaboración grupal | Todo el equipo valida en vivo las preguntas y propuestas de la IA |
| Mob Construction | Construcción grupal | El equipo aclara decisiones técnicas en tiempo real |
| Deployment Unit | Unidad de despliegue | Lo que se lleva a producción [VERIFICAR] |
| Semantic context (brownfield) | Contexto semántico | Construcción de contexto sobre código existente |
| Adaptive workflow | Flujo adaptativo | La profundidad se ajusta al tipo de trabajo |
| Harness (en la CLI `aidlc`) | Arnés destino | Agente destino de la CLI `aidlc` (claude, codex, cursor…). Es un uso específico; el concepto general de [arnés](00-glosario.md#agentes-y-arnes) está en el glosario |

## 6. Instalación y requisitos
**v2 (CLI neutral):**
```bash
curl -fsSL https://github.com/awslabs/aidlc-workflows/releases/latest/download/install.sh | sh
# Windows: irm .../install.ps1 | iex
cd tu-proyecto
aidlc config --harness claude    # kiro, kiro-ide/kiro-v2, codex, cursor, opencode, copilot
aidlc doctor
```
**v1 (reglas Markdown):** descargar `ai-dlc-rules-vX.zip` y copiar `aws-aidlc-rules/` y `aws-aidlc-rule-details/` a la ruta de reglas del agente.

| Agente | Ruta v1 |
|---|---|
| Amazon Q | `.amazonq/rules/` |
| Kiro | `.kiro/steering/` |
| Cursor | `.cursor/rules/` |
| Cline | `.clinerules/` |
| Claude Code | `CLAUDE.md` / `.claude/` |
| GitHub Copilot | `.github/copilot-instructions.md` |

## 7. Flujo de trabajo paso a paso
### Metodología (3 fases)
| Fase | Qué hace la IA | Ritual humano | Salida |
|---|---|---|---|
| Inception | Intención → requisitos, historias, unidades | Mob Elaboration | Requisitos validados |
| Construction | Arquitectura, modelo de dominio, código, tests | Mob Construction | Código + tests |
| Operations | IaC y despliegues con el contexto acumulado | Supervisión | Infraestructura y despliegue |

### Arnés v2
- Fases numeradas: Initialization (0.x), Ideation (1.x), Inception (2.x), Construction (3.x), Operation (4.x).
- 11 agentes: Product, Architect, Developer, Quality, Pipeline & Deploy, Operations… [VERIFICAR lista completa].
- Invocación: `/aidlc <petición>` (Codex: `$aidlc`). En v1: "Using AI-DLC, …".
- TODO: detallar las etapas 0.1–4.7 desde la especificación v2 (PDF).

## 8. Artefactos y estructura de archivos
```
aidlc-docs/            # [VERIFICAR nombre en v2]
├── inception/
├── construction/
└── operations/
# estado, audit trail y artefactos: ver guías "state tracking", "audit trail", "artifacts"
```
- TODO: confirmar la estructura real tras ejecutar `/aidlc`.

## 9. Personalización y extensiones
- Plugins (ejemplo `test-pro`), guía para escribir plugins.
- Personalización de reglas, *sensors* y conocimiento del equipo.
- Modos de interacción y bucle de aprendizaje de reglas [VERIFICAR].
- Scopes, profundidad y estrategia de testing.

## 10. Uso en brownfield
- Construcción de contexto semántico para proyectos brownfield (re:Invent 2025).
- TODO: documentar la etapa de ingeniería inversa / análisis de código existente en v2.

## 11. Compatibilidad con agentes
| Agente | v1 (reglas) | v2 (CLI) | Invocación |
|---|---|---|---|
| Claude Code | ✅ | ✅ | `/aidlc` |
| Codex | ✅ | ✅ | `$aidlc` |
| Cursor | ✅ | ✅ | `/aidlc` |
| GitHub Copilot | ⚠️ parcial | ✅ | `/aidlc` |
| Kiro (CLI/IDE) | ✅ | ✅ | `/aidlc` |
| OpenCode | — | ✅ | `/aidlc` |
| Amazon Q | ✅ | ? | "Using AI-DLC…" |
| Cline | ✅ | ? | |

## 12. Ejemplo práctico
- TODO: simular una Mob Elaboration en solitario (roles PO, dev y QA asumidos por una persona) para el sistema de pedidos; documentar las preguntas que hizo la IA.
- TODO: ejecutar Construction y Operations (IaC mínima: Docker Compose o CDK).
- [OPINIÓN] Para un dev solo el valor está en las fases y artefactos, no en los rituales mob.

## 13. Ventajas
- Único pensado para organizaciones y equipos multidisciplinarios.
- Humano en el centro de las decisiones críticas.
- Cubre Operations (IaC, despliegue).
- La IA aplica estándares de la organización; trazabilidad de requisito a despliegue.
- Agnóstico en la práctica gracias a la v2.

## 14. Desventajas y críticas
- Exige cambio cultural y formación de PM, QA y Ops; si no, la Mob Elaboration se vuelve una demo del dev.
- Saltarse los rituales mob por ser remotos es un antipatrón oficial.
- Mide velocidad, no resultado de mercado: no hay Sprint Review ni retro explícitas (Scrum.org).
- Riesgo de que los juniors no aprendan (caso Brother).
- Material y casos con sesgo hacia AWS (Kiro, Q).
- DORA: más adopción de IA ≠ más estabilidad en la entrega.

## 15. Dónde brilla y dónde evitarlo
| Brilla en | Evitar en |
|---|---|
| Empresas medianas/grandes en transformación | Dev solo o equipos de 2 |
| Equipos multidisciplinarios síncronos | Equipos 100 % asíncronos o en muchas zonas horarias |
| Programas con patrocinio del liderazgo | Organizaciones sin buy-in de producto y QA |

## 16. Combinaciones con otros frameworks
- AI-DLC como proceso + Spec Kit u OpenSpec como formato.
- RPI para el contexto semántico en brownfield [OPINIÓN].
- Ralph para migraciones en Construction, en sandbox.

## 17. Preguntas abiertas
- ¿Cómo se ve la v2 fase por fase? (leer la especificación PDF).
- ¿Cómo se adaptan los rituales mob a equipos remotos?
- ¿Qué métricas propone para medir el éxito?
- Diferencias reales entre v1 y v2.

## 18. Referencias
### Oficiales
- Blog original (jul-2025) — https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/
- *Open-Sourcing Adaptive Workflows for AI-DLC* — https://aws.amazon.com/blogs/devops/open-sourcing-adaptive-workflows-for-ai-driven-development-life-cycle-ai-dlc
- *Building with AI-DLC using Amazon Q Developer* — https://aws.amazon.com/blogs/devops/building-with-ai-dlc-using-amazon-q-developer
- Repo — https://github.com/awslabs/aidlc-workflows
- Docs — https://awslabs.github.io/aidlc-workflows/
- Especificación v2 (PDF) — https://github.com/awslabs/aidlc-workflows/blob/v2/assets/AI-DLC-Workflows-2.0-Specification.pdf
- White paper / method definition — https://prod.d13rzhkk8cj2z0.amplifyapp.com/
- AWS Builder Center — https://builder.aws.com/content/30HonEOE2KzaYCYlyDRdbE25yxz/ai-driven-development-life-cycle-reimagining-software-engineering
- Kiro steering — https://kiro.dev/docs/steering/
### Casos
- Dynata — https://aws.amazon.com/blogs/architecture/how-dynata-scaled-ai-across-the-development-lifecycle-with-ai-dlc-and-kiro-ide/
- Brother Industries (japonés) — https://aws.amazon.com/jp/blogs/news/brother-ai-dlc-interview/
- AWS, *Everyone adopted AI. Why is tech debt still growing?* — https://aws.amazon.com/blogs/migration-and-modernization/everyone-adopted-ai-why-is-tech-debt-still-growing-and-two-moves-to-close-the-gap/
### Artículos, guías y críticas
- Scrum.org, *Why AI Coding Still Needs Human Accountability* (ago-2026) — https://www.scrum.org/resources/blog/aws-ai-dlc-why-ai-coding-still-needs-human-accountability
- Ascendient, *The Bottleneck Just Moved* — https://www.ascendientlearning.com/blog/the-bottleneck-just-moved
- Jellyfish, *What Is the AI-DLC?* — https://jellyfish.co/?p=35231 [VERIFICAR URL canónica]
- *AI-DLC: giving structure to AI-assisted development* — https://dev.to/tharindukn/ai-dlc-giving-structure-to-ai-assisted-development-3k4f
- *AWS AI-DLC: the agentic dev lifecycle that works everywhere* — https://dev.to/devtoaaron/aws-ai-dlc-the-agentic-dev-lifecycle-that-works-everywhere-379j
- AIDLC en readthedocs — https://agentic-ai.readthedocs.io/en/latest/Standards/aidlc/
- DeepWiki — https://deepwiki.com/awslabs/aidlc-workflows
- Antipatrones (japonés) — https://builder.aws.com/content/3IfZkcIT2A6Dwtgg9LV89LNYPdX/aiai-dlc-ai-dlc
- *AI Co-Developing and the SDLC* — https://datamase.substack.com/p/ai-co-developing-and-the-sdlc
### YouTube / charlas
- AWS re:Invent 2025, *Introducing AI driven development lifecycle (AI-DLC)* (DVT214) — canal AWS Events [VERIFICAR URL]
- Transcripción y resumen — https://dev.to/kazuya_dev/aws-reinvent-2025-introducing-ai-driven-development-lifecycle-ai-dlc-dvt214-32b
- DevSparks Hyderabad 2025, keynote de Raja SP — https://yourstory.com/2025/09/devsparks-hyderabad-2025-aws-unveils-ai-driven-development-lifecycle
### Libros (base conceptual)
- Woody Zuill & Kevin Meadows, *Mob Programming: A Whole Team Approach*
- Forsgren, Humble & Kim, *Accelerate*
- Skelton & Pais, *Team Topologies*

---

## Checklist de completitud
- [ ] Etapas v2 detalladas desde la especificación
- [ ] Estructura de artefactos confirmada ejecutando la herramienta
- [ ] URL del video de re:Invent confirmada
- [ ] Coherente con el brief (01)
