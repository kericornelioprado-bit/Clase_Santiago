# Clase 4 — MCP: conectar a la IA con herramientas reales

> Notas para releer después de la sesión. No reemplazan la clase: la complementan.

---

## 1. Del agente que sabe al agente que actúa

En la Clase 3 trabajamos sobre cómo darle al agente *contexto*: las reglas, convenciones y conocimientos específicos que necesita para no improvisar mal. Un buen CLAUDE.md y una buena Skill convierten a un asistente genérico en un asistente que entiende cómo se hacen las cosas en tu proyecto.

Pero hay un techo natural en ese modelo. Por más contexto que tenga, un agente que solo conversa contigo en una terminal sigue estando aislado del mundo donde vive tu trabajo real: tus correos, tus documentos, tu calendario, los issues del repositorio, los mensajes del equipo en Slack. Si quieres que el agente haga algo en uno de esos sistemas, hoy por hoy lo más común es que tú copies, pegues, traduzcas y ejecutes manualmente lo que el agente propone.

Esa fricción es lo que MCP resuelve. La idea central es simple: en la Clase 3 le dimos al agente *contexto* — saber. En la Clase 4 le damos *manos* — poder hacer.

---

## 2. Qué es MCP

MCP son las siglas de *Model Context Protocol*. La forma más útil de pensarlo es como un estándar para que la IA hable con tus herramientas. Antes de que existiera, cada vez que alguien quería conectar un agente con, digamos, Notion, tenía que escribir una integración a la medida. Lo mismo para conectarlo con Google Calendar. Lo mismo para conectarlo con su base de datos. Cada conexión era un proyecto en sí mismo, y nadie podía reutilizar el trabajo de nadie.

MCP unifica eso. Hoy, cuando alguien quiere que un agente pueda hablar con su producto, publica un "servidor MCP" — una pieza que sigue el estándar. Y cualquier herramienta que entienda MCP (Claude Code, por ejemplo) puede conectarse a ese servidor sin trabajo adicional. El resultado práctico es que hay un catálogo creciente de servidores listos para usar, y conectar uno se parece más a instalar una extensión del navegador que a programar una integración.

No necesitas saber cómo está construido un MCP por dentro para usarlo bien — de la misma forma que no necesitas saber cómo funciona HTTP para usar internet. Lo que sí importa es entender qué puede y qué no puede hacer cada uno, y qué riesgos vienen con ello. A eso volvemos más adelante.

---

## 3. Por qué cambia el juego

Vale la pena pausar en el cambio cualitativo que esto representa, porque es fácil subestimarlo.

Sin MCP, el flujo típico con un agente es: tú le explicas qué quieres, él te genera un texto o un código, tú lo tomas y lo aplicas a mano en los sistemas donde vive tu trabajo. El agente es un consultor brillante encerrado en una habitación: te puede recomendar qué hacer, pero no puede hacerlo él.

Con MCP, el agente sale de la habitación. Puede leer el correo del que estás hablando, abrir el documento que mencionaste, crear el issue que acordaron, agendar la reunión, mandar el mensaje al canal. La consecuencia es que tareas que antes no valía la pena delegarle — porque la parte de "ejecutar lo que el agente propuso" costaba más que hacerlo todo a mano — ahora sí valen la pena. El umbral de qué es automatizable baja muchísimo.

Ese es el cambio: no es que el agente sea más inteligente. Es que ahora puede actuar sobre los sistemas reales donde sucede tu trabajo.

---

## 4. Tres MCPs canónicos

El catálogo es grande y crece cada semana. En vez de un recorrido superficial por todos, conviene entender tres a fondo, porque cada uno representa una *familia* distinta de casos de uso. Si entiendes estos tres, sabrás reconocer cuándo otro MCP encaja en tu flujo de trabajo.

### 4.1 Google Drive — el MCP de "conocimiento"

Pertenece a la familia de MCPs que dan acceso a documentos y archivos. El agente puede buscar, leer y, en algunos casos, crear o modificar contenido dentro de tu Drive.

**Cómo cambia el flujo:** sin este MCP, si quieres que el agente trabaje con la información de un reporte, tienes que abrir el documento, copiarlo y pegarlo en la conversación. Con el MCP, basta con que le digas el nombre del documento o de la carpeta. El agente lo encuentra y lo lee directamente.

**Caso de uso típico:** *"Lee los tres reportes mensuales más recientes de la carpeta de finanzas y dame un resumen ejecutivo con los tres puntos clave de cada mes y las tendencias que veas a través de los tres."* Sin el MCP, esa tarea son veinte minutos de abrir documentos y copiar texto antes de poder ni siquiera empezar a delegarla. Con el MCP, es una sola instrucción.

La familia completa: Notion, Google Docs, Dropbox, OneDrive. Todos cumplen el mismo papel — darle al agente acceso al "cuerpo de conocimiento" disperso en archivos.

### 4.2 GitHub — el MCP de "proyectos y código"

Pertenece a la familia de MCPs que conectan al agente con los sistemas donde vive el código y la coordinación de proyectos técnicos. El agente puede leer issues, comentar pull requests, abrir tareas nuevas, ver el historial de cambios.

**Cómo cambia el flujo:** sin este MCP, Claude Code trabaja sobre los archivos locales de tu computadora, pero está ciego frente a la conversación que sucede *alrededor* del código — los issues, las discusiones, los pull requests. Con el MCP, esa frontera desaparece.

**Caso de uso típico:** *"Revisa los issues abiertos del repositorio que están etiquetados como 'bug', identifica los que tienen información suficiente para resolverlos, y para cada uno propón un plan de cómo lo abordarías."* El agente lee los issues, los clasifica, te devuelve un plan. Tú decides cuáles atacar. Sin el MCP, esto implica tener veinte pestañas del navegador abiertas y leer cada issue a mano.

