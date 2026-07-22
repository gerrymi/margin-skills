# Margin Write

The public document-writing skill for [Margin](https://margin-meadow-rfc-lab.gerrymi.chatgpt.site),
a collaborative annotation layer for generated web documents.

Install it with:

```sh
npx skills add gerrymi/margin-skills --skill margin-write
```

Margin Write creates and revises reviewable documents. It teaches compatible coding
agents to emit stable `data-mrgn` targets, preserve target identity through rewrites,
carry renamed targets as aliases, register opaque authorship through authenticated
Margin MCP, and apply authorized review conversations during regeneration.

Generated documents record `margin-write-version`. Margin can compare that document
marker to its recommended release, but it does not inspect the user's machine.

The skill contains instructions only. It does not contain application credentials,
Margin review data, or access to private repositories.
