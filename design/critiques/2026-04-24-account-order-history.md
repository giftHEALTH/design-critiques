---
title: "Design Critique: Account Order History"
status: draft
area: cross-cutting
author: "Digital Experience Critique (Claude)"
invoked_by: alexturvy
generated_by: gifthealth-digital-experience
created: 2026-04-24
last-updated: 2026-04-24
artifact: "https://www.figma.com/design/awfubM5IdTkKcvrHn2hW8C/test-temp?node-id=3877-34896&t=gzHdVRnYXPwxq7Ds-0"
artifact_type: figma
reviewed_lenses: [patient-advocate, commercial-lens, craft-critic]
summary: "Account page has a clean white-label shell but is mislabeled (vs Order History), Edit affordance is inconsistent, and no active-therapy context."
top_heuristics: [N4, N1, N6, law-miller, law-peak-end]
top_biases: []
scope: multi-lens
figma_write_blocked: false
revised_page_url: ""
---

# Design Critique: Account Order History

**Date:** 2026-04-24
**Artifact:** https://www.figma.com/design/awfubM5IdTkKcvrHn2hW8C/test-temp?node-id=3877-34896&t=gzHdVRnYXPwxq7Ds-0
**Scope:** multi-lens
**Lenses:** patient-advocate, commercial-lens, craft-critic

## Summary

**Top 5 actions** (severity-ranked, cross-lens):
1. Reconcile the artifact name vs. page content: the frame is titled "Account" but the brief was routed as "Order History," and no order/shipment/tracking surface exists on this screen. Either rename the artifact to Account-only or add an Order History section above Personal Details (most-recent order, status chip, tracking link, next-refill date). Flagged by: Patient Advocate, Commercial Lens, Craft Critic.
2. Standardize Edit affordance across all three accordion sections. Today only 2 of 9 data rows expose Edit; the pattern is unpredictable. Commit to per-row Edit or a single section-level Edit Profile CTA, applied uniformly. Flagged by: Patient Advocate, Commercial Lens, Craft Critic.
3. Fix brand typography and palette (conditional on whether this is a Gifthealth-primary surface or a LillyDirect co-brand). Today: IBM Plex Sans instead of Funnel Display, #0F3A85 links instead of a Purple-derived token, #EEF0F8 background instead of Soft Beige, and Purple #440b39 absent entirely from the page. Flagged by: Craft Critic.
4. Disambiguate Mobile Phone vs. Landline. Both rows show the same number; label the SMS-of-record explicitly ("Mobile Phone (for text updates)") or remove the Landline row. Critical for SMS-driven checkout and refill flows. Flagged by: Patient Advocate, Commercial Lens, Craft Critic.
5. Add open/close affordance to the accordion headers (chevron, rotation on expand, focus and hover states) or drop the "accordion-item" container naming and render as static cards. Today the pattern advertises interactivity the design doesn't deliver. Flagged by: Patient Advocate, Craft Critic.

**Consensus:**
- The page title and IA mismatch (node 3877:34946) is a structural defect flagged by all three lenses. A patient who clicks Orders and lands on a demographics page is the exact "where is my Zepbound" moment Amazon and Walmart win.
- Edit-affordance inconsistency across rows is flagged independently by all three lenses. It is the single change that most impacts both patient self-service and multi-tenant onboarding.
- The Account page is thin on active-therapy context. Patient Advocate sees missing tracking and refill data; Commercial Lens sees competitive cede; Craft Critic sees no peak moment. Three lenses, one underlying gap.
- Mobile Phone and Landline duplicate values are a shared concern: patient safety (which number receives SMS?), commercial (demo hazard for a pitch), and usability (same value in two rows is confusing).

