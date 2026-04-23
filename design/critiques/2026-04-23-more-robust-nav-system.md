---
title: "Design Critique: More Robust Nav System"
status: draft
area: cross-cutting
author: "Digital Experience Critique (Claude)"
invoked_by: alexturvy
generated_by: gifthealth-digital-experience
created: 2026-04-23
last-updated: 2026-04-23
artifact: "https://www.figma.com/design/awfubM5IdTkKcvrHn2hW8C/test-temp?node-id=3497-931&p=f&t=ko7G1bXTVkNsOPks-0"
artifact_type: figma
reviewed_lenses: [patient-advocate, commercial-lens, craft-critic]
summary: "Nav system has right shape but is trapped in LillyDirect chrome and missing top-level status, motion specs, and multi-tenant proof."
top_heuristics: [N6, N3, N4, N2, law-jakob]
top_biases: [bias-zeigarnik, bias-recognition-over-recall]
scope: multi-lens
---

# Design Critique: More Robust Nav System

**Date:** 2026-04-23
**Artifact:** https://www.figma.com/design/awfubM5IdTkKcvrHn2hW8C/test-temp?node-id=3497-931&p=f&t=ko7G1bXTVkNsOPks-0
**Scope:** multi-lens
**Lenses:** patient-advocate, commercial-lens, craft-critic

## Summary

**Top 5 actions** (severity-ranked, cross-lens):
1. Elevate live order / prescription status and financial-assistance / payment-options to first-class top-level nav destinations (not buried under Account), with a visible change indicator in the collapsed state — flagged by: Patient Advocate, Commercial Lens
2. Produce sibling artifacts rendering this nav on (a) a neutral white-label tenant and (b) a second, non-Lilly manufacturer shell, and annotate which nav elements are tenant-configurable vs. platform-locked — flagged by: Commercial Lens
3. Add a motion-spec frame plus an explicit scrim/backdrop specification — enter/exit easing and duration for drawer and submenu, tap-to-dismiss behavior, and `prefers-reduced-motion` handling — flagged by: Craft Critic
4. Specify the SMS / email deep-link landing state inside this artifact, and localize nav chrome itself (not just content) so "Language" reads "Idioma" in Spanish mode — flagged by: Patient Advocate, Craft Critic
5. Add a visible active-state treatment for current page and current language that does not rely on color alone, and rewrite nav labels in plain, outcome-oriented language ("Track my order," "My medications," "Get help") — dry-run with one low-agency patient profile — flagged by: Patient Advocate, Craft Critic

**Consensus:**
- The "Example Problem" frame is the right instinct but under-developed — all three lenses ask for it to be sharpened (Patient: named patient scenario; Craft: lead with one sentence; Commercial: name the commercial lever the design moves)
- Localization being in the nav from the start is a strength — but all three lenses want more: Patient (plain-language labels), Commercial (locale matrix, not hard-coded EN/ES), Craft (localize the chrome itself)
- The artifact is early-fidelity and partially inspection-blocked (metadata truncated, design-context unavailable); every lens flagged this as a review-confidence limit

**Tensions requiring human decision:**
- **Patient need vs. craft restraint on nav density.** Patient Advocate wants support and financial-help surfaced prominently in-nav; Commercial wants financial-assistance as a first-class entry. Craft Critic invokes Hick's Law and wants utility items clustered under a single group. Decision: which items earn a top-level slot vs. a utility group?
- **Patient fulfillment framing vs. commercial platform framing.** Patient reads the LillyDirect chrome as natural (patients arrive that way today). Commercial reads it as multi-tenant risk and wants sibling tenant renderings. Decision: is this artifact intended as a Lilly deliverable or as a Gifthealth platform nav system? The answer changes what "done" means.
- **Scope of this review round.** Patient wants SMS deep-link landing specified now. Commercial wants admin-configurable annotations and a sibling non-Lilly render now. Craft wants motion specs and per-frame inspection now. All three are real deficits, but only one can be the next pass's focus. Decision: which dimension leads the next iteration?

---

## Patient Advocate

