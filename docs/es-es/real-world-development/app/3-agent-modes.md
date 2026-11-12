---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: usa Plan para acordar un enfoque, Autopilot para crear el filtrado desde una incidencia e Interactive para revisar y comprobar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una pequeña función al proyecto, pero los cambios mayores necesitan un proceso más completo. La aplicación GitHub Copilot está diseñada para integrarse en el flujo de trabajo de una organización y ayudarnos a crear lo adecuado de la manera correcta. En esta lección iniciarás un proceso de desarrollo habitual con agentes: partirás de una incidencia para crear una función, comprobarás el código, validarás el comportamiento y, finalmente, integrarás el cambio en el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante el resto del flujo de trabajo de esta función. Normalmente se usarían sesiones o PR diferentes para distintos tipos de archivo, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una nueva sesión de agente desde una incidencia de GitHub.
- definirás los requisitos en modo **Plan**.
- implementarás la nueva función con el modo **Autopilot**.
- revisarás el código.
- validarás manualmente la función en un lienzo de navegador.

En los siguientes pasos de este flujo de trabajo actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación MCP, crearás un agente QA y abrirás la PR de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan acotar los juegos por categoría y editorial. La incidencia del backlog describe la función, pero hay que acordar detalles como la combinación de categorías antes de programar. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en el flujo de desarrollo no cambia los principios fundamentales. De hecho, adquieren aún más importancia. La mayoría de los desarrolladores siguen un flujo similar al siguiente:

1. Abrir una incidencia registrada con los detalles del trabajo.
2. Crear un plan de lo que debe desarrollarse.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código después de que se revise y de que se supere el proceso de integración continua.

> [!NOTE]
> Los detalles concretos variarán según el equipo y la organización, pero la mayoría de los flujos de trabajo serán una variante del proceso descrito.

Este proceso estándar ayuda a comprobar que el código generado con IA cumple los requisitos y pasa por las mismas revisiones que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla la autonomía del agente. Puedes establecerlo en el menú desplegable situado debajo del campo de prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente. Propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que la PR de valoraciones por estrellas está combinada y que tu `main` local está actualizado.

1. En la barra lateral, dentro de **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, después, **Issues** en la navegación superior del repositorio. Abre **Allow users to filter games by category and publisher**. Es una incidencia existente creada a partir de la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

   ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal, pero no en la sesión, comprueba la configuración de `PATH` o del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones que fallaron; no reinstales npm solo porque el agente no lo encuentre.

## Planifica la función de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud de la función como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios de acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Solicita cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para la validación posterior.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de suponer que la aprobación lo cambia. Confirma que el indicador muestra **Autopilot**.

Copilot empezará a implementar la funcionalidad. Verás cómo sigue el plan acordado, genera código e incluso ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa el plan primero. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y comprueba la implementación

Como cualquier código, el código generado debe revisarse antes de combinarse. Revisemos los cambios y ejecutemos el sitio para comprobar que todo funciona correctamente.

1. Abre **Changes** y examina la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de varias categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Examina los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los errores y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; asegúrate de que el servidor probado pertenece a este árbol de trabajo, no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en un lienzo de navegador.

1. Utiliza este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo:

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y confirma que los resultados coinciden con ella.
4. Combina los filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión:

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR de filtrado. Conserva esta sesión, árbol de trabajo y rama para las lecciones 4 a 8. A continuación aplicarás instrucciones personalizadas al código de filtrado y añadirás las mejoras de la habilidad y de QA antes de crear y combinar la PR conjunta en la lección 8. Puedes dejar la sesión inactiva después de detener el servidor y volver a ella en la siguiente lección.

## Resumen y próximos pasos

Has utilizado distintos modos de agente para crear y revisar una función. En esta lección:

- has iniciado una nueva sesión de agente desde una incidencia de GitHub.
- has definido los requisitos en modo **Plan**.
- has implementado la función con el modo **Autopilot**.
- has revisado el código.
- has validado manualmente la funcionalidad en un lienzo de navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
