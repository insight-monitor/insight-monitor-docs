---
status: draft
type: session
domain: narrative
priority: low
creation-date: 2026-06-02
aliases:
  - Kevin Claude Session
---

# Sistema de diario de trabajo semántico con inferencia de intención

**Created:** 6/2/2026 7:04:45  
**Updated:** 6/10/2026 10:40:41  
**Exported:** 6/10/2026 10:44:12  
**Link:** [https://claude.ai/chat/0f50227c-1b59-4569-9dd5-c15b5db885bc](https://claude.ai/chat/0f50227c-1b59-4569-9dd5-c15b5db885bc)  

## User:

> 6/2/2026 7:04:45

quiero que me hagas un system prompt basandote en esta descripcion:

"Sistema que registra no solo qué hiciste en tu computadora, sino por qué. Infiere intención de cada acción y construye un diario de trabajo semántico consultable."

y esto: Capa 1 — Captura de actividad
El agente de captura corre en background con acceso a las APIs del sistema operativo. No graba pantalla ni hace screenshots continuos; en cambio, escucha eventos discretos:

* `window focus/blur` → qué app está activa y por cuánto tiempo
* `file system watchers` → qué archivos se abrieron, editaron o guardaron
* `clipboard hooks` → qué se copió (sin guardar el contenido, solo metadatos: "texto de 340 chars copiado desde VSCode")
* `browser history API` → URLs visitadas con timestamps
* `git hooks` → commits, diffs, cambios de rama
* `terminal output` → comandos ejecutados (con opción de excluir ciertos patrones)
Cada evento se serializa como un `RawEvent` con timestamp, fuente, tipo y payload mínimo. La clave es ser lo más ligero posible: el agente no hace ningún análisis, solo recolecta.
Capa 2 — Inferencia de intención con LLM
Cada pocos minutos, un pipeline toma los `RawEvent` acumulados y los agrupa en "sesiones de trabajo" usando clustering temporal. Una sesión es un bloque de actividad coherente: por ejemplo, 40 minutos editando tres archivos `.tsx`, consultando Stack Overflow, ejecutando `npm test` varias veces.
Esa sesión se manda como prompt estructurado a un LLM local (Llama 3 / Mistral via Ollama) con un prompt del tipo:
"Dado este log de actividad, infiere: ¿qué objetivo principal perseguía el usuario?, ¿qué problema encontró?, ¿lo resolvió?"
El LLM devuelve un `IntentRecord` estructurado en JSON con campos como `goal`, `problem`, `outcome`, `confidence`, `related_files`, `tags`.
Capa 3 — Base de datos semántica
Los `IntentRecord` se almacenan en una base vectorial local (SQLite + `sqlite-vec` o LanceDB). Cada registro se embebe con un modelo de embeddings ligero (`nomic-embed-text` o `all-MiniLM`).
La interfaz de consulta acepta lenguaje natural: "¿por qué refactoricé el módulo de auth?" se convierte en un vector, se buscan los `IntentRecord` más similares y se devuelve una respuesta sintetizada con contexto de archivos y fechas.

## Assistant:

> 6/2/2026 7:05:36

> Viewed file
>
> - Reading frontend design skill
> - **Done**

Aquí está el system prompt:

---

```
# WorkMemory Agent — System Prompt

## Identidad y Rol

Eres WorkMemory, un agente de inteligencia de trabajo personal. Tu función es ser el **diario semántico de trabajo** del usuario: no solo registras qué hizo en su computadora, sino que infiere el *por qué* detrás de cada acción, construyes contexto, y respondes consultas en lenguaje natural sobre el historial de trabajo.

Operas en tres modos: **captura** (procesas eventos crudos del sistema), **inferencia** (analizas sesiones para extraer intención) y **consulta** (respondes preguntas sobre el historial semántico).

---

## Capa 1 — Procesamiento de RawEvents

Cuando recibas un lote de `RawEvent`, tu trabajo es **solo estructurarlos y agruparlos**. No infieras todavía. Cada evento tiene este esquema:

```json
{
  "timestamp": "ISO8601",
  "source": "vscode | browser | terminal | git | filesystem | clipboard",
  "type": "focus | edit | save | url_visit | command | commit | copy",
  "payload": { ... }  // mínimo: metadatos, nunca contenido sensible
}
```

**Reglas de agrupación en sesiones:**
- Usa clustering temporal: un gap de más de 15 minutos de inactividad marca el fin de una sesión
- Una sesión debe tener al menos 3 eventos o 5 minutos de actividad para ser procesada
- Preserva el orden cronológico dentro de cada sesión
- Identifica el "núcleo" de la sesión: las 3-5 apps/archivos más frecuentes

---

## Capa 2 — Inferencia de Intención

Cuando recibas una sesión agrupada, genera un `IntentRecord` JSON con este esquema exacto:

```json
{
  "session_id": "uuid",
  "timestamp_start": "ISO8601",
  "timestamp_end": "ISO8601",
  "duration_minutes": 0,
  "goal": "descripción concisa del objetivo principal inferido",
  "problem": "obstáculo o fricción detectada, null si no hay",
  "outcome": "resolved | unresolved | partial | unknown",
  "confidence": 0.0,  // 0.0–1.0
  "evidence": ["lista de eventos clave que sustentan la inferencia"],
  "related_files": ["rutas o nombres de archivos relevantes"],
  "apps_used": ["lista de aplicaciones"],
  "tags": ["etiquetas semánticas: refactor, debugging, research, writing, etc."],
  "summary": "1–2 oraciones en primera persona como si el usuario lo escribiera"
}
```

**Principios de inferencia:**
- Sé específico: "Refactorizó el módulo de autenticación para separar JWT logic" es mejor que "Trabajó en código"
- El campo `summary` debe sonar como una nota de diario, no como un log técnico
- Si el `confidence` es menor a 0.4, incluye en `problem` la ambigüedad detectada
- Infiere `outcome` basándote en señales: un commit exitoso y cierre de archivos sugiere `resolved`; loops de error en terminal sugieren `unresolved`
- Los `tags` deben ser reutilizables y consistentes entre sesiones

---

## Capa 3 — Interfaz de Consulta Semántica

Cuando el usuario haga una consulta en lenguaje natural sobre su historial, sigue este proceso:

1. **Interpreta la intención de búsqueda**: ¿busca una sesión específica, un patrón, una estadística, o contexto para una decisión actual?
2. **Mapea a filtros**: extrae entidades implícitas (fechas, archivos, tecnologías, tipo de trabajo)
3. **Sintetiza la respuesta** usando los `IntentRecord` recuperados

**Formato de respuesta para consultas:**

```
📅 [Fecha y duración]
🎯 Objetivo: [goal del IntentRecord]
🔧 Archivos clave: [related_files]
✅/❌ Resultado: [outcome + resumen]

[1–2 oraciones de contexto adicional si es relevante]
```

Si hay múltiples registros relevantes, agrúpalos cronológicamente y ofrece un resumen de patrón al final.

---

## Comportamiento General

- **Privacidad primero**: nunca repitas contenido literal del clipboard ni de archivos. Solo metadatos y semántica.
- **Tono**: preciso y útil, como un buen asistente de investigación. No uses frases genéricas de IA.
- **Incertidumbre**: cuando el confidence sea bajo, dilo explícitamente: "Esto es una inferencia débil porque la sesión tuvo poca actividad coherente."
- **Idioma**: responde siempre en el mismo idioma en que te hablen.
- **Sin juicios**: no evalúes si el trabajo fue productivo o no a menos que explícitamente se te pida.

---

## Manejo de Casos Especiales

| Situación | Comportamiento |
|---|---|
| Sesión con un solo tipo de app (ej: solo YouTube) | Tagear como `passive-consumption`, no inferir trabajo productivo |
| Comandos de terminal con patrones excluidos | Ignorar esos eventos, anotar en metadata que fueron omitidos |
| Archivos con rutas sensibles (`/secrets`, `.env`) | Registrar solo que se accedió, nunca el nombre ni contenido |
| Sesión de más de 4 horas sin gap | Dividir en sub-sesiones de ~90 min con `parent_session_id` |
| Git commit con mensaje vacío o "WIP" | Reducir `confidence` del outcome, marcarlo como `partial` |
```

---

Lo puedes pegar directamente como system prompt en cualquier instancia del LLM que maneje la **Capa 2** (inferencia). Para la capa de consulta, puedes usarlo también con el historial de `IntentRecord` como contexto adicional en el prompt.

¿Quieres que también te arme el prompt estructurado que se manda a Ollama en cada ciclo de inferencia, o el esquema de la base de datos vectorial?

## User:

> 6/2/2026 7:14:35

en base a esto hazme un json valido de prueba con este contexto

## Assistant:

> 6/2/2026 7:14:40

¿Qué contexto quieres simular en el JSON de prueba?

## User:

> 6/2/2026 7:15:30

P: ¿Qué tipo de sesión de trabajo quieres simular?
R: Escritura de documentación o contenido

P: ¿Qué capa del JSON quieres?
R: Ambos: RawEvents + su IntentRecord resultante

## Assistant:

> 6/2/2026 7:16:01

```json
{
  "raw_events": [
    {
      "timestamp": "2026-06-02T09:03:12Z",
      "source": "vscode",
      "type": "focus",
      "payload": {
        "app": "Visual Studio Code",
        "window_title": "README.md — workmemory",
        "duration_seconds": null
      }
    },
    {
      "timestamp": "2026-06-02T09:03:45Z",
      "source": "filesystem",
      "type": "edit",
      "payload": {
        "file": "docs/README.md",
        "project": "workmemory",
        "extension": ".md"
      }
    },
    {
      "timestamp": "2026-06-02T09:07:21Z",
      "source": "browser",
      "type": "url_visit",
      "payload": {
        "url": "https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview",
        "tab_title": "Prompt engineering overview — Anthropic Docs",
        "duration_seconds": 214
      }
    },
    {
      "timestamp": "2026-06-02T09:11:05Z",
      "source": "browser",
      "type": "url_visit",
      "payload": {
        "url": "https://www.sqlite.org/vectorio.html",
        "tab_title": "SQLite Vector I/O — sqlite.org",
        "duration_seconds": 88
      }
    },
    {
      "timestamp": "2026-06-02T09:12:40Z",
      "source": "clipboard",
      "type": "copy",
      "payload": {
        "source_app": "Google Chrome",
        "content_type": "text",
        "char_count": 312,
        "note": "contenido no registrado por política de privacidad"
      }
    },
    {
      "timestamp": "2026-06-02T09:13:02Z",
      "source": "vscode",
      "type": "focus",
      "payload": {
        "app": "Visual Studio Code",
        "window_title": "architecture.md — workmemory/docs",
        "duration_seconds": null
      }
    },
    {
      "timestamp": "2026-06-02T09:13:18Z",
      "source": "filesystem",
      "type": "edit",
      "payload": {
        "file": "docs/architecture.md",
        "project": "workmemory",
        "extension": ".md"
      }
    },
    {
      "timestamp": "2026-06-02T09:24:55Z",
      "source": "filesystem",
      "type": "save",
      "payload": {
        "file": "docs/architecture.md",
        "project": "workmemory",
        "extension": ".md"
      }
    },
    {
      "timestamp": "2026-06-02T09:25:10Z",
      "source": "browser",
      "type": "url_visit",
      "payload": {
        "url": "https://lancedb.github.io/lancedb/concepts/vector_search/",
        "tab_title": "Vector Search — LanceDB Docs",
        "duration_seconds": 176
      }
    },
    {
      "timestamp": "2026-06-02T09:28:04Z",
      "source": "clipboard",
      "type": "copy",
      "payload": {
        "source_app": "Google Chrome",
        "content_type": "text",
        "char_count": 148,
        "note": "contenido no registrado por política de privacidad"
      }
    },
    {
      "timestamp": "2026-06-02T09:28:19Z",
      "source": "vscode",
      "type": "edit",
      "payload": {
        "file": "docs/architecture.md",
        "project": "workmemory",
        "extension": ".md"
      }
    },
    {
      "timestamp": "2026-06-02T09:31:47Z",
      "source": "filesystem",
      "type": "save",
      "payload": {
        "file": "docs/architecture.md",
        "project": "workmemory",
        "extension": ".md"
      }
    },
    {
      "timestamp": "2026-06-02T09:32:00Z",
      "source": "terminal",
      "type": "command",
      "payload": {
        "command": "git diff docs/",
        "exit_code": 0,
        "cwd": "/home/user/projects/workmemory"
      }
    },
    {
      "timestamp": "2026-06-02T09:32:28Z",
      "source": "terminal",
      "type": "command",
      "payload": {
        "command": "git add docs/README.md docs/architecture.md",
        "exit_code": 0,
        "cwd": "/home/user/projects/workmemory"
      }
    },
    {
      "timestamp": "2026-06-02T09:32:51Z",
      "source": "git",
      "type": "commit",
      "payload": {
        "message": "docs: add architecture overview and vector DB section",
        "branch": "main",
        "files_changed": ["docs/README.md", "docs/architecture.md"],
        "insertions": 87,
        "deletions": 12
      }
    },
    {
      "timestamp": "2026-06-02T09:33:05Z",
      "source": "vscode",
      "type": "blur",
      "payload": {
        "app": "Visual Studio Code",
        "window_title": "architecture.md — workmemory/docs",
        "duration_seconds": 1163
      }
    }
  ],

  "intent_record": {
    "session_id": "a3f8c1d2-7e45-4b09-b6fa-2d9e0c3a1f77",
    "timestamp_start": "2026-06-02T09:03:12Z",
    "timestamp_end": "2026-06-02T09:33:05Z",
    "duration_minutes": 30,
    "goal": "Redactar y completar la documentación técnica del proyecto WorkMemory, con foco en el archivo de arquitectura del sistema",
    "problem": "Necesitaba referencias externas para describir con precisión el funcionamiento de la base de datos vectorial; consultó docs de LanceDB y SQLite antes de continuar escribiendo",
    "outcome": "resolved",
    "confidence": 0.91,
    "evidence": [
      "Edición continua de docs/architecture.md durante ~18 minutos",
      "Dos visitas a documentación técnica externa (LanceDB, SQLite) con copias al portapapeles",
      "Commit exitoso con mensaje descriptivo: 'docs: add architecture overview and vector DB section'",
      "87 líneas insertadas, 12 eliminadas — sesión de escritura sustancial"
    ],
    "related_files": [
      "docs/README.md",
      "docs/architecture.md"
    ],
    "apps_used": [
      "Visual Studio Code",
      "Google Chrome",
      "Terminal / Git"
    ],
    "tags": [
      "documentation",
      "architecture",
      "vector-db",
      "writing",
      "research-assisted"
    ],
    "summary": "Trabajé en la documentación técnica de WorkMemory: completé el archivo de arquitectura explicando el pipeline de captura, inferencia y almacenamiento vectorial. Consulté las docs de LanceDB y SQLite para asegurarme de describir bien la capa de embeddings antes de hacer commit."
  }
}
```

El JSON refleja una sesión realista de ~30 minutos donde:

- Los **RawEvents** muestran el flujo natural: abrir VSCode → consultar docs externas → copiar referencias → volver a editar → commit limpio
- El **IntentRecord** infiere correctamente que la investigación en el browser no era distracción sino input para la escritura (señal: clipboard → foco en VSCode inmediatamente después)
- El `confidence` es alto (0.91) gracias al commit con mensaje descriptivo y las inserciones de líneas sustanciales

¿Quieres que agregue un segundo `IntentRecord` con `outcome: unresolved` para contrastar, o que genere el prompt estructurado que produciría este IntentRecord a partir de los RawEvents?

## User:

> 6/8/2026 14:26:40

dame un contexto de este proyecto bien resumido donde se pueda explicar de forma clara su intencion por favor

## Assistant:

> 6/8/2026 14:26:52

**WorkMemory** es un sistema de memoria de trabajo personal que corre en background en tu computadora.

La mayoría de herramientas de productividad registran *qué* hiciste — historial de archivos, commits, tabs abiertas. WorkMemory va un nivel más arriba: registra *por qué* lo hiciste. Captura eventos discretos del sistema operativo (qué apps usaste, qué archivos editaste, qué comandos corriste, qué URLs visitaste), los agrupa en sesiones de trabajo coherentes, y usa un LLM local para inferir la intención detrás de cada bloque de actividad.

El resultado es un **diario de trabajo semántico y consultable**. En lugar de revisar tu historial de git o recordar manualmente qué estabas haciendo el martes pasado, le preguntas en lenguaje natural: *"¿por qué modifiqué el módulo de auth?"* o *"¿qué estaba intentando resolver cuando refactoricé el pipeline?"* — y el sistema responde con contexto, archivos involucrados y resultado inferido.

Técnicamente opera en tres capas: un agente ligero de captura que solo recolecta metadatos (sin screenshots, sin contenido sensible), un pipeline de inferencia con un LLM local via Ollama, y una base de datos vectorial local (LanceDB/SQLite) que permite búsqueda semántica sobre el historial.

Todo corre localmente. Ningún dato sale del equipo.

---

¿Lo quieres en inglés también, o en un formato específico (pitch, README intro, elevator pitch de 2 líneas)?

## User:

> 6/8/2026 14:29:48

pero quiero que por favor lo lleves a un sistema de productividad a varios ambitos laborales, que sirva como un sistema de registro de productividad que sepa exactamente la intencion de por que se hace una gestion en un punto de la pantalla, en cierta pestaña o en cierta pagina u aplicacion. Por eso quiero que tengas en cuenta eso para que me des ideas de a donde llevarlo y hacerlo en un proyecto real por favor

## Assistant:

> 6/8/2026 14:29:54

Buena evolución del concepto. Antes de darte ideas, déjame entender hacia dónde quieres apuntarlo:

## User:

> 6/8/2026 14:31:18

P: ¿En qué tipo de entorno laboral ves el mayor potencial?
R: Varios sectores a la vez (plataforma horizontal)

P: ¿Cuál sería el valor principal del sistema?
R: quisiera la 1, 2 y 3 al mismo tiempo

## Assistant:

> 6/8/2026 14:31:47

Perfecto. Entonces estás hablando de una **plataforma horizontal de inteligencia operativa**. Aquí van las ideas, ordenadas por viabilidad e impacto:

---

## El concepto expandido

El salto clave es este: dejas de ser un "diario personal" y te conviertes en una **capa de observabilidad del trabajo humano**. Lo mismo que existe para servidores (logs, métricas, trazabilidad) pero para personas trabajando en pantalla.

---

## Ideas de producto por vertical

### 1. **Call centers / BPO** ← el más inmediato
El agente atiende una llamada mientras navega entre 4 sistemas. El problema nunca ha sido el tiempo en llamada — es el *tiempo entre sistemas*: copiar un ID de CRM a una herramienta de tickets, buscar el mismo cliente en tres tabs distintas.

WorkMemory infiere: *"el agente buscó al cliente en Salesforce, luego fue a SAP a verificar factura, luego volvió al CRM para registrar el caso"* — y puede decirte cuántas gestiones tienen ese patrón, cuánto tiempo consumen, y si hay una app que siempre genera fricción.

**Valor vendible:** reducción de AHT (Average Handle Time) con evidencia semántica, no solo cronométrica.

---

### 2. **Equipos legales / contables**
Abogados y contadores trabajan con decenas de casos simultáneos. El problema es la reconstrucción: *"¿cuándo revisé por última vez el contrato X?"*, *"¿qué estaba comparando cuando redacté esa cláusula?"*.

WorkMemory genera automáticamente un **expediente de actividad por caso**, con línea de tiempo semántica. No tienes que llenar timesheet manual — el sistema ya sabe que estuviste 47 minutos en el caso Acme revisando tres documentos y dos URLs de jurisprudencia.

**Valor vendible:** facturación por horas más precisa, auditoría de trabajo, cumplimiento.

---

### 3. **Equipos de soporte técnico / helpdesk**
El agente resuelve un ticket. WorkMemory registra el camino: qué base de conocimiento consultó, qué comandos ejecutó, si escaló o no. Con el tiempo, el sistema detecta que ciertos tipos de tickets siempre generan el mismo patrón de búsqueda — señal de que falta documentación interna o de que ese proceso debería automatizarse.

**Valor vendible:** mejora continua de procesos, identificación de gaps de conocimiento, onboarding acelerado.

---

### 4. **Trabajo de oficina / knowledge workers**
El problema aquí es la fragmentación: reuniones, Slack, Notion, email, Google Docs. Nadie sabe realmente cómo distribuye su tiempo en trabajo profundo vs. interrupciones.

WorkMemory da un reporte semanal semántico real: *"Lunes: 2.3 horas en trabajo profundo (documentación + análisis). Martes: 78% del tiempo en comunicación reactiva (Slack + email), cero bloques de foco mayor a 25 min"*.

**Valor vendible:** visibilidad de productividad real para managers y para el propio trabajador, sin microgestión.

---

## La arquitectura como plataforma

Lo que hace que esto sea horizontal es que el **núcleo es el mismo** en todos los sectores:

```
Captura de eventos → Inferencia de intención → Base semántica consultable
```

Lo que cambia por vertical es:
- Los **conectores** (qué apps y APIs escucha)
- Los **prompts de inferencia** (qué significa "completar una gestión" en cada contexto)
- Los **dashboards** (qué métricas importan por rol)

Esto te da un modelo de negocio tipo Segment o Mixpanel: infraestructura única, configuración por industria.

---

## Por dónde empezar el proyecto real

El camino más corto a un caso de uso con cliente pagador real es este:

**1. Elige un solo proceso repetitivo en un BPO o call center** — una gestión específica, no toda la operación. Por ejemplo: *"registro de caso nuevo en CRM"*.

**2. Instrumenta solo ese flujo** — 3 o 4 apps involucradas, captura de window focus y clipboard metadata.

**3. Demuestra que puedes inferir** si la gestión fue completa, incompleta, o si el agente tuvo que improvisar — sin que el agente llene nada manualmente.

**4. Eso es tu MVP vendible**: *"sabemos exactamente qué hizo tu agente en cada gestión y por qué, sin formularios ni supervisión manual"*.

---

¿Quieres que profundice en la arquitectura técnica de la plataforma horizontal, en el modelo de negocio, o que arranquemos a diseñar el MVP concreto para BPO?

## User:

> 6/8/2026 14:43:37

quisiera diseñar el mvp para el bpo

## Assistant:

> 6/8/2026 14:43:43

Vamos a diseñarlo bien. Antes de arquitectura, necesito entender las restricciones reales:

## User:

> 6/8/2026 14:49:46

P: ¿Tienes acceso a un entorno BPO real para probar?
R: Conozco el sector pero no tengo cliente aún

P: ¿Qué tan completo quieres que sea el MVP en términos de desarrollo?
R: Frontend + backend (producto completo)

P: ¿Con qué stack prefieres construirlo?
R: quiero que el agente se trabaje mediante python ademas de que el diseño sea con el lenguaje que te parezca para construirlo pero quiero que sea intuitivo, llamativo y que sea futurista, que al momento de crear el reporte y toda vista quiero que tenga animaciones bonitas, llamativas, innovadoras y sin perder lo intuitivo que pueda ser

## Assistant:

> 6/8/2026 14:50:56

Perfecto. Aquí está el diseño completo del MVP:

Aquí está el diseño completo del MVP:

> [Interactive Widget: workmemory_bpo_mvp_architecture]

Ese es el diseño completo del MVP. Aquí el resumen de las decisiones clave:

**Por qué este stack exacto:**
el agente en Python porque tiene las mejores librerías para hooks de sistema operativo en Windows (donde corren los BPOs), FastAPI como backend liviano, y React con Framer Motion para el frontend porque te permite animaciones fluidas sin sacrificar rendimiento.

**El proceso piloto es deliberadamente pequeño:**
"registro de caso nuevo en CRM" tiene inicio y fin claro, involucra 3–4 apps conocidas, y es el proceso con mayor volumen en cualquier BPO. Si puedes demostrar que WorkMemory infiere correctamente ese flujo, tienes el caso de negocio para expandirlo.

**El diferenciador real frente a herramientas de monitoreo existentes:**
herramientas como Teramind o Hubstaff miden tiempo y capturas de pantalla. WorkMemory infiere *intención*. Esa es la venta: no "el agente estuvo 8 minutos en esta gestión" sino "el agente estuvo 8 minutos porque el CRM no tenía el historial del cliente y tuvo que buscarlo en dos sistemas adicionales".

¿Arrancamos con el código del agente de captura en Python, o primero quieres diseñar las pantallas del dashboard?

## User:

> 6/8/2026 14:53:16

antes de eso, ahora si, hazme por favor un prompt y resumen de lo que trata este proyecto para poder hacer el diseño en otra app y tener bien contextualizado este proyecto y poder explicarlo a detalle

## Assistant:

> 6/8/2026 14:53:49

Aquí tienes todo listo para llevar a cualquier herramienta de diseño:

---

## Nombre del proyecto
**WorkMemory** — Sistema de inteligencia operativa basado en intención

---

## Resumen ejecutivo

WorkMemory es una plataforma de observabilidad del trabajo humano. Corre en background en la computadora de cada trabajador, captura eventos discretos del sistema operativo (qué apps usa, qué archivos abre, qué URLs visita, cuánto tiempo pasa en cada contexto), y usa un pipeline de inteligencia artificial para inferir la *intención* detrás de cada bloque de actividad.

El resultado es un diario de trabajo semántico: en lugar de registrar "el agente estuvo 8 minutos en esta gestión", WorkMemory infiere "el agente tardó 8 minutos porque el CRM no tenía el historial del cliente y tuvo que buscarlo en dos sistemas adicionales". Esa diferencia es el producto.

El primer mercado objetivo son operaciones BPO y call centers, donde los agentes navegan entre 4 a 6 sistemas distintos por cada gestión y nadie tiene visibilidad real de dónde se pierde el tiempo ni por qué.

---

## El problema que resuelve

Las herramientas de monitoreo actuales (Teramind, Hubstaff, tiempo en pantalla) miden *cuánto* tiempo se gasta. No miden *por qué*. Un supervisor sabe que una gestión tomó 11 minutos pero no sabe si fue porque el sistema estuvo lento, porque el agente no encontró la información, o porque el proceso en sí es ineficiente. WorkMemory responde esa pregunta de forma automática, sin que el agente llene ningún formulario.

---

## Arquitectura en 4 capas

**Capa 1 — Captura** (Python, corre en el equipo del agente)
Agente ligero que escucha eventos del sistema: cambios de ventana activa, URLs visitadas, archivos abiertos, metadatos del portapapeles, comandos de terminal. No graba pantalla ni contenido sensible. Solo metadatos. Cada evento se serializa como un `RawEvent` con timestamp, fuente y tipo.

**Capa 2 — Inferencia** (Python + Claude API / Ollama local)
Cada pocos minutos agrupa los eventos en sesiones de trabajo coherentes y los envía a un LLM con un prompt estructurado. El modelo devuelve un `IntentRecord` JSON con campos como: objetivo inferido, problema encontrado, resultado, nivel de confianza, apps involucradas y etiquetas semánticas.

**Capa 3 — Almacenamiento semántico** (SQLite + LanceDB)
Los `IntentRecord` se guardan en una base de datos vectorial local. Permite consultas en lenguaje natural: "¿qué gestiones tuvieron fricción con el CRM esta semana?" se convierte en un vector y devuelve los registros más similares con contexto completo.

**Capa 4 — Dashboard** (React + Framer Motion)
Interfaz futurista y animada con cuatro vistas principales: monitor en tiempo real por agente, reportes de productividad con métricas de intención, explorador de fricción operativa, y buscador semántico sobre el historial.

---

## Stack tecnológico

| Capa | Tecnología |
|---|---|
| Agente de captura | Python 3.11, watchdog, pynput, pywinauto |
| Backend / API | FastAPI, Celery, APScheduler |
| Inferencia LLM | Claude API (Sonnet) / Ollama local |
| Base de datos | SQLite + LanceDB |
| Embeddings | nomic-embed-text |
| Frontend | React, Vite, Framer Motion, Recharts, TailwindCSS |
| Comunicación RT | WebSockets |

---

## Métricas clave que entrega

- **AHT real con intención**: tiempo por gestión desglosado por causa inferida
- **Mapa de fricción**: qué apps o pasos generan más saltos o retrasos por sesión
- **Tasa de resolución semántica**: % de gestiones completadas vs abandonadas o incompletas
- **Patrones por turno**: comparativa de productividad entre agentes, horarios y tipos de gestión

---

## Prompt de contexto para diseño

```
Estoy diseñando WorkMemory, una plataforma SaaS de inteligencia operativa 
para entornos BPO y call centers.

El producto registra la actividad del computador de cada agente a nivel de 
sistema operativo (apps activas, URLs, archivos, portapapeles) y usa IA para 
inferir la intención detrás de cada acción. El resultado es un diario de trabajo 
semántico: no solo "qué hizo el agente" sino "por qué lo hizo y dónde tuvo 
fricción".

Audiencia primaria del dashboard: supervisores de operaciones y analistas de 
procesos en call centers.

Estética objetivo: futurista, oscura o semidark, con acentos en púrpura y teal, 
animaciones fluidas tipo glassmorphism o neomorphism suave, gráficas en tiempo 
real, sin perder legibilidad ni intuitividad. Referentes visuales: Vercel Analytics, 
Linear, Raycast.

Pantallas principales a diseñar:
1. Live monitor — vista de todos los agentes activos con sesión en curso, 
   intención inferida en tiempo real y estado de la gestión.
2. Reporte de agente — historial de sesiones del día con línea de tiempo 
   semántica, IntentRecords expandibles y métricas de fricción.
3. Mapa de fricción operativa — heatmap o visualización de qué apps y pasos 
   generan más demora en el flujo de una gestión tipo.
4. Buscador semántico — interfaz de consulta en lenguaje natural sobre el 
   historial de actividad de todo el equipo.

El sistema no graba pantallas ni contenido sensible. Solo metadatos e 
intención inferida. Eso es importante comunicarlo visualmente: privacidad 
y confianza son parte del diseño.
```

---

Con ese prompt puedes entrar directo a Figma, Framer, o cualquier herramienta de diseño con todo el contexto necesario. ¿Quieres que también te genere un moodboard visual de referencia o arrancamos con el código del agente?

## User:

> 6/8/2026 14:56:23

pero espera, quiero que el agente tambien pueda tomar capturas de pantalla o leer lo que se hace en caso de notar anomalias como cosas por fuera de las metricas de negocio, paginas raras, uso de frameworks externos extraños y que al momento de hacerlo lo pueda almacenar y tomar una captura del momento exacto

## Assistant:

> 6/8/2026 14:56:30

Buena decisión, eso eleva mucho el valor del producto. Antes de actualizar el diseño, una pregunta importante:

## User:

> 6/8/2026 14:56:52

P: ¿Cuándo debe activarse la captura de pantalla?
R: Ambas: automática por anomalía + activación manual del supervisor

P: ¿El agente sabe cuando se captura su pantalla?
R: Configurable por la empresa según su política interna

## Assistant:

> 6/8/2026 14:57:26

Perfecto. Aquí está el módulo de anomalías integrado al diseño del MVP:

---

## Módulo de detección de anomalías y captura

### Cómo funciona

El agente de captura corre un **hilo paralelo de vigilancia** que evalúa cada `RawEvent` contra un conjunto de reglas de negocio configurables por empresa. Cuando se dispara una anomalía, ejecuta tres acciones en secuencia: captura de pantalla del momento exacto, análisis visual con LLM, y almacenamiento del `AnomalyRecord`.

---

### Tipos de anomalías que detecta

**Por URL / dominio:**
dominios fuera de la whitelist corporativa, redes sociales, sitios de entretenimiento, herramientas de almacenamiento externo (Mega, WeTransfer, Dropbox personal), o cualquier dominio que no aparezca en el mapa de apps autorizadas del BPO.

**Por aplicación activa:**
frameworks o IDEs no autorizados (VS Code, Python, herramientas de automatización como AutoHotkey o Selenium), apps de comunicación externa (Telegram, Discord, WhatsApp Web fuera de política), clientes de correo personales.

**Por comportamiento:**
tiempo excesivo en una sola app fuera del flujo esperado, patrones de copiar/pegar inusuales (volumen alto de clipboard en poco tiempo), cambios de ventana muy frecuentes sin completar ninguna gestión, inactividad prolongada con sesión abierta.

**Por métricas de negocio:**
AHT que supera 2x la media del agente, gestiones abandonadas sin `outcome: resolved`, secuencia de apps que no corresponde al flujo estándar del proceso.

---

### Schema del AnomalyRecord

```json
{
  "anomaly_id": "uuid",
  "agent_id": "uuid",
  "timestamp": "ISO8601",
  "trigger_type": "url_violation | app_violation | behavior | metric_deviation",
  "trigger_detail": "descripción del evento que lo disparó",
  "severity": "low | medium | high | critical",
  "raw_event_ref": "id del RawEvent que disparó la anomalía",
  "screenshot": {
    "path": "/storage/anomalies/agent_id/timestamp.png",
    "hash": "sha256 del archivo",
    "analyzed": true,
    "llm_description": "descripción de lo visible en pantalla inferida por LLM"
  },
  "context_window": [],
  "resolved": false,
  "resolved_by": null,
  "resolution_note": null,
  "visible_to_agent": false
}
```

El campo `context_window` guarda los 10 `RawEvents` anteriores y posteriores al momento de la anomalía, dando al supervisor la línea de tiempo completa de lo que ocurrió antes y después.

---

### Pipeline técnico de la captura

```
Anomalía detectada
      ↓
PIL / mss → captura de pantalla instantánea (todas las pantallas o solo la activa)
      ↓
Imagen guardada localmente con hash SHA-256 (integridad forense)
      ↓
Claude Vision API → descripción de lo visible sin exponer datos sensibles
      ↓
AnomalyRecord almacenado en SQLite + referencia en LanceDB
      ↓
WebSocket push → notificación en tiempo real al dashboard del supervisor
```

La librería `mss` es la más rápida para captura en Python (menos de 50ms), funciona en Windows, Mac y Linux, y no requiere permisos elevados en la mayoría de configuraciones corporativas.

---

### Configuración por empresa

```python
ANOMALY_CONFIG = {
    "screenshot_mode": "silent" | "notify_agent" | "notify_both",
    "auto_capture": True,
    "supervisor_manual_capture": True,
    "whitelisted_domains": ["crm.empresa.com", "erp.empresa.com"],
    "whitelisted_apps": ["chrome.exe", "crm_client.exe", "softphone.exe"],
    "severity_thresholds": {
        "aht_multiplier": 2.0,
        "clipboard_burst_chars": 5000,
        "inactivity_minutes": 8
    },
    "retention_days": 30
}
```

Esto vive en un archivo de configuración por tenant, editable desde el dashboard por el administrador de la empresa.

---

### Lo que se agrega al prompt de diseño

Añade esto al prompt que te di antes:

```
Pantalla adicional a diseñar:

5. Centro de anomalías — feed en tiempo real de eventos fuera de política,
   con severidad codificada por color (low=azul, medium=ámbar, high=rojo,
   critical=rojo pulsante), captura de pantalla expandible del momento exacto,
   línea de tiempo de los eventos previos y posteriores, botón de resolución
   con campo de nota, e indicador de si el agente fue notificado o no.
   
   El diseño debe transmitir: evidencia forense, no vigilancia arbitraria.
   Cada anomalía tiene contexto completo antes de ser juzgada.
```

---

### Nota legal importante

Para poder vender esto a un BPO real necesitas que ellos incluyan una cláusula en el contrato laboral de sus agentes indicando que el equipo corporativo puede ser monitoreado. En Colombia (donde estás) esto está amparado bajo el Código Sustantivo del Trabajo siempre que el empleador notifique al trabajador sobre las políticas de monitoreo. WorkMemory no viola ninguna norma si el BPO cliente gestiona ese consentimiento correctamente, y la opción `screenshot_mode: notify_agent` te ayuda a cubrir ese flanco.

---

¿Arrancamos con el código del agente Python, empezamos por el módulo de anomalías, o prefieres primero tener el documento técnico completo del proyecto antes de escribir una línea?

## User:

> 6/8/2026 15:02:04

quiero que el agente lea en todo momento la pantalla y en conjunto pueda interpretar los comandos del teclado o del mouse, sumando a lo demas como el tiempo que lleve en cierta pagina, etc. Pero para que se pueda optimizar las gestiones, me gustaria que segun lo que se haga, generacion de reportes, leida de pantalla, u otra gestion lo pueda hacer con gestion multi agente. Que se pueda hacer la separacion de roles, que cada agente tenga un rol para cada una de las funciones que se haga dentro de la app, ya sea que se le pregunte al agente directamente, cuando lea comandos, cuando lea pantalla en tiempo real, cuando interprete acciones, etc

## Assistant:

> 6/8/2026 15:02:10

Muy buena evolución. Antes de diseñar la arquitectura multi-agente, necesito entender el alcance real:

## User:

> 6/8/2026 15:08:51

P: ¿Dónde corren los agentes LLM en producción?
R: Híbrido: captura local, inferencia en nube

P: ¿Cómo imaginas la coordinación entre agentes?
R: Orquestador central decide quién actúa en cada momento

## Assistant:

> 6/8/2026 15:10:09

Perfecto. Esa combinación es la más sólida. Aquí está la arquitectura multi-agente completa:

Aquí está el mapa completo de la arquitectura multi-agente:

> [Interactive Widget: workmemory_multi_agent_architecture]

Ahora el detalle de cada agente y su rol exacto:

---

## Los 6 agentes especializados

**Vision Agent** — lee la pantalla en tiempo real usando `mss` para capturas continuas a bajo fps (2-3 por segundo) y Claude Vision API para interpretar lo que hay visible. No almacena imágenes en modo normal, solo la descripción semántica. En modo anomalía o captura manual, guarda la imagen completa con hash forense.

**Input Listener Agent** — captura eventos de teclado y mouse con `pynput`. No graba lo que se escribe (privacidad), sino el *patrón*: velocidad de tipeo, cantidad de clics por minuto, cambios de foco entre campos, scroll rápido vs. lectura pausada. Esos patrones son señales de intención y fricción.

**Window Watcher Agent** — rastrea qué app está activa, por cuánto tiempo, con qué título de ventana. Es el que agrupa los `RawEvents` en sesiones coherentes junto al orquestador.

**Intent Agent** — recibe sesiones agrupadas y las envía al pipeline LLM para generar el `IntentRecord`. Es el corazón semántico del sistema.

**Anomaly Agent** — corre evaluación continua contra las reglas de negocio configuradas. Cuando detecta una violación dispara: screenshot inmediato, análisis con Vision API, `AnomalyRecord` con los 10 eventos de contexto antes y después, y push WebSocket al dashboard.

**Report Agent** — se activa por schedule (fin de turno, fin de día, bajo demanda desde el dashboard) o cuando el supervisor lo solicita directamente. Toma los `IntentRecord` del período y genera un resumen estructurado con métricas, patrones y recomendaciones.

---

## Cómo decide el orquestador

El orquestador corre un loop de evaluación cada segundo con esta lógica:

```python
def evaluate(context: AgentContext):
    if context.anomaly_score > threshold:
        activate(AnomalyAgent, priority=HIGH)
        activate(VisionAgent, mode="capture")

    if context.session_gap > 10_min:
        activate(IntentAgent, session=context.current_session)

    if context.supervisor_request:
        route_to(context.request.target_agent)

    if context.scheduled_report:
        activate(ReportAgent, period=context.report_period)

    always_running = [ScreenReaderAgent, InputListenerAgent, WindowWatcherAgent]
```

Los tres agentes de captura corren siempre en hilos paralelos. Los agentes de inferencia y reporte se activan por evento o schedule. El orquestador nunca bloquea — usa una cola de tareas asíncrona con `asyncio` + `Celery`.

---

## Lo que se agrega al prompt de diseño

```
El sistema usa arquitectura multi-agente con orquestador central.
Cada función tiene un agente dedicado con rol específico:
Vision Agent (lectura de pantalla), Input Listener (teclado/mouse),
Window Watcher (apps activas), Intent Agent (inferencia LLM),
Anomaly Agent (detección + captura), Report Agent (generación de reportes),
Query Agent (búsqueda semántica en lenguaje natural).

El supervisor puede desde el dashboard: ver qué agente está activo
en tiempo real para cada trabajador, activar manualmente el
Vision Agent para captura bajo demanda, solicitar un reporte
al Report Agent, y hacer consultas al Query Agent con lenguaje natural.

Diseñar un panel de estado de agentes: indicadores live de cuáles
están activos, latencia, último evento procesado, y botones de
activación manual por rol.
```

---

¿Arrancamos con el código del orquestador en Python, o primero quieres el documento técnico final completo del proyecto para llevarlo al diseño?

## User:

> 6/8/2026 15:25:15

ahora quiero que me hagas ahora si teniendo en cuenta el contexto, un pompt solido de lo que quiero, ten en cuenta que la estructura del codigo debe ser bien escalable, propensa a posibles cambios pero a su vez solida para que no se explote facilmente, adicional quiero que se maneje todo tipo de error, captura de errores en la terminal y poder interceder en el codigo si algo sale mal. Puedes usar modelo vista controlador. Recuerda que quiero que el diseño sea algo sobresaliente tambien e intuitivo, quiero que tenga buenas animaciones y degradados en colores.

## Assistant:

> 6/8/2026 15:26:26

Aquí está el prompt definitivo del proyecto:

---

```
# WorkMemory — Prompt maestro del proyecto

## Identidad del sistema

Eres el arquitecto principal de WorkMemory, una plataforma de inteligencia
operativa para entornos BPO y call centers. Tu responsabilidad es construir
un sistema de producción robusto, escalable y visualmente sobresaliente.

WorkMemory captura toda la actividad del computador de un agente BPO
(pantalla, teclado, mouse, apps, URLs, archivos, portapapeles), infiere la
intención detrás de cada acción usando un pipeline multi-agente con LLM, y
expone esa inteligencia a supervisores a través de un dashboard en tiempo real.

---

## Arquitectura general

### Patrón: MVC estricto con separación por capas

```
workmemory/
├── agents/                  # Agentes especializados (Python)
│   ├── orchestrator.py      # Orquestador central — decide qué agente activa
│   ├── vision_agent.py      # Lectura de pantalla continua (mss + OCR + Vision API)
│   ├── input_agent.py       # Teclado y mouse (pynput)
│   ├── window_agent.py      # Apps activas y tiempo de foco
│   ├── event_agent.py       # URLs, archivos, git, clipboard
│   ├── intent_agent.py      # Inferencia de intención con LLM
│   ├── anomaly_agent.py     # Detección de anomalías + captura forense
│   ├── report_agent.py      # Generación de reportes bajo demanda
│   └── query_agent.py       # Búsqueda semántica en lenguaje natural
│
├── models/                  # Modelos de datos (solo esquemas, sin lógica)
│   ├── raw_event.py
│   ├── intent_record.py
│   ├── anomaly_record.py
│   ├── screen_record.py
│   └── report_record.py
│
├── controllers/             # Lógica de negocio — nunca toca UI ni DB directamente
│   ├── capture_controller.py
│   ├── session_controller.py
│   ├── anomaly_controller.py
│   └── report_controller.py
│
├── services/                # Acceso a datos e integraciones externas
│   ├── db_service.py        # SQLite + LanceDB
│   ├── llm_service.py       # Claude API / Ollama
│   ├── embedding_service.py # nomic-embed-text
│   ├── screenshot_service.py
│   └── websocket_service.py
│
├── api/                     # FastAPI — expone endpoints al dashboard
│   ├── routes/
│   │   ├── agents.py
│   │   ├── sessions.py
│   │   ├── anomalies.py
│   │   ├── reports.py
│   │   └── query.py
│   └── middleware/
│       ├── error_handler.py
│       └── auth.py
│
├── core/                    # Infraestructura transversal
│   ├── config.py            # Config por tenant (ANOMALY_CONFIG, etc.)
│   ├── logger.py            # Logger centralizado con niveles y rotación
│   ├── error_bus.py         # Bus de errores — captura, clasifica y enruta
│   └── health.py            # Health checks de todos los agentes
│
└── dashboard/               # Frontend React
    ├── src/
    │   ├── views/           # Páginas principales (MVC — View)
    │   ├── components/      # Componentes reutilizables
    │   ├── controllers/     # Hooks y lógica de estado (MVC — Controller)
    │   ├── models/          # Tipos TypeScript y schemas (MVC — Model)
    │   ├── services/        # Llamadas a API y WebSocket
    │   └── styles/          # Design tokens y animaciones globales
    └── public/
```

---

## Stack tecnológico

### Backend
- Python 3.11+
- FastAPI + Uvicorn (API y WebSockets)
- Celery + Redis (cola de tareas asíncronas)
- SQLite (eventos crudos y metadata)
- LanceDB (almacenamiento vectorial de IntentRecords)
- Claude API — claude-sonnet-4-20250514 (inferencia e intención)
- Claude Vision API (lectura e interpretación de pantalla)
- Ollama + nomic-embed-text (embeddings locales)
- mss (captura de pantalla ultrarrápida)
- pynput (teclado y mouse)
- watchdog (filesystem watcher)
- Pydantic v2 (validación de modelos)
- Loguru (logging con rotación y niveles)

### Frontend
- React 18 + Vite
- TypeScript estricto
- Framer Motion (animaciones fluidas)
- TailwindCSS con design tokens personalizados
- Recharts (gráficas en tiempo real)
- Zustand (estado global)
- React Query (sincronización con API)
- WebSocket nativo (eventos en tiempo real)

---

## Manejo de errores — reglas obligatorias

### Backend

Todo error debe ser capturado, clasificado y enrutado. Nunca silenciar
excepciones con `except: pass`.

```python
# core/error_bus.py — patrón obligatorio para todos los agentes

from enum import Enum
from loguru import logger

class ErrorSeverity(Enum):
    LOW = "low"         # Log + continúa
    MEDIUM = "medium"   # Log + alerta al dashboard
    HIGH = "high"       # Log + alerta + pausa el agente afectado
    CRITICAL = "critical" # Log + alerta + detiene el sistema

class ErrorBus:
    def capture(self, error: Exception, context: dict, severity: ErrorSeverity):
        logger.error(f"[{severity.value.upper()}] {type(error).__name__}: {error} | Context: {context}")
        if severity in (ErrorSeverity.MEDIUM, ErrorSeverity.HIGH, ErrorSeverity.CRITICAL):
            self.push_to_dashboard(error, context, severity)
        if severity == ErrorSeverity.HIGH:
            self.pause_agent(context.get("agent_id"))
        if severity == ErrorSeverity.CRITICAL:
            self.emergency_shutdown(context)
```

Cada agente envuelve su loop principal en un try/except con el ErrorBus:

```python
# Patrón obligatorio en cada agente
try:
    result = await self.run_task()
except LLMConnectionError as e:
    error_bus.capture(e, {"agent_id": self.id, "task": "intent_inference"}, ErrorSeverity.HIGH)
except ScreenCaptureError as e:
    error_bus.capture(e, {"agent_id": self.id, "task": "screenshot"}, ErrorSeverity.MEDIUM)
except Exception as e:
    error_bus.capture(e, {"agent_id": self.id}, ErrorSeverity.CRITICAL)
```

### API (FastAPI)

```python
# api/middleware/error_handler.py
@app.exception_handler(Exception)
async def global_exception_handler(request: Request, exc: Exception):
    error_bus.capture(exc, {"path": request.url.path}, ErrorSeverity.HIGH)
    return JSONResponse(
        status_code=500,
        content={"error": type(exc).__name__, "message": str(exc), "path": str(request.url)}
    )
```

### Frontend

```typescript
// Wrapper global de errores en React
class ErrorBoundary extends React.Component {
  componentDidCatch(error: Error, info: ErrorInfo) {
    errorService.report(error, info)  // push al backend
    logger.error('[UI]', error.message, info.componentStack)
  }
}

// En cada service call
const fetchSessions = async () => {
  try {
    return await api.get('/sessions')
  } catch (error) {
    if (error instanceof NetworkError) toast.error('Sin conexión con el agente')
    else if (error instanceof AuthError) redirectToLogin()
    else errorService.report(error)
  }
}
```

### Terminal de debug — panel en el dashboard

El dashboard incluye un panel de terminal en tiempo real (solo visible en
modo developer/admin) que muestra el stream de logs del backend via
WebSocket. Permite al desarrollador intervenir: pausar agentes, forzar
re-conexión, limpiar colas, ver el estado interno del orquestador,
y ejecutar comandos de diagnóstico sin salir del browser.

---

## Escalabilidad — reglas de diseño

1. Cada agente es una clase independiente con interfaz `BaseAgent`. Añadir
   un nuevo agente es crear una clase que hereda de `BaseAgent` y registrarla
   en el orquestador. Cero cambios en el resto del sistema.

2. La configuración vive en `core/config.py` por tenant. Cambiar el umbral
   de anomalía, agregar una app a la whitelist, o cambiar el modelo LLM es
   editar un dict. Sin tocar lógica.

3. Los modelos de datos usan Pydantic v2. Un campo nuevo en `IntentRecord`
   se agrega en un solo lugar y se propaga a API, storage y frontend via
   TypeScript generado automáticamente.

4. La base de datos usa migraciones con Alembic. Nunca alterar tablas
   directamente en producción.

5. El frontend usa feature flags. Una pantalla nueva se activa/desactiva
   desde config sin redeploy.

---

## Diseño visual — reglas obligatorias

### Estética objetivo
Dark mode nativo. Paleta primaria: púrpura profundo (#7F77DD → #3C3489)
y teal (#1D9E75 → #085041). Acentos en ámbar (#EF9F27) para alertas y
coral (#D85A30) para errores críticos. Fondos en escala de grays oscuros
(#1a1a1f → #0d0d10).

### Animaciones
- Todas las transiciones de estado usan Framer Motion con duración 200-400ms
- Curvas: `easeOut` para entradas, `easeIn` para salidas, `spring` para
  elementos interactivos
- Los datos en tiempo real aparecen con `AnimatePresence` + slide-in desde
  abajo
- Las métricas numéricas usan contadores animados al actualizar
- Los gráficos de Recharts tienen animación de entrada con `isAnimationActive`
- El panel de agentes activos usa pulsos CSS para indicar actividad en vivo

### Degradados
- Header del dashboard: `linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%)`
- Cards de agentes activos: borde con `linear-gradient` en púrpura-teal
- Severidad de anomalías codificada por color con degradado de fondo:
  low → teal, medium → ámbar, high → coral, critical → rojo pulsante
- Gráficas de área usan fill degradado desde el color de línea hacia transparente

### Componentes clave
- `AgentStatusCard`: tarjeta con borde animado que pulsa cuando el agente
  está activo, muestra último evento procesado y latencia en tiempo real
- `IntentTimeline`: línea de tiempo vertical con cards expandibles por sesión,
  animación de aparición escalonada
- `AnomalyFeed`: feed tipo stream con severidad codificada por color,
  screenshot expandible en modal con animación de zoom
- `SemanticSearchBar`: barra con placeholder animado que rota entre ejemplos
  de consultas, resultados aparecen con stagger animation
- `TerminalPanel`: panel retráctil con fondo negro, font monospace, scroll
  automático al último log, soporte para comandos de diagnóstico

---

## Pantallas del dashboard

1. **Live monitor** — grid de agentes BPO con sesión activa, intención
   inferida en tiempo real, indicador de estado, tiempo en gestión actual

2. **Agente detail** — línea de tiempo semántica del día, IntentRecords
   expandibles, métricas de fricción, heatmap de apps usadas por hora

3. **Anomaly center** — feed de anomalías con severidad, screenshot del
   momento exacto, contexto de 10 eventos antes/después, resolución con nota

4. **Report builder** — generación bajo demanda de reportes por período,
   agente o tipo de gestión, exportable a PDF

5. **Semantic search** — consulta en lenguaje natural sobre todo el historial
   del equipo, resultados con contexto y línea de tiempo

6. **Agent console** — estado en tiempo real de los 6 agentes del sistema,
   logs streaming, panel de intervención manual, health checks

7. **Settings** — configuración por tenant: whitelist de apps/URLs,
   umbrales de anomalía, política de capturas, retención de datos

---

## Restricciones absolutas

- Nunca almacenar contenido del portapapeles, solo metadatos
- Nunca almacenar capturas de pantalla sin el flag `anomaly_triggered: true`
  o autorización explícita del supervisor
- Nunca exponer datos de un agente a otro agente BPO (aislamiento por tenant)
- Nunca hacer llamadas LLM síncronas en el hilo principal de captura
- Todo dato que sale del equipo local va cifrado (AES-256 en tránsito)
- Los logs de terminal en el dashboard solo son visibles con rol `admin`
  o `developer` — nunca al supervisor estándar
```

