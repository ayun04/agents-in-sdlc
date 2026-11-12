---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, pediste al agente de GitHub Copilot en la nube que se ocupara de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque trabaja de forma asíncrona y autónoma, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de agente que tienen solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas y orientar las sesiones en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decidió que sería buen momento para añadir también un modo claro. Como ya estaba trabajando en actualizar los estilos del sitio y añadir una opción de selección, parece lógico incluirlo. Orientarás el trabajo del agente para que añada tanto el modo claro como el de alto contraste.

## Revisa las tareas del agente de GitHub Copilot en la nube

Consulta el estado de las tareas que se han asignado al agente de GitHub Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que asignaste a Copilot, entre ellas:
   - Actualizar la documentación del código.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones que ha realizado Copilot. Pueden incluir:
   - Obtener el código del repositorio.
   - Crear el entorno de trabajo de Copilot.
   - Configurar servidores MCP.
   - Realizar varias operaciones para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios de la derecha. Desde ahí puedes consultar la PR y los archivos modificados para seguir el trabajo.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que Copilot haya elegido y del estado del trabajo.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes similares a los que se muestran en el ejemplo.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso en el que estaba trabajando al recibir el mensaje.

El proceso tardará varios minutos, así que puedes aprovechar para repasar lo que has aprendido hasta ahora.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- explorar mission control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
