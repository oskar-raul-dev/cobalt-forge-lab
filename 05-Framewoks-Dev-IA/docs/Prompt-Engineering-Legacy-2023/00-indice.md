# 00 · Índice — Ingeniería de prompts "legacy" (2022–2025)

> Módulo de documentación **Prompt-Engineering-Legacy-2023** (Cobalt-Forge-Lab / 05-Framewoks-Dev-IA). El nombre de la carpeta conserva "2023" por historia; el alcance real es **2022–2025** (CoT y Zero-shot CoT son de 2022).
> **Propósito:** documentar las técnicas y marcos de prompting de la era 2022–2025 (chat, antes de los agentes de código) y extraer qué sigue sirviendo en el arnés agéntico de 2026 ([App-dev-2026](../App-dev-2026/00-indice.md)).
> **Estado:** ⬜ por crear · 🟡 sketch · 🟠 en progreso · 🟢 completo · 🔵 verificado con ejemplos probados.
> `brief.md` (nota inicial sobre marcos de plantillas) se conserva tal cual como insumo; el brief del módulo es `01-brief-prompt-engineering-legacy.md`.
> **Última revisión del índice:** 2026-11-25 (L01).

## Documentos base

| # | Documento | Estado |
|---|---|---|
| 00 | `00-indice.md` (este archivo) | 🟢 L01 · 2026-11-25 |
| 00 | `00-glosario.md`: glosario general de prompting legacy | ⬜ |
| 01 | `01-brief-prompt-engineering-legacy.md`: brief del módulo | ⬜ |
| — | `img/`: diagramas SVG del módulo (`NN-<slug>.svg`) | ⬜ |
| — | `brief.md`: nota original sobre marcos de plantillas (insumo; no se modifica) | 📎 |

## Capítulos

> Columna **PR**: categoría de *The Prompt Report* (Schulhoff et al., 2024, arXiv:2406.06608, v6) a la que pertenecen las técnicas del capítulo. "—" = no es una técnica de la taxonomía (práctica, guía o marco).

### Parte A · Fundamentos

| # | Archivo | Tema | Época | Estado | PR | Incluye |
|---|---|---|---|---|---|---|
| 02 | `02-anatomia-y-principios.md` | Anatomía de un prompt y principios básicos | 2023–2025 | ⬜ | §1.2 Componentes | Claridad, especificidad, delimitadores, instrucciones positivas, contexto, formato de salida; evolución de las guías oficiales (OpenAI "six strategies" 2023 → guía GPT-4.1 2025; Anthropic; whitepaper de Google/Boonstra 2025) |
| 03 | `03-zero-shot-y-few-shot.md` | Zero-shot, few-shot e in-context learning | 2020→2024 | ⬜ | ICL | Selección y orden de ejemplos (KNN, Vote-K), ejemplos negativos, sesgos; técnicas zero-shot de la taxonomía: Re-reading (RE2), Rephrase and Respond (RaR), System 2 Attention (S2A), Self-Ask |
| 04 | `04-roles-personas-system-prompt.md` | Roles, personas y system prompts | 2023–2024 | ⬜ | ICL · zero-shot | "Actúa como…", system vs user vs developer, jerarquía de instrucciones; role, style y emotion prompting (EmotionPrompt) y su eficacia real |
| 05 | `05-salida-estructurada.md` | Salida estructurada, delimitadores y *answer engineering* | 2023–2024 | ⬜ | Answer Engineering | Etiquetas XML, Markdown, JSON mode, structured outputs con schema, prefill; forma, espacio y extractor de la respuesta |

### Parte B · Técnicas de razonamiento

| # | Archivo | Tema | Época | Estado | PR | Incluye |
|---|---|---|---|---|---|---|
| 06 | `06-chain-of-thought.md` | Chain-of-Thought y Zero-shot CoT | 2022–2023 | ⬜ | Thought Generation | "Let's think step by step", CoT con ejemplos, scratchpad; Auto-CoT, Contrastive CoT, Analogical prompting, Thread-of-Thought |
| 07 | `07-self-consistency-y-ensembles.md` | Self-Consistency y ensambles | 2022–2023 | ⬜ | Ensembling | Votación por mayoría, muestreo múltiple, Universal Self-Consistency, DiVeRSe, paráfrasis de prompts |
| 08 | `08-descomposicion.md` | Descomposición del problema | 2022–2023 | ⬜ | Decomposition | Least-to-Most, Plan-and-Solve, Decomposed Prompting, Step-Back (zero-shot CoT en PR), Skeleton-of-Thought; razonar con código: Program-of-Thoughts, Faithful CoT |
| 09 | `09-tree-y-graph-of-thoughts.md` | Tree / Graph of Thoughts | 2023 | ⬜ | Decomposition | Búsqueda en árbol, evaluación de ramas, costo |
| 10 | `10-autocritica-y-refinamiento.md` | Autocrítica y refinamiento | 2023 | ⬜ | Self-Criticism (+ Agents) | Self-Refine, Self-Verification, Chain-of-Verification, Reflexion, crítica constitucional |

### Parte C · Prompts como sistema