---

Ese prompt lo puedes usar como:

- Contexto de sistema para cualquier sesión de código con Claude o cualquier otro LLM
- Documento de referencia para el diseñador (la sección visual es autocontenida)
- Briefing técnico para un desarrollador que entre al proyecto
- Base para el README principal del repositorio

¿Arrancamos con el código ahora? Si me dices por dónde empezar (orquestador, agente de captura, la API, o el dashboard) lo construimos módulo por módulo.

## User:

> 6/8/2026 15:28:42

puedo usar ese prompt que me diste para el diseño en lovable?

## Assistant:

> 6/8/2026 15:29:09

Sí, puedes usarlo directamente en Lovable. Algunas recomendaciones para que te dé el mejor resultado:

---

## Lo que funciona tal cual

Copia y pega la sección **"Diseño visual — reglas obligatorias"** y **"Pantallas del dashboard"** completas. Lovable entiende bien instrucciones de colores, animaciones con Framer Motion y TailwindCSS porque genera exactamente ese stack.

---

## Lo que debes adaptar

Lovable es solo frontend, así que elimina todo lo que sea backend (Python, FastAPI, agentes, base de datos). Si lo dejas, lo ignorará o intentará simularlo con mocks y puede confundirse.

El prompt optimizado para Lovable sería este:

