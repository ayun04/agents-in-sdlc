---
title: "Ejercicio 2 - Añadir funcionalidades con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones, como ejecutar pruebas. El flujo de trabajo de desarrollo normalmente consiste en localizar los archivos necesarios, hacer cambios, ejecutar pruebas, depurar, descubrir qué archivo falta y volver a modificarlo. Y así sucesivamente.

El modo de agente de Copilot puede ayudar con este trabajo.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a ver cómo utilizarlo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, con ciclos rápidos de comentarios y colaboración. El agente en la nube trabaja de forma asíncrona como otro miembro del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo de agente de Copilot explora el proyecto, identifica los archivos relevantes y coordina cambios.
- el modo de agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales. Ahora terminarás el trabajo pendiente de la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y consultar sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en sus datos frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API DOM estándar.

### Inicia el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve al codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal nuevo en el codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando se inicie el servidor de desarrollo Astro, verás un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir un navegador con `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección `http://localhost:4321` del servidor Astro.

> [!NOTE]
> En un codespace, al seleccionar un enlace localhost en el terminal, se te redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu codespace, donde se está ejecutando el servidor web.

### Explora el sitio web

Con el sitio en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los siguientes pasos, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; las instrucciones los indicarán cuando sea necesario.

La implementación inicial funciona, pero queremos ampliarla. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó un backlog de incidencias automáticamente. Pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos de terminal y utilizar herramientas.

   ![Selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no muestran preferencia por un modelo concreto. Al preparar el taller utilizamos Claude Sonnet 4.5 y lo indicamos para que las sugerencias fueran relativamente coherentes y la experiencia, satisfactoria. Sin embargo, como los modelos de lenguaje grandes (LLM) son probabilísticos, las sugerencias pueden diferir de las descritas. Es normal y previsible.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot podría utilizar un comando MCP distinto, pero debería poder completar la tarea.

3. Pide a Copilot que consulte el backlog con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado los elementos según su utilidad para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que dará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices generales para añadir funciones, entre ellas:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a instrucciones específicas para varios tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar instrucciones, dividir conjuntos complejos en partes manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado se necesitan al menos tres cambios:

- Añadir o mejorar la lógica de filtrado que falta en la capa de datos (`src/lib/`).
- Añadir o actualizar pruebas del comportamiento del filtrado.
- Actualizar la página de la lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo de agente de Copilot puede realizar estas tareas.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la función descrita en la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad: los helpers de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código base. Mientras trabaja, es posible que aparezcan los controles **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer todos los cambios; no es necesario seleccionarlos mientras el agente trabaja.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat para confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot Chat en modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva función.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has aprendido cómo:

- el modo de agente de GitHub Copilot implementa funcionalidades en la interfaz y la capa de datos.
- el modo de agente de Copilot explora el proyecto, identifica los archivos relevantes y coordina cambios.
- revisar los cambios y las pruebas generados por el modo de agente de Copilot antes de combinarlos.

Ahora [probarás la función con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio para pedir a Copilot que la implemente en el sitio. Ten en cuenta lo siguiente:

- Sigue las buenas prácticas existentes, incluidos los archivos de instrucciones.
- Decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- Al redactar el prompt, proporciona a Copilot las instrucciones necesarias para crear la implementación que quieres.
- Es posible que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es el flujo normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
