---
title: "Design Critique: Portal MVP End to End"
status: draft
area: cross-cutting
author: "Digital Experience Critique (Claude)"
invoked_by: alexturvy
generated_by: gifthealth-digital-experience
created: 2026-04-24
last-updated: 2026-04-24
artifact: "https://www.figma.com/design/awfubM5IdTkKcvrHn2hW8C/test-temp?node-id=3895-7741&p=f&t=keU0OMWoZJ29ZMKb-0"
artifact_type: figma
reviewed_lenses: [patient-advocate, commercial-lens, craft-critic]
summary: "Portal MVP solid; placeholder copy, silent Canceled/pre-tracking states, clinician data on Rx, and single-tenant header all block ship."
top_heuristics: [N4, N5, N1, N6, N9]
top_biases: [bias-curiosity-gap, bias-zeigarnik]
scope: multi-lens
figma_write_blocked: false
revised_page_url: ""
---

# Design Critique: Portal MVP End to End

**Date:** 2026-04-24
**Artifact:** https://www.figma.com/design/awfubM5IdTkKcvrHn2hW8C/test-temp?node-id=3895-7741&p=f&t=keU0OMWoZJ29ZMKb-0
**Scope:** multi-lens
**Lenses:** patient-advocate, commercial-lens, craft-critic

## Summary

**Top 5 actions** (severity-ranked, cross-lens):
1. Strip placeholder template copy from every patient-facing surface — dashboard active-order card leaks `#{Order Number}`, `{Date}`, `{Date Range}`, `{Rx Details}`; checkout card leaks `{Rx Details}`. These read as system errors to a patient. — flagged by: Patient Advocate, Craft Critic
2. Fix the two silent-failure states — Canceled order (no refund timing, stale "Leave package at back door" Order Notes still rendering) and pre-tracking empty state (no pharmacy-review status line). These are the highest-risk call-drivers. — flagged by: Patient Advocate
3. Replace the hardcoded `LillydirectLogo` (2002:296) with a tenant-driven `PartnerLogo` slot; rename `gifthealth_lilly/*` design tokens to partner-agnostic `theme/*`. Ship a second-tenant storybook instance as proof. — flagged by: Commercial Lens
4. Unify navbar contract: Chat is present on the Account nav but missing on OrderDetails. Chat belongs on every surface where a patient considers abandoning — this is the containment OKR's load-bearing surface. — flagged by: Commercial Lens (dedup-related: Patient also asks for persistent self-serve help)
5. Strip clinician-grade data from Rx Details — Drug NDC, Doctor Fax, Doctor NPI, raw ICD-10 diagnosis codes. Translate diagnosis codes to plain-language condition names. Add tooltips on DAW. — flagged by: Patient Advocate

**Consensus:**
- Placeholder/template copy will leak to patients (Patient flags 5+ tokens on dashboard; Craft flags `{Rx Details}` in checkout card) — two lenses, same underlying production-hygiene issue
- Nav contract is inconsistent across portal surfaces (Commercial flags Chat present/absent split; Patient asks for persistent self-serve help affordance on every frame)
- Account page friction is real (Patient: only Shipping Address and Allergies have Edit links; Commercial: Personal Details accordion items read-only) — two lenses, different fields, same underlying rule-inconsistency
- Out-of-scope list underprioritizes risk: Patient (Cancel flow left in UI but marked out-of-scope creates silent handoffs), Commercial (pre-checkout cost visibility deferred vs. documented 80% non-fill driver), Craft (interaction states not specified pre-handoff)
- Status/stepper surfaces are strong where they exist (Patient praises persistent dashboard tracker; Craft approves mobile/desktop parity) but miss earlier pharmacy-review phase and miss `:active`/`:focus` states

**Tensions requiring human decision:**
- **Platform abstraction vs. v1 ship.** Commercial wants `LillydirectLogo` → `PartnerLogo` + token namespace rename NOW, before V1 locks. Craft treats Lilly as the governing brand and focuses on polish. Decision: do we pay the abstraction cost before V1 (short-term design lift) or eat re-skin debt when the second pharma lands?
- **Cost visibility scope.** Commercial wants pre-checkout cost visibility pulled into V1 scope as a Rx-Details-level card, pointing at the documented 80% non-fill driver. Patient doesn't explicitly name cost; Craft doesn't weigh in on scope. Decision: is the portal going to have anything to say to a cost-anxious patient at V1, or does that conversation defer until full payment methods land?
- **Depth of PII editability.** Patient wants Mobile/Landline/Email/Name/Gender/Birthday all patient-editable (visibility is agency). Commercial notes this should be per-program-configurable (future specialty programs may need identity-verification hooks). Decision: does V1 ship editable-for-all, or edit-configurable with stubbed affordances?