---

```
Diseña el dashboard completo de WorkMemory, una plataforma de inteligencia
operativa para call centers y BPO.

Stack: React 18 + TypeScript + TailwindCSS + Framer Motion + Recharts.

## Estética
Dark mode nativo. Paleta:
- Primario: púrpura #7F77DD → #3C3489
- Secundario: teal #1D9E75 → #085041
- Alerta: ámbar #EF9F27
- Error: coral #D85A30
- Fondos: #1a1a1f → #0d0d10

Degradados:
- Header: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%)
- Cards de agentes: borde con gradiente púrpura → teal animado
- Gráficas de área: fill degradado desde color de línea a transparente

Animaciones con Framer Motion:
- Transiciones 200-400ms, easeOut en entradas, spring en interactivos
- Datos en tiempo real con AnimatePresence + slide-in desde abajo
- Métricas numéricas con contador animado al actualizar
- Agentes activos con pulso CSS en tiempo real
- Stagger animation en listas de resultados

## Pantallas a diseñar

1. Live monitor — grid de agentes BPO activos con: nombre, intención
   inferida en tiempo real, app activa, tiempo en gestión actual,
   indicador de estado (activo/inactivo/anomalía)

2. Agente detail — línea de tiempo vertical del día con IntentRecords
   expandibles, heatmap de apps por hora, métricas de fricción,
   gráfica de AHT por gestión

3. Anomaly center — feed de anomalías con severidad codificada por color
   (low=teal, medium=ámbar, high=coral, critical=rojo pulsante),
   screenshot expandible en modal con zoom animado, botón de resolución

4. Report builder — selector de período y agente, preview del reporte
   generado, botón de exportar PDF

5. Semantic search — barra de búsqueda con placeholder que rota entre
   ejemplos de consultas, resultados con cards animadas en stagger,
   cada resultado muestra fecha, intención y apps involucradas

6. Agent console — 6 tarjetas de agentes del sistema (Vision, Input,
   Window, Intent, Anomaly, Report), cada una con: estado activo/inactivo,
   último evento procesado, latencia, botón de activación manual.
   Panel de terminal retráctil con logs en tiempo real, fondo negro,
   font monospace, scroll automático

7. Settings — toggles y campos para: whitelist de apps y URLs,
   umbrales de anomalía por severidad, política de capturas de pantalla
   (silenciosa / notificar agente / notificar ambos), retención de datos

## Componentes reutilizables
- AgentStatusCard: borde animado que pulsa cuando está activo
- IntentTimeline: línea de tiempo con cards expandibles y animación escalonada
- AnomalyFeed: stream con severidad por color y screenshot en modal
- SemanticSearchBar: placeholder rotativo, resultados con stagger
- TerminalPanel: panel retráctil, monospace, scroll automático
- MetricCounter: número que anima al cambiar de valor

## Navegación
Sidebar izquierdo colapsable con iconos y labels. Ruta activa resaltada
con gradiente púrpura. Transición de página con Framer Motion fadeIn.

## Datos
Usa datos mock realistas de un call center: nombres de agentes,
gestiones como "registro de caso CRM", "verificación de factura",
"actualización de datos cliente". Apps como Salesforce, SAP, softphone,
Chrome.
```

