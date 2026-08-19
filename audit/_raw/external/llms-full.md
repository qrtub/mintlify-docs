# Audit: `https://help.qrtub.com/llms-full.txt`

Source fetched with `curl -s` and saved to scratch as `llms-full.txt`. All counts below are exact
(`grep -c` / `grep -Fc` on the literal string, not estimates) unless explicitly marked "approx".

## 1. Overall size

| Metric | Value |
| --- | --- |
| Total bytes | **142,968** |
| Total lines | 3,734 |
| Total Unicode characters (proper UTF-8 decode) | 142,470 |
| Estimated tokens (chars/4) | **~35,618** (using UTF-8 char count) / ~35,742 if bytes/4 is used instead |
| Number of pages concatenated (H1 sections / `Source:` lines) | **19** |
| Average bytes per page | ~7,525 bytes (~1,880 tokens) |

The 19 pages break down as:
- 12 `/help/*` pages (product documentation)
- 5 `/industries/*` pages (vertical marketing/use-case pages: arboriculture, civil construction,
  contract cleaning, electrical test & tag, local government)
- 2 `/integrations/*` pages (CMMS systems, Mitti/SafetyCulture)

## 2. Repeated boilerplate strings — exact counts

### 2a. The page-footer CTA (appears on literally every page, 19/19)

Every one of the 19 pages ends with the identical three-part footer, separated from body
content by a `***` horizontal rule:

```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)


```

Exact counts (verified with `grep -Fc` for the literal lines, `grep -c` for the `***` pattern):

