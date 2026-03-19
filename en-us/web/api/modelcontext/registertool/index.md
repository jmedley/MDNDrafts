---
title: "ModelContext: registerTool() method"
short-title: registerTool()
slug: Web/API/ModelContext/registerTool
page-type: web-api-instance-method
status:
  - experimental
browser-compat: api.ModelContext.registerTool
---

{{APIRef("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The **`registerTool()`** method of the {{domxref("ModelContext")}} interface registers
a single tool, making it available for AI agents to invoke. The tool is described by a
{{domxref("ModelContextTool")}} dictionary that includes a name, a natural-language
description, an optional JSON Schema for the input, and an execute callback.

Calling `registerTool()` with the name of an already-registered tool throws an error.
To replace a tool, call {{domxref("ModelContext.unregisterTool()")}} first.

## Syntax

```js-nolint
registerTool(tool)
```

### Parameters

- `tool`
  - : A {{domxref("ModelContextTool")}} dictionary describing the tool to register.

### Return value

None ({{jsxref("undefined")}}).

### Exceptions

- `InvalidStateError` {{domxref("DOMException")}}
  - : Thrown if a tool with the same `name` is already registered, or if `name` or
    `description` is an empty string.
- {{jsxref("TypeError")}}
  - : Thrown if `inputSchema` cannot be serialized to JSON (for example, if it contains
    a circular reference or if its `toJSON()` method returns `undefined`).

## Examples

### Registering a simple tool

```js
navigator.modelContext.registerTool({
  name: "getPageTitle",
  description: "Returns the title of the current page.",
  execute: async (input, client) => {
    return document.title;
  },
  annotations: { readOnlyHint: true },
});
```

### Registering a tool with an input schema

```js
navigator.modelContext.registerTool({
  name: "addToCart",
  description: "Adds a product to the shopping cart by its SKU.",
  inputSchema: {
    type: "object",
    properties: {
      sku: {
        type: "string",
        description: "The unique product SKU to add.",
      },
      quantity: {
        type: "integer",
        description: "Number of units to add. Defaults to 1.",
        default: 1,
      },
    },
    required: ["sku"],
  },
  execute: async ({ sku, quantity = 1 }, client) => {
    await cart.add(sku, quantity);
    return { success: true };
  },
});
```

### Handling registration errors

```js
try {
  navigator.modelContext.registerTool({
    name: "getPageTitle",
    description: "Duplicate tool.",
    execute: async () => document.title,
  });
} catch (err) {
  if (err.name === "InvalidStateError") {
    console.warn("A tool with that name is already registered.");
  }
}
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("ModelContext.unregisterTool()")}}
- {{domxref("ModelContextTool")}}
- {{domxref("Navigator.modelContext")}}