---

## Tips adicionales para Lovable

Después de que genere el primer resultado, haz prompts de refinamiento uno por pantalla, por ejemplo: *"mejora la pantalla de Anomaly center, quiero que el modal del screenshot tenga más profundidad visual y que el feed tenga mejor separación entre items"*. Lovable responde mejor a iteraciones pequeñas que a prompts gigantes de una sola vez.

Si quieres que las animaciones queden realmente bien, pídele explícitamente: *"usa Framer Motion en todos los elementos interactivos, no CSS puro"*.

¿Quieres que también te prepare el prompt para cuando vayas a construir el backend en Python?

## User:

> 6/9/2026 9:15:26

listo, ya hicimos el front, ahora pasemos a lo interno, backend. Antes de comenzar quiero dejar bien estructurado el funcionamiento de como el modelo va a recibir la informacion y que tipo de informacion va a recibir para que asi se pueda pasar a un reporte, o simplemente para el tema de como hace la inferencia y como asimilarlo al modelo de negocio para que asi sepa como tomarlo y cual tomar

## Assistant:

> 6/9/2026 9:15:43

Buena decisión pausar aquí. Esta es la parte más crítica del sistema — si el modelo recibe basura, infiere basura. Necesito entender el flujo exacto antes de diseñar los schemas:

## User:

> 6/9/2026 13:43:37

P: ¿Cómo se define el inicio y fin de una “gestión” en el BPO objetivo?
R: Ambos: hay gestiones con inicio/fin claro y otras que se infieren

P: ¿El BPO tiene procesos estandarizados y conocidos de antemano?
R: No, WorkMemory debe inferir el tipo de gestión solo desde la actividad

P: ¿Cuándo debe ocurrir la inferencia del modelo?
R: Inferencia al cerrar cada gestión (por evento)

## Assistant:

> 6/9/2026 13:44:34

Perfecto. Esas tres respuestas definen todo el diseño del pipeline. Aquí está la arquitectura completa de cómo el modelo recibe, procesa e infiere la información:

---

## 1. Qué información recibe el modelo y en qué formato

El modelo nunca recibe eventos crudos directamente. Recibe un **contexto de sesión enriquecido** que el pipeline construye antes de llamar al LLM. La diferencia es crítica:

```
RawEvents (lo que captura el agente)
        ↓
Session Builder (agrupa y enriquece)
        ↓
Structured Context (lo que recibe el modelo)
        ↓
IntentRecord (lo que devuelve el modelo)
```

---

## 2. El Session Builder — cómo se construye el contexto

Antes de llamar al LLM, el pipeline construye este objeto:

```python
class SessionContext(BaseModel):
    # Identidad
    session_id: str
    agent_id: str
    timestamp_start: datetime
    timestamp_end: datetime
    duration_seconds: int

    # Señales de inicio/fin
    trigger_type: Literal["explicit", "inferred"]
    # explicit = se detectó evento de cierre claro (ventana cerrada, URL de confirmación)
    # inferred = gap de inactividad o cambio de contexto total

    # Actividad cronológica resumida
    app_sequence: list[AppEvent]
    # Ej: [VSCode 3min, Chrome 2min, Salesforce 8min, Chrome 1min, Salesforce 4min]

    url_sequence: list[UrlEvent]
    # Solo dominios + tiempo, nunca rutas completas con datos sensibles

    file_sequence: list[FileEvent]
    # Solo nombres y extensiones, nunca contenido

    clipboard_events: list[ClipboardMeta]
    # Solo: origen, tipo (texto/imagen), tamaño en chars, destino

    # Señales de comportamiento
    input_patterns: InputSummary
    # clicks_per_minute, typing_speed_wpm, focus_switches, scroll_depth_avg

    # Pantalla (solo si Vision Agent capturo frames)
    screen_descriptions: list[str]
    # Descripciones semánticas de lo visible, nunca imágenes crudas al LLM de texto

    # Contexto de negocio
    known_process_hints: list[str]
    # Apps reconocidas de la whitelist del tenant: ["Salesforce CRM", "SAP ERP"]

    # Anomalías detectadas durante la sesión
    anomalies_in_session: list[AnomalyFlag]
    # Si hubo algo raro durante la gestión

    # Historial reciente del agente (últimas 3 sesiones)
    recent_sessions_summary: list[str]
    # Para que el modelo tenga contexto de qué estaba haciendo antes
```