| # | Archivo | Tema | Época | Estado | PR | Incluye |
|---|---|---|---|---|---|---|
| 11 | `11-prompt-chaining.md` | Encadenamiento de prompts y flujos | 2023–2024 | ⬜ | — | Pipelines, router prompts, map-reduce, LangChain/LCEL |
| 12 | `12-react-y-tool-use.md` | ReAct, tool use y function calling | 2022–2024 | ⬜ | Agents | Thought/Action/Observation, MRKL, PAL (código + intérprete), function calling nativo; precursor de MCP |
| 13 | `13-rag-y-contexto-largo.md` | Prompting para RAG y contexto largo | 2023–2025 | ⬜ | Agents · RAG | Grounding, citas, "responde solo con el contexto", manejo de "no sé"; IRCoT, Demonstrate-Search-Predict; generated knowledge; documentos largos (orden, citas previas) |
| 14 | `14-meta-prompting-y-optimizacion.md` | Meta-prompting y optimización automática | 2022–2025 | ⬜ | Prompt Engineering (§2.4) | APE (*Automatic Prompt Engineer*, no confundir con el acrónimo del cap. 18), OPRO, DSPy, Meta Prompting, generadores de prompts de los proveedores |
| 15 | `15-prompts-para-codigo.md` | Prompts para programar (pre-agentes) | 2023–2025 | ⬜ | — | Copilot Chat, plantillas de refactor/tests/review, `.cursorrules`, custom instructions |

### Parte D · Calidad y seguridad

| # | Archivo | Tema | Época | Estado | PR | Incluye |
|---|---|---|---|---|---|---|
| 16 | `16-evaluacion-de-prompts.md` | Evaluación de prompts | 2023–2025 | ⬜ | Evaluation | Golden sets, LLM-as-judge, A/B, regresiones, versionado de prompts |
| 17 | `17-prompt-injection-y-jailbreaks.md` | Prompt injection, jailbreaks y defensas | 2022–2025 | ⬜ | Security | Inyección directa e indirecta, spotlighting, separación de instrucciones y datos |

### Parte E · Marcos de plantillas (acrónimos)

> La mayoría no tiene paper. **Origen** resume lo verificado en L01: ✅ fuente primaria identificada · 🟠 solo fuentes secundarias coherentes · ❌ folclore (letras distintas según la fuente, sin autor).

| # | Archivo | Marcos | Época | Estado | Origen | Para qué |
|---|---|---|---|---|---|---|
| 18 | `18-marcos-minimos-rtf-race-tag.md` | RTF, RACE, TAG, APE-acrónimo (*Action, Purpose, Expectation*), CARE | 2023–2024 | ⬜ | ❌ todos (letras variables) | Pedidos simples; capítulo crítico sobre el folclore de acrónimos |
| 19 | `19-marcos-de-comunicacion-costar-craft.md` | CO-STAR, CRAFT | 2023–2024 | ⬜ | CO-STAR ✅ GovTech Singapur / S. Teo · CRAFT ❌ | Redacción con audiencia, tono y formato |
| 20 | `20-marcos-de-tareas-risen-crispe.md` | RISEN, CRISPE | 2023 | ⬜ | CRISPE ✅ M. Nigh (GitHub, 2023) · RISEN 🟠 K. Balmer | Tareas multietapa y exploración de variaciones |
| 21 | `21-marcos-de-calidad-clear-mpf.md` | CLEAR, MPF | 2023 | ⬜ | CLEAR ✅ L. S. Lo (JAL, 2023) · MPF 🟠 CodeSignal | Revisar, documentar y estandarizar prompts en equipo |

### Parte F · Transición

| # | Archivo | Tema | Época | Estado | PR | Incluye |
|---|---|---|---|---|---|---|
| 22 | `22-modelos-de-razonamiento.md` | Modelos de razonamiento y cómo cambiaron el prompting | 2024–2025 | ⬜ | — | o1/o3, extended thinking, menos CoT manual, presupuestos de razonamiento, seguimiento literal de instrucciones |
| 23 | `23-de-prompt-a-context-engineering.md` | De prompt engineering a context/harness engineering | 2025→2026 | ⬜ | — | Qué murió, qué se transformó, qué sobrevive; puente al módulo App-dev-2026 |

## Fuera de alcance

- **Prompting multilingüe y multimodal** (§3 de *The Prompt Report*: imagen, audio, video): el módulo cubre texto y código.
- **Soft prompts / prompt tuning** y cualquier técnica con actualización de pesos (también excluidos por *The Prompt Report*).
- **Marcos descartados como entrada propia:** ROSES y otros acrónimos sin fuente localizable (ERA, PECRA, RIDE…); se mencionan solo en la tabla de folclore del cap. 18.

## Fuentes del índice (consultadas 2026-11-25)

- Schulhoff et al., *The Prompt Report: A Systematic Survey of Prompt Engineering Techniques*, arXiv:2406.06608 (v1 2024-06-06, v6 2025-02-26). https://arxiv.org/abs/2406.06608
- DAIR.AI, *Prompt Engineering Guide*, lista de técnicas. https://www.promptingguide.ai/techniques
- Anthropic, *Prompt engineering overview*. https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview
- OpenAI, *Prompt engineering* ("six strategies", 2023). https://platform.openai.com/docs/guides/prompt-engineering [VERIFICAR URL: no se abrió directamente; contenido confirmado por fuentes secundarias]
- Boonstra, L. (Google), *Prompt Engineering* (whitepaper, feb. 2025). https://www.kaggle.com/whitepaper-prompt-engineering [VERIFICAR URL]
- Orígenes de la Parte E: Lo (2023) DOI 10.1016/j.acalib.2023.102720 · GovTech Singapur, *Prompt Engineering Playbook* (beta v3, ago. 2023) · Nigh, `mattnigh/ChatGPT3-Free-Prompt-List` (GitHub, ene. 2023) · CodeSignal, lección *Markdown Prompts Framework*.

> **Nota:** la agrupación sigue la taxonomía de *The Prompt Report* donde existe una técnica equivalente, y las guías oficiales de los proveedores para las prácticas sin paper (caps. 02, 11, 15, 16, 22, 23). Los capítulos 09 y 13 se mantienen separados aunque la taxonomía los incluya en categorías más amplias, por su peso histórico.
