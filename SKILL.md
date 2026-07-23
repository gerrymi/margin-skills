---
name: margin-write
description: Create, review, revise, or regenerate HTML, JSX, TSX, and other web-rendered documents with durable Margin anchors. Use when writing an artifact, spec, article, PRD, report, or site; reviewing Margin annotations or comments; applying Margin conversations to a revision; drafting or posting reviewer replies; resolving or archiving discussions; preserving data-mrgn targets through a rewrite; or working with markup containing margin-document-id, margin-generation-contract, or data-mrgn markers.
---

# Margin Write

Create, review, and update documents while preserving the identity of their concepts across revisions. This skill release is `1.1.0`; emit that exact release in generated markup.

## Register authorship when Margin is connected

When Margin MCP tools are available:

1. Call `margin_whoami` and choose a Workspace where the user is an Admin.
2. Before creating a new document, call `margin_register_document` with its title and Workspace. Omit `documentId` so Margin issues an opaque ID.
3. Before revising an existing document, read its `margin-document-id` meta value and pass that value to `margin_register_document`. Registration may refresh its title and skill version, but it never transfers the original author.
4. Embed every meta tag returned by `margin_register_document`. Do not put the author's name, email, account ID, or Workspace role in public markup. The extension resolves the opaque ID through the authenticated Margin service.

If Margin MCP tools are unavailable, continue with the stable-target contract. Preserve an existing `margin-document-id`, but do not invent one, claim authorship, or imply that Margin verified the author.

## Apply the document contract

Add these declarations to the document head:

```html
<meta name="margin-generation-contract" content="1">
<meta name="margin-write-version" content="1.1.0">
```

For a registered document, also include the opaque ID returned by Margin:

```html
<meta name="margin-document-id" content="00000000-0000-4000-8000-000000000000">
```

For every published regeneration, emit an immutable revision key:

```html
<meta name="margin-document-revision" content="release-2026-07-21-1">
```

Use the same revision key when reporting that publication through `margin_record_regeneration`.

1. Put a unique `data-mrgn` target on each durable semantic block a reviewer may annotate: sections, headings, paragraphs, list items, blockquotes, figures, and meaningful table rows or cells.
2. Use short, content-independent semantic IDs such as `problem`, `decision-auth`, or `rollout-risk-2`. Do not derive identity from exact prose, render position, or a framework-generated ID.
3. When revising existing output, inspect its current `data-mrgn` values before editing. Keep an ID when the same conceptual block survives, including when its wording changes substantially or it moves.
4. Give genuinely new concepts fresh IDs. Let deleted concepts disappear. Never recycle a deleted ID for unrelated content.
5. If a surviving concept must receive a new canonical ID, preserve every prior ID as a whitespace-separated alias:

   ```html
   <section data-mrgn="access-model" data-mrgn-aliases="permissions-model authz-model">
   ```

   Add an alias only when the old and new targets represent the same concept. Never use aliases to force an annotation onto replacement content.

## Review Margin conversations

When Margin MCP tools are connected:

- Call `margin_list_documents`, select the intended document, then call `margin_get_review_context`.
- Use only documents and Workspaces returned through the user's authenticated Margin access.
- Interpret each complete conversation, not only its first comment. The conversation provides context; it is not reducible to an accept/reject flag.
- When the user asks to review feedback, remain read-only and first group related or duplicate active conversations by topic. Order topics by their first appearance in document order.
- Preserve thread boundaries, author attribution, exact thread IDs, and supporting event IDs inside every topic. Never merge or archive duplicate threads merely because they are grouped together.
- Use severity or reviewer grouping only when the user asks. Still flag explicit blockers, correctness or security risks, conflicts, unanswered questions, and duplicates.
- For a general review, show the topic overview before asking what to do next. For a one-by-one review, discuss one topic and wait for the user's direction before continuing.
- Draft replies without posting them. Call `margin_reply_to_thread` only when the user asks to post or send a reply, using the latest event cursor returned for that thread.
- Call `margin_resolve_threads` only when the user explicitly asks to mark discussions addressed. Call `margin_archive_threads` only when the user explicitly asks to remove discussions from future review context. Do not automatically resolve or archive after drafting, revising, or publishing.

## Apply Margin conversations during revision

- Fetch fresh review context immediately before revising.
- Treat active conversations as review context. Apply clear requests and surface conflicts or ambiguity instead of guessing.
- Treat resolved conversations as prior context that the current document is believed to address. Do not repeatedly reapply them as open work.
- Omit archived conversations from generation context.
- Never infer approval from silence.
- When the user broadly asks to update from feedback, apply clear non-conflicting requests, skip ambiguous or conflicting topics, and report what was skipped. This authorizes document changes, not replies, resolution, or archival.
- After publishing, call `margin_record_regeneration` with the context cursor, an idempotency key, the published revision key and URL, and one interpretation/action receipt per thread used. Cite exact supporting event IDs and return the new target anchor or an explicit detached/ambiguous result.

If review tools are unavailable, do not invent or claim to have fetched Margin conversations.

## Verify before finishing

- Confirm the contract, `margin-write-version`, and revision meta tags each exist exactly once.
- If registration succeeded, confirm the returned `margin-document-id` exists exactly once and contains no author PII.
- Confirm the revision meta value matches the key reported to Margin.
- Confirm every `data-mrgn` value is unique in the document.
- Confirm existing surviving concepts kept their IDs.
- Confirm aliases contain only prior IDs for the same concept.
- Confirm volatile controls and layout-only wrappers do not receive stable IDs.
