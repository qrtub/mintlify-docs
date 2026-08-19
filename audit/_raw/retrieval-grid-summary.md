# Retrieval Audit — Grade Distribution & Priority Gaps

Source: 40 simulated retrieval runs (`audit/_raw/retrieval/q01.md`–`q40.md`), compiled into
`audit/retrieval-grid.csv`.

## Grade distribution (out of 40)

| Grade | Count | Share |
|---|---|---|
| hallucinate | 23 | 57.5% |
| answered | 8 | 20.0% |
| partial | 7 | 17.5% |
| absent | 2 | 5.0% |

**Headline finding:** more than half of the simulated questions (23/40) land in the
"hallucinate" bucket — cases where the docs are silent on a question that a retrieval-backed
assistant would nonetheless feel strong pressure to answer confidently and specifically
(billing/plans, account/team permissions, data lifecycle on deletion/cancellation, and a
handful of product-mechanics edge cases). Only 8 questions were fully and safely answerable
from a single retrieved page.

## Priority gaps — every question graded 'absent' or 'hallucinate' (25 of 40)

### hallucinate (23)

1. Can two people edit the same Tub at the same time? — no multi-user/collaboration content anywhere.
2. How many links do I get on the free plan? — no pricing/plan/quota content in the help docs.
3. Can I use my own custom domain instead of qrtub.com? — no domain/white-labeling content; "Custom" Link slug is a false friend.
4. What happens to my QR codes if I cancel my subscription? — no billing/cancellation policy documented.
5. What happens if I reserve a numbered sequence and then run out of numbers in that range? — "reserved range" behavior is undefined.
6. Is there a read-only role, or does everyone with access to a Tub have full edit rights? — no roles/permissions model documented.
7. Does QRtub track who scans each code, or how many times it's been scanned? — one dangling "scan data" phrase, no analytics feature described.
8. Can I export all my data if I decide to leave QRtub? — no account-level export/offboarding policy.
9. How many team members can I add on each plan? — no seats/team-limit content anywhere.
10. What happens to a Link if I delete the Tub it belongs to? — cascade behavior for Items/Links on Tub deletion is undocumented.
11. Is there an API to create Links programmatically? — no API/SDK/webhook mentioned; only the manual dashboard flow is documented.
12. Can I password-protect a Page? — no page confirms or denies any access-gating feature for Pages.
13. Is there a maximum number of Destinations I can put on one Page? — no numeric or practical limit stated anywhere.
14. Can I remove the "Powered by QRtub" branding from my public pages? — no mention of such a badge or its removability.
15. What happens to my account data if I stop paying — is everything deleted right away? — no data-retention/grace-period policy documented.
16. Can I show different content on a Page depending on the time of day? — no time-of-day/timezone field exists in the documented field tables, but the gap is never stated outright.
17. Is there a QRtub mobile app, or is it web-only? — no page states this; "App Links" page is easily conflated with a QRtub-branded app.
18. Can I transfer ownership of a team to someone else? — no team/account-ownership concept documented at all.
19. Can I reuse a Link's slug after I delete it? — deletion rules exist, but slug-reuse/namespace policy is never addressed.
20. Is there any analytics dashboard showing scan counts over time? — no dashboard/report described; only a dangling "scan data" FAQ reference.
21. Can I give an external contractor access to just one Tub, not my whole account? — no per-Tub access-scoping/permissions content.
22. Is my data hosted in Australia, or overseas? — no hosting/data-residency content in the help docs (strong AU/NZ branding could tempt a confident but ungrounded guess).
23. If I downgrade my plan, do I lose access to Links I already created over the new limit? — no downgrade/plan-limit policy documented.

### absent (2)

1. Do you offer a discount for paying annually instead of monthly? — homepage honestly has no pricing detail and redirects to the marketing site rather than guessing.
2. What's actually different between the Starter, Professional and Scale plans? — same pattern: no in-docs comparison, honest redirect instead of fabrication.

## Notes on method

- `retrieved_page` in the CSV is the page a title/description-only retrieval pass would most
  plausibly surface first, per each audit file's own Step 1 reasoning.
- `answered_without_following_links` records whether that first-retrieved page alone (without
  needing to consult a second page) supplied the answer used for the grade. It is "no" for
  every absent/hallucinate row, since by definition the first page (and any others checked)
  did not contain the answer.
- Billing/pricing/account-administration topics (plans, seats, team roles, downgrade/
  cancellation behavior, data export) account for roughly half of the hallucinate gaps and are
  the single biggest thematic cluster — there is no billing/account page anywhere in the
  indexed help-docs corpus.
