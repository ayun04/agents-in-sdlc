---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

A lo largo de este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que añadiera documentación al código, creara una función de juegos relacionados para que el equipo de diseño pudiera probarla e implementara funciones de accesibilidad con alternancias para el modo de alto contraste y el modo claro. Vamos a revisar los cambios y, si hace falta, pedir a Copilot que mejore su trabajo.

### Escenario

Como ya hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisaremos las propuestas de GitHub Copilot para crear la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de activar la revisión del resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube realiza sus tareas de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad para proteger el entorno:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para su trabajo.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente de GitHub Copilot en la nube: la que añade documentación al código. La revisarás con la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Al abrir la PR, es posible que veas una advertencia que indica que el firewall bloquea a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot aún está trabajando, la solicitud tendrá la marca **[WIP]**. Espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo aprendido hasta ahora.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado, que incluye nuevos comentarios de documentación TSDoc y otros documentos. Los cambios exactos variarán.

   Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca el indicador que muestra que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprueba y ejecuta los flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

El trabajo con Copilot no es unidireccional. Puedes mencionarlo en comentarios de la PR —igual que a otros miembros del equipo— o en comentarios insertados en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como el resultado no es determinista, no podemos darte un texto exacto. Algunas ideas de cambios que podrías pedirle:

- Añadir encabezados de comentario al principio de cada archivo de código con una breve descripción de su contenido.
- Añadir comentarios TSDoc a archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario que solicite un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores o propón otro cambio de documentación.
2. Selecciona **View Session** para observar cómo Copilot realiza el trabajo. Verás que inicia una sesión nueva para aplicar los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás un nuevo commit en la solicitud.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marca la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot generó para resolver la incidencia de los juegos relacionados en la página de detalles.

1. Abre el repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Busca el indicador que señala que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprueba y ejecuta los flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente la función de juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes iterar sobre el resultado con el agente en la nube. Por ejemplo, puedes pedirle que cambie el número de juegos relacionados o que añada encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para solicitar cambios, añade otro comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado. Esta PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde mission control en el ejercicio 4.

1. Abre el repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - Los controles de la interfaz para cambiar entre modos.
   - El uso del almacenamiento local para guardar las preferencias.
   - Los cambios CSS o de estilo para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las etiquetas ARIA y la navegación con el teclado.
   - El código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca el indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprueba y ejecuta los flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu codespace para probar las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Después, inicia la aplicación y prueba los controles para cambiar entre el modo de alto contraste y el modo claro en el navegador. Comprueba que funcionan y que la selección se conserva al volver a cargar la página.

El agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo buenas prácticas de accesibilidad. Si detectas algún problema o mejora, puedes mencionarlo en un comentario con **@copilot**, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otras tareas. Para practicarlo, crea varias incidencias en tu repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario para indicar interés en un juego en su página de detalles.
- Implementar la paginación en la página con la lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para reconocer su efecto posterior en las respuestas del agente en la nube.
- **Has asignado incidencias al agente de GitHub Copilot en la nube** y has observado cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para implementar mejoras de accesibilidad, con controles para los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado las solicitudes de incorporación de cambios del agente en la nube y has iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Repaso y próximos pasos

Has completado el recorrido del agente en la nube. Para seguir aprendiendo, puedes explorar otros recorridos complementarios:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP directamente en el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` con el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para configurar el agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
