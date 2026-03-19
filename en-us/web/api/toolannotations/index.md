---
title: ToolAnnotations
slug: Web/API/ToolAnnotations
page-type: web-api-interface
status:
  - experimental
browser-compat: api.ToolAnnotations
---

{{APIRef("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The **`ToolAnnotations`** dictionary of the [WebMCP API](/en-US/docs/Web/API/WebMCP_API)
provides optional metadata about a tool registered with
{{domxref("ModelContext.registerTool()")}}. Annotations give agents additional hints
about the tool's behavior without affecting how the tool is invoked.

## Instance properties

- `readOnlyHint` {{optional_inline}}
  - : A boolean. When `true`, indicates that the tool does not modify any state and only
    reads data. Agents may use this hint to determine when it is safe to call the tool
    without user confirmation. Defaults to `false`.

## Examples

### Annotating a read-only tool

```js
navigator.modelContext.registerTool({
  name: "getCartContents",
  description: "Returns the items currently in the shopping cart.",
  execute: async (input, client) => cart.getItems(),
  annotations: {
    readOnlyHint: true,
  },
});
```

### Omitting annotations

The `annotations` property is optional. When omitted, `readOnlyHint` defaults to
`false`, signaling to agents that the tool may modify state.

```js
navigator.modelContext.registerTool({
  name: "placeOrder",
  description: "Places an order for the items in the current cart.",
  execute: async (input, client) => cart.placeOrder(),
});
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("ModelContextTool")}}
- {{domxref("ModelContext.registerTool()")}}
