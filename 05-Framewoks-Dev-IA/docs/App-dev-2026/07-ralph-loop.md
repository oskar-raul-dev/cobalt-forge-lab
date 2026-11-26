# 07 · Ralph loop / loop engineering

> **Estado:** 🟡 sketch · **Última verificación:** 2026-11-25 · **Versión documentada:** técnica (bash loop) + plugin oficial de Claude Code (dic-2025)
> **Nivel SDD:** 2 · Spec-first con specs fijas + tests como spec ejecutable (en greenfield se acerca en espíritu al nivel 4)
> **Relacionado:** [01 · Brief](01-brief-frameworks-dev-ia.md) · [06 · RPI](06-rpi-qrspi.md)

Convenciones: `TODO:` falta escribir · `[VERIFICAR]` confirmar con fuente primaria · `[OPINIÓN]` valoración propia.

---

## 1. Resumen (TL;DR)
Correr un agente de código en un bucle (`while :; do cat PROMPT.md | agente; done`) contra verificación externa (tests, tipos, lint) hasta que el trabajo pase. Quien declara "terminado" es el arnés, no el modelo. *Loop engineering* generaliza la idea: diseñar el sistema externo que alimenta al agente.

## 2. Origen y contexto
- **Autor:** Geoffrey Huntley. El nombre viene de Ralph Wiggum (Los Simpson).
- **Línea de tiempo:** 19-jun-2025 demo en un meetup de SF · jul-2025 post original · ago-2025 hackathon de YC (6 repos en una noche) · sep-2025 lenguaje CURSED · oct-2025 Horthy en Claude Code Anonymous · **dic-2025 plugin oficial de Anthropic** · ene-2026 conversación Huntley vs Horthy (bash vs plugin).
- **Problema:** prompt → esperar → leer diff → repetir no escala; la generación es barata y la verificación es el cuello de botella.

## 3. Filosofía y principios
- "Determinísticamente malo en un mundo no determinístico": los defectos se identifican y se corrigen afinando el prompt.
- **Una cosa por vuelta.**
- El contexto principal coordina; los subagentes hacen el trabajo caro.
- **Backpressure:** la verificación automática es la que dirige.
- Diseño monolítico: un proceso, un repo; evitar multiagente.
- "Afinarlo como una guitarra": las fallas reflejan la habilidad del operador.

## 4. Nivel SDD y ubicación en el espectro
- **Nivel:** 2 · Spec-first: `specs/` se escribe antes y no cambia durante el loop.
- Los tests funcionan como **spec ejecutable**.
- En greenfield el código es reemplazable (`git reset --hard` y volver a correr), lo que lo acerca en espíritu a spec-as-source [OPINIÓN].

## 5. Conceptos y glosario propio
| Término (EN) | Español | Definición |
|---|---|---|
| Loop | Bucle | Repetición del mismo prompt con contexto limpio |
| fix_plan.md | Plan de arreglos | Lista de pendientes priorizada que el agente mantiene |
| Stop hook | Hook de parada | Implementación del plugin: al intentar terminar, revisa condiciones y reinyecta el prompt |
| Completion condition | Condición de cierre | Verificación ejecutable que define "listo" |
| Gutter | Canaleta | Falla autorregresiva: el agente cae en un mal patrón y no sale |
| Compound loops | Bucles compuestos | Análisis → planeación → ejecución encadenados |

