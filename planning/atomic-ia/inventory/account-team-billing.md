# Concept inventory — Account, Team & Billing

Source verified against `/workspace/qrtub/src` (app routes, API routes, `lib/database/server-teams.ts`,
`server-team-users.ts`, `server-billing-accounts.ts`, `lib/stripe-plans.ts`, `lib/stripe-webhook-handlers.ts`,
`components/blocks/{InviteTeamMember,TeamSwitcher,CreateTeamModal,TeamSubscriptionCard,UpgradeModal,SearchEverything}`),
plus `GLOSSARY.md` and `BRAND.md` for terminology/voice, and the existing `/workspace/mintlify-docs/*.mdx`
files (none of which currently cover this domain — every row below is new territory, not a replacement).

No existing Mintlify page overlaps this domain. `pricing`, `team`, `billing`, `account`, `search` do not
appear as filenames or headings anywhere in `/workspace/mintlify-docs`; the only touchpoint is a
recurring CTA linking out to `https://qrtub.com/pricing`, which is exactly the boundary the "Plan Limits"
row below is built around.

Legend for the last column: **Standalone** = enough real, distinct content to earn its own page
(150+ words, per the Linear calibration where "Priority" is 243 words). **Merge → X** = too thin on
its own; fold into sibling page X as a subsection.

## Account

| Concept | Definition | Nav category | Page title | Slug | Standalone or merge |
| --- | --- | --- | --- | --- | --- |
| Account overview | The read-only identity panel on `/app/profile`: email, internal user ID, account-created date, email-verification status, and auth provider (email vs. OAuth). | Account | Account overview | `account/overview` | **Standalone.** Five distinct display fields plus the "what's editable vs. not" framing (see Avatar row) is enough for a short reference page, same weight as Linear's own "Profile" page. |
| Avatar / profile picture | The circular photo shown next to your name. Sourced only from OAuth provider metadata (`user.user_metadata.avatar_url`) — there is no in-app upload for a personal avatar; without one, initials are shown on an orange circle. | Account | *(no page)* | — | **Merge → Account overview.** One paragraph of real, brand-honesty-relevant content ("you can't upload a personal photo today — only teams have a logo you can set") but not enough to stand alone. |
| Change your password | The form on `/app/profile` that sets a new password for email/password login, independent of how you originally signed in. Same validation rules as signup: 8+ characters, upper + lower case, a digit, a special character. | Account | Change your password | `account/change-password` | **Standalone.** Covers the rule set, why it exists (kept in sync with the Supabase auth policy), the first-time "set a password" banner shown to invited/magic-link users, and why the request goes through a server route instead of the client SDK (httpOnly cookie sessions). Comfortably 250+ words of non-obvious behavior. |
| Your teams (account-level list) | The read-only list of every team you belong to, shown on the Account overview page. | Account | *(no page)* | — | **Merge → Account overview.** Purely a display list, not interactive from here — switching teams happens via the Team Switcher, not this list. |

## Team