**Top-line verdict:** This is early-to-mid fidelity exploration of a navigation system, not a finished flow. The shape of a persistent, top-level nav is right and would close a real gap the checkout-flow notes already called out ("How does someone know they can come back to track shipping? They won't know"). The screenshot I can see is low-resolution and most labels are unreadable at this zoom, and the metadata payload exceeded the token limit, so I am critiquing the navigational *intent* visible in the layout and frame titles rather than final content. Treat this as a directional read.

**Gauntlet stage(s) this touches:** Fulfillment (primary) and Persistence. Secondarily Approvals, because a nav that exposes prescription status also touches the approval-visibility gap.

**What's working:**
- **A persistent nav finally gives patients a way back in.** The patient checkout-flow doc explicitly names the "tracking-awareness gap" — patients today have no documented way back into the portal to check shipping. A top-level nav on a lillydirect-branded mobile surface is a direct response to that.
- **Multiple states shown (closed, open, EN variants).** Designing the closed *and* open states together is the right instinct — it forces the team to answer "what does a patient see when they are not actively exploring?" which is where the 30% fulfillment-stage abandoners live.
- **The "Example Problem" frame is present.** Anchoring the design in a stated problem (rather than jumping to a solution) is exactly the "validated problem" test the strategy doc asks for. Keep that frame visible for reviewers.
- **Localization (EN label) is baked in early.** LDZ patients include Spanish-preferring populations; planning for language switching at the nav layer, not bolted on later, avoids a downstream seam.

**What's not:**

- **[usability] Invisible entry point to status/tracking.** The Zeigarnik Effect (people remember incomplete tasks) and Recognition Over Recall both tell us a patient whose order is in flight is mentally "on hold" until delivered. If the nav does not surface an at-a-glance order/script status item at the top level (not buried under "Account" or "Orders"), the patient's primary reason to return is one tap too deep. Fix: promote "Your order" or "Prescription status" as a first-class nav destination with a live status indicator (e.g., a dot when something has changed). This is the single highest-leverage change for the persistence outcome.

- **[content] Label clarity is unverifiable at this fidelity but load-bearing.** In the screenshot the nav items read as short icon+label pairs. For a patient who is overwhelmed, in pain, or low health-literacy, the difference between "Orders," "Prescriptions," "Rx," and "My medication" is the difference between finding status and calling a PCR. Fix: write the labels in plain, outcome-oriented language ("Track my order," "My medications," "Get help") and usability-test with at least one low-agency patient profile before locking. The Curse of Knowledge bites hard here — designers know what "Rx" means; many patients do not.

- **[usability] No visible re-entry breadcrumb from SMS/email.** The checkout-flow doc asks, "How does someone know they can come back to track shipping? Will they be sent there via SMS?" The nav redesign should be paired with a deep-link contract: every SMS/email status message should land on a page that shows the nav in its "status" state, so the patient learns by doing that the portal is their home base. Fix: specify the SMS→portal landing state alongside the nav states so this is not a later integration surprise.

- **[content+design] Help/support surface not obviously named.** A patient who is confused at 9pm needs to see either "Chat" or "Call us" in the nav itself — not a settings sub-menu. Absent that, the silent failure mode is that the patient abandons instead of asking. Fix: give support a persistent, high-contrast slot (even in the collapsed state) and show wait-time or response expectation where possible.

- **[usability] Multi-script households are not obviously addressed.** The checkout-flow doc flags the "fragment" problem (one script moves, others stay). If a patient has two scripts in different states (one shipped, one pending PA), the nav must handle both without forcing the patient to "switch" contexts. Fix: confirm the status surface supports a list-of-scripts view, not a single-script hero.

- **[content] "Example Problem" is a scaffold, not a persona story.** For a PM reader, a single patient scenario ("Maria got an SMS at 6pm saying her order shipped; two days later she wants to check when it arrives; today she cannot") will carry more weight than an abstract problem frame. Fix: rewrite the example as a named patient moment tied to a gauntlet stage.

