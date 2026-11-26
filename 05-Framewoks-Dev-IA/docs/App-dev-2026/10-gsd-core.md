# 10 · GSD Core (antes Get Shit Done)

> **Estado:** 🟡 sketch · **Última verificación:** 2026-11-25 · **Versión documentada:** GSD Core 1.7.0 [VERIFICAR]
> **Nivel SDD:** 2 · Spec-first (por milestone y fases, con estado persistente)
> **Familia:** arneses de skills (con [08 · Superpowers](08-superpowers.md) y [09 · Compound Engineering](09-compound-engineering.md))
> **Relacionado:** [01 · Brief](01-brief-frameworks-dev-ia.md) · [06 · RPI](06-rpi-qrspi.md)

Convenciones: `TODO:` falta escribir · `[VERIFICAR]` confirmar con fuente primaria · `[OPINIÓN]` valoración propia.

---

## 1. Resumen (TL;DR)
Framework ligero de context engineering y SDD. Ataca el *context rot*: la investigación, la planeación y la ejecución pesadas corren en subagentes con contexto limpio, y el estado vive en archivos. Loop por fase: Discuss → Plan → Execute (oleadas paralelas) → Verify → Ship. Lema: "Git. Ship. Done."

## 2. Origen y contexto
- **Original:** `gsd-build/get-shit-done` (~64k estrellas, MIT), hoy **archivado y en solo lectura**.
- **Continuación:** `open-gsd/gsd-core` (~11.8k estrellas, MIT). Paquete npm `@opengsd/gsd-core`. Port original para OpenCode: `gsd-opencode`.
- **Motivo de la migración:** TODO [VERIFICAR].
- Forks: `open-gsd` / `get-shit-done-redux` [VERIFICAR relación].
- **Posicionamiento:** "SDD sin el teatro empresarial".

## 3. Filosofía y principios
- El contexto es el recurso escaso; la sesión principal debe estar liviana.
- Artefactos estructurados que llevan el estado entre sesiones.
- Verificar antes de dar una fase por terminada.
- Planes que quepan en una ventana de contexto limpia.

## 4. Nivel SDD y ubicación en el espectro
- **Nivel:** 2 · Spec-first por milestone.
- **Justificación:** proyecto, requisitos y roadmap se definen antes; cada fase se planea y se ejecuta; `STATE.md` mantiene la continuidad.
- No fusiona una spec viva del sistema [VERIFICAR].

## 5. Conceptos y glosario propio
| Término (EN) | Español | Definición |
|---|---|---|
| Phase | Fase | Unidad del loop Discuss→Ship |
| Wave | Oleada | Grupo de planes ejecutados en paralelo |
| Executor | Ejecutor | Subagente con 200k tokens de contexto limpio |
| STATE.md / CONTEXT.md | Estado / contexto | Artefactos persistentes entre sesiones |
| Fix plan | Plan de arreglos | Generado por Verify |

