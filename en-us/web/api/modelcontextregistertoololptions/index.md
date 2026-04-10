---
title: ModelContextRegisterToolOptions
slug: Web/API/ModelContextRegisterToolOptions
page-type: web-api-interface
status:
  - experimental
browser-compat: api.ModelContextRegisterToolOptions
---

{{APIRef("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The **`ModelContextRegisterToolOptions`** dictionary of the [WebMCP API](/en-US/docs/Web/API/WebMCP_API)
provides optional configuration for {{domxref("ModelContext.registerTool()")}}. It
currently exposes a single member, `signal`, which allows a tool to be automatically
unregistered when an {{domxref("AbortSignal")}} is aborted.

## Instance properties

- `signal` {{optional_inline}}
  - : An {{domxref("AbortSignal")}}. When provided, the tool is automatically unregistered
    from the {{domxref("ModelContext")}} when this signal is aborted. If the signal is
    already aborted at the time `registerTool()` is called, the tool is not registered
    and a warning may be reported to the console.

## Examples

### Unregistering a tool via AbortSignal

Pass an `AbortController`'s signal when registering a tool. Aborting the controller
later removes the tool automatically.

```js
const controller = new AbortController();

navigator.modelContext.registerTool(
  {
    name: "getCurrentUrl",
    description: "Returns the URL of the current page.",
    execute: async () => location.href,
    annotations: { readOnlyHint: true },
  },
  { signal: controller.signal },
);

// Unregister the tool when it is no longer needed.
controller.abort();
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("ModelContext.registerTool()")}}
- {{domxref("ModelContextTool")}}
- {{domxref("AbortSignal")}}
- [WebMCP API](/en-US/docs/Web/API/WebMCP_API)
