# 09 · Compound Engineering (Kieran Klaassen / Every)

> **Estado:** 🟡 sketch · **Última verificación:** 2026-11-25 · **Versión documentada:** plugin 3.23.4 (ago-2026) [VERIFICAR actual en GitHub Releases]
> **Nivel SDD:** 1 · Plan-first + memoria acumulada (anclado en conocimiento, no en spec)
> **Familia:** arneses de skills (con [08 · Superpowers](08-superpowers.md) y [10 · GSD Core](10-gsd-core.md))
> **Relacionado:** [01 · Brief](01-brief-frameworks-dev-ia.md)

Convenciones: `TODO:` falta escribir · `[VERIFICAR]` confirmar con fuente primaria · `[OPINIÓN]` valoración propia.

---

## 1. Resumen (TL;DR)
Filosofía y flujo en el que cada unidad de trabajo abarata la siguiente: los aprendizajes de cada feature (errores, decisiones, soluciones) se capturan en `docs/solutions/` y alimentan el trabajo futuro. "80 % planear y revisar, 20 % ejecutar." El plugin oficial trae 38 skills para 14 agentes.

## 2. Origen y contexto
- **Autores:** Kieran Klaassen (GM de Cora, Every) y Trevin Chow (producto).
- **Contexto:** nació construyendo Cora (bandeja de email nativa para agentes) durante ~18 meses; Every lo usa para sacar productos con equipos de una persona.
- **Licencia:** TODO [VERIFICAR].
- **Comunidad:** Compound Engineering Camp (Every) [VERIFICAR fechas].

## 3. Filosofía y principios
- Cada feature debe hacer más fácil el siguiente.
- El loop: brainstorm → plan → work → review → polish → **compound** → repetir.
- **Sándwich humano–IA:** el humano pone el pan (intención y juicio), la IA el relleno.
- **Polish:** calidades que los chequeos automáticos no ven (consistencia visual, responsividad, deleite).
- Recuperar la planeación, el paso olvidado del coding con IA.

## 4. Nivel SDD y ubicación en el espectro
- **Nivel:** 1 · Plan-first (planes en `docs/plans/`).
- **Particularidad:** no mantiene una spec del sistema, sino una base de conocimiento acumulativa (`docs/solutions/`). Es "anchored" en lecciones aprendidas, no en requisitos [OPINIÓN].

## 5. Conceptos y glosario propio
| Término (EN) | Español | Definición |
|---|---|---|
| Compound | Componer / acumular | Capturar aprendizajes para trabajos futuros |
| Solutions | Soluciones | `docs/solutions/`: lecciones reutilizables |
| Polish | Pulido | Calidad de producto más allá de "funciona" |
| LFG | Pipeline autónomo | `/lfg`: pipeline completo sin pausas |
| Review lenses | Lentes de revisión | Revisores especialistas integrados en las skills |

## 6. Instalación y requisitos
| Agente | Instalación |
|---|---|
| Claude Code | `/plugin marketplace add EveryInc/compound-engineering-plugin` → `/plugin install compound-engineering` |
| Cursor | `/add-plugin compound-engineering` |
| Codex CLI | `codex plugin marketplace add EveryInc/compound-engineering-plugin` → `codex plugin add compound-engineering@compound-engineering-plugin` |
| Grok Build | `grok plugin install EveryInc/compound-engineering-plugin` |
| Antigravity | `agy plugin install https://github.com/EveryInc/compound-engineering-plugin` |
| Otros | Cline, Copilot, Devin CLI, Factory Droid, Kimi, Qwen, OpenCode, Pi, oh-my-pi |

Configuración: `.compound-engineering/config.yaml` (`docs_root`).

## 7. Flujo de trabajo paso a paso
| Paso | Comando | Qué hace | Salida |
|---|---|---|---|
| 0 | `/ce-ideate` | Generar ideas | TODO |
| 1 | `/ce-brainstorm` | Preguntas y respuestas → plan de requisitos | Requisitos |
| 2 | `/ce-plan` | Plan listo para implementar | `docs/plans/*` |
| 3 | `/ce-work` | Ejecuta, simplifica, revisa, commit, despliega | Código / PR |
| 4 | `/ce-code-review` | Revisión multiperspectiva | Hallazgos |
| 5 | `/ce-compound` | Captura aprendizajes (5 subagentes en paralelo [VERIFICAR]) | `docs/solutions/*` |
| — | `/lfg` | Pipeline autónomo completo | — |
| — | `/ce-debug`, `/ce-simplify-code`, `/ce-doc-review`, `/ce-setup`, `/ce-help` | Utilidades | — |

