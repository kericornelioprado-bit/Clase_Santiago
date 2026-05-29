# Clase 3 — Context engineering: CLAUDE.md, Skills y el problema del "drift"

> Notas para releer después de la sesión. No reemplazan la clase: la complementan.

---

## 1. Por qué el contexto es la meta-habilidad

En la Clase 1 vimos la anatomía de un buen prompt: intención, contexto, restricciones, formato y ejemplos. En la Clase 2 trabajamos con Claude Code y notamos que, sesión tras sesión, hay que repetirle muchas de las mismas cosas: cómo está organizado el proyecto, qué convenciones siguen, qué archivos no debe tocar.

Esa repetición es el síntoma de un problema más profundo. Cada conversación nueva con un agente arranca en cero: no recuerda decisiones anteriores, no sabe qué intentaste y descartaste, no conoce las reglas implícitas de tu código. Si el único lugar donde vive el contexto es tu cabeza, el agente va a improvisar — y va a improvisar mal cada vez que la improvisación no coincida con lo que tú esperabas.

**Context engineering** es la práctica de diseñar la capa persistente de contexto que el agente lee automáticamente: archivos que se cargan en cada sesión, instrucciones modulares que se activan cuando hacen falta, y reglas claras sobre qué tocar y qué no. Es una meta-habilidad porque no produce código directamente — produce las condiciones para que todo el código que el agente escriba salga mejor.

Una forma útil de pensarlo: si tuvieras que onboardear a un desarrollador nuevo a tu proyecto, ¿qué le explicarías el primer día? Eso es lo que tu CLAUDE.md y tus Skills deben contener. La diferencia es que al agente se lo explicas una sola vez, en archivos versionados, y todas las sesiones futuras heredan ese conocimiento.

---

## 2. CLAUDE.md: el archivo que el agente lee siempre

`CLAUDE.md` es un archivo de markdown en la raíz de tu proyecto (también puede haber unos secundarios en subcarpetas) que Claude Code carga automáticamente al iniciar cada sesión. Su contenido se inyecta como contexto base, así que todo lo que pongas ahí está presente en cada conversación sin que tengas que pegarlo de nuevo.

La regla de oro: **un CLAUDE.md no es documentación para humanos, es instrucciones para un agente.** Eso cambia cómo se escribe. Tiene que ser preciso, jerarquizado y conciso. Cada línea que metes consume contexto y compite por la atención del agente; si una sección es decorativa, sobra.

### Anatomía de un buen CLAUDE.md

No hay una plantilla única, pero sí una estructura recurrente que funciona. Una versión típica incluye, en este orden:

**1. Descripción del proyecto.** Dos o tres líneas: qué es, para quién, qué problema resuelve. Le da al agente el "para qué" antes que el "cómo".

**2. Stack técnico.** Lenguajes, frameworks, versiones, dependencias clave. Evita que el agente sugiera soluciones con librerías que no usas o sintaxis de versiones que no aplican.

**3. Estructura del repositorio.** Qué hay en cada carpeta principal y para qué sirve. Esto reduce muchísimo el tiempo que el agente pierde explorando archivos antes de actuar.

**4. Convenciones de código.** Naming, formato, patrones de diseño preferidos, cómo se manejan errores, cómo se nombran tests. Si tienes un linter o formateador configurado, menciónalo y dile al agente que lo respete.

**5. Comandos comunes.** Cómo se corre el proyecto, cómo se construye, cómo se prueban los tests, cómo se despliega. El agente ejecuta comandos en terminal; si no le dices cuáles, los va a adivinar.

**6. Reglas duras / "no toques esto".** Archivos críticos, decisiones cerradas, código generado automáticamente que no debe editarse a mano. Esta sección se escribe en imperativo: "no modifiques X", "siempre haz Y antes de Z". Sin matices.

**7. Workflow esperado.** Cómo se hacen commits, branches, pull requests. Si el agente debe correr tests antes de declarar una tarea terminada, dilo aquí.

**8. Gotchas y decisiones históricas.** Las trampas no obvias del proyecto y por qué ciertas cosas están como están. Es la sección que más se subestima y la que más ahorra tiempo a largo plazo.

