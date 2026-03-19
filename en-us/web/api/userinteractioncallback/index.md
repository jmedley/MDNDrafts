---
title: UserInteractionCallback
slug: Web/API/UserInteractionCallback
page-type: web-api-callback-function
status:
  - experimental
browser-compat: api.UserInteractionCallback
---

{{APIRef("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The **`UserInteractionCallback`** callback function type of the
[WebMCP API](/en-US/docs/Web/API/WebMCP_API) defines the signature of the function
passed to {{domxref("ModelContextClient.requestUserInteraction()")}}. It is invoked to
perform a user-facing interaction — such as displaying a confirmation dialog or prompt
— and must return a {{jsxref("Promise")}} that resolves with the result of that
interaction.

## Syntax

```js
async function callback() { /* … */ }
```

### Parameters

None.

### Return value

A {{jsxref("Promise")}} that resolves with the result of the user interaction. The
resolved value is passed back as the return value of
{{domxref("ModelContextClient.requestUserInteraction()")}}.

## Examples

### A confirmation dialog

```js
const confirmed = await client.requestUserInteraction(async () => {
  return confirm("Are you sure you want to proceed?");
});
```

### A prompt for text input

```js
const name = await client.requestUserInteraction(async () => {
  return prompt("Enter your display name:");
});
```

### A custom modal dialog

```js
const result = await client.requestUserInteraction(() => {
  return new Promise((resolve) => {
    const dialog = document.querySelector("#confirm-dialog");
    dialog.addEventListener(
      "close",
      () => resolve(dialog.returnValue === "confirm"),
      { once: true }
    );
    dialog.showModal();
  });
});
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("ModelContextClient.requestUserInteraction()")}}
- {{domxref("ToolExecuteCallback")}}