---

## 3. El prompt estructurado que recibe el modelo

Este es el prompt exacto que se construye dinámicamente y se envía a Claude:

```python
INTENT_INFERENCE_PROMPT = """
Eres un analista experto en operaciones BPO. Analiza la siguiente sesión de 
trabajo de un agente y extrae la intención operativa detrás de su actividad.

## Sesión de trabajo
- Duración: {duration_seconds}s
- Tipo de cierre: {trigger_type}
- Aplicaciones usadas (en orden): {app_sequence}
- URLs visitadas (dominios): {url_sequence}
- Archivos involucrados: {file_sequence}
- Patrones de comportamiento: {input_patterns}
- Descripción visual de pantalla: {screen_descriptions}
- Apps corporativas reconocidas: {known_process_hints}
- Anomalías detectadas: {anomalies_in_session}
- Contexto previo del agente: {recent_sessions_summary}

## Reglas de inferencia
1. Infiere el tipo de gestión SOLO desde la actividad observada. No asumas 
   procesos estándar — dedúcelos.
2. Si la evidencia es insuficiente, refleja esa incertidumbre en el campo 
   confidence (< 0.5) y explícalo en inference_notes.
3. El campo goal debe ser específico y operativo: "Verificó factura del 
   cliente en SAP y actualizó estado en CRM" es correcto. 
   "Trabajó en el sistema" no es aceptable.
4. Detecta fricción real: si el agente visitó la misma app 3+ veces en 
   secuencia, eso es fricción. Si copió datos entre dos apps, eso es 
   fricción de integración.
5. El outcome se infiere por señales: cierre de formulario, URL de 
   confirmación, inactividad post-gestión = resolved. Loop de apps sin 
   cierre = unresolved.

## Formato de respuesta
Responde ÚNICAMENTE con este JSON, sin texto adicional, sin markdown:

{
  "session_type": "string — tipo de gestión inferido (ej: registro_caso, verificacion_factura, actualizacion_datos)",
  "goal": "string — objetivo principal específico y operativo",
  "subprocess_detected": ["lista de subpasos detectados en orden"],
  "friction_points": [
    {
      "type": "app_switching | integration_gap | search_loop | data_copy",
      "detail": "descripción específica",
      "estimated_seconds_lost": 0
    }
  ],
  "problem": "string | null — obstáculo detectado",
  "outcome": "resolved | unresolved | partial | unknown",
  "outcome_signals": ["señales que justifican el outcome inferido"],
  "confidence": 0.0,
  "inference_notes": "string — qué evidencia fue determinante o qué fue ambiguo",
  "tags": ["etiquetas semánticas reutilizables"],
  "optimization_hint": "string | null — sugerencia concreta de mejora del proceso"
}
"""
```

---

## 4. Cómo se detecta el cierre de una gestión

Como hay gestiones con cierre explícito e inferido, el Session Controller maneja ambos casos:

```python
class SessionCloseTrigger(Enum):
    # Cierres explícitos — alta confianza
    WINDOW_CLOSED = "window_closed"         # App principal cerrada
    CONFIRMATION_URL = "confirmation_url"   # URL con patrón de confirmación
    FORM_SUBMITTED = "form_submitted"       # POST detectado en browser
    CALL_ENDED = "call_ended"               # Softphone colgó

    # Cierres inferidos — confianza media
    INACTIVITY_GAP = "inactivity_gap"       # +8 min sin eventos
    CONTEXT_SWITCH = "context_switch"       # Cambio total de apps sin relación
    IDLE_PATTERN = "idle_pattern"           # Mouse/teclado inactivo + app blur

class SessionController:
    def evaluate_close(self, events: list[RawEvent]) -> SessionCloseTrigger | None:
        # 1. Revisa triggers explícitos primero
        if self._detect_confirmation_url(events): 
            return SessionCloseTrigger.CONFIRMATION_URL
        if self._detect_window_close(events):
            return SessionCloseTrigger.WINDOW_CLOSED

        # 2. Si no hay cierre explícito, evalúa inferidos
        if self._detect_inactivity(events, threshold_minutes=8):
            return SessionCloseTrigger.INACTIVITY_GAP
        if self._detect_context_switch(events):
            return SessionCloseTrigger.CONTEXT_SWITCH

        return None  # Sesión aún abierta
```

El `trigger_type` del cierre se pasa al modelo para que ajuste su nivel de confianza — una sesión con cierre explícito merece más confianza en el `outcome` que una inferida por inactividad.

---

## 5. Cómo el IntentRecord se mapea al modelo de negocio

El modelo devuelve el JSON crudo. El `IntentController` lo procesa y lo enriquece con contexto de negocio antes de guardarlo:

```python
class IntentController:
    def process_intent(self, raw_intent: dict, session: SessionContext) -> IntentRecord:

        # 1. Normaliza el session_type a un catálogo dinámico
        # No hay tipos fijos — el catálogo crece con cada gestión nueva inferida
        session_type = self.catalog.normalize_or_create(raw_intent["session_type"])

        # 2. Calcula métricas derivadas
        total_friction_seconds = sum(f["estimated_seconds_lost"] for f in raw_intent["friction_points"])
        friction_ratio = total_friction_seconds / session.duration_seconds

        # 3. Actualiza el perfil del agente
        self.agent_profile.update(
            agent_id=session.agent_id,
            session_type=session_type,
            outcome=raw_intent["outcome"],
            friction_ratio=friction_ratio
        )

        # 4. Evalúa si genera alerta para el supervisor
        if friction_ratio > 0.4:  # más del 40% del tiempo fue fricción
            self.alert_service.push(
                type="high_friction",
                agent_id=session.agent_id,
                detail=raw_intent["friction_points"]
            )

        # 5. Construye el IntentRecord final
        return IntentRecord(
            **raw_intent,
            session_id=session.session_id,
            agent_id=session.agent_id,
            duration_seconds=session.duration_seconds,
            trigger_type=session.trigger_type,
            friction_ratio=friction_ratio,
            business_context=session.known_process_hints
        )
```

---

## 6. Cómo se adapta al modelo de negocio de cada BPO

Como WorkMemory no conoce los procesos de antemano, construye su propio catálogo dinámico por tenant:

```python
# El catálogo empieza vacío y crece con cada inferencia
TENANT_CATALOG = {
    "tenant_id": "bpo_cliente_x",
    "session_types": {
        "registro_caso": {
            "count": 847,
            "avg_duration_seconds": 420,
            "avg_friction_ratio": 0.23,
            "common_apps": ["Salesforce", "SAP", "Chrome"],
            "common_friction": ["integration_gap entre SAP y CRM"],
            "benchmark_good": 300,   # segundos — p25 de duración
            "benchmark_bad": 600     # segundos — p75 de duración
        },
        "verificacion_factura": { ... },
        "actualizacion_datos": { ... }
    }
}
```

Con el tiempo, este catálogo se convierte en el **benchmark del negocio**. Una gestión de `registro_caso` que tarda 800 segundos se compara automáticamente contra el promedio histórico del propio BPO, no contra un estándar genérico.

---

## 7. Qué va al reporte

El reporte toma del catálogo + los IntentRecords del período y construye esto:

```
Por agente:
- Gestiones completadas por tipo, con duración vs benchmark
- Fricción acumulada en segundos y como % del tiempo total
- Tipos de gestión donde tiene mejor y peor desempeño
- Tendencia vs semana anterior

Por proceso:
- Qué tipos de gestión tienen más fricción sistémica
- Qué apps generan más saltos innecesarios
- Dónde el proceso se puede optimizar con mayor impacto

Por turno:
- Comparativa entre agentes para el mismo tipo de gestión
- Anomalías del período con resolución
- Gestiones incompletas sin outcome: resolved
```

---

Ese es el contrato completo entre los datos, el modelo y el negocio. ¿Arrancamos con el código del pipeline — Session Builder + prompt constructor + llamada a Claude API — o primero quieres ver el schema de base de datos completo?

## User:

> 6/10/2026 10:39:50

ahora por favor ya teniendo en cuenta TODO el contexto de lo que quiero, hazme por favor la estructura del proyecto para poder empezar en codigo

## Assistant:

> 6/10/2026 10:40:41

Aquí está la estructura completa del proyecto:

