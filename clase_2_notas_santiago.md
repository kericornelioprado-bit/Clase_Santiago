# Clase 2 — Claude Code: el coding agent de terminal en serio

> Notas para releer después de la sesión. No reemplazan la clase: la complementan.

---

## 1. Por qué saltar de Lovable a Claude Code

Los app builders (Lovable, Bolt, v0) son fenomenales para empezar. Le describes una idea y ves algo funcionando en minutos. El problema empieza cuando el proyecto deja de ser un prototipo y se vuelve algo que importa.

Tres techos típicos:

- **Control limitado.** Lo que el builder no te deja tocar, no lo tocas. Si quieres algo que se sale de su molde, te frustras.
- **Caja negra.** No siempre sabes qué archivos existen, qué hace cada uno, dónde está la lógica. Cuando algo se rompe, no tienes a dónde meter las manos.
- **Tope de complejidad.** Funcionan increíble para una app de una pantalla. Para algo con varias piezas conectadas, base de datos seria, lógica de negocio, integraciones — empiezan a inventar, a olvidarse de cosas, a romper lo que ya funcionaba.

Claude Code juega en otra liga porque no es un constructor de apps con IA por encima. Es una IA que **trabaja directamente sobre tus archivos**, en tu computadora, como lo haría un programador. Eso te da control total y, paradójicamente, menos magia y más herramienta.

El precio: tienes que estar dispuesto a ver una pantalla negra con texto en lugar de una interfaz bonita con botones. Pero el techo se mueve muy arriba.

---

## 2. Qué es un coding agent de terminal

Tres ideas para fijar el modelo mental.

**Vive en tu computadora, sobre tus archivos.** No es una página web a la que entras. Lo instalas y lo abres desde la terminal, parado en la carpeta de un proyecto. Lo que ve es lo que hay en esa carpeta. Lo que cambia, lo cambia ahí.

**Ejecuta, no solo conversa.** Un chatbot te dice qué hacer. Un agente lo hace por ti: lee archivos, escribe archivos, corre comandos, instala cosas, prueba que algo funcione. Tú apruebas las acciones importantes, él las ejecuta.

**La terminal es la interfaz.** Sí, es texto sobre fondo negro. No, no es tan intimidante como parece — para esto que vamos a hacer, solo necesitas escribir frases en español y leer lo que te responde. La terminal solo es la "ventana" por donde le hablas.

Una analogía que ayuda: piénsalo como un programador junior remoto, muy rápido, al que le entregaste las llaves de un proyecto. Tú diriges, él ejecuta. Tu trabajo no es escribir código — es **decidir qué pedir, revisar lo que entregó, y corregir cuando algo no cuadra**.

---

## 3. Cómo se ve una sesión real

El loop básico, que se parece mucho al de la Clase 1 pero aterrizado:

**Descripción → Plan → Ejecución → Revisión**

1. **Descripción.** Le dices en lenguaje natural lo que quieres. *"Agrega un botón para exportar la tabla a CSV."*
2. **Plan.** Antes de tocar nada, te dice qué piensa hacer: qué archivos va a modificar, qué va a crear, qué dependencias va a instalar. Tú lees y decides.
3. **Ejecución.** Si aceptas, ejecuta. Vas viendo cada paso. Para cambios sensibles te pide permiso uno por uno; para cambios obvios, sigue de frente.
4. **Revisión.** Cuando termina, pruebas el resultado. Si sirve, lo guardas. Si no, le explicas qué falló y vuelve a intentarlo.

### Los dos modos que importan

Claude Code tiene dos formas de operar y se cambian con **Shift+Tab**.

- **Modo Plan.** Solo planea, no toca nada. Lee tu código, lo analiza, te propone qué haría y se queda esperando tu OK. Es el modo para cambios delicados o cuando no le tienes confianza todavía.
- **Modo Auto.** Ejecuta directo, pidiéndote permiso solo en los pasos importantes. Es el modo para cuando ya sabes lo que quieres y confías en que la tarea es relativamente acotada.

Heurística: **plan para cambios grandes o delicados; auto para tareas chiquitas o repetitivas que ya conoces**.

---

## 4. Git: la red de seguridad invisible

No vamos a meternos a comandos de Git en este curso, pero necesitas entender **por qué es la pieza que hace que todo esto sea seguro**.

Git es un sistema que guarda fotografías ("commits") del estado completo de tu proyecto en distintos momentos. Es como tener "Ctrl+Z" infinito y selectivo: puedes regresar a cualquier punto donde guardaste, comparar versiones, o tirar a la basura todo lo que hiciste en la última hora sin perder nada de antes.

**Por qué importa con un agente como Claude Code:**

