---
title: ModelContext
slug: Web/API/ModelContext
page-type: web-api-interface
status:
  - experimental
browser-compat: api.ModelContext
---

{{APIRef("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The **`ModelContext`** interface of the [WebMCP API](/en-US/docs/Web/API/WebMCP_API)
provides methods for web applications to register and manage JavaScript-based tools
that can be invoked by AI agents. Tools registered through this interface are
analogous to tools in the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/specification/latest),
implemented entirely in client-side script.

A `ModelContext` instance is not constructed directly. Instead, it is accessed through
the {{domxref("Navigator.modelContext")}} property.

## Instance methods

- {{domxref("ModelContext.registerTool()")}} {{Experimental_Inline}}
  - : Registers a single tool, described by a {{domxref("ModelContextTool")}} dictionary,
    making it available for agents to invoke.
- {{domxref("ModelContext.unregisterTool()")}} {{Experimental_Inline}}
  - : Removes a previously registered tool by name.

## Examples

### Registering and unregistering a tool

```js
const ctx = navigator.modelContext;

ctx.registerTool({
  name: "getCurrentUrl",
  description: "Returns the URL of the current page.",
  execute: async (input, client) => {
    return location.href;
  },
  annotations: { readOnlyHint: true },
});

// Later, remove the tool when it is no longer needed.
ctx.unregisterTool("getCurrentUrl");
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("Navigator.modelContext")}}
- {{domxref("ModelContextTool")}}
- {{domxref("ModelContextClient")}}
- [WebMCP API](/en-US/docs/Web/API/WebMCP_API)