La familia completa: GitLab, Linear, Jira, Asana. Todos cumplen el mismo papel — darle al agente acceso al sistema donde se coordina el trabajo en equipo, sea de código o de proyectos en general.

### 4.3 Slack — el MCP de "comunicación"

Pertenece a la familia de MCPs que conectan al agente con los canales donde habla tu equipo. El agente puede leer hilos, buscar mensajes anteriores, enviar mensajes a canales o personas específicas.

**Cómo cambia el flujo:** sin este MCP, el agente vive desconectado de la conversación humana que rodea el trabajo. Con el MCP, puede insertarse en ese flujo, leer el contexto de una discusión, y producir o consumir mensajes igual que cualquier otro miembro del equipo.

**Caso de uso típico:** *"Lee los últimos cincuenta mensajes del canal #producto, identifica las tres preguntas que se quedaron sin responder, y prepárame un resumen para llevar al stand-up de mañana."* O al revés: *"Cuando termine de generar el reporte, mándalo al canal #reportes con un mensaje corto explicando qué contiene."*

La familia completa: Microsoft Teams, Discord, WhatsApp Business. Todos cumplen el mismo papel — meter al agente en el tejido de comunicación de tu organización.

---

## 5. Cómo se conecta un MCP

A nivel conceptual, el flujo es siempre el mismo, sin importar el MCP que quieras conectar:

1. **Elegir el MCP.** Hay catálogos públicos donde los servidores MCP están listados. Buscas el que necesitas — Google Drive, GitHub, Slack, lo que sea.
2. **Instalarlo en tu cliente.** En el caso de Claude Code, esto se hace con un comando específico que registra el servidor en tu configuración. El detalle exacto lo veremos en la demo.
3. **Autorizar el acceso.** El MCP necesita permiso para hablar con la cuenta correspondiente. Esto típicamente abre una ventana en tu navegador donde inicias sesión y das permisos explícitos — igual que cuando una app pide acceso a tu cuenta de Google.
4. **Verificar que está activo.** Una vez instalado, el agente reconoce que tiene una nueva capacidad. A partir de ese momento, puedes invocarlo en lenguaje natural.

Lo importante de este flujo no es memorizar los pasos — vas a olvidar los detalles entre clase y clase. Lo importante es entender que **conectar un MCP es una operación deliberada, con permisos explícitos**. No pasa por accidente. Esa es exactamente la propiedad que necesitamos para hablar del siguiente tema.

---

## 6. Los riesgos que hay que tener claros

Conectar un MCP es darle al agente acceso real a sistemas reales. Eso es lo que lo vuelve útil, pero también es lo que lo vuelve potencialmente costoso si se hace a la ligera. Hay tres riesgos que conviene entender desde el principio.

**Permisos.** Casi todos los MCPs piden permisos amplios por defecto — "leer todo tu Drive", "leer y escribir en todos tus repositorios", "enviar mensajes en cualquier canal". Antes de aceptar, vale la pena preguntarte si necesitas ese nivel de acceso o si puedes restringirlo. Una buena regla práctica: dale al agente el mínimo permiso que necesite para la tarea que tienes en mente, no el máximo que el MCP ofrece. Si más adelante necesitas más, lo amplías.

**Secretos.** Algunos MCPs requieren tokens de acceso, claves de API, o credenciales que se guardan en tu configuración local. Esos archivos son tan sensibles como una contraseña: si alguien tiene acceso a ellos, tiene acceso a los sistemas conectados. Nunca los compartas, no los pegues en chats con clientes, y verifica que estén en archivos ignorados por Git si trabajas en un repositorio.

**Qué no conectar a la ligera.** Hay sistemas donde una acción mal hecha es difícil o imposible de revertir: bases de datos productivas, sistemas financieros, herramientas de envío masivo de correos. Para esos casos, lo más sensato es: primero, conectar el MCP en modo lectura si la herramienta lo permite; segundo, probar contra un ambiente de pruebas antes que contra el real; tercero, en producción, mantenerte siempre en el loop — que el agente proponga y tú confirmes, no que el agente ejecute directamente.

El principio que une los tres riesgos es el mismo: el poder que ganas con MCP es el poder de ejecutar. Y ejecutar es lo que distingue un consejo malo de un problema real. La velocidad nueva no exime del juicio.

---

## 7. Hacia dónde se abre esto

Con MCP, el agente deja de ser un asistente que conversa y se vuelve un asistente que ejecuta sobre tus sistemas. Eso resuelve un problema enorme, pero abre uno nuevo: ahora que el agente *puede* tocar tus datos y tus herramientas, ¿qué tipo de cosas tiene sentido construir encima?

La pregunta de la Clase 5 es justamente esa. Vamos a ver qué distingue a un chatbot de un agente, y cómo se ve uno construido específicamente para "hablar con tus datos" — un patrón que se ha vuelto el caso de uso más común para todo lo que estamos viendo en este curso.

---

## Glosario rápido

- **MCP (Model Context Protocol):** estándar que define cómo un agente de IA se conecta con herramientas y servicios externos.
- **Servidor MCP:** la pieza específica que conecta a un agente con una herramienta particular (un servidor MCP de GitHub, uno de Slack, etc.).
- **Permisos / scopes:** las acciones específicas que un MCP puede realizar sobre la cuenta a la que se conecta. Pueden ser amplios o restringidos.
- **Token de acceso:** la credencial que un MCP usa para hablar con un servicio en tu nombre. Equivalente, en sensibilidad, a una contraseña.