---

## Patient Advocate

**Top-line verdict:** This is mid-to-polish fidelity, and the shape is genuinely patient-first. The dashboard's persistent tracker answers Edward Liu's open "how does someone know they can come back to track shipping" question from the checkout-flow doc. But several patient-visible states silently fail the overwhelmed patient: the Canceled path has no reassurance, the Rx Details page leaks clinician-grade data, and the Account page forces a phone call for routine edits.

**Gauntlet stage(s) this touches:** Fulfillment (primary) and Persistence (refill awareness on dashboard). Upstream of Awareness, Prescription, and Approvals, which are handled before the portal.

**What's working:**
- The persistent dashboard card with embedded Processing / Shipped / Delivered tracker (node 2044:16252) gives patients a stable home for status. It closes the "tracking link is separate" gap the patient-checkout-flow doc flags.
- Processing-state copy "We're processing your order" + "We've received your order and are working on it" (nodes 2042:5495, 2042:5497) matches the calm, uncomplicated tone patients praised in the strategy doc verbatim quote.
- The Address Verification modal (node 3877:34885) catches bad-address failures before a shipment goes to the wrong place. Rare upstream fulfillment save.
- Default-state refill countdown "Your refill will be available in 18 days" with "We'll update you when it is available" (node 3877:20843) signals persistence without pressuring. Good Zeigarnik framing.
- The "Need Help?" triple CTA (node I2044:24717) gives FAQs, Chat, and Call as three visible exits.

