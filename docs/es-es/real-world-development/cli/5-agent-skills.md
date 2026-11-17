---
title: "Lección 5 - Personalizar y usar una skill de comprobaciones de calidad"
description: "Explora la skill quality-checks, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que funciona y hemos usado archivos de instrucciones para garantizar que cumple los estándares. Pero ¿qué ocurre con las pruebas, lint y los demás procesos de integración continua (CI)?

Las **skills de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo debe ejecutar operaciones como estas.

En esta lección:

- explorarás la skill existente `quality-checks`.
- personalizarás el formato de los resultados.
- volverás a cargar la skill y la ejecutarás.

## Escenario

Tailspin Toys usa la skill `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las skills de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la skill que contiene un archivo Markdown `SKILL.md`. El archivo incluye un frontmatter con el nombre y la descripción de la skill, una descripción general de sus funciones y directrices sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos que use la skill.

> [!NOTE]
> Las skills no necesitan otros archivos o carpetas. La skill `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos existentes del proyecto.

Las skills pueden estar en la carpeta `.github/skills` del proyecto para compartirlas y reutilizarlas en el equipo, o en la carpeta de skills del usuario `~/.copilot/skills`.

## Explora la skill

Veamos la skill que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a entender cuándo debe ejecutar la skill.
3. Lee las instrucciones y observa cómo orientan a Copilot durante las pruebas y la ejecución de lint.
4. Fíjate en que la skill aún no tiene una sección **Results output formatting**.

## Ejecuta la skill antes de modificarla

Puedes invocar las skills directamente mediante Copilot CLI o con lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la skill.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa la siguiente solicitud:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe que aparece al final.

## Personaliza el informe

Queremos que el informe indique qué se ha ejecutado, si se ha completado correctamente y qué han comunicado realmente las herramientas. Actualicemos la skill para generar ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la skill actualizada

Ya has hecho el cambio. Veamos qué tal funciona. Copilot CLI puede volver a cargar las skills modificadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente la misma solicitud que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una skill de agente existente. En esta lección:

- has explorado la skill `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutado la skill.

Ese cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el [servidor Playwright MCP][next-lesson].

## Más ejemplos de skills

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de trabajo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
