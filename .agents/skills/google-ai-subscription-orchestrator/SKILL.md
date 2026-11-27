---
name: google-ai-subscription-orchestrator
description: >-
  Scan a user's live Google AI subscription (Google One / Google AI Pro / Ultra)
  via the built-in browser to inventory all active benefits, unclaimed perks, and
  quotas, then orchestrate and use those surfaces (Gemini Web, Google AI Studio,
  Google Flow, NotebookLM, Google Colab) through browser automation to build,
  develop, or create whatever the user requests. Use when the user asks to audit
  their Google AI subscription benefits, check remaining quotas/credits, or use
  their Google AI plan surfaces from Antigravity.
---

# Google AI Subscription Orchestrator

A generic, account-agnostic skill that turns Antigravity into a central
coordinator for your entire Google AI subscription. It first audits your live
account to discover every included benefit and quota, then routes creative,
research, and developer tasks across Google AI surfaces using the built-in
`browser` subagent.

## Prerequisites & Guardrails

- **Authentication (Single Sign-In & Session Reuse)**:
  - Only ask the user to sign in **once**. Once the user signs into their
    Google account in the browser pane, related Google surfaces
    (`gemini.google.com`, `aistudio.google.com`, `labs.google`,
    `notebooklm.google.com`, `colab.research.google.com`) normally share that
    session automatically.
  - Always navigate to and verify whether a surface is already signed in before
    asking the user again; only pause to ask if a specific product genuinely
    still presents an `accounts.google.com` sign-in or CAPTCHA/2FA gate, and
    then resume the *same* `browser` subagent via `send_message`. Never attempt
    to bypass login gates or bot checks.
- **Account-agnostic**: Never hardcode emails, account names, or region-specific
  assumptions. Always read live state from the user's active browser session.
- **Confirm before spending**: Before spending finite credits (e.g., Google Flow
  video credits, Google Cloud/API credits, or Colab compute units), state the
  intended action and estimated credit usage if non-trivial.
- **Responsible Use & ToS Compliance**:
  - Use browser automation only for human-paced, single-account, user-initiated
    tasks on your own subscription.
  - Never run unthrottled loops, bulk scraping, or rapid retries after
    rate-limit errors on consumer web UIs (`gemini.google.com`, `labs.google`,
    etc.).
  - For programmatic, high-volume, or app-backend workloads, use the official
    **Gemini API** (via Google AI Studio / Google Cloud credits) rather than
    automating consumer web interfaces.
  - Never rotate multiple accounts to multiply quotas or resell web-session
    access.

---

## Workflow

### Phase 1: Scan Subscription & List All Benefits

When the user asks to scan their subscription, find missed benefits, or start
using their Google AI plan:

1. **Delegate to the `browser` subagent** (pass `Model: "flash"` unless the
   user requested another model) to inspect the user's Google One & AI surfaces.
   Read [surfaces-and-urls.md](./references/surfaces-and-urls.md) for the exact
   URLs and extraction checklist.