**Patient-visibility check:**
- **Can a confused patient self-serve here?** Partial. A persistent nav helps, but without a visible support affordance and plain-language labels, a stuck patient still has to call. The nav alone does not close the self-serve loop.
- **Does this design surface status or hide it?** Mixed — surfaces *access* to status (good) but I cannot tell from this fidelity whether status itself (order in flight, PA pending, refill available) is visible from the collapsed nav. Surfacing a status dot or count in the collapsed state is the difference between "available if you look" and "visible without thinking."
- **Where does friction compound downstream?** If the nav does not expose order/PA status at the top level, patients who wonder "where is my order?" default to SMS-searching or calling PCRs — which is exactly the contact volume the strategy doc is trying to reduce (OKR 2: "Reduce support contacts"). Every invisible status here becomes a PCR ticket later.

**Next actions:**
1. Promote live order/prescription status to a first-class nav destination with a visible change indicator in the collapsed state. This is the single change most aligned with the "persist longer" outcome.
2. Write patient-facing labels in plain language and dry-run them with one low-agency patient profile (or a teammate reading them cold) before visual polish.
3. Define the SMS/email deep-link landing state in the same artifact, so the "how do they know to come back" question is answered inside this design, not deferred.
4. Add a persistent, high-contrast support affordance (chat or call) that appears even when the nav is collapsed.
5. Replace the generic "Example Problem" frame with a named patient scenario tied to Fulfillment/Persistence so reviewers evaluate the nav against a real moment, not an abstraction.
6. Request a higher-resolution export or frame-by-frame screenshots of each state (Mobile Closed: EN, Mobile Open: EN, and the other variants) so label-level critique can be grounded rather than inferred — flagging this as a fidelity limit on the current review.

---

## Commercial Lens

**Top-line verdict:** A nav system rendered end-to-end in LillyDirect branding is a platform design trapped inside a partner skin. The underlying IA work may be sound, but as shipped this artifact reads as a Lilly deliverable, not a multi-tenant nav system — and that quietly deepens concentration risk instead of reducing it. (Note: full layer metadata could not be loaded, so this critique is grounded in the screenshot plus file title / state labels.)

**Commercial stakeholder(s) this touches:** Pharma client admin (who configures tenant nav), Account Manager (who pitches the next logo using current work), Program Manager (who onboards programs in < 24 hours), end-patient in a multi-tenant context, and — implicitly — the Lilly renewal owner.

**What's working:**
- **Named state coverage.** Showing "Mobile Closed: EN" and implied sibling states (open, localized) is exactly the discipline a white-label template needs. A nav that ships without state matrices is the kind of work that breaks on the second tenant; this one at least acknowledges the surface area.
- **Localization is in scope from the start.** An EN/ES toggle visible in the nav chrome is a commercial asset. Many pharma programs (Lilly included) have Spanish-speaking patient populations, and localization-in-the-template is table stakes for landing the next manufacturer.
- **"More Robust" framing is honest.** The file is positioned as a system, not a one-pager fix. That matches the platform-first principle ("today's programs are instances of the platform, not the scope of it").
- **An "Example Problem" frame exists.** Anchoring the proposal to a stated problem, even visually, aligns with the "validated problem" pre-build test.

