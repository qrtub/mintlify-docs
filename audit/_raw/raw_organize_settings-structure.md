> ## Documentation Index
> Fetch the complete documentation index at: https://www.mintlify.com/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Site structure

> Configure navbar, navigation groups, footer links, banner, contextual menu, redirects, and other structural elements in your docs.json file.

Use these settings in your `docs.json` file to control your site's information architecture and user experience. Modify the navbar, footer, banners, navigation behavior, contextual menus, redirects, and global content variables.

## Settings

### `navigation` - <Badge color="red">required</Badge>

**Type:** `object`

The navigation structure of your content. This is where you define your site's full page hierarchy using groups, tabs, dropdowns, anchors, and more.

See [Navigation](/docs/organize/navigation) for complete documentation on building your navigation structure.

<ResponseField name="navigation.global" type="object">
  Global navigation elements that appear across all pages and locales.

  <Expandable title="navigation.global">
    <ResponseField name="tabs" type="array of object">
      Top-level navigation tabs for organizing major sections. See [Tabs](/docs/organize/navigation#tabs).

      <Expandable title="tabs">
        <ResponseField name="tab" type="string" required>
          Display name of the tab. Minimum length: 1.
        </ResponseField>

        <ResponseField name="icon" type="string">
          The icon to display.

          Options:

          * [Font Awesome](https://fontawesome.com/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `fontawesome` in your `docs.json`
          * [Lucide](https://lucide.dev/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `lucide` in your `docs.json`
          * [Tabler](https://tabler.io/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `tabler` in your `docs.json`
          * URL to an externally hosted icon
          * Path to an icon file in your project
          * SVG code wrapped in curly braces

          For custom SVG icons:

          1. Convert your SVG using the [SVGR converter](https://react-svgr.com/playground/).
          2. Paste your SVG code into the SVG input field.
          3. Copy the complete `<svg>...</svg>` element from the JSX output field.
          4. Wrap the JSX-compatible SVG code in curly braces: `icon={<svg ...> ... </svg>}`.
          5. Adjust `height` and `width` as needed.
        </ResponseField>

        <ResponseField name="iconType" type="string">
          The [Font Awesome](https://fontawesome.com/icons) icon style. Only used with Font Awesome icons.

          Options: `regular`, `solid`, `light`, `thin`, `sharp-solid`, `duotone`, `brands`.
        </ResponseField>

        <ResponseField name="hidden" type="boolean">
          Whether to hide this tab by default.
        </ResponseField>

        <ResponseField name="href" type="string (uri)" required>
          URL or path for the tab destination.
        </ResponseField>
      </Expandable>
    </ResponseField>

    <ResponseField name="anchors" type="array of object">
      Anchored links that appear prominently in the sidebar. See [Anchors](/docs/organize/navigation#anchors).

      <Expandable title="anchors">
        <ResponseField name="anchor" type="string" required>
          Display name of the anchor. Minimum length: 1.
        </ResponseField>

        <ResponseField name="icon" type="string">
          The icon to display.

          Options:

          * [Font Awesome](https://fontawesome.com/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `fontawesome` in your `docs.json`
          * [Lucide](https://lucide.dev/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `lucide` in your `docs.json`
          * [Tabler](https://tabler.io/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `tabler` in your `docs.json`
          * URL to an externally hosted icon
          * Path to an icon file in your project
          * SVG code wrapped in curly braces

          For custom SVG icons:

          1. Convert your SVG using the [SVGR converter](https://react-svgr.com/playground/).
          2. Paste your SVG code into the SVG input field.
          3. Copy the complete `<svg>...</svg>` element from the JSX output field.
          4. Wrap the JSX-compatible SVG code in curly braces: `icon={<svg ...> ... </svg>}`.
          5. Adjust `height` and `width` as needed.
        </ResponseField>

        <ResponseField name="iconType" type="string">
          The [Font Awesome](https://fontawesome.com/icons) icon style. Only used with Font Awesome icons.

          Options: `regular`, `solid`, `light`, `thin`, `sharp-solid`, `duotone`, `brands`.
        </ResponseField>

        <ResponseField name="color" type="object">
          Custom colors for the anchor icon.

          <Expandable title="color">
            <ResponseField name="light" type="string">
              Anchor color for light mode. Must be a hex code beginning with `#`.
            </ResponseField>

            <ResponseField name="dark" type="string">
              Anchor color for dark mode. Must be a hex code beginning with `#`.
            </ResponseField>
          </Expandable>
        </ResponseField>

        <ResponseField name="hidden" type="boolean">
          Whether to hide this anchor by default.
        </ResponseField>

        <ResponseField name="href" type="string (uri)" required>
          URL or path for the anchor destination.
        </ResponseField>
      </Expandable>
    </ResponseField>

    <ResponseField name="dropdowns" type="array of object">
      Dropdown menus for organizing related content. See [Dropdowns](/docs/organize/navigation#dropdowns).

      <Expandable title="dropdowns">
        <ResponseField name="dropdown" type="string" required>
          Display name of the dropdown. Minimum length: 1.
        </ResponseField>

        <ResponseField name="icon" type="string">
          The icon to display.

          Options:

          * [Font Awesome](https://fontawesome.com/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `fontawesome` in your `docs.json`
          * [Lucide](https://lucide.dev/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `lucide` in your `docs.json`
          * [Tabler](https://tabler.io/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `tabler` in your `docs.json`
          * URL to an externally hosted icon
          * Path to an icon file in your project
          * SVG code wrapped in curly braces

          For custom SVG icons:

          1. Convert your SVG using the [SVGR converter](https://react-svgr.com/playground/).
          2. Paste your SVG code into the SVG input field.
          3. Copy the complete `<svg>...</svg>` element from the JSX output field.
          4. Wrap the JSX-compatible SVG code in curly braces: `icon={<svg ...> ... </svg>}`.
          5. Adjust `height` and `width` as needed.
        </ResponseField>

        <ResponseField name="iconType" type="string">
          The [Font Awesome](https://fontawesome.com/icons) icon style. Only used with Font Awesome icons.

          Options: `regular`, `solid`, `light`, `thin`, `sharp-solid`, `duotone`, `brands`.
        </ResponseField>

        <ResponseField name="hidden" type="boolean">
          Whether to hide this dropdown by default.
        </ResponseField>

        <ResponseField name="href" type="string (uri)" required>
          URL or path for the dropdown destination.
        </ResponseField>
      </Expandable>
    </ResponseField>

    <ResponseField name="languages" type="array of object">
      Language switcher configuration for localized sites. See [Languages](/docs/organize/navigation#languages).

      <Expandable title="languages">
        <ResponseField name="language" type="&#x22;ar&#x22; | &#x22;ca&#x22; | &#x22;cn&#x22; | &#x22;cs&#x22; | &#x22;de&#x22; | &#x22;en&#x22; | &#x22;es&#x22; | &#x22;fi&#x22; | &#x22;fr&#x22; | &#x22;fr-CA&#x22; | &#x22;he&#x22; | &#x22;hi&#x22; | &#x22;hu&#x22; | &#x22;id&#x22; | &#x22;it&#x22; | &#x22;ja&#x22; | &#x22;ja-JP&#x22; | &#x22;jp&#x22; | &#x22;ko&#x22; | &#x22;lv&#x22; | &#x22;nl&#x22; | &#x22;no&#x22; | &#x22;pl&#x22; | &#x22;pt&#x22; | &#x22;pt-BR&#x22; | &#x22;ro&#x22; | &#x22;ru&#x22; | &#x22;sv&#x22; | &#x22;tr&#x22; | &#x22;uk&#x22; | &#x22;uz&#x22; | &#x22;vi&#x22; | &#x22;zh&#x22; | &#x22;zh-CN&#x22; | &#x22;zh-Hans&#x22; | &#x22;zh-Hant&#x22; | &#x22;zh-TW&#x22;" required>
          Language code in [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1) format.
        </ResponseField>

        <ResponseField name="default" type="boolean">
          Whether this is the default language.
        </ResponseField>

        <ResponseField name="hidden" type="boolean">
          Whether to hide this language option by default.
        </ResponseField>

        <ResponseField name="href" type="string (uri)" required>
          A valid path or external link to this language version of your documentation.
        </ResponseField>
      </Expandable>
    </ResponseField>

    <ResponseField name="versions" type="array of object">
      Version switcher configuration for multi-version sites. See [Versions](/docs/organize/navigation#versions).

      <Expandable title="versions">
        <ResponseField name="version" type="string" required>
          Display name of the version. Minimum length: 1.
        </ResponseField>

        <ResponseField name="default" type="boolean">
          Whether this is the default version.
        </ResponseField>

        <ResponseField name="hidden" type="boolean">
          Whether to hide this version by default.
        </ResponseField>

        <ResponseField name="href" type="string (uri)" required>
          URL or path to this version of your documentation.
        </ResponseField>
      </Expandable>
    </ResponseField>

    <ResponseField name="products" type="array of object">
      Product switcher for sites with multiple products. See [Products](/docs/organize/navigation#products).

      <Expandable title="products">
        <ResponseField name="product" type="string" required>
          Display name of the product.
        </ResponseField>

        <ResponseField name="description" type="string">
          Description of the product.
        </ResponseField>

        <ResponseField name="icon" type="string">
          The icon to display.

          Options:

          * [Font Awesome](https://fontawesome.com/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `fontawesome` in your `docs.json`
          * [Lucide](https://lucide.dev/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `lucide` in your `docs.json`
          * [Tabler](https://tabler.io/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `tabler` in your `docs.json`
          * URL to an externally hosted icon
          * Path to an icon file in your project
          * SVG code wrapped in curly braces

          For custom SVG icons:

          1. Convert your SVG using the [SVGR converter](https://react-svgr.com/playground/).
          2. Paste your SVG code into the SVG input field.
          3. Copy the complete `<svg>...</svg>` element from the JSX output field.
          4. Wrap the JSX-compatible SVG code in curly braces: `icon={<svg ...> ... </svg>}`.
          5. Adjust `height` and `width` as needed.
        </ResponseField>

        <ResponseField name="iconType" type="string">
          The [Font Awesome](https://fontawesome.com/icons) icon style. Only used with Font Awesome icons.

          Options: `regular`, `solid`, `light`, `thin`, `sharp-solid`, `duotone`, `brands`.
        </ResponseField>
      </Expandable>
    </ResponseField>
  </Expandable>
</ResponseField>

<ResponseField name="navigation.languages" type="array of object">
  Language switcher for [multi-language](/docs/organize/navigation#languages) sites. Each entry can include language-specific `banner`, `footer`, and `navbar` configurations in addition to the navigation structure.

  <Expandable title="navigation.languages">
    <ResponseField name="language" type="&#x22;ar&#x22; | &#x22;ca&#x22; | &#x22;cn&#x22; | &#x22;cs&#x22; | &#x22;de&#x22; | &#x22;en&#x22; | &#x22;es&#x22; | &#x22;fi&#x22; | &#x22;fr&#x22; | &#x22;fr-CA&#x22; | &#x22;he&#x22; | &#x22;hi&#x22; | &#x22;hu&#x22; | &#x22;id&#x22; | &#x22;it&#x22; | &#x22;ja&#x22; | &#x22;ja-JP&#x22; | &#x22;jp&#x22; | &#x22;ko&#x22; | &#x22;lv&#x22; | &#x22;nl&#x22; | &#x22;no&#x22; | &#x22;pl&#x22; | &#x22;pt&#x22; | &#x22;pt-BR&#x22; | &#x22;ro&#x22; | &#x22;ru&#x22; | &#x22;sv&#x22; | &#x22;tr&#x22; | &#x22;uk&#x22; | &#x22;uz&#x22; | &#x22;vi&#x22; | &#x22;zh&#x22; | &#x22;zh-CN&#x22; | &#x22;zh-Hans&#x22; | &#x22;zh-Hant&#x22; | &#x22;zh-TW&#x22;" required>
      Language code in ISO 639-1 format.
    </ResponseField>

    <ResponseField name="default" type="boolean">
      Whether this is the default language.
    </ResponseField>

    <ResponseField name="banner" type="object">
      Language-specific banner configuration. Accepts the same options as the top-level [`banner`](#banner) field.
    </ResponseField>

    <ResponseField name="footer" type="object">
      Language-specific footer configuration. Accepts the same options as the top-level [`footer`](#footer) field.
    </ResponseField>

    <ResponseField name="navbar" type="object">
      Language-specific navbar configuration. Accepts the same options as the top-level [`navbar`](#navbar) field.
    </ResponseField>

    <ResponseField name="hidden" type="boolean">
      Whether to hide this language option by default.
    </ResponseField>
  </Expandable>
</ResponseField>

<ResponseField name="navigation.versions" type="array of object">
  Version switcher for sites with [multiple versions](/docs/organize/navigation#versions).

  <Expandable title="navigation.versions">
    <ResponseField name="default" type="boolean">
      Set to `true` to make this the default version. If omitted, the first version in the array is the default.
    </ResponseField>

    <ResponseField name="tag" type="string">
      Badge label displayed next to the version in the selector. Use to highlight versions such as `"Latest"`, `"Recommended"`, or `"Beta"`.
    </ResponseField>
  </Expandable>
</ResponseField>

<ResponseField name="navigation.tabs" type="array of object">
  Top-level navigation [tabs](/docs/organize/navigation#tabs).
</ResponseField>

<ResponseField name="navigation.anchors" type="array of object">
  Sidebar [anchors](/docs/organize/navigation#anchors).
</ResponseField>

<ResponseField name="navigation.dropdowns" type="array of object">
  [Dropdowns](/docs/organize/navigation#dropdowns) for grouping related content.
</ResponseField>

<ResponseField name="navigation.products" type="array of object">
  Product switcher for sites with multiple [products](/docs/organize/navigation#products).
</ResponseField>

<ResponseField name="navigation.groups" type="array of object">
  [Groups](/docs/organize/navigation#groups) for organizing content into sections.
</ResponseField>

<ResponseField name="navigation.pages" type="array of string or object">
  Individual [pages](/docs/organize/navigation#pages) that make up your documentation.
</ResponseField>

<ResponseField name="navigation.directory" type="&#x22;none&#x22; | &#x22;accordion&#x22; | &#x22;card&#x22;">
  Directory layout for root pages in navigation groups. When set, groups with a `root` page automatically display a listing of their children below the page content. Values inherit recursively through the navigation tree. Descendants can override. See [Directory listings](/docs/organize/navigation#directory-listings).
</ResponseField>

***

### `navbar`

**Type:** `object`

Links and buttons displayed in the top navigation bar.

<ResponseField name="navbar.links" type="array of object">
  Links to display in the navbar.

  <Expandable title="navbar.links">
    <ResponseField name="type" type="&#x22;github&#x22; | &#x22;discord&#x22;">
      Optional link type. Omit for a standard text link. Set to `github` to link to a GitHub repository and show its star count. Set to `discord` to link to a Discord server and show its online user count.
    </ResponseField>

    <ResponseField name="label" type="string">
      Link text. Required when `type` is not set. Optional for `github` and `discord`. If omitted, Mintlify generates the label from API data.
    </ResponseField>

    <ResponseField name="href" type="string (uri)" required>
      Link destination. Must be a valid external URL. For `github`, must be a GitHub repository URL. For `discord`, must be a Discord invite URL.
    </ResponseField>

    <ResponseField name="icon" type="string">
      The icon to display.

      Options:

      * [Font Awesome](https://fontawesome.com/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `fontawesome` in your `docs.json`
      * [Lucide](https://lucide.dev/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `lucide` in your `docs.json`
      * [Tabler](https://tabler.io/icons) icon name, if you have the `icons.library` [property](/docs/organize/settings-appearance#param-icons) set to `tabler` in your `docs.json`
      * URL to an externally hosted icon
      * Path to an icon file in your project
      * SVG code wrapped in curly braces

      For custom SVG icons:

      1. Convert your SVG using the [SVGR converter](https://react-svgr.com/playground/).
      2. Paste your SVG code into the SVG input field.
      3. Copy the complete `<svg>...</svg>` element from the JSX output field.
      4. Wrap the JSX-compatible SVG code in curly braces: `icon={<svg ...> ... </svg>}`.
      5. Adjust `height` and `width` as needed.
    </ResponseField>

    <ResponseField name="iconType" type="string">
      The [Font Awesome](https://fontawesome.com/icons) icon style. Only used with Font Awesome icons.

      Options: `regular`, `solid`, `light`, `thin`, `sharp-solid`, `duotone`, `brands`.
    </ResponseField>
  </Expandable>
</ResponseField>

<ResponseField name="navbar.primary" type="object">
  Primary call-to-action button in the navbar.

  <Expandable title="navbar.primary">
    <ResponseField name="type" type="&#x22;button&#x22; | &#x22;github&#x22; | &#x22;discord&#x22;" required>
      Button style. Choose `button` for a standard button, `github` for a GitHub repository link with star count, or `discord` for a Discord invite with online user count.
    </ResponseField>

    <ResponseField name="label" type="string">
      Button text. Required when `type` is `button`. Optional for `github` and `discord`.
    </ResponseField>

    <ResponseField name="href" type="string (uri)" required>
      Button destination. Must be an external URL. For `github`, must be a GitHub repository URL. For `discord`, must be a Discord invite URL.
    </ResponseField>
  </Expandable>
</ResponseField>

```json docs.json theme={null}
"navbar": {
  "links": [
    { "type": "github", "href": "https://github.com/your-org/your-repo" },
    { "label": "Community", "href": "https://example.com/community" }
  ],
  "primary": {
    "type": "button",
    "label": "Get started",
    "href": "https://example.com/signup"
  }
}
```

***

### `footer`

**Type:** `object`

Footer content and social media links.

<ResponseField name="footer.socials" type="object">
  Social media profiles to display in the footer. Each key is a platform name and each value is your profile URL.

  Valid keys: `x`, `website`, `facebook`, `youtube`, `discord`, `slack`, `github`, `linkedin`, `instagram`, `hacker-news`, `medium`, `telegram`, `twitter`, `x-twitter`, `earth-americas`, `bluesky`, `threads`, `reddit`, `podcast`

  ```json theme={null}
  "socials": {
    "x": "https://x.com/yourhandle",
    "github": "https://github.com/your-org"
  }
  ```
</ResponseField>

<ResponseField name="footer.links" type="array of object">
  Link columns displayed in the footer. Maximum 4 columns.

  <Expandable title="footer.links">
    <ResponseField name="header" type="string">
      Column header title. Minimum length: 1.
    </ResponseField>

    <ResponseField name="items" type="array of object" required>
      Links to display in the column.

      <Expandable title="items">
        <ResponseField name="label" type="string" required>
          Link text. Minimum length: 1.
        </ResponseField>

        <ResponseField name="href" type="string (uri)" required>
          Link destination URL.
        </ResponseField>
      </Expandable>
    </ResponseField>
  </Expandable>
</ResponseField>

```json docs.json theme={null}
"footer": {
  "socials": {
    "x": "https://x.com/yourhandle",
    "github": "https://github.com/your-org"
  },
  "links": [
    {
      "header": "Company",
      "items": [
        { "label": "Blog", "href": "https://example.com/blog" },
        { "label": "Careers", "href": "https://example.com/careers" }
      ]
    }
  ]
}
```

***

### `banner`

**Type:** `object`

A site-wide banner displayed at the top of every page.

<ResponseField name="banner.content" type="string" required>
  The text content displayed in the banner. Supports basic MDX formatting including links, bold, and italic text. Custom components are not supported.

  ```json theme={null}
  "content": "We just launched something new. [Learn more](https://example.com)"
  ```
</ResponseField>

<ResponseField name="banner.dismissible" type="boolean">
  Whether to show a dismiss button so users can close the banner. Defaults to `false`.
</ResponseField>

```json docs.json theme={null}
"banner": {
  "content": "We just launched something new. [Learn more](https://example.com)",
  "dismissible": true
}
```

***

### `interaction`

**Type:** `object`

Controls user interaction behavior for navigation elements.

<ResponseField name="interaction.drilldown" type="boolean">
  Controls automatic navigation when selecting a navigation group. Set to `true` to automatically navigate to the first page when a group expands. Set to `false` to only expand or collapse the group without navigating. Leave unset to use the theme's default behavior.
</ResponseField>

***

### `contextual`

**Type:** `object`

The contextual menu gives users quick access to AI tools and page actions. It appears in the page header or table of contents sidebar.

<Note>
  The contextual menu is only available on preview and production deployments.
</Note>

<ResponseField name="contextual.options" type="array" required>
  Actions available in the contextual menu. The first option in the array appears as the default action.

  Built-in options:

  * `"add-mcp"`—Add your MCP server to the user's configuration
  * `"aistudio"`—Send the current page to Google AI Studio
  * `"assistant"`—Open the AI assistant with the current page as context
  * `"copy"`—Copy the current page as Markdown to the clipboard
  * `"chatgpt"`—Send the current page to ChatGPT
  * `"claude"`—Send the current page to Claude
  * `"cursor"`—Install your hosted MCP server in Cursor
  * `"devin"`—Send the current page to Devin
  * `"devin-mcp"`—Install your hosted MCP server in Devin
  * `"download-pdf"`—Download the current page as a PDF
  * `"download-spec"`—Download the deployment's OpenAPI specs (single file, or zipped if multiple)
  * `"grok"`—Send the current page to Grok
  * `"mcp"`—Copy your MCP server URL to the clipboard
  * `"perplexity"`—Send the current page to Perplexity
  * `"view"`—View the current page as Markdown in a new tab
  * `"vscode"`—Install your hosted MCP server in VS Code
  * `"devin-desktop"`—Open Devin Desktop with the current page as context

  Define custom options as objects:

  <Expandable title="Custom option">
    <ResponseField name="title" type="string" required>
      Display title for the custom option.
    </ResponseField>

    <ResponseField name="description" type="string" required>
      Description text for the custom option.
    </ResponseField>

    <ResponseField name="icon" type="string">
      Icon for the custom option. Supports icon library names, URLs, paths, or SVG code.
    </ResponseField>

    <ResponseField name="href" type="string or object" required>
      Link destination. Can be a URL string or an object with `base` and optional `query` parameters.

      Available placeholder values:

      * `$page`—Current page content
      * `$path`—Current page path
      * `$mcp`—MCP server URL
    </ResponseField>
  </Expandable>
</ResponseField>

<ResponseField name="contextual.display" type="&#x22;header&#x22; | &#x22;toc&#x22;">
  Where to display the contextual options. Choose `header` to show them in the top-of-page context menu, or `toc` to show them in the table of contents sidebar. Defaults to `header`.
</ResponseField>

```json docs.json theme={null}
"contextual": {
  "options": ["copy", "view", "chatgpt", "claude"],
  "display": "header"
}
```

***

### `redirects`

**Type:** `array of object`

Redirects for moved, renamed, or deleted pages. Use these to preserve links when you reorganize your content.

<ResponseField name="redirects[].source" type="string" required>
  The path to redirect from. Example: `/old-page`
</ResponseField>

<ResponseField name="redirects[].destination" type="string" required>
  The path to redirect to. Example: `/new-page`
</ResponseField>

<ResponseField name="redirects[].permanent" type="boolean">
  If `true`, issues a permanent redirect (308). If `false`, issues a temporary redirect (307). Defaults to `true`.
</ResponseField>

```json docs.json theme={null}
"redirects": [
  {
    "source": "/old-page",
    "destination": "/new-page"
  },
  {
    "source": "/temp-redirect",
    "destination": "/destination",
    "permanent": false
  }
]
```

***

### `errors`

**Type:** `object`

Custom error page settings.

<ResponseField name="errors.404" type="object">
  Settings for the 404 "Page not found" error page.

  <Expandable title="errors.404">
    <ResponseField name="redirect" type="boolean">
      Whether to automatically redirect to the home page when a page is not found. Defaults to `true`.
    </ResponseField>

    <ResponseField name="title" type="string">
      Custom title for the 404 page.
    </ResponseField>

    <ResponseField name="description" type="string">
      Custom description for the 404 page. Supports MDX formatting including links, bold, and italic text, and custom components.
    </ResponseField>
  </Expandable>
</ResponseField>

```json docs.json theme={null}
"errors": {
  "404": {
    "redirect": false,
    "title": "Page not found",
    "description": "The page you're looking for doesn't exist. [Go home](/)."
  }
}
```

***

### `variables`

**Type:** `object`

Global variables for use throughout your documentation. Mintlify replaces `{{variableName}}` placeholders with the defined values at build time.

<ResponseField name="variables.[variableName]" type="string">
  A key-value pair where the key is the variable name and the value is the replacement text.

  * Variable names can contain alphanumeric characters and hyphens.
  * You must define all variables referenced in your content or the build fails.
  * Mintlify sanitizes values to prevent XSS attacks.
</ResponseField>

```json docs.json theme={null}
"variables": {
  "version": "2.0.0",
  "api-url": "https://api.example.com"
}
```

In your content, reference variables with double curly braces:

```mdx theme={null}
The current version is {{version}}. Make requests to {{api-url}}.
```

***

### `metadata`

**Type:** `object`

Page-level metadata settings applied globally.

<ResponseField name="metadata.timestamp" type="boolean">
  Enable a last-modified date on all pages. When enabled, pages display the date the content was last modified. Defaults to `false`.

  You can override this setting on individual pages using the `timestamp` frontmatter field. See [Pages](/docs/organize/pages#last-modified-timestamp) for details.
</ResponseField>

```json docs.json theme={null}
"metadata": {
  "timestamp": true
}
```


## Related topics

- [Global settings](/docs/organize/settings.md)
- [docs.json schema reference](/docs/organize/settings-reference.md)
- [How to improve documentation SEO](/docs/guides/seo.md)