Sin Git, dejar que una IA modifique tus archivos da pavor — porque si algo sale mal, no hay vuelta atrás. Con Git, puedes pedirle cosas atrevidas, dejar que experimente, ver qué pasa. Si rompe algo: regresas a la última fotografía y como si nada. Si te gusta el cambio: guardas una nueva fotografía y sigues.

Es lo que separa dos estados mentales:

- *"Tengo miedo de que me rompa algo."*
- *"Adelante, intenta. Si no jala, lo borro."*

El segundo es donde se hace el verdadero vibecoding. Y Git es lo que te lleva ahí. En este curso vamos a usar git "por debajo" — Claude Code se integra con él sin que tengas que pensar mucho — pero vale la pena que sepas que es la red abajo del trapecista.

---

## 5. El día a día: comandos y atajos esenciales

No son muchos. Estos cinco te resuelven el 90% de las sesiones.

| Comando / Atajo | Para qué sirve |
|---|---|
| `claude` | Arranca una sesión en la carpeta donde estás parado. |
| **Shift+Tab** | Cambia entre modos (normal, auto, plan). El modo activo se ve abajo. |
| `/clear` | Limpia el contexto y empieza fresco. Úsalo cuando la conversación ya se enredó. |
| `/cost` | Te muestra cuánto llevas gastado en la sesión actual. Bueno para no llevarte sorpresas. |
| `Ctrl+C` | Cancela lo que esté haciendo ahora mismo. Tu botón rojo. |

Dos más que vale conocer aunque uses menos:

- `/init` — la primera vez que abres Claude Code en un proyecto, esto le genera un archivo `CLAUDE.md` inicial con el contexto del proyecto. *(De esto va toda la Clase 3.)*
- `/exit` — cierra la sesión limpiamente.

Y un truco más: si quieres ver todos los comandos disponibles dentro de una sesión, escribe `/` y se despliegan.

---

## 6. Errores típicos al empezar

Los cinco que te van a costar más caro las primeras semanas:

1. **Aceptar cambios sin leerlos.** El plan está ahí precisamente para que lo leas. Si haces "sí, sí, sí" a todo, eventualmente le vas a dar permiso de algo que no querías.
2. **Pedir todo de golpe.** *"Hazme la app completa con login, pagos, dashboard y notificaciones"* vs. tres tareas más pequeñas, una tras otra. Lo segundo casi siempre sale mejor — y cuando algo se rompe, sabes exactamente dónde.
3. **No usar Plan Mode cuando es delicado.** Si vas a tocar algo que ya funciona y te importa, entra en Plan Mode. Es gratis y te ahorra desastres.
4. **No limpiar contexto.** Después de mucho ir y venir, la conversación acumula confusión. La IA empieza a referirse a cosas viejas, a contradecirse, a alucinar. `/clear` y empieza fresco. No es retroceso, es higiene.
5. **Pelearte con un plan malo en vez de reiniciar.** Si después de dos intentos no estás cerca, el problema casi siempre es tu prompt original, no la IA. Cancela, reescribe el pedido desde cero, y empieza otra vez. Como vimos en Clase 1: no pelees con un prompt malo.

---

## 7. Lo que vimos en demo

*Tu demo, tu material.*

- **Demo 1:** Sesión real sobre un proyecto mío. Pedido → plan → revisión → commit.
- **Demo 2:** Caso de cuando se equivoca, cómo lo diagnostico y cómo lo corrijo sin frustrarme.

---

## Glosario rápido

- **Terminal** — la ventana de texto donde escribes comandos directo al sistema. Antes era la única forma de usar una computadora; hoy sigue siendo la forma más poderosa.
- **Repositorio (o "repo")** — la carpeta de un proyecto, vigilada por Git. Es el "expediente" donde viven todos los archivos y todas las versiones pasadas.
- **Commit** — una fotografía guardada del estado del proyecto en un momento dado. Las fotos son acumulables y puedes regresar a cualquiera.
- **Plan Mode** — el modo donde Claude Code propone qué haría pero no toca nada hasta que apruebes.
- **Contexto** — todo lo que la IA "tiene en mente" en esta sesión: la conversación, los archivos que leyó, las instrucciones que le diste. Cuando se vuelve mucho, conviene limpiarlo.

---

## Para antes de la Clase 3

Una sola cosa: **abre Claude Code en algún proyecto tuyo y pásate 20-30 minutos pidiéndole cosas chicas**. No importa qué. El objetivo no es lograr algo en particular, es que tus dedos se acostumbren al loop: pedir, leer el plan, aceptar o rechazar, ver el resultado. Cuando llegues a la Clase 3 con esa experiencia encima, todo lo que vamos a hablar de CLAUDE.md va a aterrizar mucho mejor.
