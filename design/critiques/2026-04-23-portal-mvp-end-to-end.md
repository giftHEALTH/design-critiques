---
title: "Design Critique: Portal MVP End to End"
status: draft
area: cross-cutting
author: "Digital Experience Critique (Claude)"
invoked_by: alexturvy
generated_by: gifthealth-digital-experience
created: 2026-04-23
last-updated: 2026-04-23
artifact: "https://www.figma.com/design/awfubM5IdTkKcvrHn2hW8C/test-temp?node-id=3895-7741&p=f&t=seCTlC9HWSZhpGcT-0"
artifact_type: figma
reviewed_lenses: [patient-advocate, commercial-lens, craft-critic]
summary: "Portal MVP shape is right but hardcodes Lilly branding, defers cost and transfer, and leaves error states with hidden support."
top_heuristics: [N1, N4, N6, law-von-restorff, law-goal-gradient]
top_biases: [bias-zeigarnik, bias-choice-overload]
scope: multi-lens
figma_write_blocked: false
revised_page_url: ""
---

# Design Critique: Portal MVP End to End

**Date:** 2026-04-23
**Artifact:** https://www.figma.com/design/awfubM5IdTkKcvrHn2hW8C/test-temp?node-id=3895-7741&p=f&t=seCTlC9HWSZhpGcT-0
**Scope:** multi-lens
**Lenses:** patient-advocate, commercial-lens, craft-critic

## Summary