2. **Audit these primary endpoints** (skipping any the user says they don't use):
   - **Google One Benefits & Plan**: `https://one.google.com/benefits` and
     `https://one.google.com/settings` (plan tier, storage, family sharing,
     YouTube/Home/Health perks, unclaimed promo offers).
   - **Gemini Web**: `https://gemini.google.com` (available models, Deep
     Research, Canvas, Gems, image/video tools).
   - **Google AI Studio**: `https://aistudio.google.com` (available models,
     Playground/Build access, API key & monthly Cloud/Developer Program credits).
   - **Google Flow**: `https://labs.google/fx/tools/flow` (remaining monthly
     Flow Credits, Veo/image/audio model access).
   - **NotebookLM**: `https://notebooklm.google.com` (Pro/Plus badge, notebook
     and source limits, Audio Overview availability).
   - **Google Colab** *(optional/if developer)*: `https://colab.research.google.com`
     (remaining compute units / GPU tier).
3. **Present a structured Benefits & Quota Inventory** to the user:
   - **Plan Tier & Billing Status**
   - **Active AI Surfaces & Quotas** (with remaining credits/limits where visible)
   - **Unclaimed / Overlooked Benefits** (e.g., monthly Developer Program API
     credits, Google One perks, Workspace Gemini, storage, YouTube/Home/Health)
   - **Quota-Grounded Options & Recommended Next Steps** *(strictly conditional)*:
     - **Rule**: Only recommend surfaces, models, and features that are
       **actually unlocked and have available quota** in the user's scanned
       account. **Never** recommend locked models, unavailable regional tools,
       or paid compute features when the user's balance is `0` (e.g., do not
       recommend Colab Pro GPU runs if Colab shows `0 compute units`).
     - **Antigravity as Orchestrator + Gemini Web & AI Studio Offloading**:
       Explain that `gemini.google.com` and `aistudio.google.com` often carry
       much larger quotas than Antigravity alone, so Antigravity can act as the
       **orchestrator & local builder** while offloading heavy reasoning, Deep
       Research, Canvas drafting, prompt prototyping, and content/code
       generation to `gemini.google.com` and `aistudio.google.com` via the
       built-in browser.
     - **Google Flow (`flow.google.com`)** *(only if Flow Credits > 0)*: Ask if
       the user wants to create **images** or **videos**, and offer concrete
       ideas (e.g., product/app promo videos, hero visuals, storyboards) using
       their available Flow models and credits.
     - **Gemini Notebook (`notebook.google.com`)** *(if quota available)*: Give
       actionable ideas based on their unlocked Studio tools (e.g., generating
       an **Audio Overview**, **Video Overview**, **Slide deck**, **Mind Map**,
       **Report**, **Flashcards**, or **Infographic** from their docs/URLs).
     - **Google Colab (`colab.research.google.com`)**: Only suggest paid GPU or
       compute-unit workloads if the scan confirmed active Pro/Pro+ or
       `compute units > 0`; otherwise note free-tier limits or omit heavy
       compute recommendations.

---

### Phase 2: Route User Tasks Across Google AI Quotas

When the user asks to build, develop, research, or create something using their
subscription, decompose the goal and route each piece to the surface where it
best fits and where quota is available:

| Task Type | Best Surface | Why |
| :--- | :--- | :--- |
| Orchestration, codebase edits, architecture, terminal, tests | **Antigravity (Local)** | Central orchestrator with direct workspace file & shell access |
| Heavy reasoning, Deep Research, Canvas drafts, offloading Antigravity quota | **Gemini Web** (`gemini.google.com`) | Large Pro/Ultra quota for Deep Research, Canvas & long-context work |
| Prompt prototyping, structured outputs, model comparison, AI Studio Build | **Google AI Studio** (`aistudio.google.com`) | High rate limits, Playground, Build apps, and API credits |
| Video, cinematic scenes, storyboards, image/audio assets | **Google Flow** (`flow.google.com`) | Uses monthly Flow Credits (when balance > 0) |
| Multi-document synthesis, Audio/Video Overviews, Slide decks, Mind Maps | **Gemini Notebook** (`notebook.google.com`) | High-capacity grounded RAG & Studio outputs |
| Heavy Python/ML training or GPU experiments | **Google Colab** (`colab.research.google.com`) | Only when active compute units > 0 or Pro/Pro+ is verified |

---

### Phase 3: Execute via Built-in Browser & Sync to Workspace

1. **Read [browser-playbooks.md](./references/browser-playbooks.md)** for the
   surface-specific `browser` subagent prompt template and completion criteria.
2. **Launch or reuse a `browser` subagent** for each target surface with a
   clear, verifiable completion condition (e.g., *"Generate the asset, download
   it, and report the downloaded file path"*, or *"Run the prompt in AI Studio
   and return the full response text / exported code"*).
3. **Bridge outputs back into Antigravity**:
   - Copy or move downloaded media (videos, images, audio) or exported files
     from the browser download location into the active workspace.
   - Integrate generated code, research summaries, or prompts directly into
     the user's project files.
4. **Update quota awareness**: After each heavy generation (e.g., Flow credits
   or Colab units), note the updated balance if visible in the UI so the user
   always knows what remains.

---

## Verification

Before concluding a scan or build task:
- Confirm all reported benefits and quota numbers came directly from the live
  `browser` subagent inspection (verified against `BROWSER_HOST_FACTS`), never
  from guesswork.
- Confirm any generated assets or code from web surfaces are saved in the
  user's workspace and linked with clickable `file://` links.