### Ejemplo de estructura

```markdown
# Proyecto: [Nombre]

Plataforma de [descripción breve]. Stack: Next.js + Postgres + Stripe.

## Estructura
- `/app` — rutas y componentes de UI (Next.js App Router)
- `/lib` — lógica de negocio reutilizable
- `/db` — esquemas y migraciones (Drizzle)
- `/scripts` — utilidades de mantenimiento, no se importan desde la app

## Convenciones
- TypeScript estricto. No usar `any`.
- Componentes en PascalCase, hooks en camelCase con prefijo `use`.
- Errores de negocio se lanzan como instancias de `AppError` (ver `lib/errors.ts`).
- Tests en el mismo folder que el código, con sufijo `.test.ts`.

## Comandos
- `pnpm dev` — corre el servidor local
- `pnpm test` — corre la suite de tests
- `pnpm db:migrate` — aplica migraciones pendientes

## Reglas duras
- Nunca modifiques `db/schema.ts` sin generar la migración correspondiente.
- No instales librerías nuevas sin preguntar.
- El archivo `lib/auth.ts` está bajo revisión de seguridad: no lo edites.

## Workflow
- Cada cambio: branch nueva desde `main`, commit pequeño, correr `pnpm test` antes de declarar terminado.
- Mensajes de commit en presente, en inglés, formato `[scope] resumen breve`.

## Gotchas
- La integración con Stripe usa webhooks en `/app/api/webhooks/stripe`.
  La firma se valida ahí — no muevas esa lógica a middleware.
- El cron de limpieza corre cada 6h y depende del campo `deleted_at`.
```

Notar lo que **no** está: introducciones largas, justificaciones, historia del proyecto. Solo lo que el agente necesita para actuar correctamente.

### Principios de escritura

- **Imperativo, no descriptivo.** "Usa TypeScript estricto" funciona mejor que "el proyecto está configurado con TypeScript estricto".
- **Lo crítico arriba.** Los modelos atienden mejor lo que está al principio del contexto. Reglas duras y convenciones primero; gotchas y notas al final.
- **Concreto, no aspiracional.** No escribas reglas que no piensas hacer cumplir. Si dices "siempre escribir tests" pero tu repo está medio sin testear, el agente recibe una instrucción contradictoria.
- **Vivo, no monumento.** El CLAUDE.md se edita constantemente. Cada vez que el agente se equivoca por falta de contexto, esa es una entrada nueva que prevenir.

---

## 3. Skills: contexto modular bajo demanda

Si CLAUDE.md es el contexto que se carga **siempre**, las Skills son contexto que se carga **cuando hace falta**. Una Skill es una carpeta con un archivo `SKILL.md` que describe un procedimiento concreto, y el agente decide invocarla cuando reconoce que la tarea entra en su dominio.

### Cuándo escribir una Skill

Una Skill se justifica cuando se cumple al menos una de estas condiciones:

- **El procedimiento se repite.** Cada vez que creas un endpoint nuevo, cada vez que generas un reporte, cada vez que migras datos de un formato a otro. Si lo haces más de dos o tres veces, vale la pena documentarlo como Skill.
- **El procedimiento tiene varios pasos con orden importante.** Algo que tiene que pasar antes que otra cosa, validaciones intermedias, un checklist que se olvida.
- **El conocimiento es muy específico y no aplica al resto del proyecto.** Si lo metieras en CLAUDE.md, sería ruido para el 95% de las tareas. Como Skill se carga solo cuando se necesita.

### Anatomía de una Skill

Una `SKILL.md` típica tiene tres partes:

**1. Descripción de cuándo usarla.** Esto es lo más importante: el agente decide invocar la Skill leyendo esta sección. Si está mal redactada, la Skill nunca se va a activar o se va a activar en el momento equivocado. Hay que ser explícito sobre los disparadores: qué palabras, qué tipo de tarea, qué archivos involucrados.

**2. Instrucciones del procedimiento.** Pasos numerados, en orden, con detalle suficiente para ejecutarse sin ambigüedad. Si hay decisiones que el agente tiene que tomar, se explicitan los criterios.