**What's not:**
- **[Severity: content+design] LillyDirect branding is the substrate, not a swappable skin.** — Affects: **multi-tenant readiness, land motion, concentration risk.** The Lilly wordmark reads as the chrome of the nav rather than as content slotted into a tokenized brand zone. If a Fuzehealth, ConnectiveRx, or AssistRx evaluator (or our own AM pitching the next logo) opens this file, the takeaway is "built for Lilly." Fix: produce at least one sibling artifact on a neutral white-label tenant and a second, non-Lilly manufacturer shell (even a fictional one) to prove the nav is tenant-agnostic. Make the logo/brand area visibly a slot, not a fixture.
- **[Severity: content] No visible tenant-admin surface.** — Affects: **time-to-onboard, expand motion.** The OKR is "decrease program onboarding time" and the strategy calls for an "admin white-label template portal." A nav system without a paired admin-configuration view (what a pharma admin toggles to add a menu item, reorder, localize, hide a section) means onboarding a new tenant's nav still requires engineering. Fix: add an admin-configuration artifact or at minimum annotate which nav elements are tenant-configurable vs. platform-locked.
- **[Severity: content] Localization shown as EN/ES hard-coded, not as a language matrix.** — Affects: **multi-tenant readiness.** If the next manufacturer wants EN/ES/ZH or a different default, is that a config or a build? Fix: label the language list as "tenant-configured locales" and show a second state with a different locale set.
- **[Severity: usability] No visible Account Manager / Program Manager signal.** — Affects: **expand motion, Lilly renewal.** The design speaks only to the patient. In a Hub+Pharmacy product where we charge pharma for outcomes, surfaces that hide tenant/program context from internal commercial stakeholders make renewal conversations harder. Fix: note how this nav composes with a tenant-scoped admin view; if out of scope, say so explicitly so reviewers know it wasn't forgotten.
- **[Severity: content] Competitive-bar gap around speed and self-serve.** — Affects: **competitive positioning vs. LillyDirect, Amazon/Zepbound same-day, Walmart.** A nav on a LillyDirect mobile shell inherits LillyDirect's pace expectation. If "order status," "same-day delivery," "refill in one tap" aren't first-class items in the nav, we visually cede the speed narrative to the very competitor we're rendering against. Fix: make fulfillment status and refill action visible at the top level of the patient nav.
- **[Severity: content] No "lost-to-competitor" recovery hook.** — Affects: **competitive positioning.** The patient-checkout doc names Walmart and LillyDirect as destinations where patients are lost. A nav is a recovery surface (pricing help, payment options, PAP/financial assistance). If those entry points aren't in the IA, the nav is pretty but commercially silent at the moment of risk. Fix: ensure financial-assistance and payment-options routes are first-class nav entries, not buried.

**Platform-vs-partner check:**
- **Platform-native or partner-specific?** **Mixed, tilting partner-specific.** The system labels read platform (state matrix, localization), but every visible pixel is Lilly-branded. Without a neutral or second-tenant rendering, reviewers will read it as a Lilly asset.
- **Can a new pharma partner adopt this in < 24 hours?** **With work.** The design doesn't expose what is tokenized, what is slotted content, and what is engineering-locked. Until the admin-configurable surface is documented, a new tenant's nav is still a custom build.
- **Where does this cede ground to a named competitor?** **LillyDirect, primarily.** Rendering our "more robust nav" inside LillyDirect's chrome quietly positions our work as a component of their experience, not a differentiated platform. Against **Phil** (our only other Hub+Pharmacy peer), we lose the chance to show this as a multi-tenant advantage. Against **Amazon/Zepbound same-day** and **Walmart**, the nav as drawn does not surface speed-to-patient (refill status, delivery ETA) at the top level.

**Next actions:**
1. Produce a sibling artifact set: same nav system rendered on (a) a neutral white-label tenant, (b) a second, non-Lilly manufacturer shell. Ship them in the same file. This is the single highest-leverage change — it flips the commercial read from "Lilly deliverable" to "platform system."
2. Add a tenant-admin companion artifact (or annotations) showing which nav elements are configurable vs. locked. Tie it explicitly to the "Decrease program onboarding time" OKR.
3. Elevate refill status, delivery ETA, and financial-assistance entry points to first-class nav items, so the IA answers the Amazon-same-day and cost-as-abandonment signals by construction.
4. Treat the EN/ES toggle as an instance of a locale matrix; document which locales are configurable per tenant.
5. Before review with commercial stakeholders, add one slide or frame that names the buyer (pharma client admin) and the commercial lever (time-to-onboard, multi-tenant readiness) the design is trying to move.

---

## Craft Critic

**Top-line verdict:** This is an exploration for LillyDirect, not a Gifthealth-branded surface — the brand system being exercised is Lilly red, not Gifthealth purple. Through the Gifthealth craft lens, the canvas is doing the right structural work (multiple states laid out side-by-side, light + dark variants, an "Example Problem" frame anchoring the rationale) but the artifact is too early and too low-resolution at the viewing zoom to judge brand compliance or interaction fidelity precisely. Treat this as a shape-right critique, not a production sign-off.

