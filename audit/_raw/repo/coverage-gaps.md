# Repo vs. docs coverage audit

Method: read every route under `qrtub/src/app/api/`, every page under `qrtub/src/app/app/`,
the "Key Features" section of `qrtub/CLAUDE.md`, `qrtub/src/lib/stripe-plans.ts`, and the
settings/configuration screens (`EditTubForm`, team page, profile page), then cross-referenced
against the full text of all 20 pages in `help/`, `industries/`, `integrations/`, and
`index.mdx`. "Doc status" is from the reader's point of view — a feature can be technically
mentioned and still be "partially-documented" if the mention is a passing aside rather than
something a user could act on.

Git history was checked for the 17 Aug 2026 binding-syntax fix (commit `20e4168`). Its own
scope (20 examples across 8 pages, `{fieldName}` → `{{item.fieldName}}`, the URL-encoding
correction in key-concepts.mdx, and the SafetyCulture/Mitti attribution fix) is not repeated
below. I re-grepped the current docs for any remaining single-brace URL examples the fix might
have missed — none found; the two remaining `item.{fieldName}` occurrences (conditional-visibility.mdx,
using-fields.mdx) are prose describing the naming convention, exactly as the commit says it
left them.

---

## Undocumented — no page or section covers this at all

### 1. Team management: invites, roles, removal, leave/transfer-ownership
- **Source**: `src/app/app/team/page.tsx` (619 lines), `src/app/api/team-users/*` (invite,
  invite/resend, `[id]/accept`, `[id]/reject`, `[id]/leave`, `[id]/user-data`, `bulk`,
  `pending`, `pending-email-invitations`, `process-pending`), `src/app/api/teams/[id]/transfer-ownership/route.ts`,
  `src/components/blocks/NotificationBell/NotificationBell.tsx`, `src/app/api/notifications/route.ts`.
- **Doc status**: undocumented. No help page covers teams at all — "Team management" is
  listed only as a one-line bullet in the Professional/Scale plan feature lists
  (`stripe-plans.ts`), never explained.
- **Note**: this is a substantial, multi-screen feature: send/resend/revoke email invites,
  accept/reject an invite from a notification-bell dropdown (the only UI surface for
  `NotificationType = 'team_invite'`), bulk-remove members, edit team name/slug/logo, and the
  guarded "leave team" flow that forces an ownership transfer first if the leaving user is the
  owner (`handleLeaveTeam` in `team/page.tsx` walks the remaining active members and calls
  `apiClient.teams.transferOwnership` before `teamUsers.leave`). None of it appears anywhere in
  `help/`.

