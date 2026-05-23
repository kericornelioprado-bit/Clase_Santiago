# Curso de Vibecoding — Santiago

**Formato:** 5 sesiones de 1 hora, online.

**Cómo trabajamos:** Te explico los conceptos y te muestro en vivo cómo se ven los flujos reales desde mi propio trabajo. Observas, preguntas lo que necesites aclarar, y al salir tienes claro qué hace cada herramienta y cómo se usa en serio. Lo que decidas aplicar en tu plataforma o en tu día a día lo trabajas tú entre clase y clase — el curso te da el mapa, no te lo recorre.

---

## Clase 1 — Panorama, modelo mental y anatomía de un buen prompt

**Tema:** El mapa del ecosistema y cómo se construye un prompt que sí jala.

**Contenido:**
- El loop de vibecoding: Intención → Especificación → Generación → Revisión → Iteración.
- Mapa del ecosistema y cuándo usar cada cosa:
  - App builders (Lovable, Bolt, v0).
  - IDEs con IA (Cursor, Antigravity).
  - Coding agents de terminal (Claude Code, Gemini CLI).
  - Doing agents (Anton, Cowork).
- Anatomía de un prompt: intención, contexto, restricciones, formato de salida, ejemplos.
- Errores típicos que hacen que un prompt falle.

**Demos en vivo:**
- Misma tarea con un prompt malo vs. uno bien estructurado, lado a lado.
- Cómo reescribo un prompt cuando el primer intento no me sirvió.

---

## Clase 2 — Claude Code: el coding agent de terminal en serio

**Tema:** Qué es un agente de terminal, cómo se ve trabajar con uno, y por qué juega en otra liga que un app builder.

**Contenido:**
- Cuándo Lovable y compañía se quedan cortos.
- Instalación y setup.
- Flujo básico: descripción → plan → ejecución → revisión.
- Git como red de seguridad: por qué con buen control de versiones puedes iterar rápido sin miedo.
- Comandos y atajos del día a día.

**Demos en vivo:**
- Una sesión real de Claude Code sobre un proyecto mío: pedirle un cambio, revisar el plan, aceptarlo o rechazarlo, commit.
- Caso de cuando se equivoca y cómo lo corrijo sin frustrarme.

---

## Clase 3 — Context engineering: CLAUDE.md, Skills y el problema del "drift"

**Tema:** La meta-habilidad de vibecoding. Cómo se le da contexto a un agente para que no improvise mal.

**Contenido:**
- Por qué el contexto es la diferencia entre un agente útil y uno que te hace perder horas.
- Anatomía de un buen CLAUDE.md: reglas, convenciones, restricciones, "no toques esto".
- Skills (SKILL.md): qué son, cuándo escribir una, cómo se reutilizan.
- El problema del "drift" y cómo se previene.

**Demos en vivo:**
- Un CLAUDE.md real de uno de mis proyectos, comentado línea por línea.
- Una Skill que uso a diario, explicando por qué cada parte está donde está.

---

## Clase 4 — MCPs: conectar la IA a herramientas reales

**Tema:** Cómo la IA pasa de "asistente que conversa" a "asistente que ejecuta sobre tus sistemas".

**Contenido:**
- Qué es MCP (Model Context Protocol) y por qué amplía radicalmente lo que se puede automatizar.
- Catálogo de MCPs útiles: Slack, WhatsApp, Google Calendar/Drive, Notion, GitHub, bases de datos.
- Cómo se instala y configura un MCP en Claude Code.
- Riesgos a tener claros: permisos, secretos, qué no conectar a la ligera.

**Demos en vivo:**
- Instalación de un MCP de cero y primer uso.
- Un caso real donde un MCP cambia el flujo de trabajo — no demo de juguete.

---

## Clase 5 — Agentes: el patrón "talk to your data"

**Tema:** Qué es un agente, qué lo distingue de un chatbot, y cómo se ve uno construido para hablar con datos.

**Contenido:**
- Diferencia entre LLM, asistente y agente.
- El patrón "talk to your data": qué piezas tiene y cómo se conectan.
- Arquitectura básica: fuente de datos → capa de acceso → agente → interfaz.
- Riesgos: permisos, alucinaciones sobre datos, validación.
- Hacia dónde se puede llevar: agentes autónomos con memoria, agentes que viven en Slack/WhatsApp, agentes con cron.

**Demos en vivo:**
- Un agente "talk to your data" que armé en mi trabajo, mostrando la arquitectura por dentro.
- Cómo se ve interactuar con él y dónde están sus límites.

---

## Lo que dejamos para sesiones futuras

- Testing y calidad de código generado por IA.
- Anton para flujos de análisis y reportes sin código.
- Hermes Agent: agentes autónomos con memoria, cron, multi-plataforma.
- Despliegue y escalamiento.
- Generación de imágenes y video.