| Concept | Definition | Nav category | Page title | Slug | Standalone or merge |
| --- | --- | --- | --- | --- | --- |
| Team overview | The shared workspace: a team owns Collections, holds members, and optionally carries one subscription. A user can belong to — and own — more than one team. | Team | Team overview | `team/overview` | **Standalone.** Orientation page, deliberately short (like Linear's "Concepts"), establishing the entity before the narrower pages below. |
| Switching between teams | The Team Switcher dropdown (sidebar, collapsed or expanded) that changes which team's data the dashboard shows. The active team is remembered in `localStorage` and restored on next login; switching to a different team navigates you back to the dashboard root. | Team | Switching between teams | `team/switching-teams` | **Standalone.** Explains a real, otherwise-confusing side effect (why the page reloads/navigates on switch) and the persistence behavior — enough distinct mechanism for its own short page. |
| Creating a team | The "Create Team" modal: name, an auto-generated (but editable) URL slug, and an optional logo. You become the new team's owner immediately and are switched into it. | Team | Creating a team | `team/create-a-team` | **Standalone.** Distinct task with its own form, validation, and side effects (auto-slug generation, immediate switch-and-refresh). |
| Team settings: name, slug & logo | The owner-only edit panel on the Team page for renaming a team, changing its URL slug (lowercase letters, numbers, hyphens only), and uploading a logo. | Team | Team settings | `team/team-settings` | **Standalone.** Same fields as team creation but a distinct, recurring task (editing an existing team) with its own entry point and owner-only gating — worth documenting on its own rather than only inside the creation flow. |
| Team roles: Owner vs. Editor | The only two membership roles. **Non-obvious nuance confirmed in code:** a member's `role` field (shown in the Owners/Editors counts and settable when inviting) is just a label — actual owner authority is a single `owner_id` column on the team. Inviting someone with the "Owner" role does **not** give them owner privileges; only Transfer Ownership does. | Team | Team roles | `team/roles` | **Standalone.** This mismatch is exactly the kind of narrow-but-real behavior worth its own page — silently getting this wrong causes real support confusion ("I made them an owner but they still can't edit team settings"). |
| Inviting team members | Two invite paths from one form: search-and-invite an existing QRtub user (autocomplete), or invite by email (creates a pending email invitation, sends a magic-link email, and the account is created on acceptance). Rate-limited to 50 invites/day per team. Requires the team to have an active subscription. | Team | Inviting team members | `team/invite-members` | **Standalone.** Two distinct code paths, a rate limit, and a subscription gate — well past the atomicity bar. |
| Managing pending invitations | The separate "Pending Invitations" list (email invites only — not existing-user invites, which appear as notifications to the invitee instead) with **Resend** and **Revoke** actions and a visible expiry date. | Team | Managing pending invitations | `team/pending-invitations` | **Standalone.** Distinct actions (resend/revoke), a UI surface of its own, and a common support scenario ("the invite expired / went to spam") that deserves direct coverage rather than a footnote on the invite page. |
| Accepting or declining a team invitation | The invitee's side: for an existing user, the invite shows as a notification (bell icon) with Accept/Reject actions. For a brand-new user invited by email, clicking the magic link creates the account and **auto-joins** the team on first login — no separate accept step. | Team | Accepting a team invitation | `team/accept-invitation` | **Standalone.** Two genuinely different mechanisms (explicit accept/reject vs. implicit auto-join) that both need explaining, and it's the invitee-facing counterpart to the owner-facing invite pages above. |
| Removing team members | Bulk-select members in the team table and remove them; owner-only; asks for confirmation and reports how many were removed. | Team | Removing team members | `team/remove-members` | **Standalone.** A distinct, destructive, owner-gated bulk action with its own confirmation flow. |
| Leaving a team | Any member can leave. Behavior branches sharply by role: a non-owner leaves immediately after confirming; the **owner cannot leave without transferring ownership first**, and if they're the only active member they can't leave at all. | Team | Leaving a team | `team/leave-a-team` | **Standalone.** The role-dependent branching alone is enough content, and it's the natural jumping-off point to the Transfer Ownership page. |
| Transferring team ownership | The atomic operation (single DB transaction via RPC) that reassigns `owner_id` to another active member and updates both members' role labels. Triggered directly, or automatically prompted when an owner tries to leave. **Implementation nuance:** the confirmation dialog lists all other active members, but the code always transfers to the *first* one in that list — it is not a picker. | Team | Transferring ownership | `team/transfer-ownership` | **Standalone.** Distinct mechanism, distinct entry points (direct vs. forced-by-leave), and a real UI/behavior gap worth calling out precisely rather than describing it as a free member picker. |

## Billing

| Concept | Definition | Nav category | Page title | Slug | Standalone or merge |
| --- | --- | --- | --- | --- | --- |
| Billing is per-team, not per-account | Subscriptions attach to a **team**, never to your user account. A user who owns several teams sees each team's plan separately (the "Your teams & subscriptions" list on the Account overview page is the concrete proof of this), and must subscribe/upgrade each team independently. | Billing | Billing is per-team | `billing/per-team-billing` | **Standalone.** This is the single most common source of billing confusion for a multi-team owner and is architecturally load-bearing enough to deserve its own explainer rather than being buried in a "how to pay" page. |
| Plans overview | The three self-serve tiers (Starter / Professional / Scale) plus an offline-only Enterprise tier, each with a monthly and an annual price. Deliberately does **not** restate the current price points, seat counts, or link caps here — those live on qrtub.com/pricing and drift too easily for a docs page to own. | Billing | Plans overview | `billing/plans-overview` | **Standalone.** Structural/conceptual content (tiers exist, annual = pay upfront for a multi-month discount, Enterprise is contact-only) that's stable even when numbers change — distinct from the numbers themselves. |
| Subscribing to a plan (checkout) | The public sign-up checkout at `/app/checkout`: pick a plan and monthly/annual, enter an email, get redirected to Stripe Checkout. On success, a new user account, team, and billing account are created together, and a magic-link email grants first access. | Billing | Subscribing to a plan | `billing/subscribe` | **Standalone.** A full, self-contained task with its own page, its own side effects (account+team+billing created together), and its own success/cancelled states. |
| Upgrading a team's plan | The in-app "Upgrade" flow (from the Team page's Subscription card) for an already-signed-in owner adding a paid plan to a team they own. Distinct from checkout: no new account or team is created — the new billing account attaches to the existing team, and no magic link is needed. | Billing | Upgrading a team's plan | `billing/upgrade-a-team` | **Standalone.** Same underlying checkout API but a meaningfully different flow, authorization check (must own the team), and result — worth its own page rather than a subsection of Subscribing, precisely because the two are easy to conflate. |
| Managing billing in the Stripe portal | The "Manage subscription" button (owner-only, per-team) that opens the Stripe Customer Portal for updating a payment method, viewing/downloading invoices, or cancelling. QRtub has no in-app equivalents for any of these — the portal is the only place they happen. | Billing | Managing your subscription | `billing/customer-portal` | **Standalone.** Covers three real actions (payment method, invoices, cancellation) that all live in one hand-off destination — natural single page, and important to state plainly that QRtub itself has no invoice list or cancel button. |
| Subscription status | What each status means and what it changes: `active`/`trialing` = full access; `past_due` = a grace period after a failed payment (still counts as "subscribed" for feature gating); anything else (`canceled`, `unpaid`, etc.) = blocked from premium features, including inviting new members. | Billing | Subscription status | `billing/subscription-status` | **Standalone.** A small, closed set of states with real behavioral consequences for each — the same shape as Linear's "Priority" page (a short enum with meaning attached to each value). |
| Plan limits & quotas | What can vary by plan — active link allowance, number of editor seats, number of reserved numbered-ID sequences — described as *categories that differ by plan* without hardcoding the current figures. Points to qrtub.com/pricing as the single source of truth for the actual numbers. | Billing | Plan limits & quotas | `billing/plan-limits` | **Standalone**, but scoped deliberately narrow — see Open Questions. Worth its own page precisely so the *numbers* never have to live in docs prose that goes stale. |

