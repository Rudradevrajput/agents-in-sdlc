---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo en Tailspin Toys. En este proceso:

- has preparado un codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado precargada;
- has añadido las valoraciones con estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR);
- has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, creado la funcionalidad con Autopilot y revisado el resultado en el modo Interactive;
- has orientado al agente con instrucciones personalizadas, personalizado la aptitud `quality-checks` existente y la has utilizado para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar el filtrado en un navegador real;
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la aptitud y las pruebas en el navegador;
- has revisado el cambio completo de filtrado y autorizado Agent Merge para combinar su PR;
- has explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Lo que has entregado

El taller tiene dos hitos de solicitudes de incorporación de cambios. Cada uno se desarrolla en una rama nueva basada en `main` actualizado:

1. **Valoraciones con estrellas:** muestra en las tarjetas de juegos el `starRating` existente y un estado explícito para los juegos sin valorar.
2. **Flujo de filtrado y calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has utilizado la misma conversación y la misma rama. Para agilizar el taller, todo ese trabajo se combina en una sola PR.

## Distintos tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, inspeccionando el navegador y observando la aplicación mediante Copilot y MCP. La aptitud `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos y las ha presentado con el nuevo formato. QA ha reunido esos resultados con una revisión de los requisitos y la cobertura antes de abrir la PR.

Las pruebas nuevas deben cerrar lagunas reales. Una ejecución de QA que concluye que no hace falta añadir pruebas puede ser correcta. Revisa el código y las pruebas, y vuelve a recopilar las pruebas de verificación pertinentes después de cualquier cambio.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una aptitud, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y adáptalas a las necesidades del equipo. Las instrucciones establecen estándares, las aptitudes describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir el enfoque antes de empezar, **Interactive** para seguir de cerca los cambios acotados y **Autopilot** para tareas bien definidas. Elige un modelo más rápido para ediciones rutinarias y uno más avanzado para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más cosas por explorar

Ya has completado el flujo de trabajo principal. Estas funciones adicionales de la CLI también pueden resultarte útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y recibir otra perspectiva;
- `/fleet` para orquestar subtareas independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para enviar una tarea al agente de GitHub Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con una herramienta es seguir utilizándola. Aplícala al código de producción, a proyectos personales o a esa pequeña aplicación que llevas años queriendo crear. Comparte lo aprendido con el equipo y escucha las experiencias de otras personas. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Administrar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