**What's not:**
- **[Severity: content+design | node: 2309:18918]** Canceled order state gives the patient no reassurance or "what happens next." The order-info card (node 2309:18934) shows "Canceled April 18, 2025 / Ordered April 18, 2025" and nothing about refund timing, cancel reason, whether a new prescription is needed, or whether the patient can reorder. Worse, the Order Notes accordion (nodes 2309:18985 through 2309:18988) still renders "Leave package at back door," which reads like a package is still coming. Exactly the kind of silent gap that drives a Zendesk call. Fix: on node 2309:18934, add a TextNode child below the "Canceled April 18, 2025" row with "Your refund of $554.00 will appear in 5-10 business days on Visa ending in 5566" in 16px Regular; add a second short row "Need to reorder? Contact your prescriber for a new prescription." Hide the Order Notes accordion (nodes 2309:18981 to 2309:18988) when stage equals Canceled.
- **[Severity: content+design | node: 2309:19019]** The pre-tracking order state is cognitively empty and leaves the patient to fill the gap with anxiety (curiosity-gap bias). The Order Info card header shows only "Ordered April 18, 2025 / Estimated Delivery April 21, 2025" with a single "Email Receipt" button, and there is no visible status line explaining that the pharmacy is reviewing. LDZ swim-lane 4a tells us a pharmacist actually touches this order first; the patient has no way to see that. Fix: on node 2309:19034, add a status TextNode reading "Our pharmacy is reviewing your prescription. We'll text you the moment it ships" in 16px Regular, color #212529, placed below "Estimated Delivery April 21, 2025" (2309:19037).
- **[Severity: content | node: 3877:31395]** The Rx Details page leaks clinician-grade data directly to patients. Specifically: Drug NDC "00002015204" (node 3877:31484), Doctor Fax "+1 (614) 111-1111" (node 3877:31508), Doctor NPI "1234567890" (node 3877:31511), and worst of all the Diagnoses accordion (nodes 3877:31521 through 3877:31526) which renders "Primary Code / Value" and "Secondary Code / Value." A patient reading a raw ICD-10 code about their own obesity or metabolic diagnosis is at best confused, at worst alarmed; these are billing artifacts, not health information designed for the patient. The "Dispense As Written (DAW): Yes" row (node 3877:31493) has no patient-readable explanation. Fix: on node 3877:31515 Diagnoses accordion, replace the raw code display with plain-language condition names resolved from the code (e.g. "Type 2 diabetes with obesity"). Remove NDC, Fax, and NPI rows (nodes 3877:31482, 3877:31506, 3877:31509). Next to DAW on node 3877:31491, add a help tooltip reading "Your prescriber requested this exact medication, not a generic."
- **[Severity: usability | node: 3877:34896]** The Account page gives the patient agency over only two of the six fields shown. Mobile Phone (node 3877:34962), Landline (3877:34963), Email (3877:34964), Full Name (3877:34953), Gender (3877:34954), and Birthday (3877:34955) render an empty 26px link slot ("Link" placeholder with width 26px but no "Edit" text rendered). Only Shipping Address (3877:34965) and Allergies (3877:34972) show the "Edit" affordance. A patient who moves and changes their phone number must now call a PCR to update a number. That is a silent concierge-of-necessity, not a concierge-by-choice. Fix: on nodes 3877:34962, 3877:34963, 3877:34964 add a TextNode "Edit" link child in 16px Regular, color #0F3A85, matching the pattern on 3877:34965;4678:787.
- **[Severity: content | node: I2044:16252;2042:5497]** The dashboard's active-order Processing card will ship with literal template placeholders visible to the patient. The body renders "Your order #{Order Number} is being processed. This typically takes {1-2} hours" with bullets "Order Date: {Date}" and "Estimated Delivery: {Date Range}." The Checkout variant in the card-active-order component (node 2042:3506) likewise shows "{Rx Details}" lines twice. Patients will read curly braces as a system error. Fix: on node I2044:16252;2042:5497, bind the hash-braced tokens to real order data; on node 2042:3506, replace "{Rx Details}" prose with an actual prescription list component or hide the block when the list is empty.
- **[Severity: usability | node: 2042:5501]** The Processing-state card-active-order (node 2042:3498 when stage is Processing) renders a "Request Cancelation" link button (node 2042:5501). Project scope (nodes 3877:33854 and 3877:33856) explicitly lists "Cancel Order" as Out of Scope for MVP. If this button opens a Zendesk ticket with no in-product confirmation, the patient experiences a classic silent handoff: they click, feel they acted, nothing visibly changes, and they call support to ask whether anything happened. Fix: either remove node 2042:5501 for MVP, or, when tapped, render an in-card confirmation TextNode in 16px Regular reading "Cancel request sent. A pharmacist will call you within 2 hours" and disable further taps.
- **[Severity: content+design | node: 3877:15083]** The tracker has three stops: Processing, Shipped, Delivered. The real LDZ pharmacy workflow (swim-lane 4a) starts earlier with prescription receipt, indexing, and pharmacist review. Between "I placed an order" and "Processing turns bold," the tracker bar is entirely gray. For a patient who clicked through an SMS at midnight, that looks like nothing is happening. Fix: on node 3877:15083 add a leading fourth step "Prescription received" (same 50px circle, checkmark icon) so the bar has a bold anchor immediately after checkout, and propagate the same change to node 3877:15707 (mobile, 40px circle).

**Patient-visibility check:**
- Can a confused patient self-serve here? Partial. Dashboard, order details, and help links are strong. Account edits, Rx explanation, and the canceled/pre-tracking paths push the patient to the phone.
- Does this design surface status or hide it? Mixed. Tracker and dashboard surface beautifully. Pre-tracking, canceled, and "Request Cancelation" tap are hidden states.
- Where does friction compound downstream? In the Persistence stage. A patient who cannot self-edit a phone number will not update it; shipping-delay SMS misses; 30% pharmacy-stage abandonment gets a fresh contributor.

**Next actions:**
1. Fix the two silent-failure states first: Canceled (node 2309:18918) needs refund-timing copy and stale Order Notes hidden; pre-tracking (node 2309:19019) needs a visible pharmacy-review status line.
2. Strip clinician artifacts from Rx Details (node 3877:31395): remove NDC, Fax, NPI; translate Diagnoses codes; annotate DAW.
3. Add Edit affordances to Mobile, Landline, Email, Name, Gender, Birthday rows on the Account page (node 3877:34896).
4. Replace all templated placeholder tokens with bound data or conditional hides before ship.
5. Reconcile the "Request Cancelation" button (node 2042:5501) with the out-of-scope list.
6. Extend the tracker (nodes 3877:15083, 3877:15707) with a "Prescription received" first step.

