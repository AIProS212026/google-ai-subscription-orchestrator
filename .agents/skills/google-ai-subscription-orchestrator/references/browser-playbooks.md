# Browser Subagent Playbooks for Google AI Surfaces

When delegating work to the `browser` subagent, pass `Model: "flash"` (unless the
user specified another model) and give a clear, single-surface or well-scoped
completion condition.

Only ask the user to sign in **once**—after signing into Google, related Google
products normally share the same session. Always navigate to each surface and
verify if it is already signed in before asking the user again. If a surface
genuinely still pauses for sign-in, 2FA, or a CAPTCHA/bot check, ask the user
to complete it in the browser pane and then resume the **same** subagent with
`send_message`.

---

## 1. Subscription Scan Playbook

Prompt template for `invoke_subagent` (`TypeName: "browser"`):

> Visit the following Google AI subscription pages in the user's logged-in browser session and report what is visibly active on each page. If any page asks for Google sign-in or verification, pause immediately and ask the user to sign in.
>
> 1. `https://one.google.com/benefits` and `https://one.google.com/settings` — read the exact plan name, storage usage/limit, and every listed benefit or unclaimed perk.
> 2. `https://gemini.google.com` — check the plan badge, open the model selector to list available models, and note available modes (Deep Research, Canvas, Gems, etc.).
> 3. `https://aistudio.google.com` — check available models in the model dropdown, whether Build mode is available, and check the API keys / usage section for any monthly Google Cloud / Developer Program credits or tier status.
> 4. `https://flow.google.com` (or `https://labs.google/fx/tools/flow`) — check if Flow is accessible in this region, read the remaining Flow Credits balance, and list available generation tools.
> 5. `https://notebook.google.com` (or `https://notebooklm.google.com`) — check for any Pro/Plus badge, usage limits, and Studio tools.
> 6. `https://colab.research.google.com` — check the user's compute units or Pro status if visible without launching a runtime.
>
> Return a structured summary of exact observed text/numbers for each page. Do not guess any missing values.

---

## 2. Gemini Web Playbook (`gemini.google.com`)

Best for: Deep Research reports, Canvas document/code drafting, brainstorming.

- Instruct the `browser` subagent to:
  1. Select the desired model (and mode, e.g., **Deep Research** or **Canvas**).
  2. Submit the prompt and wait until generation completes (streaming finishes).
  3. Copy or extract the full response text (or export/copy Canvas content) and return it to Antigravity.

---

## 3. Google AI Studio Playbook (`aistudio.google.com`)

Best for: Testing frontier models, structured JSON outputs, system-instruction tuning, or using AI Studio **Build**.

- Instruct the `browser` subagent to:
  1. Open Playground (or Build).
  2. Select the target model and configure system instructions / temperature / structured output schema if specified.
  3. Run the prompt, wait for completion, and return the full output or generated code.
  4. If claiming monthly API credits or creating an API key, **stop before revealing or copying secret API keys into logs**—instead let the user copy/store the key securely if needed.

---

## 4. Google Flow Playbook (`flow.google.com`)

Best for: Video generation (Veo), scene/storyboard creation, image/audio assets.

- Before launching, confirm with the user if the action will spend Flow Credits.
- Instruct the `browser` subagent to:
  1. Read the current Flow Credits balance before generating.
  2. Enter the user's prompt, select the requested aspect ratio/model/duration.
  3. Wait for rendering to complete, download the resulting `.mp4` / `.png` / audio file, and report the downloaded filename and updated Flow Credits balance.
- Once downloaded, move or copy the file into the user's workspace and link it.

---

## 5. Gemini Notebook / NotebookLM Playbook (`notebook.google.com`)

Best for: Grounded Q&A over large document sets, study guides, Audio Overviews.

- Instruct the `browser` subagent to:
  1. Open or create the target notebook.
  2. Upload/add the specified sources (for local files, stage them in `<Artifact Directory Path>/browser_sandbox/uploads/<filename>` first).
  3. Run the query or trigger Studio generation (e.g., Audio Overview / Briefing Doc) and return or download the result.
