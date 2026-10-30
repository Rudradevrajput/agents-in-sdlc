---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

A lo largo de este taller, has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que documente el código, cree una función de juegos relacionados para el equipo de diseño e implemente mejoras de accesibilidad con modos de alto contraste y claro. Revisemos los cambios y, si es necesario, demos indicaciones a Copilot para mejorarlos.

### Escenario

Como ya hemos mencionado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre debemos revisar el código generado y seguir nuestro proceso habitual de DevOps. Por eso, revisaremos las sugerencias de GitHub Copilot para documentar el código, añadir juegos relacionados e implementar las funciones de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y el agente de GitHub Copilot en la nube

El agente de GitHub Copilot en la nube trabaja de forma asíncrona y sin supervisión, por lo que cuenta con ciertas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente en la nube se ejecuta en GitHub Actions y crea un entorno separado y efímero para trabajar.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) que generó el agente de GitHub Copilot en la nube: añadir documentación al código. Usarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que la PR muestre un aviso de que el firewall bloquea a GitHub Copilot. Es **lo esperado**, ya que Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente de GitHub Copilot en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot aún está trabajando, la solicitud tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los comentarios TSDoc recién creados y otra documentación. Los cambios exactos pueden variar.

   Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas mejoras se deben a los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto, que es una sola aplicación de Astro, se superarán. Esto puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no tiene por qué ser unidireccional. Puedes mencionarlo en comentarios de la solicitud o en comentarios insertados en el código, como harías con otras personas del equipo. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto prescriptivo. Por ejemplo, podrías pedirle que:

- añada al principio de cada archivo de código encabezados de comentarios con una breve descripción;
- añada comentarios TSDoc a los archivos TypeScript y Astro;
- cree un archivo README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Puedes usar una de las ideas anteriores u otra sugerencia sobre la documentación del código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una nueva sesión para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una nueva confirmación en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Volvamos a la solicitud de Copilot para resolver la incidencia sobre los juegos relacionados en la página de detalles de cada juego.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un aviso de que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto, que es una sola aplicación de Astro, se superarán. Esto puede tardar unos minutos.
9. **Opcional:** Para probar manualmente la función de juegos relacionados, puedes cambiar a esta rama en el codespace. Ve al codespace, abre un terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube y solicitar cambios. Por ejemplo, puedes pedirle que ajuste cuántos juegos relacionados se muestran o que añada encabezados de comentarios y comentarios TSDoc (recuerda que asignaste esta tarea **antes** de actualizar las instrucciones personalizadas). Para ello, añade un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad que se implementaron con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste que asignaste en el Ejercicio 3 y el modo claro que solicitaste desde el centro de control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando, la solicitud tendrá la marca **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz para alternar entre modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios de CSS o estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con el teclado, etc.);
   - el código JavaScript o TypeScript que controla el cambio de modo.

6. Cuando hayas revisado los cambios y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un aviso de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto, que es una sola aplicación de Astro, se superarán. Esto puede tardar unos minutos.
10. **Opcional:** Para probar manualmente las funciones de accesibilidad, puedes cambiar a esta rama en el codespace. Ve al codespace, abre un terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama que creó Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los modos de alto contraste y claro en el navegador. Comprueba que funcionan y que las preferencias se mantienen al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a Copilot a implementar estas funciones siguiendo las prácticas recomendadas. Si detectas problemas de accesibilidad o aspectos que se puedan mejorar, puedes mencionar a **@copilot** en un comentario y pedirle que los actualice, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube es más útil cuando puedes asignarle tareas reales del backlog y centrarte en otra cosa. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para que las personas interesadas en un juego puedan registrarse en la página de detalles;
- añadir paginación a la lista de juegos;
- validar las entradas y gestionar errores en los asistentes de acceso a los datos.

## Resumen

Has completado el taller del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para observar su efecto en el resultado del agente.
- **Has asignado incidencias al agente de GitHub Copilot en la nube** y has visto cómo preparaba el entorno, planificaba y trabajaba de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para mejorar la accesibilidad con modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar la sesión de accesibilidad y orientarla mientras estaba en curso.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente en la nube**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el taller del agente en la nube. Si quieres seguir explorando, los otros talleres complementan lo que has practicado:

- 🖥️ **[Taller de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP directamente en el IDE.
- 💻 **[Taller de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, aptitudes de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional de `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de GitHub Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