- Nota: la sintaxis actual usa guion (`/ce-plan`); en artículos antiguos aparece `/ce:plan`.

## 8. Artefactos y estructura de archivos
```
docs/
├── plans/        # planes por feature
└── solutions/    # aprendizajes categorizados (la "memoria")
.compound-engineering/config.yaml
```

## 9. Personalización y extensiones
- Cambiar `docs_root`.
- TODO: cómo agregar revisores propios o categorías de soluciones.

## 10. Uso en brownfield
- Buen encaje: el valor crece con la vida del código.
- TODO: estrategia de arranque en un repo existente (¿`/ce-setup`?).

## 11. Compatibilidad con agentes
Ver la sección 6 (14 agentes). TODO: verificar qué funciones dependen de subagentes.

## 12. Ejemplo práctico
- TODO: 3 features consecutivos en el sistema de pedidos; medir si el tercero cuesta menos (tiempo/tokens) gracias a `docs/solutions/`.
- TODO: flujo "vibe en v0 → diseño → exportar → Compound Engineering para producción" (Kevin Rose).

## 13. Ventajas
- Único de la lista que mejora con el tiempo dentro del proyecto.
- Revisión multiperspectiva incluida.
- Cuida el pulido de producto.
- Muy portable (14 agentes).

## 14. Desventajas y críticas
- Sin skill de TDD.
- Pesado (~34k palabras de instrucciones, 33–38 skills).
- `docs/solutions/` necesita curación para no volverse ruido.

## 15. Dónde brilla y dónde evitarlo
| Brilla en | Evitar en |
|---|---|
| SaaS de larga vida, equipo pequeño | Scripts de un solo uso |
| Producto que despliega seguido | Proyectos sin continuidad |

## 16. Combinaciones con otros frameworks
- Compound Engineering como columna + Superpowers (TDD) por debajo, declarado en AGENTS.md.
- Spec Kit u OpenSpec como formato de specs + Compound Engineering como memoria.

## 17. Preguntas abiertas
- Licencia y versión actual.
- Cómo se evita que `docs/solutions/` crezca sin control.
- Diferencia entre `/lfg` y Ralph.

## 18. Referencias
### Oficiales
- Repo — https://github.com/EveryInc/compound-engineering-plugin
- Every — https://every.to (ensayo *Compound Engineering: How Every Codes With Agents* [VERIFICAR URL])
- Compound Engineering Camp — https://every.to/events/compound-engineering-camp [VERIFICAR]
### Artículos y guías
- *I Deep-Tested Three Claude Code Harness Skills* — https://zerofuturetech.substack.com/p/i-deep-tested-three-claude-code-harness
- Resumen del reading list (CE, gstack, Superpowers) — https://npomfret.github.io/reading-list-researcher/e502b3549aff92cb.html
- Kevin Rose y Kieran Klaassen (resumen) — https://davidguttman.github.io/every-vibe-code-camp-distilled/13_kevin_kieran.html
### Comparativas
- wotai, *Compound Engineering vs Superpowers* — https://wotai.co/blog/compound-engineering-vs-superpowers
- Ken Huang, *Compound Engineering vs gstack vs …* — https://kenhuangus.substack.com/p/compound-engineering-vs-gstack-vs
- *Superpowers Vs GSD Vs Compound Engineering* — https://theaiengineer.substack.com/p/superpowers-vs-gsd-vs-compound-engineering
### YouTube / charlas
- *The Era of Compound Engineering* (AI Engineer World's Fair 2026) — https://ai.engineer/talks/_ehJyfHg1Vk-era-compound-engineering (YouTube id `_ehJyfHg1Vk` [VERIFICAR])
- *Where Humans Belong in Compound Engineering* (Thesis 2027, nov-2026) [VERIFICAR grabación]
### Podcasts
- Behind the Craft, *How to make Claude Code better every time you use it* — https://pod.wave.co/podcast/behind-the-craft/how-to-make-claude-code-better-every-time-you-use-it-kieran-klaassen
### Libros (base conceptual)
- Hunt & Thomas, *The Pragmatic Programmer*
- Kent Beck, *Tidy First?*

---

## Checklist de completitud
- [ ] Licencia y versión confirmadas
- [ ] Experimento de 3 features ejecutado
- [ ] Coherente con el brief (01)
