# AI Content Repurposing & Publishing Pipeline

An end-to-end **n8n automation** that takes a single source link (an article or a YouTube video) and turns it into platform-ready content — with a human approval checkpoint before anything goes live.

No manual copy-pasting, no forgetting to check for duplicates, no publishing without sign-off. One form submission in, one approved LinkedIn post out.

---

## ✨ Key Features

- **Single intake point** — one form handles both article links and YouTube links
- **Duplicate detection** — content that's already been processed is caught and skipped automatically
- **Source-aware extraction** — articles are scraped and parsed; YouTube videos have their transcript pulled, based on the link type
- **AI-generated content briefs** — a local LLM (Ollama) analyzes the normalized content and produces a structured brief
- **AI-generated platform drafts** — a second AI step turns the brief into ready-to-publish drafts
- **Full audit trail** — every piece of content and its draft is logged to Airtable
- **Human-in-the-loop approval** — nothing publishes without a real approval decision made in Slack
- **Automatic publishing** — approved content is posted directly to LinkedIn, with status and notifications handled automatically

---

## 🧭 How It Works

1. **Content Intake Form** — submit a source URL
2. **Check for Duplicate** → **Is New Content?** — stops early and sends a duplicate notice if the content already exists
3. **Route by Source Type** — branches based on whether the link is an article or a YouTube video
   - Article path: **Fetch Article Page** → **Extract Article Text**
   - YouTube path: **Fetch YouTube Transcript**
4. **Normalize Content** — both paths converge into one consistent content format
5. **Analyze Content** — an AI step (Ollama chat model + brief parser) reads the content and produces a structured brief
6. **Generate Platform Drafts** — a second AI step converts the brief into platform-specific drafts
7. **Assemble Record** — compiles the brief, drafts, and metadata into a single record
8. **Save Draft to Airtable** → **Update record** — persists everything for tracking and review
9. **Request Approval in Slack** — a reviewer is asked to approve or reject the draft
10. **Approved?**
    - **Approved** → Mark Approved → **Publish to LinkedIn** → Mark Published → Notify Published
    - **Rejected** → Mark Rejected → Notify Rejected

---

## 🖼️ Workflow Diagram

![Workflow diagram](docs/workflow-diagram.png)

*(Add the exported workflow screenshot to a `docs/` folder in the repo and it will render here.)*

---

## 🛠️ Tech Stack

| Layer | Tool |
|---|---|
| Orchestration | [n8n](https://n8n.io/) |
| AI / LLM | Ollama (local model) |
| Content storage & tracking | Airtable |
| Human approval layer | Slack |
| Publishing target | LinkedIn API |

---

## ✅ Why the Approval Step Matters

The AI can draft, route, and format content all day — but nothing ships without a person confirming it's actually good. The Slack approval gate is what makes the rest of the automation safe to run unattended: it keeps a human decision at the one point that actually matters (what goes out publicly), while automating everything around it.

---

## ⚙️ Setup

1. Import the workflow JSON into your n8n instance
2. Configure credentials for:
   - Airtable (API key + base/table IDs)
   - Slack (bot token / webhook, and the channel for approvals)
   - LinkedIn (API credentials for publishing)
   - Ollama (local endpoint URL and model name)
3. Update the Airtable field mappings in the **Save Draft to Airtable** and **Update record** nodes to match your base schema
4. Publish the **Content Intake Form** trigger and share the link to start submitting content

---

## 📌 Status

Fully working end-to-end — from intake to duplicate check, AI drafting, human approval, and LinkedIn publishing.

---

## 📄 License

MIT
