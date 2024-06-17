## `lg`: a CLI tool that makes AI write better code.

## Problem:

1. You ask an LLM (ChatGPT, Mistral, Anthropic, etc) to generate code using your favorite library.
2. The LLM generates code for you, but that code is pretty out of date. The lib has changed since the LLM has indexed it.

You can remedy this by pasting _relevant parts of the lib's documentation_ into the LLM's context (aka, one-shot or few-shot training), and get way better code.

This can become time-consuming.

## Solution:

This CLI tool aims to make this process easier and faster by attempting to associate your prompt with the relevant docs.

## How it works:

1. Install this tool
2. Download the docs to any and all libraries you want to generate code for. Put them in ~/.llm-codegen/my-lib
3. Run the CLI tool with your prompt and the library name (e.g., `lg gen shadcn/form "a form for capturing patient medical information"`)

Result: up-to-date code generated by the LLM.

## Installation

```bash
  npm install -g llm-codegen

  # or

  pnpm add -g llm-codegen

  # or

  yarn global add llm-codegen

  # or

  bun install -g llm-codegen
```

## Setup

1. Run `lg keys set` to set your OpenAI API key.
2. Follow the instructions below to add docs from your favorite code libraries.

Note: More LLMs will be supported soon!

### Adding library documents

`llm-codegen` looks for library documents in `~/.llm-codegen`. Add folders named after each library you want to generate code for. Generators will be created for each document you create, using a relative path with each file extension removed.

Let's use the following folder structure below (with the great [shadcn-ui](https://ui.shadcn.com/) library as an example). I copied these docs right from the [shadcn-ui GitHub repo](https://github.com/shadcn-ui/ui/tree/main/apps/www/content/docs/components).

```
~/.llm-codegen
  └── shadcn
      ├── accordion.mdx
      ├── alert-dialog.mdx
      ├── alert.mdx
      ├── aspect-ratio.mdx
      ├── avatar.mdx
      ├── badge.mdx
      ├── breadcrumb.mdx
      ├── button.mdx
      ...
```

Running `lg gen list` will now list the following generator names:

```bash
shadcn/accordion
shadcn/alert-dialog
shadcn/alert
shadcn/aspect-ratio
shadcn/avatar
shadcn/badge
shadcn/breadcrumb
shadcn/button
# ...
```

Which you can then use to generate code e.g.:

```bash
lg gen shadcn/accordion

# or

lg gen shadcn/accordion "with headings for each US state, with content for that states' capital, population, and area."

```

## Commands

### 1. `lg gen list`

Lists all the available generators. These are read from your `~/.llm-codegen` directory.

### 1. `lg gen <model> [prompt]`

Generates code for the specified model. If a prompt fragment is provided, it will be used to tailor the code appropriately. See the examples for details.

### 1. `lg keys set <model>`

Configures your API key for the specified model. Only OpenAI supported currently - this will change soon.

### 2. `lg keys list`

List your key configuration.

## Examples

```bash
# No prompt fragment; will likely give you code directly from the docs.
lg gen shadcn/form
```

```bash
# With a prompt fragment, will treat the docs code as a starting point and tailor it to your prompt.
lg gen shadcn/form "for inputting a business address"
```

```bash
lg gen shadcn/form "a form for capturing patient medical information"
```

### TODOs

- [ ] Support multiple models
- [ ] Add custom folder support
- [x] Support config via env vars and CLI options
- [x] Export as package
- [x] Show spinner
- [x] Stream output