**3. Ejemplos o referencias.** Si la Skill produce código, un ejemplo del output esperado. Si depende de archivos específicos, dónde encontrarlos.

### Cómo se reutilizan

Las Skills viven en una carpeta convenida del proyecto (o en una global, dependiendo de tu setup) y Claude Code las descubre automáticamente. Algunas se versionan junto con el proyecto porque son específicas a él; otras viven en un repositorio personal de Skills que cargas en cualquier proyecto donde aplique. Las que producen documentos, formatos repetitivos, o flujos transversales (escribir un PDF, llenar un Excel, hacer un release) suelen ser candidatas a Skill global.

---

## 4. El problema del "drift"

**Drift** es el fenómeno por el cual un agente, a lo largo de una sesión o entre sesiones, se va alejando progresivamente de las convenciones, decisiones y restricciones que le diste. Empieza haciendo todo bien, y de pronto está usando una librería que dijiste explícitamente que no, o estructurando código de una forma que contradice lo que está en tu CLAUDE.md.

### Por qué ocurre

- **Contexto demasiado largo.** Conforme la conversación crece, las reglas del principio quedan "lejos" en el contexto y compiten con todo lo que se ha hablado después. El agente atiende lo reciente más que lo viejo.
- **Reglas contradictorias.** Si tu CLAUDE.md dice una cosa y un comentario en el código sugiere otra, el agente va a elegir — y no siempre la que tú querías.
- **Defaults del modelo.** El agente trae preferencias entrenadas (cómo nombrar variables, qué patrones usar) que en ausencia de instrucción explícita se imponen. Si tu proyecto tiene convenciones que se salen de la norma común, esas son justo donde más vas a ver drift.
- **Conversaciones que pivotan.** Empiezas con una tarea, en medio te metes a otra, y el agente arrastra el contexto de la primera a la segunda. Esto genera decisiones contaminadas.

### Cómo prevenirlo

- **CLAUDE.md jerarquizado.** Lo más crítico al principio del archivo. Reglas duras antes que convenciones de estilo, convenciones antes que gotchas.
- **Reglas en imperativo y absolutas.** "Nunca uses X" es más resistente al drift que "preferimos no usar X cuando es posible". El agente respeta mejor lo categórico.
- **Reset frecuente.** Sesiones cortas, una tarea por sesión. Cuando termines algo, cierra y abre conversación nueva. El contexto fresco siempre rinde mejor que el contexto largo.
- **Anchor en el prompt.** Cuando una regla importa especialmente para esta tarea, recuérdala explícitamente en el prompt aunque ya esté en CLAUDE.md. La redundancia ayuda.
- **Auditoría reactiva.** Cada vez que detectes drift, dos acciones: corregir en la sesión actual, y editar tu CLAUDE.md o crear una Skill para que esa categoría de error no se repita. El sistema se vuelve más sólido con el uso.
- **Revisión humana en los momentos críticos.** Sobre todo en cambios de archivos sensibles. La capa de contexto reduce el drift, no lo elimina; la revisión es la última línea de defensa.

---

## 5. Resumen

- El contexto persistente es lo que separa a un agente que ayuda de uno que estorba. Diseñarlo es una habilidad explícita.
- **CLAUDE.md** se carga siempre y debe contener lo esencial: descripción, stack, estructura, convenciones, comandos, reglas duras, workflow, gotchas. Escrito en imperativo, conciso, jerarquizado.
- **Skills** son contexto modular para procedimientos específicos que se repiten. Se activan bajo demanda. Su descripción de "cuándo usarla" es lo más importante.
- **Drift** es la degradación progresiva del cumplimiento de tus reglas. Se previene con un CLAUDE.md bien diseñado, sesiones cortas, recordatorios explícitos en prompts críticos, y un ciclo de auditoría que convierte cada error en una mejora del contexto.

La pregunta operativa que conviene hacerse después de cada sesión con un agente: *¿qué tuve que repetirle hoy que debería haber estado en CLAUDE.md o en una Skill?* Esa pregunta, hecha consistentemente, es lo que convierte el context engineering de concepto en práctica.
