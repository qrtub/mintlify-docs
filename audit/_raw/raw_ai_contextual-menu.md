> ## Documentation Index
> Fetch the complete documentation index at: https://www.mintlify.com/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Contextual menu

> Add a contextual menu to your docs with one-click AI integrations for ChatGPT, Claude, Perplexity, Google AI Studio, Devin, Devin Desktop, and MCP tools.

export const PreviewButton = ({children, href}) => {
  return <a href={href} className="text-sm font-medium text-white dark:!text-zinc-950 bg-zinc-900 hover:bg-zinc-700 dark:bg-zinc-100 hover:dark:bg-zinc-300 rounded-full px-3.5 py-1.5 not-prose">
        {children}
      </a>;
};

The contextual menu provides quick access to AI-optimized content and direct integrations with popular AI tools. When users click the contextual menu on any page, they can copy content as context for AI tools or open it in an AI conversation. Supported tools include ChatGPT, Claude, Perplexity, Google AI Studio, Grok, Devin, Devin Desktop, and any custom tool you configure.

<Tip>
  Pair the contextual menu with your hosted [`skill.md`](/docs/ai/skillmd) file and [MCP server](/docs/ai/model-context-protocol). This lets users install your product's full capabilities into their AI tools, not just the page they are reading.
</Tip>

## Menu options

The contextual menu includes several pre-built options that you can enable by adding their identifier to your configuration.

