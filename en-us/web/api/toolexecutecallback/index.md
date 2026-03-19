---
title: ToolExecuteCallback
slug: Web/API/ToolExecuteCallback
page-type: web-api-callback-function
status:
  - experimental
browser-compat: api.ToolExecuteCallback
---

{{APIRef("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The **`ToolExecuteCallback`** callback function type of the
[WebMCP API](/en-US/docs/Web/API/WebMCP_API) defines the signature of the function
that is invoked when an AI agent calls a tool registered with
{{domxref("ModelContext.registerTool()")}}. It receives the agent-supplied input and a
{{domxref("ModelContextClient")}} instance, and returns a {{jsxref("Promise")}} that
resolves to the tool's result.

## Syntax

```js
async function execute(input, client) { /* … */ }
```

### Parameters

- `input`
  - : An object containing the input parameters supplied by the agent. The shape of this
    object corresponds to the JSON Schema declared in
    {{domxref("ModelContextTool.inputSchema", "inputSchema")}}. If no `inputSchema` was
    provided, this is an empty object.
- `client`
  - : A {{domxref("ModelContextClient")}} representing the agent invoking the tool. Use
    this to call {{domxref("ModelContextClient.requestUserInteraction()")}} when the
    tool needs to pause and collect input from the user.

### Return value

A {{jsxref("Promise")}} that resolves with the tool's result. The resolved value is
returned to the invoking agent.

## Examples

### A simple synchronous-style execute callback

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

### An execute callback that uses the client

```js
navigator.modelContext.registerTool({
  name: "submitForm",
  description: "Submits the main form on the page after user confirmation.",
  execute: async (input, client) => {
    const ok = await client.requestUserInteraction(async () => {
      return confirm("Submit the form now?");
    });
    if (!ok) return { submitted: false };
    document.querySelector("form").submit();
    return { submitted: true };
  },
});
```

### An execute callback with structured input

```js
navigator.modelContext.registerTool({
  name: "navigateTo",
  description: "Navigates the page to the given URL.",
  inputSchema: {
    type: "object",
    properties: {
      url: { type: "string", description: "The URL to navigate to." },
    },
    required: ["url"],
  },
  execute: async ({ url }, client) => {
    location.href = url;
    return { navigating: true };
  },
});
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("ModelContextTool")}}
- {{domxref("ModelContextClient")}}
- {{domxref("UserInteractionCallback")}}