---

## Commercial Lens

**Top-line verdict:** This is a well-scoped V1 for a single Lilly-branded instance, but the header, surface naming, and out-of-scope list together signal that the MVP is being built to the Lilly logo first and abstracted to a platform template later. That sequencing is backwards for the land-and-expand motion, and the shipping and payment gaps cede ground to Amazon/LillyDirect on the exact dimensions the strategy document names as competitive pressure.

**Commercial stakeholder(s) this touches:** End patient (Lilly program) primarily; Pharma client admin (Lilly brand team) via header/footer arrangement; Account Manager (no visibility surfaces observed); Program Manager / onboarding engineer (the template question).

**What's working:**
- The "Powered by gifthealth" lockup in the footer (I3877:34973) is the right white-label arrangement in principle — partner brand forward, Gifthealth as the infrastructure.
- Accordion-based IA for Account (Personal Details / Contact Information / Medical History at 3877:34947, 3877:34956, 3877:34966) is pattern-consistent and maps cleanly onto whatever fields the next manufacturer's program needs.
- Breadcrumb component (3109:11615) is documented as a reusable Bootstrap-based component — this is the kind of component-reuse discipline that makes multi-tenant scaling cheaper later.
- Order Details surfaces a complete receipt model (Medications / Order Summary / Shipping / Billing at 2309:18738, 2309:18761, 2309:18787, 2309:18795, 2309:18807).

**What's not:**
- **[Severity: content+design | node: 3877:34898]** The LillydirectLogo component (2002:296) is hardcoded as the navbar brand across every patient-portal surface in the MVP. Not a theme variable, not a slot, not tenant-keyed — it is a named "lillydirect_logo" asset baked into the shared Desktop frame (also at 3877:28511). Commercial lever: multi-tenant readiness and time-to-onboard. A second pharma partner cannot inherit this header in under 24 hours without a design and code fork. Fix: replace LillydirectLogo with a generic `PartnerLogo` component driven by a tenant config (logo asset, alt text, wordmark color) and ship a storybook instance that renders it with at least two partner brands before this MVP lands.
- **[Severity: content | node: 3877:33854]** The out-of-scope list defers "Manage Payment Methods" and "Showcase available rx, costs, etc before the checkout flow." Commercial lever: abandon-less outcome + Lilly renewal. The strategy doc names ~80% of non-fill patients citing cost as the blocker and names Walmart and LillyDirect as the specific destinations patients are lost to at checkout. A portal MVP that ships with no payment-method management and no pre-checkout cost visibility is a portal that cannot respond to the #1 documented reason patients abandon. Fix: move "pre-checkout cost visibility" into scope as a read-only card on the Rx Details surface (3877:31395) even if checkout itself stays out of scope.
- **[Severity: content+design | node: 2309:18799]** The only shipping method surfaced is "UPS Standard Shipping" priced "Free." No tier, no expedited option, no same-day cue (also visible in 2309:18801). Commercial lever: competitive positioning against Amazon + LillyDirect's same-day Zepbound launch. Shipping a Lilly-branded portal for Zepbound patients with no visible path to faster delivery is a competitive giveaway on the exact SKU where the competitive gap is most visible. Fix: design the Shipping Method card as a multi-row component (method | ETA | price) so when same-day lands it's a data addition, not a redesign. Add a placeholder "Expedited shipping coming soon" row if product cannot commit to a date.
- **[Severity: content | node: 3877:34896]** The Account navbar (3877:34900) includes a "Chat" entry; the OrderDetails navbar (3877:28513) does not. Commercial lever: digital containment OKR and AI-agent transfer-rate OKR. Chat is the containment surface that keeps a patient from phoning the call center. If Chat is not persistently available on the moment-of-truth surface (the order page), the portal is asking that patient to navigate away to get help. Fix: Chat belongs in the persistent navbar across all patient-portal surfaces. Pick one nav contract and apply it.
- **[Severity: content | node: 2309:18732]** "Estimated Delivery April 21, 2025" is shown as static text with no refresh, no "updated X minutes ago," no delay-notice affordance (also 2309:18733). The out-of-scope list (3877:33854) explicitly defers "Shipment Delay Notice Banner." Commercial lever: CSAT, contact-rate OKR, and the patient-reported "calm, thoughtfully designed" differentiator. Fix: reserve a slot in the Order Info card (2309:18733) for status freshness ("Updated 2 hr ago") so the UI accommodates the eventual delay-notice banner without rework.
- **[Severity: content | node: 3877:34947]** Personal Details surfaces Full Name, Gender, Birthday as read-only — no Edit affordance on those rows (items 3877:34953, 3877:34954, 3877:34955), while Contact Information rows and Allergies have Edit links. Commercial lever: time-to-onboard for future programs. A future oncology or specialty program may require these fields to be editable with identity-verification hooks; shipping read-only in V1 with no hook makes the next program a bigger build. Fix: decide now whether edit behavior on PII fields is per-program-configurable, and stub the affordance (even disabled) so the template carries the pattern.