## Workspace (orphan features assigned to this domain)

| Concept | Definition | Nav category | Page title | Slug | Standalone or merge |
| --- | --- | --- | --- | --- | --- |
| Search Everything | The global `⌘`-style search panel: searches Collections, Items, Access Links, and Pages at once; supports a category filter, remembers recent searches (locally, clearable), and opens external results (Links/Pages) on the main domain rather than the app subdomain. | Workspace | Search Everything | `workspace/search-everything` | **Standalone.** Four searchable categories, a filter, a recents mechanism with its own storage/clearing behavior, and a same-tab-vs-new-tab navigation rule — genuinely more content than Linear's own 243-word "Priority" page, and no sibling page it naturally belongs inside. |
| Collection template gallery | The template picker at `/app/templates` (reached via "Browse all" from Collection creation): a searchable grid of pre-built field-configuration starter kits (e.g. Equipment Inspections, IT Assets, Medical Equipment) that pre-populate a new Collection's fields. Distinct from Page Templates and Media Templates — this gallery configures a Collection's *data schema*, not a Page's layout. | Workspace | Collection template gallery | `workspace/collection-templates` | **Standalone.** Worth flagging precisely *because* of the naming collision with "Page Template" and "Media Template" elsewhere in the product — a page that exists mainly to disambiguate needs to be findable on its own, not buried as a subsection of Collection creation. |

