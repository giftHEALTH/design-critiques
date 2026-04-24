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
summary: "Portal MVP ships as LillyDirect-branded not platform; brand casing broken; status pills, rejected-Rx state, tenant tokens all missing."
top_heuristics: [N1, N6, N4, N3, N5]
top_biases: [bias-recognition-over-recall]
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
1. Fix the `Gifthealth` → `gifthealth` casing across the entire canvas (Verify Personal Info card has 3 instances; invalid-sign-in card has another; Commercial's support-copy findings name 3 more text nodes) — trivial fix, highest leverage, two lenses — flagged by: Commercial Lens, Craft Critic
2. Rename the `Logo` master component to `PartnerLogo` with a `tenant` variant property (LillyDirect, Default); introduce `theme.partner.primary` and `theme.partner.logo` tokens; rebind every header instance on the canvas (Commercial counts ~20+ instances affected) — flagged by: Commercial Lens
3. Add color-coded status pills to order cards (Delivered green / Shipped blue / Processing amber / Awaiting refill gray) and add a "Your therapy" status card to the top of the dashboard — without these the MVP's own listed scope ("Order Status") isn't visible on the list view — flagged by: Craft Critic, Patient Advocate
4. Design three first-class error / empty states as proper frames: "Prescription — Action needed" (rejected Rx with plain-language reason token + Message support + What does this mean? buttons), "Pending Rx / No Checkout" (with explicit next-fill-window copy), and "Order delayed" — each with self-serve next step — flagged by: Patient Advocate, Commercial Lens
5. Add three multi-tenant / white-label / theme-token bullets to the Overview's "Out of Scope / Future" list (frames 2002:124 and 2279:19316) so the platform abstraction doesn't get silently dropped; fix the "Portal: Account Manaagement" typo on section 3877:31329 — flagged by: Commercial Lens

**Consensus:**
- Tenant concentration is architecturally baked in — Commercial reads every Lilly-painted pixel as multi-tenant risk; Craft notes the `Theme/Primary` token (#440B39) is wired but unused in favor of raw Lilly hex
- The brand-casing rule ("gifthealth" one word, lowercase h) is broken in several text nodes — Commercial flags three in support copy ("chat with Gifthealth..."), Craft flags three more on the Verify Personal Information card plus one on invalid-sign-in
- Status visibility is absent where patients most need it — Patient flags no status-at-a-glance hero on the dashboard and no dedicated rejected-Rx state; Craft flags order cards that show no status pill despite the MVP scope explicitly naming "Order Status" as a core feature
- The canvas is mid-fidelity and inspection-limited — Patient couldn't read individual screens (metadata exceeded token budget); Craft found placeholder copy ("Button Title", pagination "1 ... 4 5 6 7 8 ... 20", "Showing 10 of 100 orders"); Commercial caught a literal typo ("Manaagement")

**Tensions requiring human decision:**
- **Platform abstraction vs. v1 ship.** Commercial wants `Logo`→`PartnerLogo` + tenant tokens NOW, before any next-partner onboarding conversation. Craft treats the Lilly brand as the governing system for this surface. Decision: does v1 ship on the clean white-label abstraction (short-term design cost), or eat the re-skin debt when the second pharma lands?
- **Portal as Gifthealth vs. portal as Lilly.** Craft names this explicitly as a leadership question — the patient sees a Lilly-branded property with a small "Powered by gifthealth" footer. Patient lens is neutral (patients see whatever chrome is in front of them). Commercial treats every pixel of Lilly paint as concentration risk. Decision: is Gifthealth comfortable being invisible above the fold on the portal?
- **Scope of v1.** All three lenses think v1 is under-scoped, but they disagree on what leads. Patient wants rejected-Rx + empty-state + support footer pulled in now. Commercial wants multi-tenant tokens + PartnerLogo + admin view committed to roadmap. Craft wants interaction states (hover/focus/active/disabled) specced before build. Decision: which dimension leads the next iteration?

---

## Patient Advocate

**Top-line verdict:** This reads as a wide mid-fidelity canvas capturing the end-to-end portal MVP — the shape of "a patient can come back and see their therapy" is right, and that alone directly answers Edward's documented gap. Individual screen contents could not be inspected at the node level — metadata exceeded token budget, overview screenshot renders screens as thumbnails — so this critique anchors at the canvas level (3895:7741) and flags where patient-lens risks most commonly sit on a portal MVP of this scope. Confidence: medium.

**Gauntlet stage(s) this touches:** Prescription, Approvals, Fulfillment, and the cross-cutting persistence surface.

**What's working:**
- A persistent portal existing at all is the single highest-leverage answer to the documented "tracking-awareness gap" from the checkout-flow doc — patients today have no documented way back in.
- The canvas groups flows into horizontal bands (onboarding, dashboard, orders/tracking, prescriptions, account), which mirrors the LDZ swim-lane structure.
- A top-left notes/legend panel is present — the team is documenting intent on the canvas.

**What's not:**
- **[Severity: usability | node: 3895:7741]** Canvas-level inspection only — individual screens could not be read. Fix at the canvas level: split "Portal MVP End to End" into named section frames (one frame per flow: Onboarding, Dashboard, Order Tracking, Prescription Detail, Refill, Account & Address, Support Handoff) and give each a visible H1 label using Theme/Heading-L at fontSize 32, color Theme/Ink (#1A1A1A), pinned to top-left at y=-56 relative to the frame origin.
- **[Severity: content+design | node: 3895:7741]** No visible "status-at-a-glance" hero on the dashboard band. Fix: on the primary dashboard frame, add a top-of-page status card titled "Your therapy" with three stacked rows — Current order (status label + tracking link), Next refill available (date + Refill now button fill Theme/Primary #440B39 label #FFFFFF fontSize 16), Prescription (drug name + dose + View details). Card fill Theme/Surface #FFFFFF, stroke Theme/Border #E5E5E5, cornerRadius 12, padding 24.
- **[Severity: content | node: 3895:7741]** Portal MVP does not surface "you can manage dose changes here" at the moment a patient considers local pickup — hands the patient to LillyDirect/Walmart. Fix: on the fulfillment-choice frame, add inline callout (fill Theme/Accent-Soft #FFF3EE, cornerRadius 8, padding 16) with copy "Dose changes and future refills stay easier when your prescription stays with gifthealth. You can transfer back anytime." in Theme/Body-S fontSize 14 color Theme/Ink #1A1A1A.
- **[Severity: content+design | node: 3895:7741]** Rejected-Rx silent-failure state missing. Fix: add a dedicated "Prescription — Action needed" state frame showing the drug name, a plain-language reason token (one of: "Your prescriber needs to send a new prescription," "Your insurance needs to approve this first," "Your address needs to be verified"), a primary Message support button (fill Theme/Primary #440B39, label #FFFFFF, fontSize 16) and a secondary outlined "What does this mean?" button (stroke Theme/Primary, label color Theme/Primary, fontSize 16).
- **[Severity: content | node: 3895:7741]** Onboarding risk: gating the "see my order status" reward behind multi-step account setup inverts Goal Gradient. Fix: on the first onboarding frame, set the primary CTA's label TextNode characters to "See my order status" (not "Create account" or "Set up profile"), fill Theme/Primary #440B39, label color #FFFFFF, fontSize 16, full-width on mobile with horizontal padding 24.
- **[Severity: usability | node: 3895:7741]** Self-serve escape hatch (Need help?) not visible on every frame. Fix: add a persistent footer component present on every portal frame, height 56, fill Theme/Surface-Muted #F7F7F7, containing left-aligned text "Need help?" in Theme/Body-S fontSize 14 and two right-aligned links "Message us" and "Call 1-800-xxx-xxxx" in Theme/Link underlined fontSize 14 color Theme/Primary #440B39.

**Patient-visibility check:**
- Can a confused patient self-serve here? Unknown from the thumbnail, but the portal's existence is a large step toward yes.
- Does this design surface status or hide it? Surfaces at conceptual level; mixed at execution level.
- Where does friction compound downstream? Rejected-Rx and empty states without explanation push patients to the PCR queue, which compounds into longer patient wait times and 40% who never reach first fill.

**Next actions:**
1. Break "Portal MVP End to End" into labeled section frames so each flow is individually reviewable.
2. Audit the primary dashboard frame for the "Your therapy" status card above. Reorder if not at top.
3. Design three error/empty states as first-class frames: rejected Rx, order delayed, refill not yet available.
4. Add a persistent "Need help?" footer to the portal template.
5. Confirm lowercase-h "gifthealth" is used consistently in all portal copy nodes.

---

## Commercial Lens

**Top-line verdict:** The Portal MVP solves a real patient-transparency problem, but in its current state it is a LillyDirect-branded patient portal, not a platform patient portal. Every frame bakes Lilly into the shared component layer, which burns the "< 24 hour onboard" lever and makes the next partner a re-skin project.

**Commercial stakeholder(s) this touches:** End patient in a multi-tenant context, Program Manager onboarding the next manufacturer, and the PM/designer whose "Out of Scope" list determines whether the platform abstraction ever lands.

**What's working:**
- The "Powered by gifthealth" footer lockup (footer instance `2041:3803`, `2044:16305`, and siblings) is the correct white-label co-brand pattern. It is the one place the design already understands multi-tenancy.
- Language selector "Language: EN" with an ES option (e.g. `Header Actions` `2309:18724`) is a user preference, not a partner attribute. Tenant-neutral. This generalizes cleanly.
- The Overview frame (`2002:124`) scopes the project generically as "Patient Portal" — verbal intent is platform-native; execution just hasn't caught up.
- Self-serve tracker, Rx list, and order-history views map directly to "abandon less / persist longer" outcomes.

**What's not:**
- **[Severity: content+design | node: 3877:32979]** Partner logo baked into shared header as generic "Logo" component — same LillyDirect mark appears on ~20+ frames (`3877:32979`, `3895:6635`, `3976:6369`, `3887:6627`, `3887:9047`, etc.), master is named "Logo" rather than tenant-aware. Rename master component `Logo` to `PartnerLogo`; add a Figma variant property `tenant` with `LillyDirect` as variant 1 and `Default` (gifthealth mark) as variant 2; rebind the logo fill/image to `theme.partner.logo`. Replace every `Logo` instance with `PartnerLogo` and set `tenant="LillyDirect"` explicitly.
- **[Severity: content+design | node: 3877:32966]** Header treatment is partner-branded rather than tenant-themed — red Lilly wordmark color applied as raw hex inside `Header` frame (`3877:32966`, `2041:3448`, `3877:31373`, and mobile siblings). Introduce `theme.partner.primary` (seed `#D52B1E` for LillyDirect, `#440B39` for Gifthealth default), bind header logo tint and any accent strokes inside `Header` / `Header Container` to that token, stop using raw hex on any node inside a `Header*` subtree.
- **[Severity: content | node: 2002:124]** Overview's "Out of Scope / Future" list has zero multi-tenant / white-label / theme-token items. In the `Out of Scope / Future` TextNode inside frame `2002:124`, append three bullets verbatim: "Tenant theme tokens (`theme.partner.primary`, `theme.partner.logo`)", "PartnerLogo variant system with per-tenant asset binding", and "Multi-tenant header/footer swap via Figma variants". Mirror the same three bullets in sibling Overview frame `2279:19316`.
- **[Severity: content | node: 2309:18749]** Prescription-card frames named after the drug (`Zepbound` at `2309:18749`, `2309:18850`, `2309:18951`, `2309:19051`). Rename each of these four frames from `Zepbound` to `PrescriptionCard` and move the drug label into a text child node named `Drug Name` with the string `Zepbound 2.5MG/0.5ML Subcutaneous Solution Vial` as content, not structure.
- **[Severity: content | node: 2147:17245]** Support copy reads "chat with Gifthealth patient care representative" (also `2147:17279`, `2147:17337`) on Lilly-headered screens. In text node `2147:17245` change the characters to "Chat with your patient care team if you have any questions." Apply to `2147:17279` ("Chat with your patient care team for any questions.") and `2147:17337` ("Chat with your patient care team if you have any questions."). Keep the vendor name in the footer lockup only.
- **[Severity: usability | node: 3877:16426]** "Pending Rx / No Checkout Window" state surfaces no explanation of *why* and no expected window. In the `Welcome Back, John` body text inside section `3877:16426`, add a second line under the "no fills available" text reading "Your next fill window opens on {nextFillDate}. We will notify you when it is available." Bind `nextFillDate` to the same prescription payload the Rx Details page already consumes.
- **[Severity: content | node: 3877:31329]** Section misspelled as "Portal: Account Manaagement" (double "a"). Rename section `3877:31329` from `Portal: Account Manaagement` to `Portal: Account Management`.

**Platform-vs-partner check:**
- Is this platform-native or partner-specific? **Partner-specific.** Overview intent is platform, but every header, logo instance, and the color treatment above the fold are bound to LillyDirect without a variant or token mechanism.
- Can a new pharma partner adopt this in < 24 hours? **No.** Onboarding a second manufacturer requires touching ~25+ individual `Logo` instances, retinting headers by hand, and chasing "Gifthealth" strings inside neutral copy. The PartnerLogo variant + `theme.partner.primary` + `theme.partner.logo` change converts this from a re-skin project into a variant swap.
- Where does this cede ground? **Walmart / LillyDirect / Phil.** The "no fills available" empty state cedes ground at the precise "lost at checkout" moment. A portal indistinguishable from a LillyDirect-operated property helps LillyDirect's brand equity, not Gifthealth's.

**Next actions:**
1. Promote `Logo` → `PartnerLogo` with `tenant` variant; introduce `theme.partner.logo` / `theme.partner.primary` tokens; rebind every header instance.
2. Add the three platform-abstraction bullets to Overview's "Out of Scope / Future" list (`2002:124` and `2279:19316`).
3. Neutralize the three "Gifthealth" support-copy strings (`2147:17245`, `2147:17279`, `2147:17337`) to "your patient care team".
4. Rename the four `Zepbound` prescription-card frames to `PrescriptionCard`; fix "Manaagement" typo on section `3877:31329`.
5. Add explicit "retention state" treatment with next-fill-window copy to `3877:16426`.

---

## Craft Critic

**Top-line verdict:** LillyDirect-branded patient portal surface where Gifthealth appears only as a "Powered by" footer logo — so Gifthealth brand standards are largely not the governing brand system here. Three load-bearing craft issues: (1) "Gifthealth" miscased in patient-facing copy, (2) the Gifthealth footer wordmark may not match the official lockup, (3) fonts are IBM Plex Sans / Inter / SF Pro — acceptable for Lilly, worth naming. Confidence: medium-high.

**Context flag:** Brand patients see is **LillyDirect**, not Gifthealth. Gifthealth brand standards (Purple #440B39, Funnel Display, approved gradients) apply only to the "Powered by" lockup.

**What's working:**
- Design token system is real (`Theme/Primary: #440B39`, `Theme/Secondary: #6C757D`, `Theme/Border: #E4E7EB`). Gifthealth's purple IS in the token library as the primary.
- Components are real, reused, and documented (breadcrumb/button/pagination all reference Bootstrap 5.1; `CardOrderHistoryMobile` reused for each order row).
- Mobile Orders and desktop Rx List share card shape, pagination, breadcrumb — Nielsen 4 across the split.
- Scope boundaries on-canvas ("Out of Scope / Future", "Open Qs / Items") — an unusually mature MVP artifact.

**Brand compliance (Gifthealth-specific):**
- Palette: Pass at token layer. Raw `#0F3A85` is LillyDirect blue, appropriate here.
- Typography: **Fail against Gifthealth brand** (IBM Plex Sans, Inter, SF Pro instead of Funnel Display / Aptos). Acceptable for a Lilly surface.
- Gradients: Not present in flows sampled.
- Wordmark: **Fail.** The "gifthealth" footer logo appears to have a small superscript glyph that does not match the company's correct wordmark. Replace with the approved 1-color purple (`#440B39`) or 1-color black lockup.
- Brand-name casing in body copy: **Fail.** "Gifthealth" capitalized in at least three places on Verify Personal Information card + one on invalid-sign-in.
- WCAG contrast: `#0F3A85` on white passes AA (~10.5:1). `#6C757D` on white is ~4.56:1 (floor). Disabled Continue CTA pale-pink almost certainly **fails AA**.

**Heuristic pass (at-risk only):** N1 (status), N3 (user control — disabled CTA), N5 (DOB split inputs), N6 (order rows missing drug name), N8 (desktop Rx List near-duplicate cards + broken pagination).

**Interaction quality:** No hover, `:active`, focus, or loading states visible. Continue CTA needs enabled and disabled variants shown side by side. Pagination numeric spans need `font-variant-numeric: tabular-nums`.

**What's not:**
- **[Severity: content | node: 2041:3770]** "Showing 10 of 100 orders" hardcoded. Set order-count TextNode characters to "Showing {visible} of {total} orders"; add `font-variant-numeric: tabular-nums` when coded.
- **[Severity: content+design | node: 3879:8215]** Order card primary title "Delivered April 21, 2025" — status conveyed only as prose. Set the card's primary TextNode characters to the drug name, move order number to a secondary gray line (`#6C757D` 14px), add status pill to the right: Delivered (`#D1E7DD` bg / `#0A3622` text), Shipped (`#CFE2FF` / `#052C65`), Processing (`#FFF3CD` / `#664D03`), Awaiting refill (`#E9ECEF` / `#495057`).
- **[Severity: content | node: 2147:17185]** Legal/body copy contains "Gifthealth" capitalized (3 instances on Verify card + 1 on invalid-sign-in at `2147:17232`). Set intro paragraph TextNode to "Pharmacy services for LillyDirect Cash Pay Pharmacy Solutions are provided by licensed, third-party service providers, including gifthealth." Second paragraph start: "To provide you with more information on your prescription(s) from <Dr. Name>, gifthealth will first need to verify your information." Terms: "By clicking 'Continue', I accept the gifthealth Terms of Service, Privacy Policy, and Notice of Privacy Practices."
- **[Severity: content | node: I2041:3803;1:90]** Verify that "Powered by" TextNode characters are exactly "Powered by gifthealth" (lowercase h, single word) and that `gh_logo` (node 2002:476) is the official 1-color purple (`#440B39`) lockup at 20px minimum height. Swap asset for approved file if not.
- **[Severity: usability]** (no concrete node_id) Disabled Continue CTA: set disabled button fill to `#E9ECEF`, label color to `#6C757D` (14px+ passes AA), add validation summary TextNode above with "Complete all required fields to continue", inline-validate each required field on blur with red `#DC3545` helper text below. (Prose-only.)
- **[Severity: content+design | node: 3879:8218]** Order-row button label reads "Button Title". Set the button's label TextNode (`I3879:8218;288:239258;1:4738`) characters to "View details" (sentence case). Pick one casing across the canvas per Nielsen 4.
- **[Severity: content+design | node: 3895:7577]** Breadcrumb uses placeholder text and mixes fonts (first "Home" IBM Plex Sans, second item Inter). Set 1st-item TextNode characters to "Home" in IBM Plex Sans Regular 16px `#0F3A85` underlined; 2nd-item to "Orders" same style (not Inter); 3rd-item to "Rx #1234567" in IBM Plex Sans Regular 16px `#6C757D` (no underline).
- **[Severity: content+design | node: I3895:25550;1:13131]** Pagination for 12-item list shows "1 ... 4 5 6 7 8 ... 20". Set container children to: prev-chevron, page-item "1" (active `#0F3A85` fill, white text), page-item "2" (`#0F3A85` text transparent), next-chevron. Drop ellipses and trailing "20". Apply `font-variant-numeric: tabular-nums`.
- **[Severity: content+design | node: 2041:3803]** Footer height 46px with only Powered-by. Add second TextNode below `gh_logo` with characters "Privacy · Terms · Accessibility · Contact patient care: 1-800-XXX-XXXX" in IBM Plex Sans Regular 12px `#6C757D`; increase Footer container height to ~96px.
- **[Severity: usability | node: 2002:128]** Project-overview header "Project / Patient Portal" adds no patient value if propagated to a patient page. For any patient-surfaced page header, set top TextNode characters to patient context ("Welcome back, Sarah") and display TextNode below to page name ("Your prescriptions").

**Next actions:**
1. Fix Gifthealth brand-name casing across the canvas: "Gifthealth" → "gifthealth" (one word, lowercase h) on every TextNode.
2. Replace footer `gh_logo` (2002:476) with approved 1-color purple `#440B39` wordmark at 20px minimum; confirm "Powered by" TextNode (`I2041:3803;1:90`) reads exactly "Powered by gifthealth" in Aptos Regular 11px.
3. Add an order-status pill to `CardOrderHistoryMobile` with color-coded fills per the mapping above.
4. Redesign disabled `Continue` CTA: neutral `#E9ECEF` fill with `#6C757D` 14px label; inline validation summary above.
5. Replace identical-placeholder row data with three or four distinct mock prescriptions so a reviewer can evaluate density and truncation.
6. Add interaction-state specs (default / hover / focus / active / disabled) to button, card, pagination.
7. Decide whether Gifthealth wants more visible brand equity on this surface — leadership conversation, not a craft fix.
