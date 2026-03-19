---
title: "ModelContextClient: requestUserInteraction() method"
short-title: requestUserInteraction()
slug: Web/API/ModelContextClient/requestUserInteraction
page-type: web-api-instance-method
status:
  - experimental
browser-compat: api.ModelContextClient.requestUserInteraction
---

{{APIRef("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The **`requestUserInteraction()`** method of the {{domxref("ModelContextClient")}}
interface asynchronously requests user input during the execution of a tool. The
provided {{domxref("UserInteractionCallback")}} is invoked to perform the interaction
(for example, displaying a confirmation dialog), and the returned promise resolves with
the callback's result.

This allows a tool's execute callback to pause mid-execution and wait for user input
before proceeding, keeping the user in control of actions taken on their behalf by an
agent.

## Syntax

```js-nolint
requestUserInteraction(callback)
```

### Parameters

- `callback`
  - : A {{domxref("UserInteractionCallback")}} function that performs the user
    interaction. It must return a {{jsxref("Promise")}} that resolves with the result
    of the interaction.

### Return value

A {{jsxref("Promise")}} that resolves with the value returned by `callback`.

## Examples

### Asking for confirmation

```js
navigator.modelContext.registerTool({
  name: "deleteFile",
  description: "Deletes the specified file after user confirmation.",
  inputSchema: {
    type: "object",
    properties: {
      filename: { type: "string", description: "Name of the file to delete." },
    },
    required: ["filename"],
  },
  execute: async ({ filename }, client) => {
    const confirmed = await client.requestUserInteraction(async () => {
      return confirm(`Are you sure you want to delete "${filename}"?`);
    });

    if (!confirmed) {
      return { deleted: false, reason: "User cancelled." };
    }

    await fileSystem.delete(filename);
    return { deleted: true };
  },
});
```

### Collecting user input via a custom dialog

```js
navigator.modelContext.registerTool({
  name: "renameFile",
  description: "Renames a file to a user-specified name.",
  inputSchema: {
    type: "object",
    properties: {
      filename: { type: "string" },
    },
    required: ["filename"],
  },
  execute: async ({ filename }, client) => {
    const newName = await client.requestUserInteraction(async () => {
      return prompt(`Enter a new name for "${filename}":`, filename);
    });

    if (!newName) {
      return { renamed: false, reason: "User cancelled." };
    }

    await fileSystem.rename(filename, newName);
    return { renamed: true, newName };
  },
});
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("ModelContextClient")}}
- {{domxref("UserInteractionCallback")}}
- {{domxref("ToolExecuteCallback")}}
