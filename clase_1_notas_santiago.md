# Clase 1 — Panorama, modelo mental y anatomía de un buen prompt

> Notas para releer después de la sesión. No reemplazan la clase: la complementan.

---

## 1. El modelo mental: el loop de vibecoding

Vibecoding no es "le pides algo a la IA y te lo escupe perfecto a la primera". Es un ciclo de cinco pasos que se repite hasta que el resultado sirve:

**Intención → Especificación → Generación → Revisión → Iteración**

- **Intención** — qué quieres lograr en el fondo. Esto no es el prompt todavía, es la idea.
- **Especificación** — traduces la intención a algo concreto. Aquí escribes el prompt.
- **Generación** — la IA produce su intento.
- **Revisión** — lees, pruebas, juzgas. ¿Sirve? ¿Falla? ¿Dónde?
- **Iteración** — ajustas y vuelves a pedir. O tiras lo anterior y empiezas con un prompt nuevo.

La trampa más común es saltarse "Intención" y "Especificación" y empezar a teclear de una. El resultado: prompts vagos, salidas mediocres, y la sensación de que "la IA no entiende". En realidad, casi siempre el problema no es la IA — es que le pediste mal.

Una heurística útil: trabajar bien con IA es 80% pensar antes de pedir, 20% lo demás.

---

## 2. El mapa del ecosistema

Hay muchas herramientas y crecen cada semana. Lo importante no es conocerlas todas, sino entender en qué categoría cae cada una para no usar un martillo donde necesitas un destornillador.

### App builders — Lovable, Bolt, v0
Les describes una app o página web y te la construyen con interfaz visual. Ves un resultado tangible en minutos. Son el punto de entrada más amigable para alguien que no programa. Excelentes para prototipos, landing pages, MVPs visuales o ideas que quieres "ver" antes de invertir en algo más serio.

Limitación: cuando el proyecto crece o necesitas algo muy a la medida, se quedan cortos.

### IDEs con IA — Cursor, Antigravity
Son editores de código tradicionales con IA integrada. Útiles cuando ya hay código de por medio y quieres editar o refinar con asistencia. Viven más cerca del mundo del programador — probablemente no es tu primera herramienta.

### Coding agents de terminal — Claude Code, Gemini CLI
Viven en la terminal (la "pantalla negra"), trabajan directamente sobre tus archivos y pueden ejecutar tareas complejas de varios pasos sin que las dirijas una por una. Es el siguiente nivel cuando los app builders se quedan cortos: más poder, más control, más responsabilidad.

Aquí nos metemos en serio en la **Clase 2**.

### Doing agents — Anton, Cowork
No programan: ejecutan tareas. Investigan, llenan documentos, mandan correos, generan reportes, hacen análisis. Para trabajo de oficina automatizable que no requiere construir software, sólo "hacer cosas".

### Regla rápida para elegir

| Quieres... | Mira primero... |
|---|---|
| Una app visible rápido | App builder |
| Automatizar una tarea de oficina | Doing agent |
| Algo a la medida, con control real | Coding agent de terminal |
| Editar código existente | IDE con IA |

---

## 3. Anatomía de un buen prompt

Un prompt sólido tiene **cinco partes**. No todas tienen que aparecer literalmente cada vez, pero los buenos prompts las consideran antes de teclear:

1. **Intención** — qué quieres que pase al final.
2. **Contexto** — qué necesita saber la IA para no inventar.
3. **Restricciones** — qué NO debe hacer, qué límites respetar.
4. **Formato de salida** — cómo quieres recibir la respuesta.
5. **Ejemplos** — muestras de "así sí" o "así no".

### Ejemplo lado a lado

**❌ Prompt malo**

> "Hazme un correo para hacerle seguimiento a un cliente."

Demasiado vago. La IA va a improvisar todo — tono, contexto, longitud, propósito. Lo que regrese va a tener un 5% de probabilidad de servir tal cual.

**✅ Prompt bueno**

> "Necesito un correo de follow-up para mi cliente Juan, de ACME Corp.
>
> **Contexto**: hace 2 semanas le mandé una propuesta de consultoría por $5k mensuales. No ha respondido. Somos amigos desde la universidad pero esto es relación profesional.
>
> **Objetivo**: retomar la conversación sin sonar desesperado ni presionar.
>
> **Restricciones**: máximo 4 párrafos, sin emojis, no preguntar directamente si vio la propuesta, no ofrecer descuentos.
>
> **Formato**: dame 2 versiones — una más cálida apoyada en la amistad, otra más estrictamente profesional.
>
> **Tono**: cordial, seguro, no necesitado."

Los cinco elementos están ahí. La IA ahora tiene a dónde apuntar y qué evitar. La probabilidad de que la primera versión sirva sube dramáticamente.

### Heurística práctica

Si tu prompt cabe en un tweet y la tarea no es trivial, probablemente está incompleto.

---

## 4. Errores típicos que arruinan un prompt

- **Demasiado vago.** "Mejóralo", "hazlo bonito", "más profesional". Sin referencia concreta, la IA adivina — y adivina mal.
- **Demasiadas cosas a la vez.** "Hazme la estrategia, el plan, el calendario y los correos". Divide. Cada tarea, su prompt.
- **Cero contexto.** La IA no sabe nada de tu negocio, tu cliente, tu producto. Si no se lo dices, se lo inventa.
- **Sin formato pedido.** Si no especificas, te puede contestar en un párrafo cuando querías una tabla, o al revés.
- **Pelearse con un prompt malo.** Si después de 2-3 intentos no sale, no insistas. Borra y reescribe desde cero con más estructura. Iterar sobre basura da basura mejor formateada.

---

## 5. Demos de la sesión

*Anclas para lo que vimos en vivo:*

- **Demo 1** — Misma tarea con un prompt malo vs. uno bien estructurado, lado a lado.
  *[Tus notas de lo que viste]*

- **Demo 2** — Cómo reescribo un prompt cuando el primero no sirve.
  *[Tus notas de lo que viste]*

---

## 6. Para la próxima clase

Entre clase y clase, dos cosas prácticas:

- **Observa tus propios prompts.** En tu día a día, cuando le pidas algo a una IA y el resultado falle, pregúntate: ¿le faltó intención, contexto, restricción, formato o ejemplo? Identificarlo es la mitad de arreglarlo.
- **Si quieres adelantar:** instala Claude Code. Lo vemos en serio en la Clase 2, pero tenerlo listo ahorra tiempo.

---

## Glosario rápido

- **Prompt** — la instrucción que le das a la IA.
- **Agente** — IA que no sólo conversa, también ejecuta acciones (lo vemos en la Clase 5).
- **Terminal** — interfaz de texto sin botones ni ventanas, donde escribes comandos. La "pantalla negra".
- **Iterar** — repetir el ciclo de pedir → revisar → ajustar hasta llegar al resultado.
