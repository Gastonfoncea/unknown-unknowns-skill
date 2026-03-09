# Unknown Unknowns Detector

> 🇬🇧 [Read in English](README.md)

Una skill para Claude Code (y herramientas de AI coding compatibles) que detecta los riesgos, gaps y puntos ciegos que no sabés que tenés — antes de que se conviertan en problemas.

## El Problema

Cuando estás construyendo algo, tu conocimiento se divide en tres:

- **Cosas que sabés que sabés** — Lo que ya manejás y tenés resuelto.
- **Cosas que sabés que no sabés** — Lo que sabés que tenés que investigar.
- **Cosas que no sabés que no sabés** — Lo que ni se te cruza por la cabeza que deberías estar pensando.

La última es la más peligrosa. No podés googlear lo que no sabés que existe. Esta skill cruza lo que estás construyendo contra conocimiento profundo de múltiples dominios y te muestra los gaps que no podés ver desde donde estás parado.

Funciona tanto si sos un developer experimentado al que se le escapan cosas fuera de su dominio, como si estás vibecodenado tu primera app real.

## Qué Hace

1. **Detecta tu modo** — ¿Tenés código o es solo una idea? Si hay código, escanea el proyecto primero y encuentra problemas automáticamente. Si es una idea, hace preguntas específicas.

2. **Escanea tu codebase (si existe)** — Lee la estructura del proyecto, dependencias, config, capa de networking y modelos de datos. Encuentra problemas sin que tengas que describirlos.

3. **Pregunta solo lo que no puede inferir** — Nada de cuestionarios genéricos. Las preguntas son específicas a lo que estás construyendo y en qué etapa estás.

4. **Analiza las dimensiones que vos elegís:**
   - Seguridad y vulnerabilidades
   - Arquitectura y escalabilidad
   - Legal / compliance / regulatorio
   - Costos y operaciones
   - Producto y negocio (blind spots)
   - Riesgos específicos de AI (agentes, LLMs)
   - UX y accesibilidad
   - Manejo de datos y privacidad

5. **Entrega hallazgos priorizados:**
   - 🔴 **Crítico** — Puede matar tu proyecto o causar daño serio
   - 🟡 **Importante** — Te va a doler si lo ignorás
   - 🟢 **Para tener en cuenta** — Para cuando escales

6. **Cierra con un top-5 accionable** — No es un resumen de los hallazgos, sino "si no hacés nada más, hacé estas 5 cosas."

## Instalación

git clone https://github.com/gastonfoncea/unknown-unknowns-skill.git

### Global (todos los proyectos)
cp -r unknown-unknowns-skill ~/.claude/skills/unknown-unknowns

### O para un proyecto específico
cp -r unknown-unknowns-skill .claude/skills/unknown-unknowns

## Uso

La skill se activa automáticamente cuando decís cosas como:

- "¿Qué me estoy perdiendo en este proyecto?"
- "Revisá mi arquitectura por blind spots"
- "¿Cuáles son los unknown unknowns acá?"
- "Auditá mi proyecto antes de lanzar"
- "¿Qué puede salir mal?"

También podés ser directo:

- "Corré la skill de unknown unknowns en este proyecto"
- "Analizá los blind spots de seguridad y arquitectura"

## Ejemplo

Un ejemplo condensado analizando una macOS app de notas con integración de AI:

**Discovery:**
> "¿Qué estás construyendo, en qué stage, y qué dimensiones analizo?"

**Usuario:** "Una macOS app en Swift que captura notas rápidas con un shortcut y usa OpenAI para clasificarlas. Pre-launch, modelo freemium. Analizá arquitectura."

**Output del análisis (abreviado):**

> 🔴 **Crítico: La API key vive en el cliente**
>
> Tu app se conecta directo a OpenAI — la key tiene que estar en algún lado de la máquina del usuario. Cualquiera puede extraerla del binario y usarla a tu costa. Necesitás un backend proxy.
>
> 🟡 **Importante: Sin abstracción sobre el provider de AI**
>
> Llamadas a OpenAI dispersas por el codebase significa que agregar Claude o un modelo local después requiere reescribir la mitad de la app. Creá un protocolo `AIProvider` ahora.
>
> 🟢 **Para tener en cuenta: Mecanismo de auto-update**
>
> Distribuyendo fuera del App Store necesitás Sparkle o similar para updates.
>
> **Acciones top:** (1) Decidí si necesitás backend — todo tu modelo de negocio depende de esto. (2) Creá la abstracción de AI provider. (3) Agregá retry + timeout en las llamadas a la API.

## Cómo Funciona

```
unknown-unknowns/
├── SKILL.md              # Lógica core y workflow de la skill
└── references/
    ├── security.md       # Checklist de blind spots de seguridad
    ├── architecture.md   # Checklist de blind spots de arquitectura
    ├── legal.md          # Checklist legal/compliance (incluye LATAM)
    ├── costs.md          # Checklist de costos y operaciones
    ├── product.md        # Checklist de blind spots de producto/negocio
    └── ai-agents.md      # Checklist de riesgos de agentes AI
```

La skill lee solo los archivos de referencia relevantes a las dimensiones que elegiste — no carga todo en contexto.

## Compatibilidad

Hecha para Claude Code, pero el formato SKILL.md funciona con cualquier herramienta que lo soporte:

- Claude Code
- Cursor
- OpenCode
- Windsurf
- Cline
- Roo Code

## Contribuir

¿Encontraste un blind spot que la skill debería detectar? Abrí un issue o PR. Los archivos de referencia están diseñados para crecer — agregar items a un checklist es la forma más fácil de contribuir.

## Licencia

MIT
