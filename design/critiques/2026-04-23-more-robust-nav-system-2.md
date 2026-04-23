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
summary: "Nav shape is right but LillyDirect-fused; missing active states, motion specs, Track/Refills, and a non-Lilly tenant render."
top_heuristics: [N6, N4, N2, N3, N1]
top_biases: [bias-zeigarnik, bias-recognition-over-recall]
scope: multi-lens
figma_write_blocked: false
---

# Design Critique: More Robust Nav System

**Date:** 2026-04-23
**Artifact:** https://www.figma.com/design/awfubM5IdTkKcvrHn2hW8C/test-temp?node-id=3497-931&p=f&t=ko7G1bXTVkNsOPks-0
**Scope:** multi-lens
**Lenses:** patient-advocate, commercial-lens, craft-critic

## Summary

**Top 5 actions** (severity-ranked, cross-lens):
1. Add active-state treatments across the nav: current page (Craft), current language (Craft), and a visible order/script status surface with badging (Patient, Commercial). No color-alone indicators. — flagged by: Patient Advocate, Commercial Lens, Craft Critic
2. Add a motion-spec annotation frame — drawer enter/exit (280ms, `cubic-bezier(0.32, 0.72, 0, 1)`), underlay fade, submenu height expand, caret rotate, nav-row `:active` scale(0.97), and `prefers-reduced-motion` handling — flagged by: Craft Critic
3. Re-render the core nav states in a non-Lilly tenant (generic / second-manufacturer skin) and add an admin/configuration frame showing tenant-tokenized vs. platform-locked elements — flagged by: Commercial Lens
4. Elevate Track Order, Refills, and Chat/Help to first-class nav destinations (not buried under Account), and reconcile the "Sign out" vs. "Logout" verb across all frames — flagged by: Patient Advocate, Craft Critic
5. Spec the unauth / re-auth state for returning patients arriving from SMS, plus the logged-out / pre-enrollment nav — flagged by: Patient Advocate, Commercial Lens

**Consensus:**
- The LillyDirect branding is fused into every frame — Commercial reads this as multi-tenant risk; Craft notes Gifthealth brand rules don't apply because this is a Lilly surface; Patient sees it as a missed chance to clarify which cohort the nav is for
- Active / selected states are absent across current page, current language, and order status — all three lenses land on Recognition over Recall (Nielsen 6 / Von Restorff / Zeigarnik) as the load-bearing missing signal
- Interaction behavior is under-specified — Craft wants motion specs, Commercial wants admin-config view, Patient wants unauth flow. Three different views of the same "this isn't done" gap
- Localization is in the nav from the start (a strength), but all three lenses want more: Patient (cohort coverage), Commercial (flatten to one-tap toggle), Craft (active-language indicator + animated expand)