Términos generales usados en este capítulo (definidos en el [glosario general](00-glosario.md)): [contrapresión](00-glosario.md#barandas-y-seguridad) · [relleno](00-glosario.md#agentes-y-arnes) · [ingeniería de bucles](00-glosario.md#agentes-y-arnes) · [sandbox](00-glosario.md#barandas-y-seguridad) · [saltarse permisos](00-glosario.md#barandas-y-seguridad).

## 6. Instalación y requisitos
- **Bash:** cualquier agente con CLI no interactiva (`claude -p`, `codex exec`, `gemini -p`, `amp`…).
- **Plugin de Claude Code:** Ralph Wiggum (Stop Hook) [VERIFICAR comando de instalación].
- **Obligatorio:** sandbox (contenedor o VM sin credenciales de producción), límite de iteraciones y de tiempo.
```bash
#!/usr/bin/env bash
MAX_ITERS=30
for i in $(seq 1 $MAX_ITERS); do
  cat PROMPT.md | claude -p
  if ./ci.sh; then
    git add -A && git commit -m "ralph: iter $i ✅"
    grep -q "^- \[ \]" fix_plan.md || { echo "DONE"; exit 0; }
  fi
done
echo "Límite alcanzado"; exit 1
```

## 7. Flujo de trabajo paso a paso
| Paso | Qué | Control humano |
|---|---|---|
| 1 | Conversación larga para generar `specs/` (una por archivo) | **Sí** |
| 2 | Escribir `PROMPT.md`, `AGENT.md` y `fix_plan.md` inicial | Sí |
| 3 | Preparar el backpressure (`ci.sh`: tests, tipos, lint, seguridad) | Sí |
| 4 | Correr el loop en sandbox con límites | No |
| 5 | Observar logs y diffs; afinar el prompt cuando se desvía | Sí (asíncrono) |
| 6 | Revisión final y merge | **Sí** |

**Reglas del prompt:** buscar antes de asumir que algo no existe; sin placeholders; una tarea por vuelta; correr solo los tests de lo que cambió; documentar en cada test por qué existe; registrar bugs en `fix_plan.md`; commit cuando los tests pasan.

## 8. Artefactos y estructura de archivos
```
PROMPT.md        # se inyecta en cada vuelta
AGENT.md         # cómo compilar y correr (breve, el agente lo mantiene)
specs/*.md       # una spec por archivo, fijas durante el loop
fix_plan.md      # pendientes priorizados
ci.sh            # backpressure
logs/            # diffs y salidas por iteración
```

## 9. Personalización y extensiones
- **Plugin vs bash:** el plugin facilita adoptarlo pero esconde la mecánica en un archivo de estado; el bash obliga a diseñar el arnés.
- **Verificador extra:** otro modelo como revisor, pero que vea la salida cruda, no resúmenes.
- **Loops compuestos:** p. ej. Compound Product (análisis → PRD/tareas → ejecución).
- **Variantes:** BMad Loop (BMAD), "ralph offspring" que corren en cron [VERIFICAR].

## 10. Uso en brownfield
- Huntley **no** lo usaría en código existente.
- Excepción razonable: tareas mecánicas con cobertura de tests alta (migraciones, upgrades) [OPINIÓN].
- Ante conflictos de merge, volver a correr sobre el main actual en vez de hacer rebase.

## 11. Compatibilidad con agentes
| Agente | Soporte | Cómo |
|---|---|---|
| Claude Code | ✅ | `claude -p` o plugin |
| Codex | ✅ | `codex exec` |
| Gemini CLI | ✅ | `gemini -p` |
| Amp | ✅ | usado por Huntley |
| Cualquiera con modo headless | ✅ | |

## 12. Ejemplo práctico
- TODO: migrar los tests del sistema de inventario de Jest a Vitest (o JS → TS) con Ralph en Docker; medir iteraciones, costo y calidad.
- TODO: greenfield: generar el módulo de pedidos desde `specs/` y comparar con el resultado de Spec Kit.
- TODO: documentar al menos 3 ajustes de `PROMPT.md` ("afinar la guitarra").

## 13. Ventajas
- Simplicidad extrema; agnóstico.
- Trabajo nocturno sin supervisión.
- Estado en archivos: contexto limpio en cada vuelta.
- ROI potencial alto (contrato de 50k USD como MVP por 297 USD, como anécdota).
- Capaz de proyectos ambiciosos (CURSED).

## 14. Desventajas y críticas
- Rompe el código, duplica, deja placeholders o se desvía.
- Las fallas suelen ser del arnés: specs vagas, tests flojos, cierre ambiguo.
- `--dangerously-skip-permissions` → riesgo de seguridad.
- Errores que se acumulan en cambios grandes; scope creep.
- Horthy: los agentes sin supervisión degradan la arquitectura ("spaghetti radiactivo"); HumanLayer archivó un código de 30–40k LOC.

## 15. Dónde brilla y dónde evitarlo
| Brilla en | Evitar en |
|---|---|
| Migraciones, refactors, limpieza, conformidad | Trabajo exploratorio |
| Arranque de greenfield (~90 %) | Código existente sin tests |
| Tareas con criterio de éxito mecánico | Acceso a producción; UI/UX |

## 16. Combinaciones con otros frameworks
- Specs con OpenSpec o Spec Kit → research y design con RPI → Ralph solo para fases acotadas.
- BMad Loop dentro de BMAD.
- AI-DLC: migraciones en Construction.

## 17. Preguntas abiertas
- ¿Cuándo conviene el plugin y cuándo el bash?
- Métricas de calidad del código generado tras N iteraciones.
- Límites seguros de presupuesto (tokens/hora).
- ¿Qué concluyó el "Great Loops Debate"?

## 18. Referencias
### Fundacionales (Geoffrey Huntley)
- *Ralph Wiggum as a "software engineer"* — https://ghuntley.com/ralph/
- Specs — https://ghuntley.com/specs/
- Stdlib — https://ghuntley.com/stdlib/
- Subagents — https://ghuntley.com/subagents/
- Gutter — https://ghuntley.com/gutter/
- Mirrors — https://ghuntley.com/mirrors/
- Repomirror field report — https://github.com/repomirrorhq/repomirror/blob/main/repomirror.md
### Artículos y guías
- DreamHost, *The Ralph Wiggum Loop, From First Principles* — https://www.dreamhost.com/blog/ralph-wiggum/
- Addy Osmani, *Self-Improving Coding Agents* (ene-2026) — https://addyosmani.com/blog/self-improving-agents/
- Tessl, *Loop Engineering* — https://tessl.io/patterns/agentic-development-workflow/loop-engineering/
- LinearB, *Ralph loops make agentic coding reliable with ruthless context resets* — https://linearb.io/blog/dex-horthy-humanlayer-rpi-methodology-ralph-loop
- VentureBeat, *How Ralph Wiggum went from The Simpsons to the biggest name in AI* — https://venturebeat.com/technology/how-ralph-wiggum-went-from-the-simpsons-to-the-biggest-name-in-ai-right-now
- qte77, *An Open Agentic Coding Harness — the Loop, the Plugins, the Senses, the Eval* — https://qte77.github.io/open-agentic-coding-harness/
- Paper *Agentic Harness Engineering* (arXiv 2604.25850) — https://arxiv.org/abs/2604.25850
- Paper *AI Harness Engineering: A Runtime Substrate* (arXiv 2605.13357) — https://arxiv.org/abs/2605.13357
### YouTube / charlas
- Huntley y Horthy, bash loop vs plugin (ene-2026) [VERIFICAR URL]
- *The Great Loops Debate* (AI Engineer World's Fair 2026) [VERIFICAR URL]
### Podcasts
- Dev Interrupted, *Dex Horthy on Ralph, RPI, and escaping the "Dumb Zone"* — https://linearb.io/dev-interrupted/podcast/dex-horthy-humanlayer-rpi-methodology-ralph-loop
### Libros (base conceptual)
- Freeman & Pryce, *Growing Object-Oriented Software, Guided by Tests*
- Winters, Manshreck & Wright, *Software Engineering at Google* (capítulos de testing y LSC)

---

## Checklist de completitud
- [ ] Comando de instalación del plugin confirmado
- [ ] Experimento de migración ejecutado en sandbox
- [ ] URLs de videos confirmadas
- [ ] Coherente con el brief (01)
