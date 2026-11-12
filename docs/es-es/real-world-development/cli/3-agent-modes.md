---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Acuerda un enfoque con Plan, usa Autopilot para crear el filtrado desde una incidencia y revisa el resultado en modo Interactive."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña función al proyecto, pero los cambios mayores necesitan un proceso más completo. GitHub Copilot CLI está diseñado para integrarse en el flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la manera correcta. En esta lección iniciarás un proceso de desarrollo habitual con agentes: partirás de una incidencia para crear una función, comprobarás el código, validarás el comportamiento y, finalmente, integrarás el cambio en el proyecto.

> [!NOTE]
> Utilizarás la misma conversación y rama durante el resto del flujo de trabajo de esta funcionalidad. Normalmente se usarían ramas o PR diferentes para distintos tipos de archivos, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás manualmente la función en un navegador reenviado.

En los siguientes pasos de este flujo de trabajo actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación MCP, crearás un agente QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la función, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Utilizarás el modo Plan para decidirlos y después autorizarás una implementación acotada con Autopilot.

## Información general

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. De hecho, son aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Planificar lo que hay que desarrollar.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código después de que se revise y de que se superen las comprobaciones de integración continua.

> [!NOTE]
> El proceso exacto dependerá del equipo y de la organización, pero la mayoría de los flujos de trabajo siguen un enfoque parecido.

Este proceso estándar ayuda a comprobar que el código generado con IA cumple los requisitos y pasa por las mismas revisiones que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: trabajas con el agente. Propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Empieza desde la incidencia

Antes de comenzar el trabajo de filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están preparados.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que se ha combinado la PR de valoraciones por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para seleccionar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de incorporación de cambios de GitHub.

## Planifica la función de filtrado

La planificación permite definir cómo se implementará una función o se realizará una tarea antes de delegársela a Copilot. Para cualquier trabajo complejo, conviene dedicar un tiempo a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe el prompt siguiente:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque para elaborar el plan.

3. Es posible que Copilot te haga preguntas sobre el funcionamiento del filtrado. Respóndelas según tus preferencias; no hay una única respuesta correcta.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique cualquier parte del plan que no se ajuste a tus decisiones.

## Autoriza Autopilot

Una vez redactado y revisado el plan, puedes implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente, así que revísalo antes. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por terminadas.

## Revisa y comprueba la implementación

Como cualquier código, el código generado debe revisarse antes de combinarse. Revisa los cambios y ejecuta el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando termines la revisión del código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa el resultado de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones acordadas en el plan.
4. Comprueba que el restablecimiento de filtros y el comportamiento cuando no hay resultados coincidan con la incidencia y tus decisiones.
5. Vuelve al Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y próximos pasos

Has utilizado distintos modos de conversación para crear y revisar una función. En esta lección:

- has iniciado una conversación de Copilot desde una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la función con el modo Autopilot.
- has revisado el código.
- has validado manualmente la función en un navegador reenviado.

A continuación, aprenderás cómo se genera el código y cómo seguir las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
