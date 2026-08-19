> ## Documentation Index
> Fetch the complete documentation index at: https://www.mintlify.com/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Markdown export

> Export clean Markdown versions of your documentation pages for AI tools, LLM integrations, and automated content processing workflows.

export const PreviewButton = ({children, href}) => {
  return <a href={href} className="text-sm font-medium text-white dark:!text-zinc-950 bg-zinc-900 hover:bg-zinc-700 dark:bg-zinc-100 hover:dark:bg-zinc-300 rounded-full px-3.5 py-1.5 not-prose">
        {children}
      </a>;
};

Markdown provides structured text that AI tools can process more efficiently than HTML, which results in better response accuracy, faster processing times, and lower token usage.

Mintlify automatically generates Markdown versions of pages optimized for AI tools and external integrations.

## .md URL extension

Add `.md` to any page's URL to view a Markdown version.

<PreviewButton href="https://mintlify.com/docs/ai/markdown-export.md">Open this page as Markdown</PreviewButton>

## Accept header

Send a request with `Accept: text/markdown` or `Accept: text/plain` to any page URL to receive the Markdown version instead of HTML. This is useful for AI tools and integrations that programmatically fetch documentation content.

```bash theme={null}
curl -L -H "Accept: text/markdown" https://mintlify.com/docs/ai/markdown-export
```

## Audience-specific content

Use the [visibility](/docs/components/visibility) component to customize content for human and AI audiences.

Content wrapped in `<Visibility for="humans">` appears on the web page, but not in Markdown output. Content wrapped in `<Visibility for="agents">` appears in Markdown output, but not on the web page.

```mdx theme={null}
<Visibility for="humans">
  Click the **Get started** button in the top-right corner to create your account.
</Visibility>

<Visibility for="agents">
  To create an account, call `POST /v1/accounts` with a valid email address.
</Visibility>
```

## API reference pages

By default, Markdown exports of API reference pages include the full OpenAPI or AsyncAPI specification so AI tools have complete context about each endpoint.

If you prefer to omit the spec from Markdown output, set `markdown.schema` to `false` in your `docs.json`:

```json theme={null}
"markdown": {
  "schema": false
}
```

## Custom agent instructions

To append your own guidance to the Markdown that Mintlify serves to AI agents, set `markdown.instructions` in your `docs.json`. Use it for site-wide directions like citing an API version, preferring a specific SDK, or following your terminology.

Provide a single string:

```json Example agent instructions string theme={null}
"markdown": {
  "instructions": "Always cite the API version. Prefer the TypeScript SDK in examples."
}
```

Or an array of strings, which Mintlify joins with line breaks:

```json Example agent instructions array theme={null}
"markdown": {
  "instructions": [
    "Always cite the API version.",
    "Prefer the TypeScript SDK in examples."
  ]
}
```

Mintlify renders your instructions as an `Agent Instructions` block in the Markdown output:

```md Example rendered agent instructions theme={null}
> ## Agent Instructions
> Always cite the API version.
> Prefer the TypeScript SDK in examples.
```

The block appears in:

* The Markdown export of every page, including API reference pages.
* Your [`llms.txt`](/docs/ai/llmstxt) file, after the site title and description.
* Your `llms-full.txt` file.

These instructions apply to every page. To tailor content for a single page or audience, use the [visibility](/docs/components/visibility) component instead.

## Authentication

Markdown export respects the same authentication rules as the HTML version of each page.

| Authentication mode    | Behavior                                                                                                                                        |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| No authentication      | All `.md` URLs are publicly accessible.                                                                                                         |
| Partial authentication | `.md` URLs for public pages are publicly accessible. `.md` URLs for protected pages require authentication and respect user group restrictions. |
| Full authentication    | All `.md` URLs require authentication and respect user group restrictions.                                                                      |

## Keyboard shortcut

Press <kbd>Command</kbd> + <kbd>C</kbd> (<kbd>Ctrl</kbd> + <kbd>C</kbd> on Windows) to copy a page as Markdown to your clipboard.


## Related topics

- [Create a help center](/docs/guides/help-center.md)
- [docs.json schema reference](/docs/organize/settings-reference.md)
- [Authentication setup](/docs/deploy/authentication-setup.md)