**Tensions requiring human decision:**
- **Partner brand vs. Gifthealth brand on white-labeled surfaces.** Craft Critic treats IBM Plex Sans and the off-palette link color as brand-compliance defects; Commercial Lens praises the white-label execution as strategically correct for land-and-expand. The fix list differs materially depending on whether this surface is Gifthealth-baseline with tenant overrides or partner-primary. A design-system decision is required before the typography and palette fixes are actioned.
- **Scope of the Account surface.** Patient Advocate wants Order History content added here (tracking, refill countdown); Commercial Lens agrees either Account must carry therapy weight or a dedicated Orders page must hit Amazon/Walmart parity. Where therapy status lives, consolidated on Account or on a separate Orders page, is a product-level IA decision this critique cannot resolve.
- **Medical History: expand for competitive parity vs. tighten for patient safety first.** Commercial Lens wants breadth (medications, conditions, immunizations) to match Phil's offering. Patient Advocate wants depth (last-confirmed date, source attribution) for safety in a dispensing context. Both are right; sequencing matters.

---

## Patient Advocate

**Top-line verdict:** This looks like polish-bound work on a read-only account summary — the shape is sensible and legible, but the page is mislabeled and under-serves a patient who reached Account looking for order tracking. Note: artifact name from orchestrator says "Account — Order History" but the rendered screen is an Account profile (Personal Details, Contact Information, Medical History) with no order history section visible; critique proceeds against what's on the canvas.

**Gauntlet stage(s) this touches:** Fulfillment (tracking, refill readiness, shipping address accuracy) — and secondarily Prescription/Approvals via address and allergies accuracy that can kick back a script.

**What's working:**
- Clear visual hierarchy: three grouped accordions ("Personal Details," "Contact Information," "Medical History") with consistent row patterns means an overwhelmed patient can scan without thinking hard. This respects Miller's Law (limiting what the working brain has to hold) and law of proximity.
- Shipping Address and Allergies have visible "Edit" affordances. In a product where a wrong address is a documented PCR ticket driver (per the LDZ global workflow), surfacing edit inline — rather than burying it behind a modal or a support call — is the right instinct. This is patient agency, not patronizing helpfulness.
- Breadcrumb present. Even a simple "Home / Account" gives a patient a visible way back, which matters when someone is tapping through on a phone in a pharmacy parking lot.
- IBM Plex at 16px body with strong label/value contrast is readable for the range of ages and literacy levels this product serves. Not flashy, and that's correct — patients said our tone feels "calm and uncomplicated," and this honors that.

