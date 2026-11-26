# 08 · Superpowers (Jesse Vincent / obra)

> **Estado:** 🟡 sketch · **Última verificación:** 2026-11-25 · **Versión documentada:** 6.3.0 (ago-2026) [VERIFICAR la actual en `.version-bump.json` / RELEASE-NOTES]
> **Nivel SDD:** 1 · Plan-first
> **Familia:** arneses de skills (con [09 · Compound Engineering](09-compound-engineering.md) y [10 · GSD Core](10-gsd-core.md))
> **Relacionado:** [01 · Brief](01-brief-frameworks-dev-ia.md)

Convenciones: `TODO:` falta escribir · `[VERIFICAR]` confirmar con fuente primaria · `[OPINIÓN]` valoración propia.

---

## 1. Resumen (TL;DR)
Conjunto de skills combinables y metodología de desarrollo para agentes de código. Impone disciplina de ingeniería senior: brainstorming → worktree → plan → ejecución con subagentes → **TDD obligatorio** → code review que bloquea → cierre de rama. Siempre se detiene en una aprobación humana antes de implementar.

## 2. Origen y contexto
- **Autor:** Jesse Vincent (Prime Radiant). Antes creó Request Tracker, lideró Perl 5 y cofundó Keyboardio y VaccinateCA.
- **Lanzamiento:** octubre de 2025 (post *Superpowers: How I'm using coding agents in October 2025*) [VERIFICAR fecha y URL].
- **Métricas:** ~321k estrellas (nov-2026) [VERIFICAR]; el plugin de Claude Code más usado.
- **Licencia:** MIT.

## 3. Filosofía y principios
- TDD y red-green-refactor siempre.
- Sistemático en vez de ad hoc.
- Reducir complejidad.
- **"Evidence over claims"** (evidencia sobre afirmaciones).
- La ceremonia escala, la aprobación no: todo camino se detiene en una decisión humana explícita antes de implementar.
- "Una suite en verde no prueba que el feature construido sea el correcto."
- El humano es dueño de las decisiones irreversibles.

## 4. Nivel SDD y ubicación en el espectro
- **Nivel:** 1 · Plan-first.
- **Justificación:** documento de diseño + plan por tareas para cada trabajo; no mantiene una spec del sistema.
- Se combina con Spec Kit u OpenSpec para subir de nivel [OPINIÓN].

## 5. Conceptos y glosario propio
| Skill | Categoría | Qué hace |
|---|---|---|
| brainstorming | Colaboración | Aclara qué y por qué; diseño escrito en trabajos grandes |
| using-git-worktrees | Colaboración | Rama aislada + setup + baseline de tests limpio |
| writing-plans | Colaboración | Tareas pequeñas con rutas, código y verificación |
| executing-plans | Colaboración | Ejecuta en línea con una revisión final (más barato) |
| subagent-driven-development | Colaboración | Subagente nuevo por tarea con revisión cada vez (más riguroso) |
| dispatching-parallel-agents | Colaboración | Trabajo en paralelo |
| test-driven-development | Testing | Ciclo red-green-refactor obligatorio |
| systematic-debugging | Debugging | Depuración metódica |
| verification-before-completion | Debugging | Verificar antes de declarar terminado |
| requesting-code-review / receiving-code-review | Colaboración | Revisión contra el plan; lo crítico bloquea |
| finishing-a-development-branch | Colaboración | Tests, merge/PR/conservar/descartar, limpieza |
| writing-skills / using-superpowers | Meta | Crear skills propias; arranque |
| diagnosing-superpowers | Debugging | Diagnóstico del propio sistema [VERIFICAR] |

## 6. Instalación y requisitos
| Agente | Instalación |
|---|---|
| Claude Code | `/plugin install superpowers@claude-plugins-official` |
| Codex | App: Plugins → Superpowers · CLI: `/plugins` → buscar "superpowers" |
| Gemini CLI | `gemini extensions install https://github.com/obra/superpowers` |
| OpenCode | Seguir `.opencode/INSTALL.md` del repo |
| Cursor | `/add-plugin superpowers` |
| Otros | Antigravity, Devin CLI, Factory Droid, Copilot CLI, Grok Build, Kimi Code, Pi, Qwen Code, Hermes, Muse |

## 7. Flujo de trabajo paso a paso
| Paso | Skill | Salida | Control humano |
|---|---|---|---|
| 1 | brainstorming | Spec/diseño aprobado | **Sí (gate)** |
| 2 | using-git-worktrees | Rama aislada, tests en verde | — |
| 3 | writing-plans | Plan con tareas exactas | Sí |
| 4 | subagent-driven-development / executing-plans | Código por tarea | Revisión entre tareas |
| 5 | test-driven-development | Tests primero | — |
| 6 | requesting-code-review | Hallazgos; lo crítico bloquea | Sí |
| 7 | finishing-a-development-branch | Merge, PR o descarte | **Sí** |

- La ruta se "mejora" (más ceremonia) si aparece complejidad oculta; nunca se degrada (v6.3.0).

## 8. Artefactos y estructura de archivos
```
docs/plans/ o docs/superpowers/   # [VERIFICAR ruta de los documentos de diseño y planes]
.worktrees/ o ../<repo>-<branch>  # [VERIFICAR convención de worktrees]
```

## 9. Personalización y extensiones
- `writing-skills` para crear las propias.
- Repo complementario `obra/superpowers-skills` [VERIFICAR relación actual].
- Forks y derivados: "Claude Code on Steroids" (GadaaLabs), puentes con Spec Kit.

## 10. Uso en brownfield
- Thoughtworks: mejor en greenfield que en brownfield.
- [OPINIÓN] Combinar con RPI (research) u OpenSpec (delta) para código existente.

## 11. Compatibilidad con agentes
Ver la tabla de la sección 6. TODO: verificar diferencias de comportamiento (subagentes, worktrees) entre agentes.

## 12. Ejemplo práctico
- TODO: módulo de pedidos con TDD estricto; contar ciclos red-green y tiempo.
- TODO: comparar `executing-plans` vs `subagent-driven-development` en costo y calidad.

## 13. Ventajas
- Disciplina senior por defecto; TDD real.
- Ligero (14 skills, ~20.7k palabras de instrucciones).
- Muy portable; en el marketplace oficial de Anthropic.
- Gate humano explícito.

## 14. Desventajas y críticas
- Sesgo greenfield.
- TDD estricto lento en UI o prototipos.
- Sin memoria entre features ni spec viva.
- "Las estrellas son un mal indicador: no todas las skills encajan en tu flujo."

## 15. Dónde brilla y dónde evitarlo
| Brilla en | Evitar en |
|---|---|
| Lógica de negocio, backends, librerías | Prototipos visuales |
| Dev solo / equipo pequeño que quiere calidad | Exploración rápida |

## 16. Combinaciones con otros frameworks
- Compound Engineering como columna + Superpowers por debajo (vía AGENTS.md).
- Spec Kit u OpenSpec (qué) + Superpowers (cómo).
- Stack con gstack y GSD (artículo *A Claude Code Skills Stack*).

## 17. Preguntas abiertas
- Versión actual y cambios desde la 6.3.0.
- Rutas exactas de los artefactos.
- Comportamiento en agentes sin subagentes nativos.

## 18. Referencias
### Oficiales
- Repo — https://github.com/obra/superpowers
- Skills — https://github.com/obra/superpowers-skills
- Blog del autor — https://blog.fsck.com (post de lanzamiento oct-2025 [VERIFICAR URL])
- Charlas del autor — https://blog.fsck.com/talks/
### Artículos y guías
- UT Austin 378AC, slides de Superpowers — https://www.cs.utexas.edu/~witchel/378AC/lectures/946_superpowers-slides.pdf
- *A Claude Code Skills Stack: Superpowers, gstack, and GSD* — https://coderlegion.com/14180/a-claude-code-skills-stack-how-to-combine-superpowers-gstack-and-gsd-without-the-chaos
- *I Deep-Tested Three Claude Code Harness Skills* — https://zerofuturetech.substack.com/p/i-deep-tested-three-claude-code-harness
### Comparativas
- *Superpowers Vs GSD Vs Compound Engineering* — https://theaiengineer.substack.com/p/superpowers-vs-gsd-vs-compound-engineering
- wotai, *Compound Engineering vs Superpowers* (ago-2026) — https://wotai.co/blog/compound-engineering-vs-superpowers
- Ken Huang, *Compound Engineering vs gstack vs Autoresearch vs Superpowers* — https://kenhuangus.substack.com/p/compound-engineering-vs-gstack-vs
- ShipOrSkip, GSD vs Superpowers — https://shiporskip.io/compare/get-shit-done-gsd-meta-prompting-context-engineering-claude-code-2026-vs-obra-superpowers-agentic-skills-framework-tdd-claude-cursor-coding-2026
- Thoughtworks Radar (mención en OpenSpec) — https://www.thoughtworks.com/radar/tools/openspec
### YouTube / charlas / video
- O'Reilly, *Give Your Agents Superpowers: Jesse Vincent Live with Tim O'Reilly* (sep-2026) — https://www.oreilly.com/videos/give-your-agents/0642572443245/
- MLOps Community, *The Creator of Superpowers: Why Real Agentic Engineering Beats Vibe Coding* — https://home.mlops.community/public/videos/the-creator-of-superpowers-why-real-agentic-engineering-beats-vibe-coding
### Podcasts
- MLOps Community #373 — https://podbay.fm/p/mlopscommunity/e/1777050000
- Open Source Ready (Heavybit) Ep. 36 — https://www.heavybit.com/library/podcasts/open-source-ready/ep-36-managing-ai-coding-agents-with-jesse-vincent
### Libros (base conceptual)
- Kent Beck, *Test-Driven Development: By Example*
- Freeman & Pryce, *Growing Object-Oriented Software, Guided by Tests*

---

## Checklist de completitud
- [ ] Versión y rutas de artefactos confirmadas
- [ ] Ejemplo TDD ejecutado
- [ ] Coherente con el brief (01)
