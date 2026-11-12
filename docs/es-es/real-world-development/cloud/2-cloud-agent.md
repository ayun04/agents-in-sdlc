---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que pocas organizaciones estén libres de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog por falta de recursos. El agente de GitHub Copilot en la nube está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios para que lo revise una persona. Así puedes delegar tareas rutinarias, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

Con el agente de GitHub Copilot en la nube explorarás:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- por qué es importante definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: comprobarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que la gente pueda seguir explorando. Todavía no necesita una implementación refinada; solo algo que permita hacer pruebas de aceptación de la experiencia. Aunque esto bloquea su trabajo, hay otras incidencias de mayor prioridad.

Estos son ejemplos de tareas que pueden quedar relegadas y que son adecuadas para asignar al agente de GitHub Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, más tarde, puede volver para revisar el resultado.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Igual que con una persona, puedes asignarle trabajo de varias maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea un borrador de solicitud de incorporación de cambios para registrar el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue activa o después de que termine. Cuando esté lista para revisar la solución propuesta, Copilot te mencionará en la solicitud.

## La importancia de unas instrucciones bien definidas

GitHub Copilot no hace magia: no puedes saltarte el trabajo de reflexión. Incluso las tareas aparentemente sencillas pueden ser complejas al examinarlas de cerca, así que [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya completaste el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, puedes echarle un vistazo ahora. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones que ya incluye este repositorio para reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en los metadatos `applyTo`, que limitan estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, busca en la solicitud de incorporación de cambios los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario. Proceden de estos archivos de instrucciones. Volveremos a señalarlos cuando revisemos las solicitudes en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube

Crear código suele requerir un entorno específico y la ejecución de scripts para asegurarse de que está preparado. Esto también se aplica al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno. Puedes personalizarlo mediante un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así se proporcionan las herramientas y dependencias necesarias. Este flujo ya está configurado para el taller y garantiza que Copilot tenga Node.js, las dependencias del proyecto, los navegadores para las pruebas end-to-end y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña Actions del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para estos pasos. Copilot recibe su propio token para operar.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitan el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación Astro con Node.js y Drizzle/libSQL.
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Esto sirve para probarlo sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot destaca en la creación de documentación, así que esta es una incidencia adecuada para el agente en la nube. Trabajará en segundo plano para generarla y, en un ejercicio posterior, revisarás el resultado.

1. Abre el repositorio en github.com en una nueva pestaña.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de una incidencia nueva.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se acaba de generar; su título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno —ejecutando el flujo de trabajo descrito antes—, analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que se van a realizar.
14. Desplázate por la cronología de la PR y busca un mensaje que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve la tarea: explora el código y su estado, se detiene a pensar y decidir un plan y, después, crea código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente en la nube es que puede trabajar de forma asíncrona y dejarte atender otras tareas. Aprovecharemos esa función asignándole otra tarea y, mientras tanto, nos ocuparemos de escribir código para añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una gran ventaja del agente de GitHub Copilot en la nube es que permite dividir el trabajo: puedes centrarte en una tarea mientras él se ocupa de otra. Aunque añadir una sección de juegos relacionados para el equipo de diseño no debería llevar mucho tiempo, puedes dedicar ese tiempo a otras tareas. Vamos a asignársela al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, aparecerá el emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube trabaja de forma parecida a un ingeniero de software, así que no hace falta supervisarlo continuamente; podrás revisar el resultado al terminar. Mientras tanto, vamos a crear y usar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. También puedes delegarle deuda técnica, nuevas funcionalidades o migraciones de código entre frameworks.

Has explorado estos conceptos:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- por qué es importante definir incidencias con un alcance claro.
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. El [agente en la nube también admite servidores MCP][cloud-agent-mcp] y puede utilizar instrucciones personalizadas, que ya has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
