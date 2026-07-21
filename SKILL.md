---
name: margin-generation
description: Generate or revise HTML, JSX, TSX, or other web-rendered documents with Margin's stable anchoring contract. Use when creating an artifact, spec, article, PRD, report, or site that people will review with Margin; when preserving annotations across a document rewrite; or when existing markup contains data-mrgn targets or the margin-generation-contract meta tag.
---

# Margin Generation

Preserve the identity of document concepts across edits so Margin can reattach human conversations without guessing.

## Apply the contract

1. Add this declaration to the generated document head:

   ```html
   <meta name="margin-generation-contract" content="1">
   ```

   For every published regeneration, also emit an immutable revision key:

   ```html
   <meta name="margin-document-revision" content="release-2026-07-21-1">
   ```

   Use the same key when reporting that publication through `margin_record_regeneration`.

2. Put a unique `data-mrgn` target on each durable semantic block a reviewer may annotate: sections, headings, paragraphs, list items, blockquotes, figures, and meaningful table rows or cells.
3. Use short, content-independent semantic IDs such as `problem`, `decision-auth`, or `rollout-risk-2`. Do not derive identity from the exact prose, a render position, or a framework-generated ID.
4. When revising existing output, inspect its current `data-mrgn` values before editing. Keep an ID when the same conceptual block survives, including when its wording changes substantially or it moves.
5. Give genuinely new concepts fresh IDs. Let deleted concepts disappear. Never recycle a deleted ID for unrelated content.
6. If a surviving concept must receive a new canonical ID, preserve every prior ID as a whitespace-separated alias:

   ```html
   <section data-mrgn="access-model" data-mrgn-aliases="permissions-model authz-model">
   ```

   Add an alias only when the old and new targets represent the same concept. Never use aliases to force an annotation onto replacement content.

## Work with Margin conversations

When Margin MCP tools are connected:

- Authenticate as the user before reading review data.
- Call `margin_list_documents`, select the intended document, then call `margin_get_review_context` before revising it.
- Use only documents and spaces returned through the user's Margin access.
- Interpret the complete conversation, not only its first comment.
- Treat active conversations as review context. Apply clear requests and surface conflicts or ambiguity instead of guessing.
- Treat resolved conversations as prior context that the current document is believed to address. Do not repeatedly reapply them as open work.
- Omit archived conversations from generation context.
- Never infer approval from silence.
- After publishing, call `margin_record_regeneration` with the context cursor, an idempotency key, the published revision key and URL, and one interpretation/action receipt per thread you used. Cite the exact supporting event IDs and return the new target anchor or an explicit detached/ambiguous result.

If the required review tools are unavailable, continue with the stable-ID contract. Do not invent or claim to have fetched Margin conversations.

## Verify before finishing

- Confirm the contract meta tag exists exactly once.
- Confirm the revision meta tag matches the key reported to Margin.
- Confirm every `data-mrgn` value is unique in the document.
- Confirm existing surviving concepts kept their IDs.
- Confirm aliases contain only prior IDs for the same concept.
- Confirm volatile controls and layout-only wrappers do not receive stable IDs.
