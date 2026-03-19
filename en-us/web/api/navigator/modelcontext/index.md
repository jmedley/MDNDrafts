---
title: "Navigator: modelContext property"
short-title: modelContext
slug: Web/API/Navigator/modelContext
page-type: web-api-instance-property
status:
  - experimental
browser-compat: api.Navigator.modelContext
---

{{APIRef("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The **`modelContext`** read-only property of the {{domxref("Navigator")}} interface
returns the {{domxref("ModelContext")}} object associated with the current document.
It provides methods to register and manage JavaScript-based tools that can be invoked
by AI agents.

## Value

A {{domxref("ModelContext")}} object.

## Examples

### Registering a read-only tool

This example registers a tool that returns the current page title. Because it does not
modify any state, `readOnlyHint` is set to `true`.

```js
navigator.modelContext.registerTool({
  name: "getPageTitle",
  description: "Returns the title of the current page.",
  execute: async (input, client) => {
    return document.title;
  },
  annotations: {
    readOnlyHint: true,
  },
});
```

### Registering a tool with an input schema

Tools can declare a JSON Schema object via `inputSchema` so that agents know what
parameters to supply.

```js
navigator.modelContext.registerTool({
  name: "searchProducts",
  description: "Searches the product catalog for items matching the given query.",
  inputSchema: {
    type: "object",
    properties: {
      query: {
        type: "string",
        description: "The search string to look up.",
      },
    },
    required: ["query"],
  },
  execute: async ({ query }, client) => {
    const results = await fetchProducts(query);
    return results;
  },
});
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("ModelContext")}}
- {{domxref("ModelContextClient")}}
- [WebMCP API](/en-US/docs/Web/API/WebMCP_API)
