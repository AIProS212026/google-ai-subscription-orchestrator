# Google AI Subscription Surfaces & Discovery URLs

Use this checklist when instructing the `browser` subagent to scan a user's
Google AI subscription. Plans, naming, and regional perks evolve over time—always
report **only** what is visibly shown on the user's live pages.

---

## 1. Google One (Plan, Storage & Ecosystem Perks)

- **URLs**:
  - `https://one.google.com/benefits`
  - `https://one.google.com/settings`
  - `https://one.google.com/storage`
- **What to extract**:
  - Exact plan name (e.g., Google AI Pro, Google AI Ultra, Google One AI Premium, or Free) and renewal/billing cadence if shown.
  - Storage used vs. total storage quota (e.g., 2 TB, 5 TB, 10 TB).
  - Family sharing status (whether benefits are shared or shareable).
  - Listed benefits & unclaimed perks: YouTube Premium / Lite, Google Home Premium, Google Health / Fitbit perks, Google Workspace Gemini (Gmail, Docs, Sheets, Slides, Vids), Google Store discounts, or partner trials.

## 2. Gemini Web

- **URL**: `https://gemini.google.com`
- **What to extract**:
  - Account badge / plan indicator (e.g., Pro / Ultra / Advanced).
  - Available models in the model selector dropdown.
  - Available tools/modes: Deep Research, Canvas, Gems, image generation, video generation, or Tasks.
  - Any visible usage limit notices.

## 3. Google AI Studio

- **URLs**:
  - `https://aistudio.google.com`
  - `https://aistudio.google.com/apikey` (or API keys / billing / usage section)
- **What to extract**:
  - Available frontier models in the Playground model selector.
  - Access to **Build** / app prototyping features.
  - Any banner or billing indicator for **Google Developer Program / monthly Google Cloud API credits** (e.g., $10/mo promotional credit) and whether it has been claimed/linked.
  - Tier status on API keys (Free vs. Pay-as-you-go / Pro-linked).

## 4. Google Flow (Creative & Video Studio)

- **URL**: `https://flow.google.com` (or `https://labs.google/fx/tools/flow`)
- **What to extract**:
  - Remaining **Flow Credits** balance and monthly refresh amount if shown.
  - Available generation modes (Text-to-Video / Veo, Image-to-Video, Scene Builder, Image/Audio tools).
  - Regional availability status (if unavailable in the user's region, note it clearly).

## 5. Gemini Notebook / NotebookLM

- **URL**: `https://notebook.google.com` (or `https://notebooklm.google.com`)
- **What to extract**:
  - Plan badge (e.g., Plus / Pro) if displayed.
  - Notebook count and any visible source-per-notebook or Audio Overview limit indicators.

## 6. Google Colab (Developer Compute)

- **URL**: `https://colab.research.google.com`
- **What to extract**:
  - Account tier / Compute Units balance (click the RAM/Disk or account/plan status if needed, without starting unnecessary runtimes).
  - Available GPU/TPU accelerator options.
