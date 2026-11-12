---
title: "Lección 10 - Repaso y pasos siguientes"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante un flujo de trabajo continuo en Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado del backlog.
- has añadido las valoraciones por estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- has partido de la incidencia de filtrado, definido el enfoque en modo Plan, implementado en modo Autopilot y revisado en modo Interactive.
- has orientado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` existente y la has utilizado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar el filtrado en un navegador real.
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- has revisado el cambio completo de filtrado y autorizado Agent Merge para la PR de filtrado.
- has explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en una rama propia creada desde `main` actualizado:

1. **Valoraciones por estrellas**: muestra el `starRating` existente y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Flujo de filtrado y calidad**: implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has utilizado la misma conversación y rama. Hemos reunido el trabajo en una PR para agilizar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, examinando la aplicación en el navegador y observando cómo Copilot explora el sitio mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y las comprobaciones de tipos, y presentó los resultados en el nuevo formato. Antes de abrir la PR, QA reunió esos resultados con una revisión de los requisitos y la cobertura de pruebas.

Las pruebas nuevas deben cubrir lagunas reales. Es correcto que una ejecución de QA no necesite añadir pruebas. Antes de autorizar la combinación, revisa el código y las pruebas de verificación; si hay cambios, actualiza las pruebas que hayan quedado obsoletas.

## Buenas prácticas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado las instrucciones, personalizado una habilidad, creado un perfil QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones definen estándares; las habilidades describen tareas repetibles; los agentes personalizados definen roles especializados, y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para cada tarea. Usa **Plan** para definir un enfoque antes de empezar, **Interactive** para mantener el control de cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto es tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* cambia significativamente el resultado.

## Más cosas por explorar

Ya has completado el flujo de trabajo principal. Puedes probar otras funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Próximos pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola: para código de producción, proyectos personales o esa pequeña aplicación que hace años que quieres crear. Comparte lo aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
