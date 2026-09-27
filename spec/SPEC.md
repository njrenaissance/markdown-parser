# SPEC.md

**Status:** approved

## Purpose
Convert Markdown text into an Abstract Syntax Tree (AST) for a web application, enabling HTML rendering and programmatic access to document structure.

## Inputs / Outputs
**Input:** A string containing standard Markdown text

**Output:** An AST object where each node has:
- `type`: string (e.g., "heading", "paragraph", "emphasis", "strong", "link", "code_block", "code", "text")
- `level`: number (for headings; 1-6)
- `children`: array of child nodes (up to 2 levels deep, or null for leaf nodes)
- `content`: string (for text nodes and code blocks)
- `href`: string (for links)
- `language`: string (for code blocks; defaults to empty string if not specified)

## What we produce
Library (with a CLI for testing/demonstration)

## Where we persist
Stateless

## Method
Rules-based

## Done criteria
- [ ] Lexer tokenizes Markdown into tokens: text, emphasis markers (`*`), strong markers (`**`), links, code fences, headings, line breaks
- [ ] Parser builds AST from tokens with correct nesting (max 2 levels deep)
- [ ] `parse("# Heading")` produces `{type: "heading", level: 1, children: [{type: "text", content: "Heading"}]}`
- [ ] `parse("*italic*")` produces `{type: "emphasis", children: [{type: "text", content: "italic"}]}`
- [ ] `parse("**bold**")` produces `{type: "strong", children: [{type: "text", content: "bold"}]}`
- [ ] `parse("***bold italic***")` produces nested `{type: "strong", children: [{type: "emphasis", children: [{type: "text", content: "bold italic"}]}]}`
- [ ] `parse("[link text](https://example.com)")` produces `{type: "link", href: "https://example.com", children: [{type: "text", content: "link text"}]}`
- [ ] `parse("```python\nprint('hi')\n```")` produces `{type: "code_block", language: "python", content: "print('hi')\n"}`
- [ ] `parse("`inline`")` produces `{type: "code", content: "inline"}`
- [ ] Parser enforces 2-level nesting max; deeper structures are flattened or treated as text
- [ ] Parser handles unclosed delimiters gracefully (treats as literal text or rejects with clear error)