---

## Notes on categorization

- **Nav categories used:** `Account`, `Team`, `Billing` map directly to the domain brief. `Workspace` is
  a placeholder category for the two orphan features (Search Everything, Collection template gallery) that this
  task explicitly assigned to this domain but that don't semantically belong under Account/Team/Billing —
  see Open Questions.
- Every "Standalone" call above was checked against real, verified code paths (not assumed from the UI
  alone) — role-authority mismatch, ownership-transfer's "first active member" behavior, the
  per-team billing architecture, and the complete absence of an avatar-upload feature were all confirmed
  by reading the relevant route/service/component files, not inferred from labels.
- Deliberately **excluded** from this inventory as out-of-domain, with a note for the drafting phase:
  - **Forgot/reset password** (`/app/forgot-password`, `/app/reset-password`) — a logged-out recovery
    flow, not an in-session "Account settings" action. Likely belongs to an Auth/Getting-Started
    domain's inventory rather than here; "Change your password" above is the in-session counterpart.
  - **Login, signup, magic-link mechanics generally** — same reasoning; only the invite-flow's *use* of
    magic links is covered here (inside Inviting team members / Accepting an invitation), not the
    underlying auth mechanism.
  - **Numbered sequence patterns** (reserved ID ranges like `CRA0001TL`…) — mentioned only as one of the
    things that varies by plan (in Plan limits & quotas); the feature itself belongs to a Links/QR-codes
    domain, not Account/Team/Billing.
  - **Notification preferences** — checked directly: no such settings exist in the code. The bell icon is
    only a pending-team-invite inbox (covered under Accepting a team invitation), not a configurable
    notifications feature, so no page was invented for it.

## Open questions for the drafting phase

1. **Where do "Search Everything" and "Collection template gallery" actually live in the final top-level nav?**
   Neither is an Account/Team/Billing concept; they were assigned to this domain only because they were
   otherwise undocumented and small. A `Workspace` (or similar) top-level category may already exist from
   another domain's inventory — reconcile rather than creating a duplicate category with one page each.
2. **How honest should "Plan limits & quotas" be about enforcement?** Direct code inspection (grepping
   for quota/limit checks across `src/lib` and `src/app/api`) found **no server-side enforcement** of the
   advertised active-link caps or editor-seat caps anywhere in the codebase — they currently exist only
   as marketing copy inside `STRIPE_PLANS[].features`. Per `BRAND.md`'s "never overstate current
   capabilities" rule, the drafted page should avoid implying these are hard technical caps unless/until
   enforcement is verified again at draft time (the code may have changed). Recommend phrasing limits as
   "what's included in your plan" rather than "the ceiling the system enforces."
3. **"Editor" role vs. "editor" seat count are the same word for two related-but-distinct things** — the
   Team role literally named "Editor" (non-owner membership role) and the per-plan "number of editors"
   seat limit both use the bare word "editor." Confirm in drafting whether every active team member counts
   toward the seat limit regardless of role label (the code gives no evidence either way — no seat-count
   enforcement was found at all, per point 2), and word the two pages so they don't read as contradicting
   each other.
4. **Transfer-ownership's "first active member" behavior may read as a bug, not a feature.** The
   confirmation dialog's copy ("Select a new owner:" followed by a list) implies user choice, but the code
   always transfers to `activeMembers[0]` regardless of which name the owner was looking at. Documenting
   this precisely is correct today, but flag it back to product — the alternative is that this is a real
   defect that gets fixed before the docs ship, which would change this page's content.
5. **Slug placement:** all slugs above are nested under `account/`, `team/`, `billing/`, `workspace/` to
   mirror Linear's per-category URL grouping. Confirm this matches the folder convention the other
   domain-inventories are using before drafting, so the eventual `docs.json` nav tree is consistent.
