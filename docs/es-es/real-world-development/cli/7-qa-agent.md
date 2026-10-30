---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente de QA personalizado que combine los requisitos de la incidencia, la aptitud quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la aptitud `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de control de calidad (QA) bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, aptitudes y herramientas MCP;
- crearás y revisarás un perfil de control de calidad reutilizable;
- seleccionarás el agente de QA y revisarás sus conclusiones según la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una solicitud de incorporación de cambios (PR). Un agente personalizado puede coordinar el proceso de QA y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil de Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La aptitud `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo utilizar estas capacidades para evaluar los requisitos y comunicar los resultados. No sustituye a los demás recursos ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil de QA reutilizable. Definirá las comprobaciones que debe realizar y los límites que debe respetar.

1. Vuelve al codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree un agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Revisa el perfil

Antes de utilizar el agente nuevo, revisa su perfil para confirmar que Copilot ha incluido el flujo de trabajo de QA y los límites previstos. Así evitarás que un agente incompleto o demasiado permisivo modifique la funcionalidad cuando solo quieres que la compruebe.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee los metadatos. `description` es obligatorio; `name` es opcional, pero proporciona un nombre claro que se muestra en la interfaz.
3. Lee las instrucciones del perfil y confirma que el agente de QA empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la aptitud `quality-checks` y utiliza Playwright MCP.
4. Comprueba que comunica pruebas de apoyo, pide permiso antes de modificar código de implementación y no crea confirmaciones ni solicitudes de incorporación de cambios.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que revise el perfil antes de continuar.

## Ejecuta QA según la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, reanuda la conversación de filtrado y selecciona QA para que pueda utilizar la incidencia y las decisiones de planificación que ya forman parte del contexto.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. El comando anterior lo activará.

2. Pide al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de QA empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado su trabajo. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, aptitudes y herramientas MCP;
- has creado y revisado un perfil de control de calidad reutilizable;
- has seleccionado el agente de QA y revisado sus conclusiones según la incidencia.

Ya tienes lista la implementación, la actualización de la aptitud, el perfil de QA, las pruebas y el informe de verificación. A continuación, [los reunirás en una PR de funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