| String | Exact count | Bytes each (no newline) | Total bytes (incl. line's own `\n`) | Approx tokens (bytes/4) |
| --- | ---: | ---: | ---: | ---: |
| `**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)` | 19 | 91 | 1,748 | ~437 |
| `Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)` | 19 | 58 | 1,121 | ~280 |
| `***` (footer-separator instances only) | 19 of 31 total | 3 | 76 | ~19 |

**Verification commands used:**
```
grep -Fc '**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)' llms-full.txt
→ 19

grep -Fc 'Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)' llms-full.txt
→ 19

grep -c '^\*\*\*$' llms-full.txt
→ 31   (19 are footer separators, 12 are inline dividers — see 2b)

grep -c '^Source: https://help.qrtub.com' llms-full.txt
→ 19   (confirms page count, matches footer count exactly)
```

**Full footer block, including the two blank lines that separate pages**, reconstructed and
measured exactly (`***\n\n` + ready-line + `\n\n` + questions-line + `\n\n\n`):

- **159 bytes per occurrence × 19 occurrences = 3,021 bytes total**
- ≈ 745–756 tokens total (chars/4)
- **This block is present on 100% of pages (19/19) — it is the only truly universal,
  byte-identical boilerplate in the file.**

### 2b. `***` used as an in-page divider (not cross-page boilerplate)

Of the 31 total `^\*\*\*$` lines, 19 precede the footer (counted above) and the remaining
**12 are all inside a single page** — "Key Concepts" (`/help/key-concepts`) — used as
plain section dividers between its Item/Link/Media/Destinations/Tubs subsections. This is
not repeated across pages, so it is not counted as cross-page boilerplate; it's just that
one page's internal formatting style.

```
grep -n '^\*\*\*$' llms-full.txt   → 31 line numbers
# Lines 1059,1080,1106,1135,1153,1174,1226,1253,1273,1288,1305,1324
# all fall between the "Key Concepts" Source: line (1034) and the next
# page's Source: line (1348) — i.e., all 12 are internal to that one page.
```

### 2c. `hi@qrtub.com` mentions overall

```
grep -c 'hi@qrtub.com' llms-full.txt
→ 21
```
19 of these are the exact footer line counted in 2a. The other 2 are unique, non-boilerplate,
in-context support mentions that differ page to page and are NOT duplicated elsewhere:
- Line 567 (Conditional Visibility, "Getting Help" section): `Need help? Contact [hi@qrtub.com](mailto:hi@qrtub.com) with your use case and we can help you construct the right condition.`
- Line 2181 (Using Fields, "Getting Help" section): `4. **Contact support** - Email [hi@qrtub.com](mailto:hi@qrtub.com) with your use case`

These read as bespoke content per page (different surrounding sentence each time), not a
copy-pasted block, so they are **excluded** from the boilerplate byte total above.

### 2d. Structural (near-duplicate, NOT byte-identical) templates — reported but not counted as boilerplate bytes

Two structural patterns recur across sets of pages, but the *wording differs* each time, so
they cannot be — and should not be — reported as an exact repeated string with a single byte
count. They are noted here because they still represent authored redundancy (a content
template), just not literal boilerplate:

- **`## Ready to Deploy?` heading**, followed by **`**Core features available:**`**, appears
  on exactly **5 pages** — all 5 `/industries/*` pages, and only those:
  ```
  grep -c '^## Ready to Deploy?$' llms-full.txt        → 5
  grep -c '^\*\*Core features available:\*\*$' llms-full.txt → 5
  ```
  The heading text is identical every time; the bullet list beneath it is unique per industry
  (different feature emphasis per vertical), so this is a **template skeleton**, not a
  boilerplate byte block.

- **`**Note on security:**` paragraph**, appears on exactly **3 pages** (arboriculture,
  contract-cleaning, local-government-councils):
  ```
  grep -c 'Note on security' llms-full.txt   → 3
  ```
  All three are paraphrases of the same idea ("buttons are visible but still
  protected/require login") with different quoted button names and one with an added
  `(Mitti, Swept, etc.)` clause — not byte-identical, so excluded from the exact boilerplate
  byte total.

- **`## Related` heading**, appears **9 times** (mostly `/help/*` pages):
  ```
  grep -c '^## Related$' llms-full.txt   → 9
  ```
  Each instance is followed by a different bullet list of related-page links specific to
  that page's topic — the heading recurs, the link list content does not. Not counted as
  boilerplate bytes for the same reason.

## 3. Boilerplate vs. unique content — fraction of the file

| Category | Bytes | % of file (142,968 bytes) |
| --- | ---: | ---: |
| Exact, byte-identical, cross-page boilerplate (footer block: `***` + Ready CTA + Questions line + spacing, ×19) | 3,021 | **2.11%** |
| Everything else (unique per-page body content, headings, tables, code blocks, industry-specific copy, field references, etc.) | 139,947 | **97.89%** |

**Conclusion:** despite this being a 19-page marketing+docs corpus concatenated into one
`llms-full.txt`, the amount of literal, repeated boilerplate is small — about **2.1% of the
file by bytes/tokens** (~755 of ~35,700 estimated tokens), all of it the single footer CTA
block that Mintlify (or the QRtub docs config) appends to every page. There is no repeated
"Related links" boilerplate in the literal sense (the `## Related` heading recurs 9 times but
its content is unique per page), and no other cross-page copy-paste block was found beyond
the footer. The 5 industry pages do share a `## Ready to Deploy?` / `**Core features
available:**` heading skeleton and a paraphrased "Note on security" paragraph, but neither is
byte-identical, so neither inflates the file with literal duplicate text — they represent an
authored content template (structural redundancy) rather than copy-pasted boilerplate.

Practically: if someone wanted to trim this `llms-full.txt` for LLM context-window
efficiency, stripping the 19 footer blocks would only save ~3KB (~755 tokens, ~2.1% of the
file) — not a meaningful reduction. The file is overwhelmingly unique, substantive content.

## 4. Commands reference (for reproducibility)

```bash
curl -s https://help.qrtub.com/llms-full.txt -o llms-full.txt
wc -c llms-full.txt                                   # 142968
wc -l llms-full.txt                                   # 3734
grep -c '^Source: https://help.qrtub.com' llms-full.txt   # 19
grep -c '^# ' llms-full.txt                               # 19
grep -Fc '**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)' llms-full.txt  # 19
grep -Fc 'Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)' llms-full.txt                                  # 19
grep -c '^\*\*\*$' llms-full.txt                          # 31 (19 footer + 12 inline in Key Concepts)
grep -c '^## Related$' llms-full.txt                      # 9
grep -c '^## Ready to Deploy?$' llms-full.txt              # 5
grep -c 'Note on security' llms-full.txt                  # 3
grep -c 'hi@qrtub.com' llms-full.txt                       # 21 (19 footer + 2 unique in-context)
```
