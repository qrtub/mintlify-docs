> ## Documentation Index
> Fetch the complete documentation index at: https://www.mintlify.com/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# SEO and search

> Configure SEO settings in docs.json including site description, search engine indexing, meta tags, search bar placeholder, and page timestamps.

Use these settings in your `docs.json` file to control how search engines index your documentation and what metadata appears in search results. You can also control how the search bar behaves and whether pages display a last-modified timestamp.

## Settings

### `description`

**Type:** `string`

A description of your documentation site for SEO and AI indexing. It appears in search engine results. AI tools use it to understand your site's purpose.

```json docs.json theme={null}
"description": "Documentation for Example Co.'s API and developer platform."
```

***

### `seo`

**Type:** `object`

Search engine indexing and metadata settings.

<ResponseField name="seo.indexing" type="&#x22;navigable&#x22; | &#x22;all&#x22;">
  Specifies which pages search engines should index.

  * `navigable`: Index only pages included in your `docs.json` navigation. Defaults to this value.
  * `all`: Index every page in your project, including pages not in the navigation.
</ResponseField>

<ResponseField name="seo.metatags" type="object">
  Custom meta tags added to every page. Provide as key-value pairs where each key is a meta tag name and the value is its content.

  See [common meta tags reference](/docs/optimize/seo#common-meta-tags-reference) for available options.

  ```json theme={null}
  "metatags": {
    "og:site_name": "Example Co. Docs",
    "twitter:card": "summary_large_image"
  }
  ```
</ResponseField>

<ResponseField name="seo.organization" type="object">
  The organization used as the publisher entity in the [structured data](/docs/optimize/seo#structured-data) emitted on every page. All fields are optional. If you omit this object, Mintlify derives the organization from your site name, docs logo, and site URL.

  * `id`: Stable `@id` URL identifying your organization across pages, for example `https://example.com/#organization`. Defaults to `<site origin>/#organization`.
  * `name`: Organization name. Defaults to the site name.
  * `legalName`: Registered legal name.
  * `url`: Canonical organization homepage URL.
  * `logo`: Canonical logo URL used in structured data. Defaults to the docs logo.
  * `sameAs`: Array of URLs for official profiles, such as X, LinkedIn, or GitHub.

  ```json theme={null}
  "organization": {
    "id": "https://example.com/#organization",
    "legalName": "Example Co. Inc.",
    "url": "https://example.com",
    "logo": "https://example.com/images/logo.png",
    "sameAs": [
      "https://x.com/example",
      "https://github.com/example"
    ]
  }
  ```
</ResponseField>

```json docs.json theme={null}
"seo": {
  "indexing": "navigable",
  "metatags": {
    "og:site_name": "Example Co. Docs"
  }
}
```

***

### `search`

**Type:** `object`

Search bar display settings.

<ResponseField name="search.prompt" type="string">
  Placeholder text displayed in the search bar when it is empty.
</ResponseField>

```json docs.json theme={null}
"search": {
  "prompt": "Search the docs..."
}
```

***

### `metadata`

**Type:** `object`

Page-level metadata settings applied globally across all pages.

<ResponseField name="metadata.timestamp" type="boolean">
  Display a last-modified date on all pages. When enabled, each page shows the date its content was last modified. The default is `false`.

  You can override this setting for individual pages using the `timestamp` frontmatter field. See [Pages](/docs/organize/pages#last-modified-timestamp) for details.
</ResponseField>

```json docs.json theme={null}
"metadata": {
  "timestamp": true
}
```

## Example

```json docs.json theme={null}
{
  "description": "Documentation for Example Co.'s API and developer platform.",
  "seo": {
    "indexing": "navigable",
    "metatags": {
      "og:site_name": "Example Co. Docs",
      "twitter:card": "summary_large_image"
    }
  },
  "search": {
    "prompt": "Search the docs..."
  },
  "metadata": {
    "timestamp": true
  }
}
```


## Related topics

- [React components](/docs/customize/react-components.md)
- [How to improve documentation SEO](/docs/guides/seo.md)
- [docs.json schema reference](/docs/organize/settings-reference.md)
