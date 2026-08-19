> ## Documentation Index
> Fetch the complete documentation index at: https://www.mintlify.com/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# GEO guide: Optimize docs for AI search and answer engines

> Optimize your documentation for AI-powered answer engines like ChatGPT, Perplexity, and Google AI Overviews with Generative Engine Optimization techniques.

AI-powered tools like ChatGPT, Perplexity, and Google AI Overviews increasingly answer users' questions directly, citing sources rather than listing links. When a developer asks "how do I authenticate with \[your product]," AI systems cite a well-optimized documentation page in the answer. AI systems skip poorly structured pages, even if they rank well in traditional search.

Generative Engine Optimization (GEO) is the practice of structuring content so AI systems can understand, trust, and cite it accurately.

## How GEO differs from SEO

Traditional SEO optimizes for search engines that rank and link to pages. GEO optimizes for AI systems that read, summarize, and cite pages in generated answers.

The mechanics differ in important ways:

|                   | SEO                                 | GEO                                     |
| ----------------- | ----------------------------------- | --------------------------------------- |
| Goal              | Rank in search results              | Get cited in AI-generated answers       |
| Key signals       | Backlinks, keywords, page authority | Content accuracy, structure, directness |
| User action       | Click a link                        | Read an AI-generated answer             |
| Format preference | Any well-structured content         | Scannable, question-answering content   |

The fundamentals overlap heavily. Accurate, well-structured content that directly answers questions performs well in both. GEO is less about tricks and more about writing clarity.

## How AI systems decide what to cite

AI answer engines evaluate content based on a few key factors:

**Directness.** AI systems prioritize content that answers the question immediately. A page that buries the answer after three paragraphs of context is less likely to earn citations than one that leads with the answer.

**Accuracy and trust signals.** AI systems favor content from authoritative sources that appears factually reliable. For documentation, this means technical accuracy, consistent versioning, and content that matches what the product actually does.

**Structural clarity.** Content that's logically organized, with meaningful headings, lists, and code blocks, is easier for AI systems to parse and excerpt correctly.

**Specificity.** Vague, high-level content ("this feature is flexible and powerful") is less citable than specific, detailed content ("this endpoint returns a 429 status code when requests exceed the rate limit of 100 per minute").

## Write content AI systems can cite

### Lead with the answer

Structure each section so the most important information comes first. Users asking AI tools want direct answers. Avoid preambles, unnecessary context, and caveats before the point.

````mdx theme={null}
<!-- Leads with the answer -->
## How to authenticate API requests

Include your API key in the Authorization header of every request:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" https://api.example.com/endpoint
```

<!-- Buries the answer -->
## Authentication

Authentication is an important part of using our API. Before you can make any requests, you'll need to understand how our authentication system works. Our API uses bearer tokens...
````

### Use headings that match questions

Write H2 and H3 headings as the questions users ask, not as topic labels. AI systems match user queries to heading text when deciding what content to surface.

```mdx theme={null}
<!-- Query-matching heading -->
## How do I rotate my API keys?

