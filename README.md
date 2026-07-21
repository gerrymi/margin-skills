# Margin Generation

The public generation skill for [Margin](https://margin-meadow-rfc-lab.gerrymi.chatgpt.site),
a collaborative annotation layer for generated web documents.

Install it with:

```sh
npx skills add gerrymi/margin-skills --skill margin-generation
```

The skill teaches compatible coding agents to emit stable `data-mrgn` targets,
preserve target identity through rewrites, carry renamed targets as aliases, and use
authorized Margin MCP conversations during regeneration.

The skill contains instructions only. It does not contain application credentials,
Margin review data, or access to private repositories.