**Tensions requiring human decision:**
- **Language as submenu vs. segmented toggle.** Commercial wants a single-tap segmented EN/ES toggle at the top of the drawer (patients at checkout shouldn't go two taps deep). Craft approves of the submenu pattern as Progressive Disclosure / Recognition over Recall. Decision: which cost matters more — the extra tap for Spanish speakers at a commercial-critical moment, or the cognitive load of a flat toggle on smaller mobile widths?
- **Lilly deliverable vs. Gifthealth platform nav.** Every frame wears Lilly paint. Commercial treats this as a concentration-risk signal that needs a sibling non-Lilly render; Craft needs to know which brand system governs; Patient reads the chrome as the natural arrival context. Decision: is this a Lilly deliverable, a platform system shown in Lilly skin, or work that will migrate to a Gifthealth-branded surface later? The answer changes what "done" means.
- **Scope of the next pass.** Craft wants a motion-spec frame; Commercial wants a non-Lilly sibling + admin-config frame; Patient wants the unauth state + patient-scenario annotations. All three are legitimate "not done" signals pulling in different directions. Decision: which dimension leads the next iteration?

---

## Patient Advocate

**Top-line verdict:** This is mid-fidelity and the structural bones are right — a persistent, multi-state nav is exactly the scaffolding patients need to re-enter the portal and check status. The patient cost hides in what the nav does *not* yet name: shipping status, refills, and a way back from "lost" moments. Critique is based on the canvas screenshot and knowledge files; the full metadata exceeded token limits, so node IDs below reference the top-level frames visible in the canvas.

**Gauntlet stage(s) this touches:** Fulfillment (primary — post-checkout re-entry and status visibility), with secondary implications for Approvals (prescription status) and Awareness (returning-user orientation).

**What's working:**
- **A persistent nav solves the single most-flagged gap in the checkout research.** Edward's annotation on the checkout flow — "How does someone know they can come back to track shipping? They won't know" — is a direct patient-harm finding, and a real nav with stable entry points is the right answer. Giving returning patients a visible home makes the portal feel like a place, not a one-way SMS link.
- **Language toggle in the nav (EN with submenu) is patient-centered.** Surfacing language as a first-class nav control, not buried in a footer or account screen, respects the fact that overwhelmed patients in a second language will not go hunting. This is genuine agency.
- **Light and dark variants, plus mobile + desktop states, show the team is designing for context.** Patients check status from a phone in bed at 11pm. That a dark variant exists at all suggests the team is imagining real-use moments, not just the demo.
- **Mobile closed state is restrained.** A patient opening the portal while anxious about a shipment does not need a wall of options — a minimal closed state with a clear affordance to expand is the right call for cognitive load (Miller's Law — people can only hold ~7 items in working memory; a closed nav respects that ceiling).

**What's not:**
- **[usability | node: 3497:931 (canvas root)]** No visible "Track order" or "Shipment status" destination in the mobile open state. This is the exact patient need the checkout-flow research flagged as unsolved. A patient who got an SMS three days ago, wants to know where their Zepbound is, and opens the portal should see "Track your order" as a first-tier nav item, not buried under "Orders" or "Account." Fix: add a dedicated Track / Status entry at the top of the mobile open panel, badged when there is active shipment activity (Zeigarnik Effect — open loops stay salient; a badge closes the loop the patient is holding in their head).
- **[usability | node: 3497:931 (mobile open state)]** Refills are not visibly surfaced. The strategic doc names Fill/Script rate and persistence as the outcomes that matter; nav is where persistence lives day-to-day. A patient who is one-and-done at day 30 is usually a patient who did not see a clear "refill now" path when they opened the portal. Fix: elevate Refills to a top-level nav item with a state (due / upcoming / none) rather than nesting it inside a generic Orders area.
- **[content | node: 3497:931 ("Example Problem" frame, top-left)]** The problem statement is sparse and internally scoped. Whoever reviews this later — another PM, a designer new to the thread — needs to see the patient scenario the nav is solving for (the "patient got an SMS and wants to track shipping" case). Fix: name one or two concrete patient journeys the new nav unlocks, tied to strategy outcomes (start sooner / abandon less / persist longer).
- **[content+design | node: 3497:931 (language submenu frame)]** The language toggle appears to offer EN and one other option. Strategy doc and personas are silent on which languages are in scope, but any real patient population using Lilly Direct will include Spanish-speaking patients at minimum. Fix: confirm the language list against the actual patient cohort (not just what design-system tokens exist); if Spanish is not available at launch, that is a patient-access decision worth naming explicitly, not silently deferring.
- **[usability | node: 3497:931 (desktop flyout frame)]** The desktop flyout appears to mirror the mobile menu structure rather than taking advantage of desktop's ability to show top-level items persistently in a horizontal bar. A patient on desktop who wants to check shipping should not have to click to open a flyout — the first-order destinations should be one click, not two. Fix: on desktop, promote Track / Refills / Account to the top bar and reserve the flyout for secondary items.
- **[usability]** (flow-level, not anchorable) Sign-in state is unclear from the canvas. The nav must behave differently for a not-yet-signed-in returning patient (who arrived from an SMS but did not auth) vs. an authed patient. An unauth patient clicking "Track" and hitting a login wall with no context is a classic silent-failure moment. Fix: spec the unauth state and the re-auth pattern as first-class design work in this file, not a follow-up.
- **[content+design | node: 3497:931 (mobile open state)]** No visible "Contact support" or "Chat" entry. The LDZ global workflow shows the entire PCR swim lane exists precisely because patients escalate via chat or phone. A nav that does not put support one tap away forces the patient to hunt in a moment of confusion — exactly when self-service fails (Tesler's Law — complexity cannot be eliminated, only moved; hiding support from the nav moves complexity onto the patient). Fix: persistent Help / Chat entry, ideally pinned at the bottom of the open mobile panel.

**Patient-visibility check:**
- **Can a confused patient self-serve here?** Partial — a returning patient can find their way back to the portal, but without a clear Track / Refills / Help trio in the nav they will still end up calling a PCR for things the nav should resolve in one tap.
- **Does this design surface status or hide it?** Mixed — the nav gives a patient a place to come back to, but no component in these states visibly carries status (order in flight, refill due, prescription rejected). Status belongs in the nav shell (badges, dot indicators) for patients who open the portal specifically to check "where are things."
- **Where does friction compound downstream?** If the nav does not name Track, patients default to calling PCRs → contact rate rises (an owned OKR) → and the patients who do not call simply exit, which looks like silent persistence loss. The nav is a lever on two OKRs at once; underbuilding it loses both.

**Next actions:**
1. Add a dedicated Track / Order Status entry (with status badging) to the mobile open and desktop states — treat this as the single highest-leverage change for patient outcomes, directly resolving Edward's flagged gap.
2. Elevate Refills from a nested item to a top-level nav entry with visible state (due / upcoming). This is where persistence lives.
3. Spec the unauth / re-auth state explicitly — a returning patient from an SMS link who is not signed in is the most common real entry point and needs its own designed path, not a generic login wall.
4. Add a persistent Help / Chat affordance in the open nav. Pair with the existing PCR / ZenDesk flow so the handoff is a continuation, not a restart.
5. Confirm language coverage against the actual patient cohort (Spanish at minimum for LDZ) and name the patient-access implication if any locale is deferred.
6. Annotate the canvas with the patient scenarios the nav is solving for — makes the intent durable for the next reviewer and ties the work back to "start sooner, abandon less, persist longer."

---

## Commercial Lens

**Top-line verdict:** This is a navigation system designed end-to-end for lillydirect — logo baked into every frame, "Example Problem" anchored in a Lilly header, no tenant abstraction visible. The interaction model (persistent drawer, language toggle, mobile/desktop parity, dark mode) is platform-worthy, but the way it is packaged reads as a partner-specific asset, not a multi-tenant nav system. That is a concentration move dressed as platform work, and it matters because nav is one of the first surfaces a new manufacturer sees during onboarding.

**Commercial stakeholder(s) this touches:** Pharma client admin (brand controls), Program Manager (onboarding new tenants in < 24 hours), end-patient in a multi-tenant context (whose brand they expect to see at checkout), Account Manager (arguing land/expand with non-Lilly logos).

**What's working:**
- Persistent chat affordance and language toggle at the nav level directly address two of the most common patient friction points in the access gauntlet (abandonment at checkout, language barriers that correlate with non-persistence). Putting these on the top-level nav instead of hiding them in footers or modals is a commercial win for "abandon less."
- ES/EN parity modeled in both mobile and desktop ("Mobile Open: ES", "Desktop: ES", language submenu) is the right bet: the documented 80% cost-related abandon reason often compounds with language friction. Building bilingual into the nav primitive (not bolted on later) keeps us competitive against DTC players like Ro and BlinkRx who under-invest in Spanish experiences.
- Mobile-first framing with checkout-aware nav ("Nav Bar - Checkout") shows the designer understood that this nav lives inside the highest-commercial-leverage flow — the checkout moment where patients are documented to be "lost to Walmart/LillyDirect." Keeping the nav slim and chat-adjacent here is correct.
- Dark-mode variant ("Desktop Closed"/"Desktop Open" dark, node 3541:137687 / 3541:138458) future-proofs the design system investment against partners who will demand brand theming. Good platform instinct.
- The "Example Problem" framing (node 3541:3616) is a healthy design practice — shows the current-state friction before the proposed fix. That is useful for an Account Manager who needs to tell a before/after story to a prospective logo.

**What's not:**
- **[Severity: content+design | node: 3545:7501]** Every canvas of the "More Robust Nav System" section renders the lillydirect logo (3602:471, 3545:6904, 3604:852, 3545:6987, 3604:1091, etc.) — the nav artifact and the Lilly brand are fused. This affects **multi-tenant readiness** and the **land motion**. Fix: show this nav in at least two tenants (a Lilly frame + a generic / second-manufacturer frame) with the logo as a swappable token slot. The artifact should demonstrate the template, not a single instance of it.
- **[Severity: content+design | node: 3604:1091]** The desktop ES variant still carries the Lilly logo. For the next pharma we pitch, this canvas reads as "the nav we built for Lilly, now in Spanish." That is a **Lilly-renewal-neutral, next-logo-negative** positioning. Fix: pair every language variant with a non-Lilly tenant example so the language capability is clearly platform-native.
- **[Severity: content | node: 3541:135861]** The designer's own sticky note asks "Side menu on desktop and mobile. Always expose chat and language?" — that is an open commercial question, not a design detail. "Always expose" is a bet on digital containment (fewer support calls, cheaper ops) vs. real estate. Affects the **support-contact OKR** and **AI-agent transfer-rate OKR**. Fix: resolve with data — if chat is the containment lever, commit to always-expose; if language is the abandonment lever, same. Do not ship the ambiguity.
- **[Severity: content | node: 3541:3616]** The "Example Problem" frame uses a Lilly header to illustrate the current-state problem. Multi-tenant hygiene says the problem artifact should be shown in a partner-agnostic or masked form, otherwise it reads as "Lilly's nav is broken" rather than "our platform nav pattern is broken." Affects how this work gets shared in sales collateral. Fix: neutralize the header, or show the problem in two tenant skins.
- **[Severity: usability | node: 3604:847]** The "Mobile Open: EN Language Open" state nests a language submenu inside an already-open drawer. For a patient at checkout, that is two taps deep to change language. Patients who need Spanish are disproportionately the cost-abandoners. Affects the **checkout conversion OKR** directly. Fix: consider surfacing EN/ES as a single-tap segmented toggle at the top of the drawer, not a submenu.
- **[Severity: content]** No visible admin / configuration view. Account Manager and Program Manager cannot see how nav items, logo, chat visibility, or language set get configured per tenant. Affects **time-to-onboard** (< 24-hour promise) and the admin white-label template portal investment. Fix: add at minimum a schematic frame showing "nav config per tenant" — which slots are tokenized, which are tenant-controlled, which are platform-locked.
- **[Severity: content | node: 3602:469]** The "Nav Bar - Checkout" pattern shows only the logo and icon cluster — no visible status cue tying it to "start sooner" (e.g., delivery ETA, order status glance). Amazon/Zepbound ships same-day; if our nav during checkout says nothing about speed-to-therapy, we cede that emotional ground to the competitor. Fix: consider a nav-adjacent delivery/status indicator — even a subtle one — to anchor speed perception in the moment patients are deciding whether to switch to Walmart.
- **[Severity: content]** No logged-out / pre-enrollment state is shown. The nav spec is complete for authenticated checkout flows but silent on the first-touch patient surface, which is where Phil and AssistRx compete aggressively. Fix: extend the nav system to cover at least one pre-auth state so the "land" side of the patient journey has parity.

**Platform-vs-partner check:**
- **Is this platform-native or partner-specific?** Mixed leaning partner-specific — the *interaction model* is platform-worthy (states, language, dark mode, mobile/desktop parity); the *asset presentation* is partner-specific (every frame wears Lilly paint).
- **Can a new pharma partner adopt this in < 24 hours?** With work. The component structure (Nav Bar → Main → Icons → logo instance) looks tokenizable, but there is no evidence of a tenant theme layer, no admin config surface, and no second-tenant render to prove portability. Program Manager onboarding a new logo today would still ask "what exactly swaps?"
- **Where does this cede ground to a named competitor?** **LillyDirect** — if LillyDirect builds pharmacy capability and we have invested design capital in a nav that visibly reads as "Lilly's nav," we accelerate their ability to absorb the pattern rather than moat against it. **Amazon/Zepbound** — the checkout nav has no speed-to-therapy cue, so the nav moment does nothing to counter the same-day delivery perception.

**Next actions:**
1. Re-render the core nav states in a **non-Lilly tenant skin** (generic "ACME Therapeutics" or a second manufacturer partner) alongside the Lilly versions. This single change converts the artifact from partner asset to platform evidence and is the highest-leverage commercial improvement.
2. Add an **admin / configuration frame** showing which nav elements are tenant-controlled (logo, brand color, optional items), platform-locked (chat availability, language toggle behavior), and how a Program Manager sees the pre-launch preview. This is the artifact Account Managers need for the "< 24-hour onboarding" pitch.
3. **Resolve the sticky-note question** (node 3541:135861) on always-exposing chat and language. Commit to a default backed by the support-contact and checkout-conversion OKRs; document the override path for tenants that want otherwise.
4. **Flatten the language toggle** (remove the submenu nesting on node 3604:847) so Spanish-speaking patients at checkout change language in one tap, not two.
5. Add a **pre-auth / logged-out nav state** and a **nav-level delivery/speed cue** in the checkout state to counter Walmart same-day and Amazon/Zepbound convenience signals.

---

## Craft Critic

**Top-line verdict:** Shape is right. The proposed nav cleanly fixes the "Example Problem" (icons colliding with the logo at narrow widths) by hiding actions behind a hamburger, and the layered disclosure for Language is the right pattern. But this is a LillyDirect-branded surface, not a Gifthealth one, so the brand-compliance section below judges craft against general best-practices (WCAG, Nielsen, Emil Kowalski) rather than the Gifthealth visual guidelines. Fidelity note: the artifact was inspected via the Figma MCP metadata and screenshots; `get_variable_defs` returned "nothing selected" so tokens vs. raw hex could not be audited — flag as low-confidence on that dimension.

**Context flag:** This is a partner/client surface (LillyDirect) and the critique frame asked for is craft. Craft is an appropriate lens here (it's patient-facing), but the Gifthealth palette/font rules in `brand-guidelines.md` do not directly apply — Lilly has its own brand. Judgment below is against what a well-run brand system should do, not enforcement of `#440b39` or Funnel Display on a Lilly canvas. If the intent is to eventually re-skin this pattern under Gifthealth, flag that in the follow-up and re-run the critique against the Gifthealth rules.

**What's working:**
- **The problem-statement frame is included on the canvas.** Showing the "Example Problem" (cramped icons colliding with the logo) next to the proposed solution is exactly right — it gives the reviewer the *why* without a deck. This is a craft move at the artifact level, not the pixel level.
- **Layered disclosure for Language.** Collapsing English/Español under a parent "Language: EN" row with a caret that rotates from down (3604:799) to up (3604:922) is the right application of **Progressive Disclosure** and **Recognition over Recall (Nielsen 6)** — the user sees their current language without having to open anything, and only loads the options when they want to change.
- **Caret direction flips on expand/collapse.** The down-caret in `Mobile Open: EN` (node 3604:799) and the up-caret in `Mobile Open: EN Language Open` (node 3604:922) give the user a correct **signifier** of state. This is a detail many teams miss.
- **Consistent 40px tap targets for nav rows, 24px icons, 6–8px icon-text gap.** These measurements are kind to Fitts's Law on mobile and match best-in-class hamburger patterns (Stripe, Linear, Shopify mobile).
- **Dark-mode variants are drawn.** Having both light and dark states of Mobile Closed/Open on the canvas (section 3545:7501) forces the contrast conversation early instead of at QA.
- **Both EN and ES are drawn.** Showing Spanish alongside English surfaces text-length overflow risk at design time, not in production. This is the right instinct for any multi-lingual surface.

**Brand compliance (LillyDirect, not Gifthealth):**
- **Palette:** Not audited — `get_variable_defs` returned empty. Visually the design reads as the familiar Lilly red wordmark on white, with neutral grays for the dimmed underlay. No approved-hex audit possible from this pass.
- **Typography:** Appears to use a single neutral sans throughout the nav. Weight hierarchy looks flat — parent nav rows, child language items, and "Sign out" all read at similar visual weight. See "What's not" below.
- **Gradients:** None used, which is correct for a utility nav.
- **WCAG contrast:** The visible text (dark gray on white) will pass AA comfortably. The "Sign out" underlined link at the bottom (node 3602:767) reads in a slightly muted gray that cannot be measured without a token dump — worth verifying the computed hex against white for AA (4.5:1 for body).

**Heuristic pass (Nielsen's 10) — risks only:**
- **#2 Match between system and real world:** "Sign out" vs. "Logout" — the Example Problem frame labels it "Logout" (node 3600:1482); the proposed nav labels it "Sign out" (nodes 3602:767, 3604:929). Pick one. This also trips **#4 Consistency and standards**.
- **#4 Consistency and standards:** The close control is an `X` glyph on the offcanvas header (nodes 3545:6934, 3604:882). On the desktop flyout variants further right on the canvas, the same concept should use the same icon, weight, and tap target. Worth spot-checking.
- **#6 Recognition over recall:** No visible active/selected state on the current page's row in either the collapsed or expanded nav. A user who opens the menu mid-flow should see which page they're on. This is also the **Von Restorff Effect** — the current page should stand out.
- **#3 User control and freedom:** The Language parent row (node 3604:916) when expanded still reads "Language: EN" with an up-caret — but the two children (English, Español) have no visible "selected" indicator (no check, no bold, no dot). The user cannot tell from the submenu alone which language is currently active; they infer it only from the parent row text. Add a selected-state treatment on the active child.
- **#1 Visibility of system status:** The hamburger trigger in Mobile Closed (node 3602:507) has no visible open/close state change drawn. When the drawer is open, the trigger usually morphs (hamburger to X) or highlights. The X lives inside the drawer instead — which works, but it means the header button disappears from the user's line of sight the moment they tap. Fine, but draw what happens to that trigger so engineering doesn't guess.

**Interaction quality (what an Emil Kowalski review would flag):**
- **No motion spec on the canvas.** A hamburger drawer is the single most animated element in this design, and the canvas shows only static states. An engineer reading this file will reach for `transition: all 300ms ease-in-out` by default, which is exactly the combination that makes drawers feel sluggish. You need either annotations or a Figma prototype link defining: enter = `transform: translateX(0)` from `translateX(100%)`, exit = same, duration 250–300ms, easing `cubic-bezier(0.32, 0.72, 0, 1)` (the iOS drawer curve), with the underlay opacity fading in parallel at 200ms.
- **Language submenu expansion needs a stated behavior.** Between `Mobile Open: EN` (node 3545:6929) and `Mobile Open: EN Language Open` (node 3604:877), the container grows by ~80px. If this animates `height: auto`, it will jank on iOS Safari. Spec it as a max-height transition to a fixed target or use `grid-template-rows: 0fr → 1fr`. The children (English, Español at nodes 3604:923, 3604:925) should also fade in with a slight stagger (30–50ms between them) rather than appear instantly; without stagger the whole block reads as mechanical.
- **No `:active`/pressed state on nav rows.** Every nav row (nodes 3602:732, 3602:737, 3602:745, 3602:759, 3604:889, etc.) needs a pressed state — a 0.97 scale or a background tint on `:active`. Without it, the row will feel dead under the finger on mobile, which is the single loudest "this app is cheap" tell on a phone.
- **Caret rotation between closed and open states.** The caret flip (3604:799 down → 3604:922 up) should tween, not swap. A 150ms `ease-out` rotation on the same glyph reads as one continuous affordance; two different triangle assets read as a state swap. Make sure engineering gets one asset with `transform: rotate(180deg)` on open, not two separate polygons.

**What's not:**
- **[usability | node: 3604:915]** Active language not distinguished in the submenu — When the user expands Language, both "English" (3604:923) and "Español" (3604:925) render at identical weight and color. Violates Nielsen 6 (Recognition over Recall) and robs the user of a "you are here" cue. Fix: bold the active option, or add a leading checkmark icon in the same size as the globe (24px) at the left edge, aligned with the other icon column.
- **[content+design | node: 3602:767]** Inconsistent auth verb — "Sign out" here and "Logout" in the Example Problem header (node 3600:1482). Violates Nielsen 2 and 4. Fix: align on one verb everywhere and update the component library. "Sign out" is the current industry standard (Apple, Google, Stripe) and reads more human than "Logout."
- **[usability | node: 3545:6938]** No active-page indicator on nav items — None of the nav rows (Prescriptions, Orders, Account, Language) show which is the current page. Violates Nielsen 6 and loses the Von Restorff Effect. Fix: apply a 2–3px left border in the Lilly red brand color, or a light gray background fill, on the active row. Match whatever the design system already uses for selected list items.
- **[content+design]** Offcanvas behavior not specified — The drawer slides in from the right over a dimmed underlay (300px wide, 90px of logo left visible per nodes 3545:6930 x=90). Behavior spec is missing: does tapping the dimmed area close? Does Escape close? Does swipe-right close on touch? Fix: annotate on the canvas, or link a prototype. These decisions cost an engineer a day if left ambiguous.
- **[content+design | node: 3604:915]** Submenu expansion should animate, not pop — The 80px height change between collapsed and expanded states will feel abrupt without a transition. Fix: spec `grid-template-rows: 0fr → 1fr` on the submenu wrapper with 200ms `cubic-bezier(0.32, 0.72, 0, 1)`; stagger child fade-ins by 40ms.
- **[content+design | node: 3602:765]** "Sign out" placement unconventional — Sign out is a row inside the same list as Prescriptions/Orders/Account, visually separated only by being the last item without an icon. Violates Nielsen 4 (consistency) and misses an opportunity to signal destructiveness. Fix: either (a) anchor Sign out to the bottom of the drawer with a divider above it, following the Gmail/Notion pattern, or (b) give it the same icon treatment as the other rows with an `arrow.right.square` glyph, so the list-item pattern is consistent.
- **[usability | node: 3602:507]** Hamburger trigger has no open-state drawn — When the drawer is open, the original hamburger button either (a) stays put and is now a no-op or (b) morphs into an X. Neither behavior is specified. Fix: pick one; best-in-class is to swap to an X with a 150ms crossfade and keep it at the same coordinate so tap memory carries.
- **[content+design]** Motion spec missing entirely — No easing curves, no durations, no stagger called out. An engineer defaulting to `transition: all 300ms ease-in-out` will produce a drawer that feels sluggish (ease-in delays the initial movement, which is the moment the user is watching). Fix: add a motion annotation frame on the canvas — drawer enter/exit 280ms with `cubic-bezier(0.32, 0.72, 0, 1)`, underlay fade 200ms `ease-out`, submenu height 200ms same curve, row press `scale(0.98)` 100ms `ease-out`.
- **[content+design | node: 3545:6930]** Drawer width fixed at 300px — On a 320px phone (SE, older Androids), this leaves only 20px of the underlay visible, which makes the "tap outside to close" gesture nearly impossible. Fix: spec as `min(300px, calc(100vw - 44px))` so there is always at least a 44pt tap-to-dismiss zone.

**Next actions:**
1. Add a motion-spec annotation frame to the canvas covering: drawer enter/exit (280ms, ease-out drawer curve), underlay fade, submenu expand, caret rotate, nav-row `:active` scale. Without this, engineering will guess and the result will not feel like the design intends.
2. Decide "Sign out" vs. "Logout" once, update the Example Problem frame (node 3600:1482) and the proposed nav (nodes 3602:767, 3604:929), and push the canonical string into the component library.
3. Draw an active/selected state for nav rows and apply it on at least one frame so the pattern is reviewable. This is the biggest usability gap in the set.
4. Add a selected-state treatment on the active language child (bold, or a leading checkmark) in node 3604:915, and confirm the same pattern is used anywhere else a submenu exists.
5. Responsive check: re-draw Mobile Open EN at 320px width to confirm the drawer still leaves a usable dismiss zone, and that ES text does not overflow inside the 267px body column (node 3604:885).
6. If this pattern is intended to migrate to a Gifthealth-branded surface later, re-run this critique against `brand-guidelines.md` (Purple `#440b39`, Funnel Display, no sharp gradient edges) once the re-skin begins — the Lilly-red assumptions in this pass would not survive that.
