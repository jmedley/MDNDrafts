---
title: WebMCP API
slug: Web/API/WebMCP_API
page-type: web-api-overview
status:
  - experimental
browser-compat: api.ModelContext
spec-urls: https://webmachinelearning.github.io/webmcp/
---

{{DefaultAPISidebar("WebMCP API")}}{{SeeCompatTable}}{{SecureContext_Header}}

The WebMCP API enables web applications to expose JavaScript-based tools that AI agents can discover and invoke.

## Concepts and usage

AI agents — autonomous assistants powered by large language models — can help users accomplish tasks on the web by calling _tools_: named functions with structured inputs and natural-language descriptions. The WebMCP API is the browser-side mechanism by which a web application registers such tools and makes them available to any agent operating in the browser.

### Tools and agents

A **tool** is a JavaScript function paired with a name, a natural-language description, and an optional [JSON Schema](https://json-schema.org/) that describes its expected input parameters. Agents use the description and schema to decide when and how to call the tool, then supply structured arguments as the `input` object when they invoke it.

A **browser's agent** is an AI agent provided by or through the browser — for example a built-in assistant, an extension, or a plugin. The WebMCP API is the contract between such agents and the web page.

### Registering tools

A page registers tools through the {{domxref("ModelContext")}} object exposed on {{domxref("Navigator.modelContext", "navigator.modelContext")}}. Each tool is described by a {{domxref("ModelContextTool")}} dictionary passed to {{domxref("ModelContext.registerTool()")}}:

- `name` — a unique string identifier. Must be between 1 and 128 characters and contain only ASCII alphanumeric characters, underscores (`_`), hyphens (`-`), or periods (`.`).
- `title` _(optional)_ — a human-readable display name for the tool, intended for native browser UIs.
- `description` — a natural-language explanation agents use to determine when to call the tool.
- `inputSchema` _(optional)_ — a JSON Schema object describing the parameters the agent should supply.
- `execute` — a {{domxref("ToolExecuteCallback")}} function that runs when an agent invokes the tool.
- `annotations` _(optional)_ — a {{domxref("ToolAnnotations")}} dictionary with behavioral hints.

Registering a tool throws an `InvalidStateError` if a tool with the same name is already registered, if `name` or `description` is empty, if `name` exceeds 128 characters or contains disallowed characters, or if the `inputSchema` cannot be serialized to JSON (which throws a `TypeError`).

### Tool execution

When an agent calls a registered tool, the browser invokes the tool's `execute` callback with two arguments: an `input` object (shaped according to `inputSchema`, or an empty object if none was provided) and a {{domxref("ModelContextClient")}} instance representing the agent. The callback returns a {{jsxref("Promise")}} that resolves to the tool's result, which is then returned to the agent.

### User interaction during tool execution

Some tools need to pause and collect input from the user — for example, to confirm a destructive action or prompt for a value. The {{domxref("ModelContextClient.requestUserInteraction()")}} method handles this: it accepts a {{domxref("UserInteractionCallback")}} that performs the user-facing interaction (such as showing a `confirm()` dialog or a custom modal), and returns a Promise that resolves with the interaction's result.

### Behavioral hints

The optional {{domxref("ToolAnnotations")}} dictionary lets a tool signal metadata to agents. The `readOnlyHint` boolean, when `true`, indicates that the tool does not modify any state and only reads data. Agents may use this hint to decide when it is safe to call the tool without user confirmation.

### Lifecycle and cleanup

A tool is removed by supplying an {{domxref("AbortSignal")}} in the {{domxref("ModelContextRegisterToolOptions", "options")}} argument to `registerTool()`; the tool is automatically unregistered when the signal is aborted.

### Security

The WebMCP API is only available in [secure contexts](/en-US/docs/Web/Security/Secure_Contexts) (HTTPS). Tool names must be non-empty, at most 128 characters, and consist only of ASCII alphanumeric characters, underscores, hyphens, or periods. Descriptions must be non-empty strings. Any `inputSchema` must be serializable to JSON or `registerTool()` will throw a `TypeError`.

## Interfaces

- [ModelContext](../modelcontext/index.md)
  - : The primary interface for registering and unregistering tools. Accessed via `navigator.modelContext`.
- [ModelContextClient](../modelcontextclient/index.md)
  - : Represents the AI agent that is executing a tool. Passed as the second argument to every {{domxref("ToolExecuteCallback")}} and provides `requestUserInteraction()` for collecting user input during execution.

## Dictionaries

- [ModelContextTool](../modelcontexttool/index.md)
  - : Describes a tool to be registered: its `name`, optional `title`, `description`, optional `inputSchema`, `execute` callback, and optional `annotations`.
- [ToolAnnotations](../toolannotations/index.md)
  - : Optional metadata about a tool's behavior, including the `readOnlyHint` boolean.
- [ModelContextRegisterToolOptions](../modelcontextregistertoololptions/index.md)
  - : Options for {{domxref("ModelContext.registerTool()")}}, including an optional `signal` ({{domxref("AbortSignal")}}) that automatically unregisters the tool when aborted.

## Callback functions

- [ToolExecuteCallback](../toolexecutecallback/index.md)
  - : The signature of the function invoked when an agent calls a tool. Receives the agent-supplied `input` object and a `ModelContextClient`, and returns a {{jsxref("Promise")}} with the tool's result.
- [UserInteractionCallback](../userinteractioncallback/index.md)
  - : The signature of the function passed to `ModelContextClient.requestUserInteraction()`. Performs a user-facing interaction and returns a {{jsxref("Promise")}} resolving to its result.

## Extensions to other interfaces

### Navigator

- [Navigator: modelContext property](../navigator/modelcontext/index.md) {{ReadOnlyInline}}
  - : Returns the `ModelContext` object for the current document.

## Examples

### Registering a tool that requests user confirmation

This example registers a tool that deletes an account after asking the user to confirm, demonstrating `inputSchema`, `execute`, and `requestUserInteraction()`.

```js
navigator.modelContext.registerTool({
  name: "deleteAccount",
  description:
    "Permanently deletes the current user account. Always ask the user to confirm before calling this tool.",
  execute: async (input, client) => {
    const confirmed = await client.requestUserInteraction(async () => {
      return confirm(
        "An AI agent wants to delete your account. Do you want to proceed?",
      );
    });

    if (!confirmed) {
      return { deleted: false, reason: "User cancelled." };
    }

    await fetch("/api/account", { method: "DELETE" });
    return { deleted: true };
  },
});
```

### Registering a read-only tool with an input schema

This example registers a tool that searches a product catalog. Because it does not modify any state, `readOnlyHint` is set to `true`.

```js
navigator.modelContext.registerTool({
  name: "searchProducts",
  description:
    "Searches the product catalog for items matching the given query.",
  inputSchema: {
    type: "object",
    properties: {
      query: {
        type: "string",
        description: "The search term.",
      },
    },
    required: ["query"],
  },
  execute: async ({ query }, client) => {
    const response = await fetch(
      `/api/products?q=${encodeURIComponent(query)}`,
    );
    return response.json();
  },
  annotations: {
    readOnlyHint: true,
  },
});
```

### Unregistering a tool via AbortSignal

Tools can be tied to an `AbortController` so they are automatically removed when no longer needed.

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

// The tool is unregistered when the controller is aborted.
controller.abort();
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- {{domxref("Navigator.modelContext")}}
- [Model Context Protocol specification](https://modelcontextprotocol.io/specification/latest)
