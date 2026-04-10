---
title: ModelContextTool
slug: Web/API/ModelContextTool
page-type: web-api-interface
status:
  - experimental
browser-compat: api.ModelContextTool
---

{{APIRef("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The **`ModelContextTool`** dictionary of the [WebMCP API](/en-US/docs/Web/API/WebMCP_API)
describes a tool that can be registered with {{domxref("ModelContext.registerTool()")}}
and subsequently invoked by AI agents. It includes a name, a natural-language
description, an optional JSON Schema describing the expected input, an execute callback,
and optional behavioral annotations.

## Instance properties

- `name`
  - : A string providing a unique identifier for the tool within the current
    {{domxref("ModelContext")}}. Agents use this name to reference the tool when making
    tool calls. Must be between 1 and 128 characters and contain only ASCII alphanumeric
    characters, underscores (`_`), hyphens (`-`), or periods (`.`).
- `title` {{optional_inline}}
  - : A string providing a human-readable display name for the tool. Unlike `name`, this
    value is intended for display in native browser UIs and has no character restrictions.
    If omitted, the tool has no display name.
- `description`
  - : A string containing a natural-language description of the tool's functionality.
    This helps agents understand when and how to use the tool. Must not be an empty
    string.
- `inputSchema` {{optional_inline}}
  - : An object representing a [JSON Schema](https://json-schema.org/draft/2020-12/json-schema-core.html)
    that describes the expected input parameters for the tool. Agents use this schema to
    construct well-formed tool calls. If omitted, the tool accepts no structured input.
- `execute`
  - : A {{domxref("ToolExecuteCallback")}} function invoked when an agent calls the tool.
    The callback receives the `input` parameters and a {{domxref("ModelContextClient")}}
    object. It may return a {{jsxref("Promise")}}, in which case the agent receives the
    result once the promise settles.
- `annotations` {{optional_inline}}
  - : A {{domxref("ToolAnnotations")}} dictionary providing optional metadata about the
    tool's behavior.

## Examples

### A minimal tool

```js
const tool = {
  name: "getPageTitle",
  description: "Returns the title of the current page.",
  execute: async (input, client) => document.title,
};

navigator.modelContext.registerTool(tool);
```

### A tool with an input schema and annotations

```js
const tool = {
  name: "searchProducts",
  description: "Searches the product catalog for items matching the given query.",
  inputSchema: {
    type: "object",
    properties: {
      query: {
        type: "string",
        description: "The search term.",
      },
    },
    required: ["query"],
  },
  execute: async ({ query }, client) => fetchProducts(query),
  annotations: {
    readOnlyHint: true,
  },
};

navigator.modelContext.registerTool(tool);
```

### A tool that requests user interaction

```js
const tool = {
  name: "deleteAccount",
  description: "Permanently deletes the current user account after confirmation.",
  execute: async (input, client) => {
    const confirmed = await client.requestUserInteraction(async () => {
      return confirm("Are you sure you want to delete your account?");
    });
    if (confirmed) {
      await account.delete();
      return { deleted: true };
    }
    return { deleted: false };
  },
};

navigator.modelContext.registerTool(tool);
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("ModelContext.registerTool()")}}
- {{domxref("ToolAnnotations")}}
- {{domxref("ToolExecuteCallback")}}
- {{domxref("ModelContextClient")}}
