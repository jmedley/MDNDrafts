---
title: "ModelContext: unregisterTool() method"
short-title: unregisterTool()
slug: Web/API/ModelContext/unregisterTool
page-type: web-api-instance-method
status:
  - experimental
browser-compat: api.ModelContext.unregisterTool
---

{{APIRef("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The **`unregisterTool()`** method of the {{domxref("ModelContext")}} interface removes
a previously registered tool by name, making it no longer available for agents to
invoke.

## Syntax

```js-nolint
unregisterTool(name)
```

### Parameters

- `name`
  - : A string specifying the name of the tool to remove. This must exactly match the
    `name` supplied when the tool was registered with
    {{domxref("ModelContext.registerTool()")}}.

### Return value

None ({{jsxref("undefined")}}).

### Exceptions

- `InvalidStateError` {{domxref("DOMException")}}
  - : Thrown if no tool with the given `name` is currently registered.

## Examples

### Removing a tool after use

```js
navigator.modelContext.registerTool({
  name: "submitCheckout",
  description: "Submits the current shopping cart for checkout.",
  execute: async (input, client) => {
    return checkout.submit();
  },
});

// Once the user completes checkout, remove the tool.
navigator.modelContext.unregisterTool("submitCheckout");
```

### Guarding against errors

```js
try {
  navigator.modelContext.unregisterTool("nonExistentTool");
} catch (err) {
  if (err.name === "InvalidStateError") {
    console.warn("No tool with that name was registered.");
  }
}
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("ModelContext.registerTool()")}}
- {{domxref("Navigator.modelContext")}}
