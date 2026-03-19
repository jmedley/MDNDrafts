---
title: ModelContextClient
slug: Web/API/ModelContextClient
page-type: web-api-interface
status:
  - experimental
browser-compat: api.ModelContextClient
---

{{APIRef("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The **`ModelContextClient`** interface of the [WebMCP API](/en-US/docs/Web/API/WebMCP_API)
represents the AI agent that is executing a tool provided by the site through
{{domxref("ModelContext")}}. An instance of `ModelContextClient` is passed as the
second argument to every {{domxref("ToolExecuteCallback")}} and provides methods that
allow a tool to interact with the user during execution.

A `ModelContextClient` cannot be constructed directly.

## Instance methods

- {{domxref("ModelContextClient.requestUserInteraction()")}} {{Experimental_Inline}}
  - : Asynchronously requests user input during the execution of a tool, pausing the
    agent until the interaction completes.

## Examples

### Requesting confirmation before a destructive action

```js
navigator.modelContext.registerTool({
  name: "clearCart",
  description: "Removes all items from the shopping cart.",
  execute: async (input, client) => {
    const confirmed = await client.requestUserInteraction(async () => {
      return confirm("Remove all items from your cart?");
    });
    if (confirmed) {
      cart.clear();
      return { cleared: true };
    }
    return { cleared: false };
  },
});
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("ModelContext")}}
- {{domxref("ToolExecuteCallback")}}
- {{domxref("Navigator.modelContext")}}
