---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya combinaste personalmente la PR de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el proceso de combinación.
- examinarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge únicamente después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo pendiente de la PR y mantener la autorización para combinar bajo el control del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo activas, la sesión de la aplicación lee la solicitud y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y reorganiza la base cuando hace falta. Después, la combina en cuanto GitHub lo permite. Trabaja en segundo plano, continúa después de reiniciar la aplicación y se desactiva cuando se combina la solicitud.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar cambios sigue necesitando tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Usa Agent Merge para gestionar la PR

Cuando hayas creado y revisado todo el código, deja que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable por sí solo no lo inicia.

Se inicia el proceso de Agent Merge. Este se encargará de:

- Crear la solicitud de incorporación de cambios con un título y una descripción.
- Si iniciaste la sesión desde una incidencia, incluir una referencia a ella en el cuerpo de la descripción.
- Reorganizar la base o resolver posibles conflictos con la rama de destino.
- Supervisar el proceso de CI para comprobar que se superan todas las comprobaciones.
- Supervisar la PR por si otros desarrolladores o la revisión de código de Copilot añaden comentarios, y realizar cambios para resolverlos.
- Combinar automáticamente la PR cuando todo termine correctamente, si así se lo indicas.

Deja que Agent Merge combine la PR cuando se superen todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** esté marcado.
8. Pide explícitamente al agente que combine la PR:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Sigue el proceso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR es **Merged**; que Agent Merge tenga permiso o siga ejecutándose no significa que la PR ya se haya combinado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación del código, y ahora el proceso de solicitud de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el proceso de combinación.
- has examinado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de la revisión y has confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
