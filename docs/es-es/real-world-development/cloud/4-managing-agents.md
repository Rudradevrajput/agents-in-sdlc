---
title: "Ejercicio 4 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios, has pedido al agente de GitHub Copilot en la nube que realice tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. El agente trabaja de forma autónoma y asíncrona, pero sigue siendo importante supervisar estas tareas.

Hay varias herramientas para administrar las tareas asignadas al agente en la nube, entre ellas, la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas, consultar las operaciones que han realizado e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- orientarás una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo ha decidido que también sería buen momento para añadir un modo claro. Como ya se está actualizando el estilo del sitio y añadiendo la funcionalidad de alternancia, parece lógico incluirlo. Orientarás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de GitHub Copilot en la nube

Veamos el estado de las tareas que se han asignado al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - actualizar la documentación del código base;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa lo que ha hecho Copilot. Entre otras cosas, puede:
   - obtener el código del repositorio;
   - preparar el entorno para trabajar;
   - configurar servidores MCP;
   - realizar distintas operaciones para completar la tarea.

> [!NOTE]
> Los pasos exactos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Desde allí puedes ver la solicitud y los archivos modificados para supervisar el trabajo.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá de cómo la haya denominado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que ha terminado de prepararse y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este mensaje:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la página de agentes con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso en el que estuviera trabajando al recibirla.

El trabajo tardará varios minutos, así que puedes aprovechar para repasar lo que has aprendido hasta ahora.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Desde allí puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para ajustar su alcance o dar indicaciones adicionales.

En esta lección has aprendido a:

- explorar el centro de control y la página de agentes para supervisar tareas;
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