**Top 5 actions** (severity-ranked, cross-lens):
1. Fix the home-screen progress tracker (active step fill, completed checks, pending outlined, 2px connecting track) — highest-leverage anxiety-reduction signal for a waiting patient — flagged by: Craft Critic, Patient Advocate
2. Pull cost / coverage / copay visibility into v1 (even a read-only "your price" chip) — the documented ~80% non-fill driver — flagged by: Patient Advocate, Commercial Lens
3. Make the chat/support affordance visible on every error and delay state (rejected Rx, processing-with-service-delay, shipment delay, expired checkout); prioritize one channel in Need Help rather than three equal-weight pills — flagged by: Patient Advocate, Craft Critic
4. De-partner the brand layer: rename `LillydirectLogo` to a generic `PartnerLogo` slot, collapse `gifthealth_lilly/*` tokens into tenant-agnostic `theme/*`, bind every `#0f3a85` affordance to `Theme/Primary` (whatever the tenant's value is) — flagged by: Commercial Lens
5. Rewrite the rejected-Rx surface to name the specific reject reason from the known enum + last-updated timestamp + next-follow-up, and narrate the out-of-refills handoff as a tracked portal item rather than a bare "reach out to Dr." — flagged by: Patient Advocate

**Consensus:**
- The MVP is coherent as a Lilly-specific patient portal but builds concentration risk into the foundation — Commercial is sharpest on it, Craft notes `Theme/Primary` is wired but unused, Patient implies it via tenant framing
- Deferred items (cost, transfer, cancel, shipment-delay banner) are not "nice-to-have v2" — they are the exact moments patients abandon to Walmart / LillyDirect / Amazon and the exact moments that generate PCR tickets
- Error and delay states are the lowest-visibility states on the canvas; chat is hidden on every non-auth screen. Silent failure modes dominate where patient anxiety is highest
- Patient-facing surfaces leak clinical fields (NDC, DAW, ICD codes on Rx Details) without glossing — audience is ambiguous
- The MVP has zero surfaces for the pharma buyer, Account Manager, or Program Manager — the audience funding ~90% of revenue sees nothing from this portal

**Tensions requiring human decision:**
- **Bind to `Theme/Primary` (purple on Gifthealth, blue on Lilly) vs. repaint Lilly buttons purple.** Commercial's `fill=#440B39` revision is a literal repaint of a Lilly-tenant button, not a token binding; Craft notes blue is intentional tenant theming. Decision: does the revised-frame clone exist to show "this should bind to the token" (in which case a purple Lilly button is the right teaching artifact) or to preview a ready-to-ship Lilly surface (in which case the revision is wrong)?
- **Patient-facing vs. internal Rx Details.** Patient says NDC/DAW/ICD are over-disclosed for patient view; Commercial says if internal, the LillyDirect branding is an even sharper concentration tell. Decision: is `Rx Details` (3877:31395) a patient surface, an internal operator surface, or both? The answer changes what gets hidden vs. glossed.
- **Scope of v1 vs. what's "deferred."** Patient wants price, shipment-delay, chat-on-error pulled in now; Commercial wants cost, transfer-retention, admin view; Craft wants tracker polish and state-aware CTAs. The current v1 scope is narrower than any of the three lenses think it should be. Decision: which items graduate from "Out of Scope / Future" to v1 before ship?

---

## Patient Advocate

**Top-line verdict:** Mid-fidelity foundational — this is the birth of a portal that didn't exist before (research confirms "Patient Portal — Doesn't Exist Yet"), and the shape is right. The gauntlet stage this most directly addresses is Fulfillment, and most of what's visible on the happy path is working. The risk is that several deferred items and silent states determine whether this portal reduces abandonment or just relocates the failure point.

**Gauntlet stage(s) this touches:** Approvals (rejected-Rx surface), Fulfillment (processing / shipment / delivery / refill), Persistence loop (out-of-refills handoff).

**What's working:**
- The processing-state card sets a concrete expectation ("typically takes 1-2 hours") and pairs it with a three-step Processing → Shipped → Delivered indicator. That's goal-gradient and feedforward working in the patient's favor — they can see where they are in line.
- A persistent "Need Help?" triad (FAQs / Live Chat / Call Support) is present across every key screen. Patients who panic have three on-ramps, not one.
- Separate Rx List and Order List entry points — patients think in two different mental models ("my medication" vs "my package"), and the portal respects both.
- Breadcrumbs on deep pages, language selector in the top nav, and a clear welcome-by-name pattern. These are small but they signal "you are oriented."
- The v1-v5 scope ladder in the overview frame is explicit and honest. The designer is telling reviewers what's deferred and in what order, which is how to have a useful conversation about this work.

**What's not:**

- **[Severity: content+design | node: 3877:33854]** Pricing is deferred out of scope — "Showcase available rx, costs, etc before the checkout flow" lives on the Out of Scope / Future list. The strategy doc states ~80% of non-fills are cost-driven. The portal's single highest-leverage lever for reducing abandonment at fulfillment is letting the patient see cost before they commit emotionally to a checkout click. Deferring this preserves sticker-shock abandonment at the counter — which is exactly the 30% fulfillment-stage dropout the strategy calls out. Recommend pulling a read-only "price you'll pay" chip onto the Rx card in v1 even before full payment management ships.

- **[Severity: usability | node: 2307:19468]** The floating chat instance is set `hidden="true"` on essentially every state screen (Order List, Rx List, Checkout Available, Processing, Shipment, Delivery, Order Details, Rx Details, Pending Rx). It only appears visible on the Auth SMS/Landline screens. That inverts the patient need curve: a confused patient on the Rejected Rx or Processing-with-Service-Delay screen is in the highest-friction moment and has the lowest-visibility escape hatch. The "Need Help?" triad lives in the footer — that's below the fold for the very states where patients are most likely to abandon.
  **Revised:** visible=true

- **[Severity: content | node: 3877:30935]** The rejected-Rx banner reads "There was an issue with one or more of your prescriptions. We've notified your healthcare provider and shared the specific issue that needs to be remedied" + a "Learn more" link that opens a generic Zendesk article. The canvas working-note shows the actual reject-reason enum (`wrong_diagnosis`, `no_hcp_signature`, `wrong_quantity`, `wrong_strength`, `invalid_sig`, etc.) is known per-script. Patients are overwhelmed, not stupid — they can act on "your prescriber didn't sign the prescription; we've requested a corrected signature on April 23" in a way they cannot act on "there was an issue." The generic copy creates a Zeigarnik open loop (an unresolved task that the patient keeps thinking about) with no closure signal — no date stamp, no follow-up-by estimate, no "last notified your provider on X." Surface the specific reason, add a timestamp, and show when Gifthealth will follow up next.

- **[Severity: content+design | node: 2279:8573]** The v1 scope line "Out of refills, reach out to Dr. (no timeline)" is a bare handoff, and bare handoffs are the canonical patient-visibility failure. The portal is telling the patient to go do something, offstage, alone, with no status afterward. The strategy's own framing is that "concierge-feeling moments build trust; visible handoffs break it." Recommend inverting — the portal requests the refill on the patient's behalf and shows status ("Refill request sent to Dr. Hurn on April 23 — we'll follow up in 3 business days"). Even if the actual backend is "patient-initiated," the portal can narrate it back as a tracked item. This is the same pattern as the rejected-Rx case: narrate the handoff, don't abandon the patient to it.

- **[Severity: content+design | node: 3877:33854]** "Shipment Delay Notice Banner" is deferred. This is the definition of a silent failure — the delivery slips, nobody tells the patient, the patient calls support, the contact-rate OKR moves the wrong way. Patients who don't know their package is delayed often re-order through a competitor (the cross-cutting patient-checkout doc explicitly names Walmart and LillyDirect as exits). A status banner is not a nice-to-have for v2; it's the difference between "we lost a day" and "we lost a patient."

- **[Severity: content+design | node: 3877:33854]** "Transfer Scripts" and "Cancel Order" both deferred. A patient who wants to cancel or move a script and cannot self-serve will either call support (contact-rate OKR cost) or exit quietly (fill-rate OKR cost). Worth acknowledging explicitly: the patient self-serve gap is the PCR ticket volume gap. The research doc's PCR swim-lane already documents "Help Cancel an Order" as one of the six most common PCR-handled issues — the portal is choosing, for now, to keep that volume with PCRs.

- **[Severity: content | node: 3877:22461]** The Rx Details page exposes clinical fields that read as patient-facing but are not: "Drug NDC", "Dispense As Written (DAW)", "Primary Code / Secondary Code" (ICD-10). This is curse of knowledge — the page appears to have been adapted from a pharmacist view. A patient doesn't know whether DAW = Yes is good or bad for them, and the diagnosis codes require a decoder ring. Either hide these for the patient view or pair each with a one-line plain-language gloss.

- **[Severity: usability]** (flow-level, not anchorable) The overview frame itself lists "Copy writing is pretty rough in a number of areas. Unhappy path messaging" as an Open Question. Just naming it: the unhappy paths are where this portal earns or loses its patient-outcome impact. Prioritize the error-state copy pass above the happy-path polish pass — if the processing card succeeds and the rejected card fails, the patient is worse off than before because now they know there's a problem and have nowhere to go.

**Patient-visibility check:**
- Can a confused patient self-serve here? **Partial** — yes on "where is my order" (processing/shipped/delivered is well-surfaced); no on "why was my script rejected" (generic copy, link-out); no on "how do I cancel or transfer" (deferred); no on "my dose changed, now what" (not modeled).
- Does this design surface status or hide it? **Mixed** — fulfillment status is surfaced clearly and is a real upgrade over the current state where patients had no portal at all. Rejection status, refill-request status, and delay status are hidden or offstage. Patients will come to rely on this portal for "where is my shipment" and then discover it goes quiet exactly when it most needs to speak.
- Where does friction compound downstream? The rejected-Rx generic copy pushes the patient into a phone call with the prescriber's office or a support call to Gifthealth, both of which fail at a far higher rate than a portal-native "here's the specific issue + here's what we're doing about it." The strategy doc's patient-quote exhibit — "My doctor submitted the same prescription 8 TIMES before acceptance" — is exactly the failure this screen is positioned to prevent, and in its current copy it doesn't.

**Next actions:**
1. Pull **price visibility** from Out of Scope into v1, even as a read-only chip on the Rx card. The 80% non-fill reason is cost; hiding it until checkout preserves the single biggest abandonment lever in the gauntlet.
2. Make the **ambient chat FAB visible on error/delay states** (Rejected Rx, Processing-with-Service-Delay, Failed Delivery, Expired Checkout). Highest-friction states should have the lowest-friction escape hatch, not the reverse.
3. Rewrite the **rejected-Rx surface** to name the specific reject reason from the enum, add a last-updated timestamp, and show the next follow-up step ("we'll re-check with Dr. Hurn on April 25"). Close the Zeigarnik loop.
4. Reframe the **out-of-refills handoff** so the portal narrates the request back to the patient, even if the action is patient-initiated. "Refill requested on [date] — you can view status here" beats "reach out to your doctor."
5. Promote **Shipment Delay Notice Banner** from Future to v1. Silent delivery slips push patients to competitor pharmacies and drive support volume simultaneously.
6. Do a **patient-voice copy pass** on Rx Details — either hide NDC / DAW / ICD codes on the patient view or pair each with a plain-language gloss.

---

## Commercial Lens

**Top-line verdict:** This MVP is functionally coherent as a patient portal for one pharma partner, but it is built as a LillyDirect product, not a Gifthealth OS instance — branding, tokens, and the explicit "base template can be different for now" note all hard-wire concentration risk into the foundation. The portal also has no answer to the #1 patient dropout reason (cost) or to the "lost to Walmart / LillyDirect" moment, and it exposes nothing to the pharma buyer whose renewal funds most of the business.

**Commercial stakeholder(s) this touches:** End-patient (primary surface), pharma client admin (invisible — no surface), Account Manager / Program Manager (no visibility), next-pharma-prospect (this is a reference build they will see).

**What's working:**
- The underlying primitives are genuinely platform-shaped: generic Bootstrap-documented `breadcrumb` (3109:11615), `button` (2002:1667), `pagination` (2031:9658), `accordion-item` components, plus IBM Plex Sans and a separate abstract `Theme/Primary`, `Theme/Secondary`, `Theme/Border` token slot. The scaffolding to run white-label exists — a land-and-expand future is reachable from here.
- `Rx Details` (3877:31395) has a clean data shape — Patient / Prescription / Doctor / Diagnoses accordions with drug NDC, NPI, DAW flags — that maps cleanly to any manufacturer's hub data model without re-thinking.
- Mobile and desktop variants exist for every major screen (Orders, Rx List, Rx Details, Account), so responsive parity is not a remediation debt.
- The "Powered by gifthealth" footer chip (e.g., `2041:3499`, `3877:34973`) is a legitimate co-brand choice for a partner-branded consumer program.

**What's not:**

- **[Severity: usability | node: 2002:296]** Partner logo is a hardcoded `LillydirectLogo` component (reused at 3877:28511, 3877:34898, 3877:31397, and every screen's navbar) rather than a themed `partnerLogo` / `brandMark` slot. This is platform vs. partner-specific made literal: re-skinning for the next manufacturer is not a token swap, it's a component rename plus asset replacement across every Figma screen. Affects **multi-tenant readiness** and **time-to-onboard**. Fix: rename the component to a generic brand slot and parameterize by partner theme.

- **[Severity: content+design | node: I2041:3635;1:13133]** Primary brand color `#0f3a85` is applied as a raw hex literal on the active pagination chip, breadcrumb links, Orders card "View Details" buttons (e.g., `I2044:24333;3877:24604`), and "Edit" affordances on Account — while a separate `Theme/Primary` token (`#440B39`) exists in the style library and is unused here. Re-branding to the next pharma partner is find-and-replace, not a theme pivot. Affects **time-to-onboard** (< 24h OKR) and **multi-tenant readiness**. Fix: bind every brand-colored affordance to `Theme/Primary`.
  **Revised:** fill=#440B39

- **[Severity: content+design | node: 3877:34896]** The Account screen's token set lists `gifthealth_lilly/White: #FFFFFF` and `gifthealth_lilly/Geyser: #DEE2E6` — partner-named tokens inside the shared style system. This is exactly the concentration move the strategy-and-vision document forbids: capabilities built in a form that only the Lilly tenant can inherit cleanly. Affects **multi-tenant readiness** and **concentration risk**. Fix: collapse `gifthealth_lilly/*` into tenant-agnostic tokens (`theme/white`, `theme/border`).

- **[Severity: content | node: 2279:8573]** The 4/28 scoping note explicitly reads "Base Template - Can be different for now" under "v1 Foundation." The partner-agnostic principle is openly being deferred in plain language. This is the right moment to challenge that — deferred abstraction has a way of calcifying into partner-specific rewrites when the second manufacturer arrives. Affects **land (next pharma)** and **Lilly renewal** (renewal is easier when Lilly sees platform reuse, harder when they see bespoke lock-in). Fix: rewrite v1 to use a minimal default template that works for any tenant; don't let "for now" ship.

- **[Severity: content | node: 2279:19150]** The stated "Portal MVP Supported Features" list includes Tracker, Order Status, Rx List, Order History, Help Links, Language Selector, Manage Account — but no cost, copay, coverage, or price-transparency surface. Per the strategy doc, ~80% of December 2025 non-fills are cost-driven. The portal cannot help the single largest abandonment driver. Affects **abandon-less outcome** and **expand revenue** (a cost-surfacing portal is a sellable tier; a silent one isn't). Fix: add a minimum-viable "your price / your coverage" surface or a placeholder module pre-wired for Phase 2 copay content.

- **[Severity: content | node: 3877:33854]** "Transfer Script" is explicitly listed as Out of Scope / Future. That is precisely the moment the operational docs name as "lost to Walmart / LillyDirect" — and the portal has no retention, re-engagement, or "stay with us" surface at it. A patient who hits this gap today walks to Amazon same-day. Affects **competitive positioning** (LillyDirect, Walmart, Amazon) and **persist-longer outcome**. Fix: add a retention surface (even a "we can help transfer back" or "here's why staying is faster" moment) before v1 ships; if not, name it as a known competitive giveaway.

- **[Severity: content | node: 3877:34942]** The MVP is 100% patient-facing. There is no Account Manager / Program Manager / pharma admin view, no program-health summary, no container for surfacing what the LillyDirect program is delivering. The buyer whose renewal funds ~90% of revenue sees nothing of their program's health from this portal and no hook for expand-motion conversations. Affects **Lilly renewal** and **land (proof-of-platform to next prospect)**. Fix: commit a Phase 2 "pharma admin / program manager" surface to the roadmap, even if v1 only exposes a read-only dashboard.

- **[Severity: usability | node: I2044:24333;3877:24604]** Order card CTAs still read literal placeholder text "Button Title" on every cancelled/delivered card in the Orders list. Minor craft issue, but this file is the reference surface that will be demo'd — worth cleaning.
  **Revised:** text="View Details"

- **[Severity: content | node: 3877:31395]** `Rx Details` displays patient ICD/diagnosis slots ("Primary Code: Value", "Secondary Code: Value"), full NPI, Drug NDC, and DAW status. If this screen is patient-facing, those fields are over-disclosed; if it's intended for an internal or prescriber audience, the LillyDirect-only branding on an internal surface is an even sharper concentration tell. Clarify audience before shipping.

**Platform-vs-partner check:**
- Is this platform-native or partner-specific? **Partner-specific.** Logo component is literally named `LillydirectLogo`; tokens are literally namespaced `gifthealth_lilly/*`; the scoping note says base template can differ per partner.
- Can a new pharma partner adopt this in < 24 hours? **No.** Re-skinning requires replacing a named logo component across ~20+ screens, find-and-replacing `#0f3a85` hex literals, renaming tokens, and re-scoping a "base template" that the team has already agreed can be partner-specific. That is days of design work, not a config swap.
- Where does this cede ground to a named competitor? **LillyDirect, Walmart, Amazon:** the portal has no cost surface (the #1 non-fill reason), no script-transfer / retention surface at the moment patients leave, and no same-day-delivery framing to counter the Amazon + Zepbound KwikPen benchmark. **Phil:** the only other Hub+Pharmacy — if they ship a white-labeled portal faster than Gifthealth, the MVP's partner-branded default becomes a competitive disadvantage at the pitch.

**Next actions:**
1. Rename `LillydirectLogo` (2002:296) to a generic `PartnerLogo` / `brandMark` slot; collapse `gifthealth_lilly/*` tokens into tenant-agnostic `theme/*`; bind every `#0f3a85` affordance to `Theme/Primary`. This is the single highest-leverage change and unblocks the < 24h onboarding OKR at the primitive level.
2. Reverse the 4/28 "base template can be different for now" decision and require v1 to ship on the white-label template. Every shortcut taken here becomes a custom-build debt against the next pharma logo.
3. Add a cost / coverage surface to the Portal MVP feature list (even a read-only "your copay is $X / covered through [plan]" module), so the portal speaks to the documented #1 abandonment reason.
4. Commit a Phase 2 pharma-admin / program-manager surface to the roadmap so the buyer funding ~90% of revenue (and the next logo the commercial team is pitching) sees program health, not just patient orders.
5. Decide whether `Rx Details` (3877:31395) is patient-facing or internal; if internal, separate it into a clearly-labeled operator surface to avoid the confusion of a LillyDirect-branded clinical-data screen leaking into patient flows.

---

## Craft Critic

**Top-line verdict:** This is a **Lilly Direct tenant surface** (Gifthealth appears in the footer only — "Powered by gifthealth"), so Gifthealth brand-compliance checks largely do not apply and the Lilly red wordmark, the dark-blue primary buttons, Inter body type, and the serif heading face are not graded against Gifthealth Purple / Funnel Display — those appear to be intentional tenant theming. The shape of the MVP is conventional and sensible; the craft cost is concentrated in Nielsen 4 (Consistency) and visibility-of-status gaps that compound across ~100 frames, plus a sub-bar Gifthealth footer lockup that is the one place Gifthealth's own brand rules still apply.

**Context flag:** Because this is a co-branded/tenant surface and mostly conventional patient-portal chrome (list, detail, account), the craft lens is useful but not the deciding one. Pair this with a **patient-advocate** pass (clarity of prescription and order status for someone anxious about a refill) and a **commercial-lens** pass (what Lilly expects from this portal as the client) for fuller coverage.

**What's working:**
- **`Theme/Primary` token = `#440B39`** is defined in the variables, which means the underlying design system is tenant-themable — Gifthealth Purple is wired in even though Lilly's dark blue is the rendered primary on this surface. That is exactly how a multi-tenant system should be structured.
- **Progress tracker concept** on the home screen (Processing → Shipped → Delivered) is the right pattern for patient anxiety reduction — it places the user on the journey the moment they land (Nielsen 1, Visibility of system status; Goal-Gradient Effect).
- **Clear primary information hierarchy** on the order card: the status verb ("Delivered," "Canceled") is bolded while the order number and ordered-date are de-emphasized. That matches how a patient actually scans — status first, identifier second.
- **Breadcrumbs are present** on secondary pages (Home › Account, Home › Orders) which preserves Nielsen 3 (User control and freedom) and Nielsen 1 for location awareness.
- **Need Help? block** on home places FAQs / Live Chat / Call Support as sibling CTAs rather than burying any of them — good respect for the fact that different patients have different comfort thresholds for support channels.

**Brand compliance (Gifthealth-owned elements only):**
- **Palette:** N/A for tenant chrome (Lilly-themed, expected). Gifthealth footer wordmark renders in the approved purple family, which is correct.
- **Typography:** N/A for tenant chrome (Inter is tenant-appropriate). Gifthealth footer lockup does not obviously use Funnel Display, but at its rendered size the face is essentially indistinguishable — acceptable for a footer wordmark context.
- **Gradients:** N/A — no gradients on these surfaces.
- **WCAG contrast:** The Lilly dark blue against white has not been verified programmatically; the pairing reads as AA-passing at the button sizes shown, but `Theme/Secondary: #6C757D` (a gray) against white used for the "Showing 10 of 100 orders" meta text sits close to the 4.5:1 threshold on small body. Worth a runtime check.

**Heuristic pass (Nielsen's 10):**
- **Consistency and standards (4):** AT RISK. Three distinct issues called out below (breadcrumb trail, edit-affordance asymmetry on Account, View Details label repeating on every state).
- **Visibility of system status (1):** AT RISK. The Processing/Shipped/Delivered tracker on home does not visually communicate which step is active vs. complete vs. pending — all three nodes appear as equal-weight circles with no connecting track between them.
- **Recognition rather than recall (6):** AT RISK on the Orders list. Two rows of dense text with identical CTAs requires the user to re-read each row to distinguish "Delivered" from "Canceled" rather than recognizing it at a glance (also engages Law of Similarity against the user).
- All other heuristics pass on the samples reviewed.

**Interaction quality:**
- Interaction specifics (easing curves, durations, `:active` feedback, transition properties) are not inspectable from a static Figma canvas. Interaction build quality cannot be graded here. Flag for the engineering partner: specify per-property transitions, use `ease-out` on the Rx and Order detail drawer/modal entries if any, cap UI durations at ~200 ms, add a `scale(0.97)` `:active` state on the primary and "View Details" buttons.
- Suggestion for the home tracker: when implemented, stage the transitions — the active-step ring pulses on first paint, the connector line animates from the previous step to the active step with `ease-out` ~250 ms. This is a rare surface (a patient sees their tracker a few times per order cycle), so a small delight is justified here in a way it would not be on, say, the header chrome.

**What's not:**

- **[usability | node: 2039:203920]** Home-screen progress tracker does not show progression. Three equal-weight circles labeled Processing / Shipped / Delivered with no connecting track and no visual differentiation between the active, completed, and pending states. Violates Nielsen 1 (Visibility of system status) — this is the single most important signal on the home screen for an anxious patient awaiting a prescription, and it currently tells them almost nothing. Fix: active step gets the solid Lilly-primary fill with a white glyph; completed steps get a filled check; pending steps stay outlined; connect them with a 2px track that fills from one node to the next as state advances.

- **[content+design | node: 2031:3747]** Breadcrumb on the Orders list includes an Rx number. The trail reads "Home › Orders › Rx #1234567" on a page that shows the *list* of orders, not a specific prescription. Violates Nielsen 4 (Consistency) and Nielsen 2 (Match between system and the real world) — the trail should match the user's actual location. Fix: on this page, trail should be "Home › Orders" only.
  **Revised:** text="Home › Orders"

- **[content+design | node: 3877:34896]** Account page edit affordances are asymmetric. "Edit" links appear on Shipping Address and Allergies but are absent from Personal Details (Full Name, Gender, Birthday) and Contact Information (Mobile, Landline, Email). If those fields are editable, Nielsen 4 demands the same affordance; if they are not editable, Nielsen 1 demands a subtle label explaining why (e.g., "Contact support to change"). Fix: decide per-section whether the field is user-editable and apply one consistent rule.

- **[content+design | node: 2031:3747]** Every order card CTA is labeled "View Details" regardless of order state. On a page where rows vary between Delivered, Canceled, Processing, and Shipped, the identical CTA erases the state differentiation the user is trying to scan for. Law of Similarity is working against the user — eleven "View Details" buttons of identical color and weight make the list feel like one undifferentiated mass. Fix: (a) keep "View Details" on active/in-progress orders, relabel to "View Receipt" on Delivered and "View Reason" or "Reorder" on Canceled; or (b) keep the label but introduce a status chip ("Delivered" green, "Canceled" gray, "Processing" blue) that lets the eye filter the list without reading prose.

- **[content+design | node: 2039:203920]** Active-order card on Home uses literal template placeholders in production-facing copy. "Your order #{Order Number} is being processed. This typically takes {1-2 hours}. Order Date: {Date}. Estimated Delivery: {Date Range}." The curly-brace placeholders appear in the rendered screen, not just the Figma notes. This is placeholder-data territory so it is almost certainly a Figma artifact, but since it ships across a patient-facing surface it is worth naming explicitly so engineering confirms real values are substituted before build.

- **[usability | node: 2031:3747]** "Showing 10 of 100 orders" pagination with no sort or filter. Hick's Law and Choice Overload: 100 orders scanned chronologically with no filter (status, date range, prescription) and no sort control forces linear recall. For MVP this may be acceptable, but flag it as the first thing to add post-launch — even a single "Status: All / Active / Delivered / Canceled" filter collapses the scan problem.

- **[content+design | node: 3877:34896]** Row labels on Account use stacked "Field Name / value" without consistent label styling. "Full Name / Thomas Peterson", "Gender / Male", "Birthday / 05/20/1991" — the field name is bolded same as section headers ("Personal Details", "Contact Information"), which flattens the hierarchy. A patient scanning the page can't quickly separate section from row. Fix: section headers stay bold; row labels drop to medium-weight in `Theme/Secondary` gray.

- **[craft | node: 2039:203920]** "Need Help?" CTAs are three outlined pills of equal visual weight. If the product has a preferred channel (say, Live Chat for deflection from the call center), Von Restorff / Visual Hierarchy should elevate it. Three identical pills is the most expensive path per support-cost dollar to the business and the most Hick's-heavy path for a patient in distress. Fix: primary the recommended channel, keep the others as secondary.

- **[craft / tenant consistency | node: 2039:203920]** Header "Welcome Back, John" uses a serif-like display face while the body runs Inter. The mixed-family look may be intentional Lilly-brand theming, but on this single screen the two faces do not share enough visual DNA to feel like one system. Confirm with Lilly's brand team whether the serif is an approved Lilly display face; if not, collapse to the body family at a larger size and heavier weight.

**Next actions:**
1. **Fix the home progress tracker first.** Active step, completed checks, pending outlined, 2px connecting track. This is the single highest-leverage change for patient-anxiety reduction (node `2039:203920`).
2. **Correct the Orders breadcrumb** to "Home › Orders" — one-line copy fix, Nielsen 4 win (node `2031:3747`).
3. **Audit Account-page edit affordances end-to-end** and apply one rule: either every field gets Edit, or non-editable fields get an explicit "Contact support to change" line (node `3877:34896`).
4. **Differentiate order cards by state** — add a status chip plus state-aware CTA label (View Receipt / Reorder / View Details) to break the Law-of-Similarity wash on the list (node `2031:3747`).
5. **Confirm with engineering** that `{Order Number}` / `{Date}` / `{Date Range}` placeholders are live-data substituted before this ships (node `2039:203920`).
6. **Post-MVP:** add a status filter to the Orders list; prioritize one support channel on the Need Help block.
7. **Spec interaction details** with the engineering partner before build: per-property transitions (not `transition: all`), `ease-out` on any drawer/modal entry, `:active { transform: scale(0.97) }` on primary buttons, UI animations capped at ~200 ms, `font-variant-numeric: tabular-nums` on the order-number and date columns so the list does not shimmer as data loads.