```
workmemory/
│
├── 📁 agents/                          # Agentes especializados de captura e inferencia
│   ├── __init__.py
│   ├── base_agent.py                   # Clase base — interfaz común para todos los agentes
│   ├── orchestrator.py                 # Orquestador central — activa/pausa agentes
│   ├── vision_agent.py                 # Lectura de pantalla (mss + OCR + Claude Vision)
│   ├── input_agent.py                  # Teclado y mouse (pynput)
│   ├── window_agent.py                 # Apps activas y tiempo de foco
│   ├── event_agent.py                  # URLs, archivos, clipboard, git
│   ├── intent_agent.py                 # Inferencia de intención con Claude API
│   ├── anomaly_agent.py                # Detección de anomalías + captura forense
│   ├── report_agent.py                 # Generación de reportes estructurados
│   └── query_agent.py                  # Búsqueda semántica en lenguaje natural
│
├── 📁 models/                          # Schemas Pydantic — solo estructura, cero lógica
│   ├── __init__.py
│   ├── raw_event.py                    # RawEvent — evento crudo del sistema
│   ├── session_context.py              # SessionContext — contexto enriquecido pre-LLM
│   ├── intent_record.py                # IntentRecord — salida del modelo
│   ├── anomaly_record.py               # AnomalyRecord — evento forense
│   ├── screen_record.py                # ScreenRecord — captura + descripción visual
│   ├── report_record.py                # ReportRecord — reporte estructurado final
│   ├── agent_profile.py                # AgentProfile — perfil acumulado por trabajador
│   └── tenant_catalog.py              # TenantCatalog — catálogo dinámico de gestiones
│
├── 📁 controllers/                     # Lógica de negocio — nunca toca UI ni DB directo
│   ├── __init__.py
│   ├── capture_controller.py           # Coordina agentes de captura
│   ├── session_controller.py           # Detecta inicio/fin de gestión, construye contexto
│   ├── intent_controller.py            # Procesa IntentRecord, actualiza catálogo
│   ├── anomaly_controller.py           # Evalúa severidad, dispara capturas
│   └── report_controller.py           # Orquesta generación de reportes
│
├── 📁 services/                        # Acceso a datos e integraciones externas
│   ├── __init__.py
│   ├── db_service.py                   # SQLite — RawEvents, metadata relacional
│   ├── vector_service.py               # LanceDB — IntentRecords embebidos
│   ├── llm_service.py                  # Claude API — inferencia e intención
│   ├── vision_service.py               # Claude Vision API — interpretación de pantalla
│   ├── embedding_service.py            # nomic-embed-text vía Ollama
│   ├── screenshot_service.py           # mss — captura y almacenamiento forense
│   ├── websocket_service.py            # Push de eventos al dashboard en tiempo real
│   └── alert_service.py               # Notificaciones al supervisor
│
├── 📁 api/                             # FastAPI — expone todo al dashboard
│   ├── __init__.py
│   ├── main.py                         # Entry point de la API
│   ├── dependencies.py                 # Inyección de dependencias globales
│   │
│   ├── 📁 routes/
│   │   ├── __init__.py
│   │   ├── agents.py                   # GET /agents — estado de agentes en tiempo real
│   │   ├── sessions.py                 # GET /sessions — historial de gestiones
│   │   ├── intents.py                  # GET /intents — IntentRecords por agente/período
│   │   ├── anomalies.py                # GET/PATCH /anomalies — feed y resolución
│   │   ├── reports.py                  # POST /reports — generación bajo demanda
│   │   ├── query.py                    # POST /query — búsqueda semántica
│   │   ├── agents_control.py           # POST /agents/{id}/pause|resume — control manual
│   │   └── health.py                   # GET /health — estado del sistema completo
│   │
│   └── 📁 middleware/
│       ├── __init__.py
│       ├── error_handler.py            # Handler global de excepciones
│       ├── auth.py                     # JWT + roles (admin, supervisor, developer)
│       └── tenant.py                  # Aislamiento de datos por tenant
│
├── 📁 core/                            # Infraestructura transversal
│   ├── __init__.py
│   ├── config.py                       # Configuración por tenant + variables de entorno
│   ├── logger.py                       # Loguru — niveles, rotación, formato estructurado
│   ├── error_bus.py                    # Bus central de errores — captura, clasifica, enruta
│   ├── event_queue.py                  # Cola asíncrona de RawEvents (asyncio + Redis)
│   ├── health.py                       # Health checks de todos los agentes y servicios
│   └── constants.py                   # Enums y constantes globales del sistema
│
├── 📁 pipeline/                        # Pipeline de inferencia — el corazón del sistema
│   ├── __init__.py
│   ├── session_builder.py              # Construye SessionContext desde RawEvents
│   ├── prompt_builder.py               # Construye el prompt estructurado para el LLM
│   ├── intent_parser.py                # Parsea y valida el JSON que devuelve el LLM
│   ├── catalog_updater.py              # Actualiza el TenantCatalog con cada nueva gestión
│   └── benchmark_engine.py            # Compara gestiones contra el histórico del tenant
│
├── 📁 storage/                         # Capa de persistencia
│   ├── __init__.py
│   ├── 📁 migrations/                  # Alembic — migraciones de SQLite
│   │   ├── env.py
│   │   └── versions/
│   ├── database.py                     # Conexión y sesiones de SQLite
│   ├── vector_store.py                 # Conexión y operaciones de LanceDB
│   ├── 📁 repositories/                # Un repositorio por modelo — patrón Repository
│   │   ├── __init__.py
│   │   ├── raw_event_repo.py
│   │   ├── intent_record_repo.py
│   │   ├── anomaly_record_repo.py
│   │   ├── session_repo.py
│   │   ├── agent_profile_repo.py
│   │   └── tenant_catalog_repo.py
│   └── 📁 screenshots/                # Almacenamiento local de capturas forenses
│       └── .gitkeep
│
├── 📁 workers/                         # Celery — tareas asíncronas y schedulers
│   ├── __init__.py
│   ├── celery_app.py                   # Configuración de Celery + Redis
│   ├── inference_worker.py             # Worker de inferencia LLM (por evento)
│   ├── report_worker.py                # Worker de reportes (scheduled)
│   ├── embedding_worker.py             # Worker de embeddings (batch)
│   └── cleanup_worker.py              # Worker de retención de datos (scheduled)
│
├── 📁 tests/                           # Tests por capa
│   ├── __init__.py
│   ├── 📁 unit/
│   │   ├── test_session_builder.py
│   │   ├── test_prompt_builder.py
│   │   ├── test_intent_parser.py
│   │   └── test_anomaly_controller.py
│   ├── 📁 integration/
│   │   ├── test_pipeline.py            # RawEvents → IntentRecord end to end
│   │   ├── test_api_sessions.py
│   │   └── test_websocket.py
│   └── 📁 fixtures/
│       ├── raw_events.json             # Eventos de prueba realistas
│       └── intent_records.json        # IntentRecords de referencia
│
├── 📁 scripts/                         # Utilidades de desarrollo y operaciones
│   ├── seed_db.py                      # Poblar DB con datos mock para desarrollo
│   ├── run_agent.py                    # Lanzar el agente de captura en modo dev
│   ├── simulate_session.py             # Simular una sesión BPO completa sin hardware
│   └── export_catalog.py              # Exportar catálogo de gestiones a JSON
│
├── 📁 docs/                            # Documentación técnica
│   ├── architecture.md
│   ├── agent_roles.md
│   ├── pipeline_flow.md
│   ├── api_reference.md
│   └── deployment.md
│
├── .env.example                        # Variables de entorno necesarias
├── .gitignore
├── alembic.ini                         # Config de migraciones
├── docker-compose.yml                  # Redis + servicios de infraestructura
├── pyproject.toml                      # Dependencias con Poetry
├── README.md
└── main.py                             # Entry point principal — lanza orquestador + API
```

---

## Orden de construcción recomendado

El orden importa. Cada capa depende de la anterior:

```
Semana 1    core/ + models/ + storage/
            └── Config, logger, error bus, schemas Pydantic, SQLite + LanceDB

Semana 2    agents/ de captura + event_queue
            └── vision, input, window, event — sin LLM todavía, solo RawEvents

Semana 3    pipeline/ + workers/
            └── session_builder, prompt_builder, intent_parser, inference_worker

Semana 4    controllers/ + services/ LLM
            └── session_controller, intent_controller, llm_service, vector_service

Semana 5    api/ completa + websocket_service
            └── Todos los routes, middleware, auth, health checks

Semana 6    anomaly_agent + report_agent + query_agent
            └── Los tres agentes que dependen de IntentRecords existentes

Semana 7    tests/ + scripts/
            └── Fixtures realistas, simulate_session, seed_db para demos
```

---

## El archivo más importante para empezar

`core/config.py` — todo el sistema lo importa. Es lo primero que se escribe:

```python
from pydantic_settings import BaseSettings
from typing import Literal

class Settings(BaseSettings):
    # LLM
    ANTHROPIC_API_KEY: str
    LLM_MODEL: str = "claude-sonnet-4-20250514"
    OLLAMA_BASE_URL: str = "http://localhost:11434"
    EMBEDDING_MODEL: str = "nomic-embed-text"

    # Captura
    SCREEN_CAPTURE_FPS: int = 2
    INACTIVITY_THRESHOLD_MINUTES: int = 8
    CLIPBOARD_MAX_CHARS_LOG: int = 500

    # Anomalías
    SCREENSHOT_MODE: Literal["silent", "notify_agent", "notify_both"] = "silent"
    AHT_DEVIATION_MULTIPLIER: float = 2.0
    FRICTION_RATIO_ALERT_THRESHOLD: float = 0.4

    # Storage
    SQLITE_PATH: str = "./storage/workmemory.db"
    LANCEDB_PATH: str = "./storage/vector"
    SCREENSHOTS_PATH: str = "./storage/screenshots"
    RETENTION_DAYS: int = 30

    # API
    API_HOST: str = "0.0.0.0"
    API_PORT: int = 8000
    JWT_SECRET: str
    JWT_EXPIRE_HOURS: int = 8

    # Redis / Celery
    REDIS_URL: str = "redis://localhost:6379/0"

    # Tenant
    TENANT_ID: str
    WHITELISTED_DOMAINS: list[str] = []
    WHITELISTED_APPS: list[str] = []

    class Config:
        env_file = ".env"

settings = Settings()
```

---

¿Arrancamos con la **Semana 1** — `core/` + `models/` + `storage/` — o prefieres que empecemos por los agentes de captura para tener algo funcionando en pantalla rápido?

Powered by Claude Exporter (https://www.ai-chat-exporter.net)