**What's not:**
- **[Severity: content | node: 3877:34946]** Page title reads "Account" but the orchestrator named this "Account — Order History," and there is no Order History content on this frame. A patient who clicked "Orders" in the nav expecting shipment status lands here with no tracking, no order list, no "when is my next refill" — just profile fields. This is the exact "how does someone know they can come back to track shipping" gap Edward flagged in the checkout flow research. Fix: either rename the page heading text to "Account" and remove the "Order History" label from the artifact title, or add an Order History accordion/section above Personal Details containing at minimum: most recent order with status chip, tracking link, and next-refill-available date. The accordion pattern already on the page scales to this.
- **[Severity: usability | node: 3877:34962]** Mobile Phone and Landline rows have no visible Edit link, while Shipping Address and Allergies do. A patient who changed phone numbers (common — patients switch plans, lose phones) cannot self-serve. Either they re-enter it at next checkout (friction that compounds) or they call a PCR. Fix: add an "Edit" TextNode styled identically to the Shipping Address Edit (color #0f3a85, IBM Plex Regular 16px) in the right-aligned "Small" slot that already exists on 3877:34962 and 3877:34963 — the slot is present but empty. Same for Email at 3877:34964.
- **[Severity: content | node: 3877:34963]** "Landline" label shown alongside Mobile Phone, both populated with the same number +1 (209) 300-2557. For an SMS-driven product — where checkout links, refill reminders, and tracking updates are delivered by text — the phone-of-record for SMS must be unambiguous. A duplicate-looking row risks a patient editing the wrong one and silently breaking their own notifications. Fix: if landline is not used for SMS, label the mobile row "Mobile Phone (for text updates)" and demote Landline to a secondary position, or remove the Landline row entirely if operations doesn't route to it.
- **[Severity: content+design | node: 3877:34972]** "Allergies" under Medical History shows a plain comma list — "Bactrim, Codeine, Hydrocodone" — with no indication of when this was last confirmed, who entered it, or how to add a missing one. A patient whose allergy was captured incorrectly at intake has no visible recourse beyond "Edit," and clicking Edit is a trust leap ("will this update my pharmacist's view, or just this page?"). For a pharmacy that dispenses, stale or wrong allergy data is a safety issue, not just a data-hygiene one. Fix: add a subtle secondary-text line under the allergy list reading "Last confirmed: [date] — [source: You / Prescriber]" at 14px, color #6C757D, and ensure the Edit action states destination ("Update allergies sent to your pharmacist").
- **[Severity: usability | node: 3877:34947]** All three accordions render expanded. "Accordion-item" components that never collapse just add visual containers without the progressive-disclosure benefit the pattern exists for. For a patient scrolling on mobile, they will scroll past the one section they came for. Fix: either collapse by default with a visible chevron and show-count hint (e.g., "Personal Details (3)"), or drop the accordion component wrapper and use simple section cards — choose one. The current pattern advertises interactivity (the rounded container, the "collapse-header" naming) that the design doesn't deliver.
- **[Severity: content | node: 3877:34898]** Top nav reads LillyDirect branding with "Prescriptions," "Orders," "Account," "Chat." Good that Chat is persistent (the escape hatch patients rely on), but there is no visible indication of *which* item is active. A confused patient who clicks Orders, lands here, and sees "Account" in the heading does not get any visual confirmation of where they are in the nav. Fix: add an active-state treatment to the Account nav item at 3877:34915 — underline, weight bump, or brand color fill — to match the breadcrumb.
- **[Severity: content | node: 3877:34941]** "Sign out" is a persistent top-right underlined link with the same visual weight as "Chat" and "Language: EN." For an older patient or one in a medicated state, an accidental sign-out mid-refill-check is a silent failure — they exit, the task is abandoned, and no error tells them what happened. Fix: either move Sign out behind an account/avatar dropdown at 3877:34930, or add a lightweight confirm ("Sign out of your Gifthealth account?") on click.

**Patient-visibility check:**
- Can a confused patient self-serve here? **Partial.** They can edit Shipping Address and Allergies, but not phone, email, or any profile detail — and cannot see order status, which is likely why they came.
- Does this design surface status or hide it? **Hides.** No order status, no refill countdown, no "last dose shipped," no pharmacy-visible confirmation that the allergies shown are what the pharmacist reads. This is the "how does the patient know they can come back to track" hole Edward flagged — it is still open on this screen.
- Where does friction compound downstream? A patient who needed tracking and didn't find it here will call a PCR (cost to ops, per strategy-and-vision OKR #2 on reducing support contacts) or exit to UPS email (where Gifthealth loses touchpoint and the tracking-to-reorder loop breaks). At the next refill, a stale phone number silently breaks the SMS that re-enters them into checkout — a classic silent failure that looks to ops like "patient didn't engage" when really the patient never got the text.

**Next actions:**
1. Reconcile the artifact name: either ship an Order History section on this screen (recommended, given the tracking-gap in research) or correct the artifact name in the orchestrator so downstream reviewers aren't critiquing against a spec this frame doesn't meet.
2. Close the edit-affordance inconsistency on 3877:34962, 3877:34963, 3877:34964 — every contact row needs an Edit link in the empty "Small" slot already in the component. This is a 20-minute change that removes a PCR call category.
3. Disambiguate Mobile Phone vs. Landline with a label that names which number receives SMS; this directly protects the checkout re-entry path documented in the patient checkout flow.
4. Add "last confirmed" provenance to Allergies. In a dispensing context this is a safety signal, not a nice-to-have.
5. Add active-state styling to the current nav item so a patient always knows where they are in the portal.

---

## Commercial Lens

**Top-line verdict:** The artifact is titled "Account — Order History" but actually renders an Account Profile (Personal Details, Contact Information, Medical History) — and it ships under the LillyDirect logo with Gifthealth relegated to a "Powered by" footer. At this stage of fidelity the page is clean, but it is a textbook concentration-risk artifact: every visible surface is branded for a single partner, with no platform-level affordances for multi-tenancy, editability, or commercial levers.

**Commercial stakeholder(s) this touches:** End patient in a multi-tenant context (LillyDirect-branded today, but structurally shared), Pharma client admin (who needs to white-label the same shell for the next manufacturer), Account Manager (who owns the renewal conversation with Lilly and the pitch to the next logo), Program Manager (who has to onboard new programs into this template in < 24 hours).

**What's working:**
- **White-label executed cleanly.** The LillyDirect logo, "Powered by gifthealth" footer, and neutral chrome (IBM Plex Sans, subdued grays) are a credible example of the white-label template the strategy calls for. This is the right packaging pattern for land-and-expand.
- **Account surface is partner-agnostic in content.** The fields shown (Name, Gender, Birthday, Mobile, Landline, Email, Shipping Address, Allergies) are universal patient-profile fields — not Lilly-specific, GLP-1-specific, or program-specific. Another manufacturer could in principle reuse this shell without content changes, which is the platform test.
- **Accordion structure scales.** Three collapsible sections (Personal Details / Contact Information / Medical History) is a container pattern that can absorb additional sections (Insurance, Payment Methods, Consents, Caregivers) as Digihub expands without re-architecting the page. That's onboarding-friendly.
- **Edit affordances present where they matter.** Shipping Address and Allergies expose "Edit" links; these are the fields most likely to change between fills, and self-service edits reduce PCR contact volume (contributes to the "Reduce support contacts" OKR).

**What's not:**

- **[Severity: content | node: 3877:34946]** Page title is "Account" but the artifact name is "Order History" — a routing/IA mismatch. If patients click "Orders" in the nav and land on an Account profile, they drop into a dead end exactly where Amazon same-day and Walmart win on "where is my Zepbound." This is a competitive-positioning giveaway at the precise moment the competitive landscape doc flags as vulnerable. Fix: confirm the intent of this page — if it's the Account profile, align the file name and any upstream links; if it's meant to be Order History, this design does not deliver that and needs to be redone with order/shipment/tracking content.

- **[Severity: content+design | node: 3877:34953]** Personal Details has NO edit affordances on Full Name, Gender, or Birthday, while Contact Information and Medical History DO. Pharma partners onboarding into this template will ask "which fields are patient-editable vs. PCR/admin-editable, and is that configurable per program?" Today it looks hardcoded. Affects time-to-onboard and multi-tenant readiness. Fix: add an Edit link to each Personal Details row (matching the pattern at node 3877:34965), OR expose explicit read-only treatment (lock icon + helper text "Contact support to change"). Either way, the pattern must be the same across all three accordions so the next manufacturer can configure per field without design changes.

- **[Severity: content | node: 3877:34966]** "Medical History" contains only "Allergies." No medications, no conditions, no immunizations, no prior therapies. For a specialty/Hub+Pharmacy platform where prior auth, step therapy, and persistence are the commercial battlegrounds, a Medical History with only allergies is thin enough to cede ground to Phil (the only other Hub+Pharmacy) and to AssistRx/ConnectiveRx on the hub side. Affects Lilly expand motion and next-logo pitch. Fix: either expand the Medical History accordion to include Current Medications, Medical Conditions, and Past Therapies as sub-rows (the node structure at 3877:34963/34964 already exists as a pattern and is literally mislabeled "Medical Conditions" and "Medications" in the layer tree — the scaffolding is there), or rename to "Allergies" to match what's actually shown.

- **[Severity: content+design | node: 3877:34897]** The navbar surfaces "Prescriptions," "Orders," "Account," "Chat," "Language: EN," "Sign out" — but no visibility into order status, refills due, or active program. For a platform whose patient positioning is "start sooner, abandon less, persist longer," the top-level nav gives no signal that the patient is mid-therapy. LillyDirect and Amazon show shipment status prominently; this does not. Affects persistence (OKR: Fill/Script rate) and competitive positioning vs. Amazon/Walmart. Fix: flow-level — consider surfacing an active-order strip or "next refill" chip above the accordions on the Account page so the profile page pulls its commercial weight.

- **[Severity: content | node: 3877:34973]** "Powered by gifthealth" is the only Gifthealth surface the patient sees. This is intentional in a white-label model and strategically correct — but it means Gifthealth has zero equity with the end patient. When Lilly considers renewal, there is no patient-side moat. Flow-level observation, not a design fix: the commercial lever here is not visual, it's contractual (data, integrations, switching cost). Worth naming because it's the silent concentration risk every Account page compounds.

- **[Severity: usability | node: 3877:34962]** Mobile Phone and Landline both show "+1 (209) 300-2557" — same number. If this is production data, it's a bug; if it's placeholder, it's a demo-to-pharma-client hazard (an Account Manager screen-sharing this in a pitch will get questioned). Fix: use distinct placeholder numbers (e.g., Mobile "+1 (209) 300-2557", Landline "+1 (209) 555-0142") and confirm upstream data model actually distinguishes the two fields.

- **[Severity: content+design | node: 3877:34901]** The nav is LillyDirect-branded but structurally shared across tenants. There's no visible tenant switcher, no program context, no indication this same shell will host Novo Nordisk or Pfizer tomorrow. For the Program Manager building the next manufacturer's onboarding in under 24 hours, the design gives no hook for "where does the next logo/color/nav-item slot in?" Fix: flow-level — ensure the design system (referenced in strategy-and-vision.md as "A design system that powers the white-label template") documents the token-level swap points for logo, nav items, and color so a new tenant is a config not a redesign.

**Platform-vs-partner check:**
- **Platform / Partner-specific / Mixed:** **Mixed, leaning Platform.** Content (fields, accordion pattern, edit affordances) is partner-agnostic. Branding (logo, footer) is swappable. But the layer tree contains telling artifacts — rows labeled "Medications" used for "Full Name," "Medical Conditions" used for "Landline" — suggesting this screen was copy-pasted from another (likely Lilly GLP-1) flow without the components being properly abstracted. That's the silent concentration move: it ships fine for Lilly, but the next pharma partner inherits naming confusion in the design source.
- **Can a new pharma partner adopt this in < 24 hours?** **With work.** Logo/footer/nav text are swappable. But the mislabeled component layer names, the missing edit affordances in Personal Details, and the thin Medical History all need decisions that are currently implicit. Until the accordion + row pattern is promoted to a documented white-label template (config-driven, not copy-paste-driven), "24 hours" is aspirational for this surface.
- **Where does this cede ground to a named competitor?** **LillyDirect** (natively) — this IS a LillyDirect-branded page, so LillyDirect's own pharmacy buildout directly threatens it; the commercial moat is whatever Gifthealth does BEHIND this page that LillyDirect can't replicate. **Amazon/Walmart** — no shipment tracking, no same-day signal, no refill CTA visible on the Account surface; the patient must leave this page to learn anything about their actual therapy status.

**Next actions:**
1. Reconcile the title/artifact-name mismatch before this goes further — clarify whether this is the Account profile or Order History, and if the latter is needed, scope it as a separate artifact (order status, shipment tracking, refill CTA are where the Amazon/Walmart competitive bar sits).
2. Standardize the edit-affordance pattern across all three accordions so it becomes a per-field config, not a per-section hardcode. This is the single change that most accelerates multi-tenant onboarding.
3. Rename the mislabeled component instances in the layer tree ("Medications" → "ProfileRow," "Allergies" → "ProfileRow") and promote the row to a documented design-system component. The strategy's < 24 hour onboarding target requires this kind of abstraction to be real, not accidental.
4. Decide whether the Account page should surface active-therapy context (next refill, recent shipment) or stay strictly profile-only. If profile-only, ensure the Orders page carries the full weight of competitive parity with Amazon same-day and Walmart.
5. Use this page as a test case for the white-label design-system work in Phase 1 — re-skin it mentally for a non-Lilly manufacturer and list every change required. That delta is the onboarding-time debt.

---

## Craft Critic

**Top-line verdict:** The shape of this account page is conventional and legible, which is the right instinct for a utility screen — but the artifact is materially off-brand (IBM Plex Sans instead of Funnel Display, a non-Gifthealth blue link color, no palette presence for Purple #440b39) and carries several craft defects that are visible even at static fidelity. Production-bound or not, the brand gap alone is the load-bearing finding.

**Context flag:** Account detail pages are medium-craft context — patient-facing, but highly conventional (view/edit a row of info). The UX shape here is fine; the lens that should get emphasis is **brand compliance**, not interaction polish. If this is a Lilly Direct co-branded surface where IBM Plex Sans is the Lilly system font, several of the typography findings below may be intentional — confirm before treating them as defects. If this is a Gifthealth-branded surface, everything below stands.

**What's working:**
- Information architecture is clear and respects Nielsen 8 (Aesthetic and minimalist): three discrete accordion sections (Personal Details, Contact Information, Medical History) chunk a long flat profile into scannable, meaningful groups — a clean application of Miller's Law and the Law of Common Region.
- Breadcrumb is present and labels match convention (Home > Account), supporting Nielsen 3 (User control) and Jakob's Law.
- Edit affordances are placed inline at the row level (Shipping Address, Allergies) rather than hidden behind a modal, which keeps the mental model simple — user sees the field and the action together (Recognition over recall).
- Rounded 12px card corners and a subtle 1px border (#E4E7EB) on each accordion match a consistent card system — good use of consistent tokens (Theme/Border).
- The page-background contrast against white cards (#EEF0F8 vs #FFFFFF) creates acceptable grouping without harsh dividers.

**Brand compliance:**
- **Palette: Fail.** The design uses none of the approved primary/secondary/accent colors. Primary Purple (#440b39) is absent entirely — no headline, no CTA, no logo-tier element carries it. The `Edit` link uses `#0f3a85` (a navy blue not in the Gifthealth palette). Body text uses `#212529` and secondary text `#6C757D` — both Bootstrap defaults, not tokenized against the Gifthealth palette. Page background is `#EEF0F8` (not Soft Beige `#FFF1EE`, the approved light-surface secondary).
- **Typography: Fail.** Every string renders in `IBM Plex Sans` (Medium, Regular, SemiBold). Brand standard is **Funnel Display** (brand) or **Aptos** (system fallback). IBM Plex is neither. This is the most visible brand defect and appears on every line of the screen. Footer also mixes in `SF Pro Text` for the "Powered by" caption — a third font family on a single page.
- **Gradients: N/A.** No gradients used — acceptable for a utility page, no violation here.
- **WCAG contrast:**
  - Body `#212529` on `#FFFFFF`: passes AA and AAA (ratio ~16.1:1). Good.
  - Edit link `#0F3A85` on `#FFFFFF`: passes AA for normal text (~10.3:1). Good — but off-palette.
  - Secondary breadcrumb `#6C757D` on `#EEF0F8` page background: ~4.3:1, passes AA for normal text by a narrow margin. Borderline — will fail AAA and is risky if the page background shifts even slightly darker.
  - "Powered by" caption at 11px `#2D112B` on `#EEF0F8`: ~14:1 contrast, but 11px is below the comfortable minimum for a caption regardless. Consider 12–13px minimum.

**Heuristic pass (Nielsen's 10):**
- **Nielsen 4 (Consistency and standards): at risk.** The three accordion sections have inconsistent internal row behavior — Personal Details rows lack an Edit affordance entirely (Full Name, Gender, Birthday are presumably editable somewhere else?), Contact Information has Edit only on Shipping Address (not on Mobile, Landline, Email — are those locked? Editable elsewhere?), and Medical History shows Edit on Allergies. A patient scanning the page cannot predict which fields are editable. Either every row gets an Edit link, or the pattern gets a clear rule ("contact details are verified through support") communicated once.
- **Nielsen 1 (Visibility of system status): at risk.** The accordions look like accordions (rounded containers, header row with title) but there is no chevron, caret, or other signifier that they are collapsible — and no visible open/closed state. If these are static sections styled as accordions, rename/restyle them as cards. If they are actually collapsible, add the affordance.
- **Nielsen 6 (Recognition rather than recall): partial pass.** Labels are clear ("Full Name", "Mobile Phone"). Good. But "Landline" with the same phone number as "Mobile Phone" is confusing in the dummy data — in production, same value in two rows should either dedupe or be explicitly marked as the same number on purpose.
- **Nielsen 8 (Aesthetic and minimalist): passes.**
- **Heuristics 2, 3, 5, 7, 9, 10: not at risk in this view** (account landing, no destructive actions, no error states shown).

**Interaction quality:**
- Static design only — no visible animation or interaction tokens in the Figma metadata to critique. What is observable: the accordion headers have no hover, focus, or press affordance baked into the design. On a page where the sections might be collapsible, the missing state set (hover/active/focus/open) is a gap. Per Emil Kowalski: buttons and pressable surfaces must feel responsive — a header that looks pressable should have at minimum a hover state and a scale-down-on-press feedback (`scale(0.97–0.99)` via `transform`).
- The `Edit` link is an unstyled text link without a visible hover, focus, or visited treatment in the comp. In a healthcare context, `focus-visible` on these keyboard-reachable links is not optional.
- Concentric radius: accordion outer radius is 12px with a 1px border + inner card starts flush to the border. That's fine for a flat card. But the header's `rounded-tl-[4px] rounded-tr-[4px]` (4px top corners on the inner header) does not match the 12px outer — that would leave a 1px stripe where the outer curve does not match the inner curve. Non-concentric by ~8px. This reads as a copy-paste residue from a Bootstrap component template rather than an intentional choice.
- Tabular numerals: the phone numbers and birthday are not marked `font-variant-numeric: tabular-nums`. Low urgency on this static page, but the moment these values become editable inline or update live (which they will, with Edit links), non-tabular digits will shift layout on value change. Cheap to add.

**What's not:**
- **[Severity: content+design | node: 3877:34898]** Wrong typeface on the entire navbar — IBM Plex Sans Medium at 15px across all nav items. Violates brand typography rule (Funnel Display is the only approved brand font; Aptos the only approved fallback). Replace the text style on every `p` under this node with `Funnel Display Medium` at 15px, falling back to `Aptos Medium` then system. If this is a Lilly-co-branded surface with a deliberate Lilly typography stack, confirm with brand before treating as a defect.
- **[Severity: content+design | node: 3877:34946]** Page title "Account" renders at 24px `IBM Plex Sans Medium` in black — for a page-level H1 on a Gifthealth surface this should be Funnel Display Bold at 24–28px in Purple #440b39 (brand-primary tier), not plain black. Set `fontFamily = Funnel Display`, `fontWeight = Bold`, `fill = Theme/Primary (#440b39)`.
- **[Severity: content+design | node: 3877:34947]** Accordion section titles ("Personal Details", "Contact Information", "Medical History") render at 18px IBM Plex Sans Medium in black. Change the font to Funnel Display Bold (per brand type hierarchy: body subheads use bold weight). Consider using `Theme/Secondary` or `Purple XD` fill rather than pure black `#000000`, which is harsher than brand intent.
- **[Severity: content+design | node: I3877:34965;4678:787]** `Edit` link color `#0F3A85` is not a Gifthealth palette value — it is Bootstrap-default navy. Replace with a token. For brand-aligned links on light surface, use Purple #440b39 with underline, or introduce an approved "link" token derived from the Purple primary. Apply the same fix to node `I3877:34972;1:837` (Allergies Edit).
- **[Severity: content+design | node: 3877:34896]** Page background `#EEF0F8` is off-palette (cool-gray/blue tint). Approved light-surface secondary is Soft Beige `#FFF1EE`, or white `#FFFFFF` with card elevation. Recommend changing the outer frame fill to `Soft Beige (#FFF1EE)` to anchor the page to brand, OR to `#FFFFFF` with borderless cards separated by dividers.
- **[Severity: content+design | node: 3877:34943]** Inconsistent Edit affordance across rows (Nielsen 4: Consistency). Of 9 data rows, only 2 show `Edit`. A patient cannot predict editability. Resolution options: (a) add `Edit` to every row where editing is supported, or (b) move all editing behind a single "Edit Profile" CTA at the section header — commit to one pattern. If some fields are genuinely locked (name, DOB on a regulated surface), add a small lock icon + helper text "Contact support to update" so the absence of Edit is legible, not mysterious.
- **[Severity: content+design | node: 3877:34947]** Accordion sections have no open/close affordance (Nielsen 1: Visibility of system status). No chevron, no indicator. Either add a 16px chevron-down icon (Purple #440b39) on the right of each header that rotates 180° on expand, or restyle these as non-collapsible cards and drop the "accordion-item" naming from the layer structure.
- **[Severity: usability | node: 3877:34943]** No primary CTA, no status banners, and no way to reach related actions (order history, payment methods, notification preferences) from the Account landing. If this is the full Account surface, surface the user's active prescription and most recent order status at the top (Peak-End Rule: what the user sees first and last defines the experience) — an account page that shows only static demographic fields misses the job-to-be-done for a pharmacy patient who came here to do something.
- **[Severity: content+design | node: I3877:34953;4678:782]** Rows use a bizarre layout spacing token: `gap-[207.31px]` on the Container that holds "Full Name" beside the value. A hardcoded 207.31px gap is a magic number that will break on anything other than 1440px canvas width. Replace with `justify-content: space-between` (label left, value right) or a normal flex spacing token (e.g., 16–24px gap). Same defect appears on nodes `I3877:34954;4678:782` and `I3877:34955;4678:782`.
- **[Severity: content+design | node: 3877:34948]** Header has inner `rounded-tl-[4px] rounded-tr-[4px]` against an outer `rounded-[12px]` container — non-concentric by 8px. Change inner header radius to `rounded-tl-[11px] rounded-tr-[11px]` so the inner curve equals outer curve minus the 1px border, OR remove the inner radius entirely and let the overflow-clip on the outer handle it.
- **[Severity: content+design | node: I3877:34973;1:88]** Footer "Powered by" caption renders in `SF Pro Text` at 11px, a third font family on the page. Change to Funnel Display Light at 12px minimum, color `#2D112B` (close enough to Purple XD but verify — Purple XD is `#2E0326`, not `#2D112B`). Also verify the Gifthealth logo here uses the 1-color Purple variant on light background per brand guidance.
- **[Severity: content+design | node: 3877:34898]** Navbar has a `padding: 12px 292px` — the 292px horizontal padding is a hardcoded magic number baking the gutter into the component. Replace with a max-width centered container pattern so the navbar adapts to viewports narrower than 1440px. Same magic-number issue on `3877:34942` (main content padding).
- **[Severity: content+design | node: 3877:34944]** Main heading "Account" has no vertical spacing rhythm against the breadcrumb above and the first card below — the current 13px gap feels cramped. Consider 24px below breadcrumb, 16–20px above the first card, and upgrade the visual weight of the H1 (Funnel Display Bold at 28–32px, Purple) so the page has a clear peak.

**Next actions:**
1. Confirm brand context: Gifthealth-primary vs Lilly Direct co-branded. If Gifthealth-primary, swap every text style to Funnel Display (Bold for headlines, Regular/Light for body) and remove IBM Plex Sans and SF Pro Text from the file. Bind every typography layer to a text style token so this cannot drift again.
2. Replace raw hex values with tokens across the file: `#0F3A85` → Link/Primary (derive from Purple), `#EEF0F8` → Background/Surface (set to Soft Beige `#FFF1EE` or white), and elevate the Account H1 fill to Theme/Primary `#440b39`. The existing variable defs (Body Text/Body Color, Theme/Border, Theme/Secondary) prove the token system exists — this page is bypassing it.
3. Decide one rule for row editability and apply it consistently across all three accordion groups — either per-row Edit links everywhere or a single section-level Edit Profile CTA. Document the rule so Contact and Medical History stop diverging.
4. Add open/close affordance to the accordion headers (chevron, Purple fill, 180° rotate on expand with `transition: transform 150–200ms cubic-bezier(0.23, 1, 0.32, 1)`) and define hover/focus/active states for the header as a pressable surface. Add `:focus-visible` ring (2px Purple offset 2px) for keyboard users on every Edit link.
5. Fix the non-concentric inner header radius (`4px` → `11px` to match `12px` outer minus `1px` border) and remove the `gap-[207.31px]` magic numbers by restructuring row layouts to `justify-content: space-between`. These two fixes remove the two most obvious "this came from a template" tells.
6. Reconsider the Account landing's job-to-be-done: a pharmacy account page that shows only demographics is flat. At minimum, surface the next shipment status, the active prescription count, and a route to Orders — the Peak-End rule says the first thing users see on a landing page defines how they remember the experience.