Términos generales usados en este capítulo (definidos en el [glosario general](00-glosario.md)): [degradación del contexto](00-glosario.md#contexto-y-prompts) · [hito](00-glosario.md#procesos-roles-y-rituales) · [subagente](00-glosario.md#agentes-y-arnes).

## 6. Instalación y requisitos
```bash
npx @opengsd/gsd-core@latest   # pregunta el runtime y si la instalación es global o local
```
- Usar el instalador; no copiar `agents/` ni `commands/` a mano.
- Hay una guía aparte para runtimes sin Node.js.
- Original (histórico): `npx get-shit-done-cc` [VERIFICAR].

## 7. Flujo de trabajo paso a paso
| Paso | Comando | Qué hace | Control humano |
|---|---|---|---|
| Inicio | `/gsd-new-project` · `/gsd-onboard` | Greenfield · código existente | Sí |
| 1 | Discuss | Captura decisiones de implementación | **Sí** |
| 2 | Plan | Investiga, descompone, comprueba que quepa en contexto limpio | Sí |
| 3 | Execute | Oleadas paralelas, ejecutores con contexto limpio | No |
| 4 | Verify | Revisa, diagnostica, genera fix plans | **Sí** |
| 5 | Ship | PR, archiva la fase, pasa a la siguiente | Sí |

- TODO: nombres exactos de los comandos de cada fase (en el original: `/gsd:discuss-phase`, `/gsd:plan-phase`, `/gsd:execute-phase`, `/gsd:verify-work`, `/gsd:quick` [VERIFICAR en GSD Core]).

## 8. Artefactos y estructura de archivos
```
STATE.md
CONTEXT.md
# original: .planning/ con PROJECT.md, REQUIREMENTS.md, ROADMAP.md [VERIFICAR en GSD Core]
```

## 9. Personalización y extensiones
- ~29 skills y 12 agentes personalizados (original).
- SDK y hooks en el repo original.
- TODO: documentación "What's new" de GSD Core.

## 10. Uso en brownfield
- `/gsd-onboard` para código existente.
- TODO: probar y documentar.

## 11. Compatibilidad con agentes
| Runtime | Soporte |
|---|---|
| Claude Code | ✅ |
| OpenCode | ✅ (+ port `gsd-opencode`) |
| Codex | ✅ |
| Copilot | ✅ |
| Cursor | ✅ |
| Windsurf | ✅ |
| Antigravity CLI, Kimi CLI, Kilo | ✅ |

## 12. Ejemplo práctico
- TODO: el sistema de inventario como milestone de 3 fases (catálogo, pedidos, descuentos); registrar las oleadas paralelas y su costo.

## 13. Ventajas
- RPI + SDD en un solo paquete con roadmap.
- Paralelismo con contexto limpio.
- Verify integrado con fix plans.
- Muy portable.

## 14. Desventajas y críticas
- Comunidad fragmentada tras la migración; documentación dispersa.
- Muchas piezas.
- El paralelismo multiplica el gasto de tokens.

## 15. Dónde brilla y dónde evitarlo
| Brilla en | Evitar en |
|---|---|
| Dev solo construyendo un producto por milestones | Cambios aislados pequeños |
| Greenfield con roadmap claro | Equipos que necesitan un proceso con roles |

## 16. Combinaciones con otros frameworks
- BMAD (planear) → GSD Core (construir).
- Superpowers para TDD dentro de Execute [OPINIÓN].

## 17. Preguntas abiertas
- Motivo de la migración y estado de los forks.
- Comandos exactos de GSD Core 1.7.
- ¿Existe un modo "quick" en Core?

## 18. Referencias
### Oficiales
- GSD Core — https://github.com/open-gsd/gsd-core
- Original (archivado) — https://github.com/gsd-build/get-shit-done
- npm — https://www.npmjs.com/package/@opengsd/gsd-core [VERIFICAR]
### Artículos y comparativas
- *Superpowers Vs GSD Vs Compound Engineering* — https://theaiengineer.substack.com/p/superpowers-vs-gsd-vs-compound-engineering
- ShipOrSkip, GSD vs Superpowers — https://shiporskip.io/compare/get-shit-done-gsd-meta-prompting-context-engineering-claude-code-2026-vs-superpowers-obra-jesse-vincent-agentic-dev-7-step-worktree-methodology-2026
- SkillsLLM, get-shit-done vs superpowers — https://skillsllm.com/compare/get-shit-done-vs-superpowers
- *A Claude Code Skills Stack* — https://coderlegion.com/14180/a-claude-code-skills-stack-how-to-combine-superpowers-gstack-and-gsd-without-the-chaos
- Paper *From Prompt to Process* (incluye GSD) — https://www.alphaxiv.org/abs/2606.04967
- Mapa de 30+ frameworks SDD — https://medium.com/@visrow/spec-driven-development-is-eating-software-engineering-a-map-of-30-agentic-coding-frameworks-6ac0b5e2b484
### YouTube / charlas
- TODO: buscar walkthroughs de GSD y GSD Core.
### Libros (base conceptual)
- Ver [06 · RPI](06-rpi-qrspi.md)

---

## Checklist de completitud
- [ ] Comandos y artefactos de GSD Core confirmados
- [ ] Ejemplo de milestone ejecutado
- [ ] Coherente con el brief (01)
