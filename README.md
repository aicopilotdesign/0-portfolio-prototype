# Design Engineer Portfolio

A chat-driven portfolio website powered by a Claude reasoning agent with tool use. Visitors interact through a conversational UI that dynamically renders project case studies, professional experience, and talks.

## Architecture

```
index.html          Single-page app (Tailwind CDN, vanilla JS)
server.js           Express server + Claude agent with agentic tool loop
data/
  portfolio.json    All project, profile, and talk data
.env                Anthropic API key
```

### Agent Tools

The backend uses Claude as a reasoning agent with four tools. Claude decides which tool to call based on user intent:

| Tool | Trigger | UI Effect |
|------|---------|-----------|
| `show_professional_experience` | "About Han", background questions | Chat slides left 30%, canvas shows bio/stats/skills |
| `list_projects` | "Design Projects" | Project list in chat + project name chips |
| `show_project` | Specific project name | Project card in chat + full case study in canvas |
| `show_youtube_talk` | Talks, presentations | Chat slides left 30%, canvas shows YouTube embed |

### Layout States

- **Initial** — Chat centered (max-width 600px) with hero text and two choice chips
- **Split** — Chat 30% left, Canvas 70% right (triggered by profile, project detail, or talk)
- **Project Card** — Rich card rendered inline in chat messages, clickable to open detail

### API Response Format

```json
{
  "reply": "Conversational text from Claude",
  "ui_action": "show_canvas | show_project_card | show_project_list | none",
  "ui_data": {},
  "chips": [{ "label": "...", "message": "..." }]
}
```

## Setup

### Prerequisites

- Node.js 18+
- An [Anthropic API key](https://console.anthropic.com/)

### Install & Run

```bash
npm install
```

Add your API key to `.env`:

```
ANTHROPIC_API_KEY=sk-ant-...
```

Start the server:

```bash
npm start
```

Open **http://localhost:3000**

## Design System

**Neon Monolith** — dark charcoal surfaces with high-contrast neon pink accents.

- Primary Glow: `#ff2d78`
- Background: `#131313`
- Surface: `#1c1b1b` / `#2a2a2a`
- Fonts: Sora (headlines), Manrope (body), Epilogue (display)

See [DESIGN.md](../DESIGN.md) for the full design token reference.

## Tech Stack

- **Frontend**: Tailwind CSS (CDN), vanilla JavaScript, Material Symbols
- **Backend**: Express.js, Anthropic SDK (Claude Sonnet)
- **No build step** — edit and reload