### 2. Billing & subscription management (in-app)
- **Source**: `src/app/app/team/page.tsx` "Subscription" card (owner-only, via
  `TeamSubscriptionCard`), `src/app/app/profile/page.tsx` `MyTeamsSubscriptionsSection`
  (per-team billing summary across all the user's teams), `src/app/api/stripe/customer-portal/route.ts`,
  `src/app/api/stripe/create-checkout-session/route.ts`, `src/app/api/billing-accounts/*`,
  `src/app/app/checkout/*`.
- **Doc status**: undocumented. Every doc page's only touchpoint with billing is the
  boilerplate footer CTA "See plans and pricing →" linking to the external marketing site.
  Nothing explains how to open the Stripe customer portal, change plan/interval, view a past
  invoice, or what "per-team" billing means when a user belongs to several teams.

### 3. Numbered-pattern reservation UI (prefix/suffix/digits, auto vs. specific vs. range, conflict checking)
- **Source**: `src/components/blocks/CreateAccessLinkForm/CreateAccessLinkForm.tsx` — prefix,
  suffix, digit-count, three modes (`auto` / `specific` / `range`), live range-conflict
  checking against the team's existing patterns (`apiClient.numberedPatterns.checkRangeConflicts`),
  a 1000-numbers-per-request cap, and pattern autocomplete from
  `apiClient.numberedPatterns.getTeamPatterns`. Also referenced from `AddEditItemForm` and
  `EditTubForm` per `CLAUDE.md` §2.
- **Doc status**: partially-documented. `creating-your-first-link.mdx` mentions only "ID-based
  (`qrtub.com/cra001`) - Sequential or branded patterns" as one of three link-type bullets — no
  page explains how to claim a numbered range, what happens on a range conflict, the
  1000-number cap, or the plan-tier limit on how many numbered patterns a team may hold ("1
  numbered sequence pattern" on Professional, "Up to 5" on Scale per `stripe-plans.ts` — itself
  only stated on the external pricing page, never cross-referenced from the docs).

### 4. Per-Tub auto-link generation mode (random / item-ID-mask / none) and the Item-ID mask builder
- **Source**: `EditTubForm.tsx` `LinkGenerationTab` ("When a new item is created"),
  `lib/types/link-generation-config.ts`, `lib/database/auto-link-for-thing.ts`,
  `ItemIdMaskEditor`. Documented internally in `CLAUDE.md` §2b ("Modes: random ... item_id ...
  none").
- **Doc status**: undocumented. This is the setting that decides whether creating an Item
  mints a link automatically and what that link's slug looks like (built from the Item's own
  Item ID via a mask) — distinct from, and prior to, the manual "Generate Links" flow in
  `creating-your-first-link.mdx`. No doc page mentions it exists.

### 5. Whole-Tub backup / restore (export a Tub as JSON, create a new Tub from a backup)
- **Source**: `src/components/blocks/TubExportImportModal/TubExportImportModal.tsx`,
  `src/components/blocks/ImportTubFromBackup/ImportTubFromBackup.tsx`,
  `src/app/api/tubs/[id]/export/route.ts`, `src/app/api/tubs/import-new/route.ts`
  (`TubExportImportService`), reachable from the Tub Settings → Admin tab
  (`onOpenExportImportModal` in `EditTubForm.tsx`).
- **Doc status**: undocumented. Nothing in `help/` distinguishes this whole-Tub JSON
  backup/restore from the CSV item export/import below — a reader would not know this exists.

### 6. General Item CSV import/export (outside the print-batch flow)
- **Source**: `src/components/blocks/TubItemsHeader/TubItemsHeader.tsx` ("Export only the
  columns shown in table" / "Export all fields for full backup" / "Upload CSV"),
  `src/app/api/tubs/[id]/items/export-csv/route.ts`, `.../import-csv/route.ts`.
- **Doc status**: partially-documented. `custom-fields.mdx` mentions, in passing, that
  required-field and allowed-value validation applies "during CSV import" — but no page walks
  through the actual workflow (column mapping, upsert behaviour, what a full-backup export
  contains vs. the print-list export in `print-batches.mdx`). A reader has no way to learn this
  button exists from the docs.

### 7. Access-link CSV import (with dry-run) and bulk assign/unassign/delete
- **Source**: `src/app/api/access-links/import/route.ts` (`importAccessLinksFromCSV`,
  `?dryRun=1`, 10 MB / 10,000-row limits), `src/app/api/access-links/bulk/route.ts`
  (`assign` / `unassign` / `delete`, single ids or a full filtered `scope`),
  `apiClient.exportAccessLinksAsCsv`.
- **Doc status**: undocumented. Distinct from the print-batch CSV described in
  `print-batches.mdx` — this is importing/bulk-editing Links themselves (e.g. migrating an
  existing numbering scheme into QRtub). No page mentions it.

### 8. Global "Search Everything"
- **Source**: `src/components/blocks/SearchEverything/SearchEverything.tsx`,
  `src/app/api/search/route.ts` — one team-scoped, debounced query across Tubs, Items, Access
  Links and profile pages from the top-nav search box.
- **Doc status**: undocumented. Not mentioned in any doc page.

### 9. In-app camera QR scanner ("Scan" button, top navbar)
- **Source**: `src/components/blocks/ScanQrModal/ScanQrModal.tsx` (triggered from
  `top-navbar.tsx` and `SearchEverything.tsx`) — uses `@yudiel/react-qr-scanner` to scan a
  physical code with the device camera, resolves it, and if unassigned captures a downscaled
  reference photo (`captureOptimizedFrame`) and offers to create/assign an Item on the spot.
- **Doc status**: partially-documented. `print-first-workflow.mdx` narrates the *behaviour*
  ("Someone on your team who scans it gets an option to assign it there and then, from their
  phone") but never names the feature, says where to find it (a persistent camera icon in the
  dashboard's top nav), or mentions that a reference photo of the scanned item gets captured
  automatically.

### 10. Account/profile settings (password change, avatar)
- **Source**: `src/app/app/profile/page.tsx` — `PasswordSection` (change password with live
  validation, same rules as signup/reset), avatar upload.
- **Doc status**: undocumented. No help page covers account settings.

### 11. Tub-creation template library ("Choose a template")
- **Source**: `src/app/app/templates/page.tsx`, `lib/types/field-config-templates.ts`
  (`FIELD_CONFIG_TEMPLATES`) — a searchable gallery of pre-built field configurations (e.g.
  "Heavy Equipment", presumably matching the illustrative examples in `key-concepts.mdx`)
  offered when creating a new Tub, each one-click-creating a fully configured Tub.
- **Doc status**: undocumented. `key-concepts.mdx` uses "Heavy Equipment" / "Meeting Rooms" /
  "Fire Safety Equipment" purely as illustrative example Tubs — it never says these are actual
  starter templates a user can pick from a gallery at Tub-creation time.

---

## Not customer-facing (flagging for completeness, no doc action expected)

### 12. Waitlist system
- **Source**: `src/app/app/waitlist/page.tsx`, `src/app/api/waitlist/submit/route.ts`,
  `lib/database/server-waitlist.ts`. Gated behind `NEXT_PUBLIC_ENABLE_SIGNUP=false`
  (`CLAUDE.md` §1).
- **N/A** — this is a signup-gating mechanism for when public registration is switched off,
  not a product feature an active customer uses. No doc action needed.

### 13. `/api/templates` (label/print template activate/deactivate)
- **Source**: `src/app/api/templates/route.ts`, `src/app/api/templates/[id]/activate/route.ts`,
  `.../deactivate/route.ts`, `ServerTemplatesService`.
- **Note**: distinct from both the Tub-creation template library (#11) and the Page Editor.
  No component under `src/components` calls these routes (`apiClient.templates` is not
  referenced anywhere in the UI layer) — this reads as backend-only or not yet wired to a
  customer-facing screen. Flagging so it isn't mistaken for a documentation gap; recommend
  re-checking before writing anything, since it may simply be unfinished.

---

## Confirmed well-covered (checked specifically per the task brief)

- **`help/print-batches.mdx`**: covers batch creation via "Print list", the four-stage status
  (Draft → Printing → Printed → Deployed, with one-step-back and Deployed-is-final), per-code
  deployment status (Printed/Deployed/Retired), the retained original CSV, filtering Items by
  batch, why links in a non-draft batch can't be deleted, and **archiving** ("Completed batches
  can be archived. They stay available for reference but drop out of the default view.") —
  archiving is explicitly covered, not a gap.
- **Page Editor section count**: `building-a-page.mdx` states "Seventeen sections are
  available." Counted the actual manifests under `src/components/page/*.tsx`
  (`createSectionManifest`/`EditorComponentManifest` exports) — exactly 17. No divergence.
- Conditional Visibility, Device Detection, Using Fields (bindings), App Links & Fallback URLs,
  Custom Fields, Pages Overview, and the core Item/Link/Media three-entity model are all
  thoroughly and accurately documented, including the item-level page **Override** toggle
  (`building-a-page.mdx` "Saving: the whole Tub, or one Item") which matches
  `ServerItemOverridesService` / the `item_overrides` table exactly.

---

## Divergences found beyond the 17 Aug 2026 binding-syntax fix

None found. Re-checked the specific claims most likely to drift from code (section count,
archiving, URL-encoding behaviour, deletion/release-on-delete policy, batch status transitions)
and all matched the source. No new binding-syntax instances were missed by commit `20e4168`
(the two remaining single-brace occurrences are prose about the naming convention, not URL
examples).