| Option                       | Identifier      | Description                                                                                                                                  |
| :--------------------------- | :-------------- | :------------------------------------------------------------------------------------------------------------------------------------------- |
| **Copy page**                | `copy`          | Copies the current page as Markdown for pasting as context into AI tools                                                                     |
| **View as Markdown**         | `view`          | Opens the current page as Markdown                                                                                                           |
| **Ask assistant**            | `assistant`     | Opens the [assistant](/docs/assistant/index) with the current page as context                                                                     |
| **Download PDF**             | `download-pdf`  | Downloads the current page as a PDF. Available on [Enterprise plans](https://mintlify.com/pricing).                                          |
| **Open in ChatGPT**          | `chatgpt`       | Creates a ChatGPT conversation with the current page as context                                                                              |
| **Open in Claude**           | `claude`        | Creates a Claude conversation with the current page as context                                                                               |
| **Open in Perplexity**       | `perplexity`    | Creates a Perplexity conversation with the current page as context                                                                           |
| **Open in Grok**             | `grok`          | Creates a Grok conversation with the current page as context                                                                                 |
| **Open in Google AI Studio** | `aistudio`      | Creates a Google AI Studio conversation with the current page as context                                                                     |
| **Open in Devin**            | `devin`         | Creates a Devin session with the current page as context                                                                                     |
| **Open in Devin Desktop**    | `devin-desktop` | Opens Devin Desktop with the current page as context. Requires installing Devin Desktop.                                                     |
| **Copy MCP server URL**      | `mcp`           | Copies your MCP server URL to the clipboard                                                                                                  |
| **Copy MCP install command** | `add-mcp`       | Copies the `npx add-mcp` command to install the MCP server                                                                                   |
| **Connect to Cursor**        | `cursor`        | Installs your hosted MCP server in Cursor                                                                                                    |
| **Connect to VS Code**       | `vscode`        | Installs your hosted MCP server in VS Code                                                                                                   |
| **Connect to Devin**         | `devin-mcp`     | Installs your hosted MCP server in Devin                                                                                                     |
| **Download API spec**        | `download-spec` | Downloads your deployment's OpenAPI spec. If there are multiple specs, downloads them as a zip archive. Only appears on API reference pages. |
| **Custom options**           | Object          | Add custom options to the contextual menu                                                                                                    |

<Frame>
  <img src="https://mintcdn.com/mintlify/GiucHIlvP3i5L17o/images/contextual-menu/contextual-menu.png?fit=max&auto=format&n=GiucHIlvP3i5L17o&q=85&s=b37c2bfffdc0db86422a7f7e692adaf7" alt="The expanded contextual menu showing the Copy page, View as Markdown, Open in ChatGPT, and Open in Claude menu items." width="1396" height="944" data-path="images/contextual-menu/contextual-menu.png" />
</Frame>

## Enable the contextual menu

Add the `contextual` field to your `docs.json` file and specify which options you want to include. Options appear in the menu in the order you list them.

```json theme={null}
{
  "contextual": {
    "options": [
      "copy",
      "view",
      "assistant",
      "chatgpt",
      "claude",
      "perplexity",
      "grok",
      "aistudio",
      "devin",
      "devin-desktop",
      "mcp",
      "cursor",
      "vscode",
      "devin-mcp",
      "download-spec",
      "download-pdf"
    ]
  }
}
```

## Display location

By default, the contextual menu appears in the page header. You can configure it to display in the table of contents sidebar instead using the `display` option.

```json theme={null}
{
  "contextual": {
    "options": ["copy", "view", "chatgpt", "claude"],
    "display": "toc"
  }
}
```

| Value    | Description                                                |
| :------- | :--------------------------------------------------------- |
| `header` | Displays options in the top-of-page context menu (default) |
| `toc`    | Displays options in the table of contents sidebar          |

## Add custom options

Create custom options in the contextual menu by adding an object to the `options` array. Each custom option requires these properties:

<ResponseField name="title" type="string" required>
  The title of the option.
</ResponseField>

<ResponseField name="description" type="string" required>
  The description of the option. Displayed beneath the title when the contextual menu expands.
</ResponseField>

<Info>
  You must include one of `icon` or `src`.
</Info>

<ResponseField name="icon" type="string">
  The icon to display from an icon library.

  Options:

  * [Font Awesome icon](https://fontawesome.com/icons) name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `fontawesome` in your `docs.json`
  * [Lucide icon](https://lucide.dev/icons) name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `lucide` in your `docs.json`
  * [Tabler icon](https://tabler.io/icons) name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `tabler` in your `docs.json`
</ResponseField>

<ResponseField name="src" type="string">
  Path or URL to an image to use as the icon. Use `src` instead of `icon` when you want to use a custom image rather than an icon from a library.

  Options:

  * Path to an image file in your project (for example, `/images/my-icon.svg`)
  * URL to an externally hosted image (for example, `https://example.com/icon.png`)
</ResponseField>

<ResponseField name="iconType" type="string">
  The [Font Awesome](https://fontawesome.com/icons) icon style. Only used with Font Awesome icons.

  Options: `regular`, `solid`, `light`, `thin`, `sharp-solid`, `duotone`, `brands`.
</ResponseField>

<ResponseField name="href" type="string | object" required>
  The href of the option. Use a string for simple links or an object for dynamic links with query parameters.

  <Expandable title="href object">
    <ResponseField name="base" type="string" required>
      The base URL for the option.
    </ResponseField>

    <ResponseField name="query" type="object[]">
      An array of query parameter objects to append to the base URL.

      <Expandable title="query object">
        <ResponseField name="key" type="string" required>
          The query parameter key.
        </ResponseField>

        <ResponseField name="value" type="string" required>
          The query parameter value. Mintlify replaces the following placeholders with the corresponding values:

          * Use `$page` to insert the current page content in Markdown.
          * Use `$path` to insert the current page path.
          * Use `$mcp` to insert the hosted MCP server URL.
        </ResponseField>
      </Expandable>
    </ResponseField>
  </Expandable>
</ResponseField>

Example custom option:

```json {9-14} wrap theme={null}
{
    "contextual": {
        "options": [
            "copy",
            "view",
            "chatgpt",
            "claude",
            "perplexity",
            {
                "title": "Request a feature",
                "description": "Join the discussion on GitHub to request a new feature",
                "icon": "plus",
                "href": "https://github.com/orgs/mintlify/discussions/categories/feature-requests"
            }
        ]
    }
}
```

## Override on individual pages

To override the global contextual menu on a specific page, add the `contextual` field to the page's frontmatter. Override the global contextual menu to surface page-specific actions like `download-pdf` on a terms of service page, or to hide the menu entirely on a landing page.

The page-level `contextual` object replaces the global one for that page. Omit the field to inherit `docs.json`, or set `options: []` to disable the contextual menu on that page.

```mdx theme={null}
---
title: "Terms of Service"
contextual:
  options:
    - copy
    - download-pdf
  display: header
---
```

The same fields and validation rules apply as in `docs.json`, including [custom options](#add-custom-options) and the `display` setting. If a page override is invalid, Mintlify falls back to the global `contextual` configuration.

### Custom option examples

<AccordionGroup>
  <Accordion title="Simple link">
    ```json theme={null}
    {
      "title": "Request a feature",
      "description": "Join the discussion on GitHub",
      "icon": "plus",
      "href": "https://github.com/orgs/mintlify/discussions/categories/feature-requests"
    }
    ```
  </Accordion>

  <Accordion title="Dynamic link with page content">
    ```json theme={null}
    {
      "title": "Share on X",
      "description": "Share this page on X",
      "icon": "x",
      "href": {
        "base": "https://x.com/intent/tweet",
        "query": [
          {
          "key": "text",
          "value": "Check out this documentation: $page"
          }
        ]
      }
    }
    ```
  </Accordion>
</AccordionGroup>


## Related topics

- [Search Model Context Protocol (MCP) server](/docs/ai/model-context-protocol.md)
- [Analytics integrations](/docs/integrations/analytics/overview.md)
- [AI-native documentation](/docs/ai-native.md)