**Context flag:** Two overlapping caveats worth naming up front. (1) This is LillyDirect work — the Gifthealth brand-compliance checklist (Purple #440b39, Funnel Display, approved gradients) does not apply directly; the relevant brand standard is Lilly's, which this document set does not carry. The craft principles (Nielsen, Yablonski, Kowalski) still apply universally and that is where this critique concentrates. (2) The Figma MCP design-context and variable-defs calls failed ("nothing selected"), so tokens, exact hex values, font names, and Code Connect mappings could not be audited. The critique below is based on the canvas screenshot plus structural reasoning; it is lower-confidence than a fully-inspected review. To lift confidence: select each state frame in the Figma desktop app one at a time and re-run `/critique-craft` on each nodeId individually.

**What's working:**
- **State matrix is the right artifact shape.** Laying out Mobile Closed, Mobile Open, Language Submenu, and Desktop equivalents on a single canvas makes the nav system legible as a *system* rather than a single screen. This is the correct way to pressure-test consistency (Nielsen 4) before handoff — reviewers can scan horizontally and catch mismatches the designer misses in isolation.
- **Explicit "Example Problem" frame.** Most nav explorations jump straight to solutions; anchoring the canvas with the problem being solved lets reviewers evaluate whether the new system actually addresses it. That framing is a craft move.
- **Light and dark background treatments side-by-side.** The canvas shows both a light-surface row and a dark-surface row, which is the right way to verify the nav works across the full brand environment instead of optimizing for one.
- **English/Spanish language affordance is visible in-nav.** Surfacing localization as a first-class nav control (rather than burying it in a footer or account menu) respects Nielsen 2 (match between system and real world) for the sizable Spanish-speaking LillyDirect patient segment.

**Brand compliance:** Not applicable in the Gifthealth sense — this is a Lilly-branded exploration and the Gifthealth palette/typography rules from `brand-guidelines.md` are not the relevant standard. From the screenshot: the treatment appears to use Lilly's red-on-white identity consistently across states, which is internally consistent (Nielsen 4). If this is intended for a Gifthealth-produced surface that co-brands with LillyDirect, the Lilly brand guidelines should be the governing document and should be named in the artifact metadata.

**Heuristic pass (Nielsen's 10):**
- **Heuristic 3 (User control and freedom):** At this zoom, close affordances (X) on each open panel are visible, which is correct. A back affordance inside the language submenu cannot be verified without a higher-resolution view — if the submenu does not have an explicit back-to-parent-menu control, that is a failure here. Users who drill into "Language" need a way out other than full-close.
- **Heuristic 4 (Consistency and standards):** The mobile and desktop nav panels appear to share the same item ordering and iconography, which is right. One thing to check at higher fidelity: the icon-to-label spacing and the language-picker placement should be identical across breakpoints, not just similar. Jakob's Law rewards near-identical behavior across viewports.
- **Heuristic 6 (Recognition rather than recall):** The current-language indicator in the submenu needs a visible check/active state so users see which language is currently selected without remembering. Cannot confirm this is present at this zoom.
- **Heuristic 8 (Aesthetic and minimalist design):** The open panels appear to use generous whitespace, which is good. Worth scrutinizing at higher zoom whether every item in the "Mobile Open" state is necessary at top-level or whether some (Help, Legal, Accessibility) should cluster under a single utility group to reduce Hick's Law load.

**Interaction quality:** Not evaluable from a static canvas. This is the largest gap in the artifact. A "more robust nav system" is fundamentally an *interaction* artifact — the quality of the drawer slide, the submenu transition, the close behavior, and the active-item affordance are what determine whether the system "feels robust" or just "looks robust." Specifically, a production-ready version of this needs explicit decisions on:
- Drawer enter/exit easing and duration (Kowalski: 200-500ms, custom `ease-out` curve like `cubic-bezier(0.23, 1, 0.32, 1)`, exit faster than enter)
- Submenu-to-parent transition — is it a slide, a fade, or a replace? Each implies a different mental model (Yablonski)
- `:active` feedback on nav items (scale 0.97, per Kowalski)
- `prefers-reduced-motion` handling (Kowalski / accessibility)
- Whether the nav drawer dismisses on backdrop tap (user control, Nielsen 3) and whether backdrop presence/opacity is specified

None of this is visible in a Figma canvas. Add interaction specs — ideally prototyped — before engineering picks this up.

**What's not:**
- **Severity: usability** - *Interaction behavior is undocumented.* The canvas shows states but not transitions. A reviewer cannot tell whether tapping "Language" slides to submenu or replaces the panel, whether the desktop menu is a flyout or a full overlay, or how escape/backdrop-tap behaves. Violates Kowalski's "every animation must have a clear answer to why does this animate." **Fix:** Add a motion spec frame next to the state matrix naming the transition for each state change (enter curve, exit curve, duration, transform-origin) and whether the dismiss pattern is swipe, backdrop-tap, or X-only.
- **Severity: content+design** - *No visible active-state indicator on the current page/language.* Cannot confirm from this zoom, but the screenshot does not clearly show which nav item is "current" in the open states, nor which language is currently selected in the submenu. Violates Nielsen 6 (recognition over recall). **Fix:** Add an explicit active treatment — filled left-edge indicator, bold weight, or a checkmark for language. Do not rely on color alone (WCAG).
- **Severity: content+design** - *Close affordance symmetry.* The X sits top-right on what looks like every panel, which is conventional (Jakob's Law — good). But the desktop flyout appears to have the X *inside* the flyout card rather than on the triggering menu row. At higher zoom confirm the pattern is: dismiss target is the same button that opened the menu *or* a clearly-separated X, not both ambiguously. Otherwise Nielsen 3 and 4 slip.
- **Severity: usability** - *No specified backdrop / scrim state.* In the "Mobile Open" frame there is no visible dim layer behind the drawer. Without a scrim the nav reads as layered on top of live content, which on a patient-facing pharmacy surface can cause misclicks into the content underneath. **Fix:** Specify scrim opacity (commonly 40-60% black or a brand-tinted equivalent) and the transition curve for the scrim itself.
- **Severity: content** - *Language submenu surfaces only EN/ES but label may not be localized.* Verify that when a user is in Spanish mode, the submenu header reads "Idioma" (or equivalent), not "Language." This is Nielsen 2 (match between system and real world) — the nav that localizes content but not nav chrome breaks the illusion of a Spanish-first experience.
- **Severity: content+design** - *"More Robust Nav System" title does the artifact a disservice.* The canvas name is an internal self-description rather than a user-facing claim. At review time, reviewers naturally ask "robust compared to what?" and that conversation is easier when the artifact leads with the specific problem the new nav solves. The "Example Problem" frame is a step in that direction — pulling one sentence from it into the canvas title would sharpen the argument.
- **Severity: usability** - *Fidelity limit on this review itself.* Because design-context and variable-defs failed, whether color values, typography weights, and component references draw from LillyDirect's token system or from raw hex / eyedropped approximations cannot be confirmed. That audit must happen before this ships.

**Next actions:**
1. Add a motion-spec frame next to the state matrix. For each transition (drawer open, drawer close, submenu slide, scrim fade), specify duration, easing curve, and transform-origin. Default to `ease-out` at 200-300ms for enter, faster (150-200ms) for exit. Never `ease-in` on drawer entry.
2. Specify the backdrop/scrim explicitly — opacity, color, tap-to-dismiss behavior, and entrance/exit curve. Call out `prefers-reduced-motion` behavior (fade only, no transform).
3. Add a visible active-state treatment to nav items and language options — filled indicator or checkmark, not color alone. Verify contrast at WCAG AA for the active-state foreground against both light and dark backgrounds.
4. Confirm the language submenu has an explicit back control (not just dismiss), and that when in Spanish mode, all nav chrome — including the word "Language" itself — is localized.
5. Select each state frame in the Figma desktop app individually and re-run `/critique-craft` on each nodeId. This unlocks `get_variable_defs` (to audit whether the design is using LillyDirect tokens or raw hex) and `get_design_context` (to see component references and Code Connect mappings). Without that, a production sign-off is not advisable.
6. Name the governing brand standard on the canvas. If this is a LillyDirect surface, link the LillyDirect brand guide. If Gifthealth is co-branding, reconcile any contact points between the two systems explicitly.