**Platform-vs-partner check:**
- Is this platform-native or partner-specific? **Partner-specific.** The LillydirectLogo is hardcoded in the shared Desktop navbar; every inspected desktop surface renders it. Design system beneath it looks platform-capable (accordion, breadcrumb, button are generic), but the top-level composition is branded.
- Can a new pharma partner adopt this in < 24 hours? **No.** Swapping the logo requires editing a named component (2002:296 LillydirectLogo), and nothing suggests a tenant-switch mechanism. The `gifthealth_lilly/White` and `gifthealth_lilly/Geyser` design tokens are namespaced per-partner — color system is not yet multi-tenant.
- Where does this cede ground? **Amazon + LillyDirect on delivery speed**; **Walmart on pre-checkout cost visibility**; **LillyDirect on portal parity** (shipping a Lilly-branded portal materially thinner than LillyDirect's own is a renewal risk).

**Next actions:**
1. Before this MVP locks, make the header tenant-driven: replace the hardcoded LillydirectLogo with a `PartnerLogo` slot and prove it with a second partner brand.
2. Rename the "gifthealth_lilly/*" design token namespace to a partner-agnostic token set.
3. Reopen the out-of-scope list (3877:33854) against the 80%-cost-abandonment finding. Add pre-checkout cost visibility on Rx Details (3877:31395) to V1 scope.
4. Add reserved slots in Order Info (2309:18733) and Shipping Method (2309:18801) for future delay-notice and expedited-tier.
5. Unify navbar contract across Account (3877:34900) and OrderDetails (3877:28513) so Chat is persistent everywhere.

---

## Craft Critic

**Top-line verdict:** The work is in strong shape for a production-bound co-brand surface — the token system is being used cleanly, mobile and desktop parity reads as intentional, and information hierarchy is serviceable. Polish gaps are concentrated in content-bound details (placeholder copy that must not ship, weak link affordance, split attribution lock-up) rather than structural craft problems. Confidence: high on the sub-frames pulled via design-context and variable-defs; root metadata exceeded token cap (inspection constraint, noted here per 1.5.2 rules — not emitted as a severity bullet).

**What's working:**
- **Token discipline is real.** Variable defs on Checkout Available and Order Details resolve to a genuine semantic token set (`Theme/Border #E4E7EB`, `Theme/Secondary #6C757D`, `Theme/Warning #FF8D6B`, `Body Text/Body Color #212529`). No raw-hex bleed visible in sampled surfaces.
- **Active-order stepper has mobile/desktop parity.** The Processing/Shipped/Delivered indicator (node 3877:31007 desktop, mirrored mobile) reads at both 856px and 390px without losing legibility or state clarity.
- **Mobile nav pairs hamburger icon with "Menu" text label** (node 3877:32785) — recognition over recall (Nielsen 6) and a signifier improvement over icon-only.
- **Rejected-scripts alert ties to a real Zendesk article**, not a dead-end "Learn More" (node 3877:34586 href). Nielsen 10 implemented, not stubbed.

**Brand compliance (scoped — Lilly white-label with "Powered by gifthealth" attribution):**
- Gifthealth mark usage: Pass — purple on light ground at legible size, meets 20px minimum.
- Attribution lock-up: **Fail** — "Powered by" text is horizontally split from the gifthealth mark in desktop footers (nodes 2307:19558 and 2307:19635).
- Internal canvas artifacts (Notes frames 2307:19575, 2307:19655): not inspected at font level; flag for Gifthealth author self-check — should use Funnel Display.
- Lilly-owned palette and type: out of scope for Gifthealth brand rules. IBM Plex Sans and #E1251B are Lilly's call.

**Heuristic pass (at-risk only):**
- **#4 Consistency and standards** — "Learn More" (3877:34586) renders in body color #212529 with only underline; other link affordances need to match or user learns two rules.
- **#5 / #9 Error prevention / recovery** — alert pattern (3877:34579, 3877:30928) uses only a 4px `#FF8D6B` left stripe as differentiation signal. Color-vision-deficient users or bright-sunlight glances will miss it. Needs a leading icon.
- **#8 Aesthetic and minimalist design** — clean overall; cards not over-decorated.

**Interaction quality:** No interaction states shown on inspected frames. Per Kowalski's "buttons must feel responsive": primary CTA and "View Details" secondary need `:active` specs (scale 0.97, 160ms ease-out) documented before handoff or they ship with default browser behavior. `Small Shadow` token is `0px 2px 4px rgba(0,0,0,0.08)` — serviceable but flat; a layered two-stop shadow would lift cards off the #F5F6FB body background more convincingly on retina. Button transition/easing unspecified.

**What's not:**
- **[Severity: content | node: 2307:19491]** Placeholder copy `{Rx Details}` shipping inside the checkout card bullet list — Nielsen 8 and basic production hygiene. Either swap to real drug/dose format ("Zepbound 2.5MG/0.5ML — 6 fills left") or constrain the card to a single summary line ("3 prescriptions ready for checkout"). Confirm strings are wired to data before handoff.
- **[Severity: content+design | node: 3877:34586]** "Learn More" link has no color affordance — Nielsen 4 and the signifier principle. Underline alone against body copy color #212529 reads as emphasized text, not as an interactive link. Introduce a `Theme/Link` token (distinct blue or the Lilly secondary blue that's already used in the "View Details" outline button) and apply here and in "View All" (nodes 3877:34364, 3877:34408).
- **[Severity: content+design | node: 3877:34579]** Alert signals differentiation only via a thin left stripe — Nielsen 5/9 and WCAG 1.4.1 (Use of Color). The 4px `#FF8D6B` border on the left edge is the only indication this block is an alert, not a neutral card. Add a leading AlertTriangle icon at 20px inside the alert container before the title text, optionally colored with `Theme/Warning`. Keeps the alert readable regardless of color perception.
- **[Severity: content+design | node: 2307:19558]** "Powered by gifthealth" attribution lock-up is split horizontally in desktop footers (also 2307:19635) — Gifthealth Brand Guidelines clear-space rule. The "Powered by" text sits in the bottom-right while the gifthealth mark sits bottom-center; they should read as a single attribution unit. Consolidate into a right-aligned `Powered by [gifthealth mark]` lock-up with the mark at 20px minimum and clear-space equal to 50% of the "h" height on all sides.
- **[Severity: usability | node: 2307:19493]** Primary CTA contrast is at the AA floor, not above it — WCAG contrast. White on Lilly red `#E1251B` at 14px Medium measures approximately 4.68:1 — passes AA (4.5:1 floor) but fails AAA (7:1) and sits close enough to the AA threshold that any rendering variance can push it under. Options: darken the red fill to around `#C5211A` to buy headroom, bump the button text to Semibold at 14px, or increase text to 15px Medium. Pick one — don't ship at the boundary.
- **[Severity: usability | node: 2307:19493]** No `:active` / `:hover` / `:focus-visible` states specified on interactive elements (also applies to "View Details" outline button pattern at 3877:34367 / 3880:13777 / 3880:13805) — Kowalski's responsiveness principle, WCAG 2.4.7 focus visible. Annotate each button variant with press and focus specs before engineering defaults to browser behavior.

**Next actions:**
1. Replace every `{Rx Details}` placeholder with the production string pattern before handoff.
2. Introduce a `Theme/Link` token and apply to "Learn More" and "View All".
3. Add a leading warning icon inside alert containers; keep the 4px left stripe as secondary signal.
4. Consolidate "Powered by gifthealth" attribution into a single right-aligned lock-up in desktop footers.
5. Darken primary button fill or bump text weight to move contrast above the AA floor with headroom.
6. Add an Interaction States page documenting press/hover/focus specs for primary red and secondary outline buttons before engineering handoff.