<!-- Topic label — weaker -->
## API key management
```

### Be specific with numbers, limits, and examples

Vague descriptions don't get cited. Specific, accurate details do. AI systems can cite "rate limit: 100 requests per minute per API key" accurately. "Our API has rate limits" gives the AI nothing useful to quote.

For every configuration option, parameter, or behavior:

* State the exact value or range
* Describe what happens at the boundary
* Show a concrete code example

### Use consistent terminology

AI systems build context across a page. If you call the same thing "API key," "access token," and "API token" interchangeably, the AI's summary may use the wrong term. It may also get confused about whether these are the same thing. Use one name per concept throughout the page to help AI systems represent your content accurately.

## Format for AI parsing

### Use sequential, non-skipping heading hierarchy

Don't skip from H2 to H4. AI systems use heading hierarchy to understand how topics relate. A flat, consistent structure is easier to parse correctly.

### Label all code blocks

Always declare the programming language on code blocks. This helps AI systems understand what they're reading and surface the right example for the user's context.

````mdx theme={null}
```python
import requests
response = requests.get(url, headers={"Authorization": f"Bearer {api_key}"})
```
````

### Write alt text for images and diagrams

AI systems can't see images. If a diagram is the primary explanation of a concept, add a text description that conveys the same information. Alt text that describes what a diagram shows—not just "architecture diagram"—gives AI systems something to work with.

### Use specific references instead of pronouns

Write "the API key" instead of "it" or "this value." AI systems excerpt content and lose surrounding context. Specific noun references stay accurate when excerpted; pronouns become ambiguous.

## Mintlify configuration for GEO

### Add descriptive page metadata

Page titles and descriptions are among the most important signals AI systems use to understand a page's topic. Write them as if answering the question "what does this page help users do?"

```mdx theme={null}
---
title: "How to authenticate API requests"
description: "Add your API key to the Authorization header to authenticate requests. Includes examples in JavaScript, Python, and cURL."
---
```

### Control indexing settings

By default, Mintlify indexes pages included in your `docs.json` navigation. To include hidden pages in AI assistant context and search:

```json docs.json theme={null}
{
  "seo": {
    "indexing": "all"
  }
}
```

### llms.txt

Mintlify automatically generates an `llms.txt` file for your documentation. The file works similarly to `sitemap.xml` for traditional search. It provides AI systems with a structured index of your documentation. No configuration is required.

You can view your `llms.txt` file by appending `/llms.txt` to your documentation URL.

### Allow AI agents in robots.txt

Your `robots.txt` file controls which bots can crawl your site. If it blocks AI user agents, tools like ChatGPT, Claude, and Perplexity cannot read your documentation nor cite it in answers.

Mintlify's auto-generated `robots.txt` allows all crawlers by default and includes [Content-Signal directives](/docs/optimize/seo#content-signal-directives) that opt your documentation in to AI training, search indexing, and AI answer generation. If you use a [custom robots.txt file](/docs/optimize/seo#custom-sitemaps-and-robotstxt-files), make sure it does not block AI agents. The most common AI user agents include:

* `GPTBot`, `OAI-SearchBot`, `ChatGPT-User` (OpenAI)
* `ClaudeBot`, `Claude-User` (Anthropic)
* `PerplexityBot` (Perplexity)
* `Google-Extended` (Gemini)

A `robots.txt` that blocks all crawlers also blocks AI agents:

```txt Bad — blocks all AI agents theme={null}
User-agent: *
Disallow: /
```

To block specific scrapers while allowing AI agents, target only the bots you want to restrict:

```txt Good — allows AI agents theme={null}
User-agent: BadBot
Disallow: /

User-agent: *
Disallow: /private/
```

Run [`mint score`](/docs/cli/commands#mint-score) to check whether your site's `robots.txt` allows AI agents. The `robotsTxtAllowsAI` check passes when no AI user agents are blocked.

## Test how AI tools represent your docs

Regularly test whether AI tools are citing your documentation accurately.

Ask specific questions about your product in ChatGPT, Perplexity, and Claude:

* "How do I authenticate API requests using \[your product]?"
* "What happens when I exceed the rate limit in \[your product]?"
* "Show me how to handle errors in \[your product]'s API."

Check the responses for:

* Whether your documentation is cited at all
* Whether the cited content is accurate
* Whether the code examples are correct
* Whether the AI is recommending the right approach

When AI tools give wrong answers about your product, it often signals that your documentation is ambiguous, missing, or contradictory rather than that the AI is broken.

## Frequently asked questions

<AccordionGroup>
  <Accordion title="Does GEO replace SEO?">
    No. Traditional search still drives significant traffic, and many users prefer clicking through to documentation rather than reading AI-generated summaries. GEO and SEO are complementary. Well-structured, accurate documentation that directly answers questions performs well in both. The practices reinforce each other.
  </Accordion>

  <Accordion title="How long does it take for GEO improvements to show up?">
    Faster than SEO in many cases. AI systems like Perplexity and ChatGPT crawl and index content more frequently than traditional search engines. Improvements to content clarity and structure can show up in AI-generated answers within days to weeks. That said, AI systems do factor in domain authority and link signals that take longer to build.
  </Accordion>

  <Accordion title="Do I need to do anything special for Google AI Overviews specifically?">
    Google AI Overviews use the same signals as Google Search with additional weight on content that directly answers the specific query. Pages that already rank well in Google Search for a query are more likely to appear in AI Overviews for the same query. The main GEO practices also apply here: lead with the answer, provide specific details, and use a clear structure.
  </Accordion>

  <Accordion title="Should I write differently for AI versus human readers?">
    No. Content that's clear and direct for humans is clear and direct for AI systems. Optimizing specifically for AI at the expense of human readability is counterproductive and tends to produce content that's neither enjoyable to read nor well-cited. Write for your users first; GEO follows naturally from good technical writing.
  </Accordion>

  <Accordion title="What is llms.txt, and do I need to configure it?">
    The `llms.txt` file is a convention similar to `robots.txt` for providing AI systems with a structured index of your documentation. Mintlify generates it automatically. You don't need to configure it. You can view your `llms.txt` file at `https://your-docs-domain.com/llms.txt`.
  </Accordion>
</AccordionGroup>


## Related topics

- [Guides](/docs/guides/index.md)
- [SEO](/docs/optimize/seo.md)
- [Configurations](/docs/editor/configurations.md)
