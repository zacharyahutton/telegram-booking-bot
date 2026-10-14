# Telegram Booking Bot

A Telegram bot for bookings, support tickets and FAQs, with an optional LLM chat mode. It runs with polling locally or as a FastAPI webhook server in production (Docker, Railway).

## Why I built it

I built this to practise the parts of a business chat bot that are easy to get wrong: multi-step flows that keep state between messages, a webhook server that can run on a small host, and a database that survives restarts.


---

## What it is

The bot (running as **Tendem Demo Bot**) shows how a business messaging bot can combine structured workflows (booking, support, FAQ) with an AI assistant that remembers your conversation, and escalates to human-style flows when needed.

Send `/start` to open the **Main Hub**: a branded home screen with quick actions, category navigation, and a persistent reply keyboard for one-tap access.

---

## Features

- **Main Hub**: Branded welcome, inline menus, and pinned shortcuts (Main Hub, Ask AI, Book, Support)
- **AI assistant**: Chat with the assistant; session memory stored per user (`/ai` or **Ask AI**)
- **Knowledge base**: 30+ curated FAQ answers across 8 categories; browse by topic or type a question for keyword matching (`/faq`)
- **Appointment booking**: 3-step flow: service → date → time slot → confirmation card (`/book`)
- **Support tickets**: 3-step flow: category → description → priority → ticket ID (`/support`)
- **Tools**: Tip of the day, UTC server time, quick calculator, bot health check
- **Settings**: Clear AI memory, view account info (`/settings`, `/forget`)
- **Capability tour**: 60-second walkthrough of all bot features (`/review`)
- **Privacy controls**: `/forget` removes your AI chat history from this demo instance

---

## Commands

| Command | What it does |
|---------|--------------|
| `/start` | Welcome message + Main Hub |
| `/menu` | Return to Main Hub |
| `/about` | Brand story and what this demo proves |
| `/review` | 60-second capability tour |
| `/ai` | Enter AI assistant mode |
| `/faq` | Browse the knowledge base |
| `/book` | Start appointment booking |
| `/support` | Open a support ticket |
| `/tools` | Utilities (tips, time, calculator, health) |
| `/settings` | Preferences and account info |
| `/forget` | Clear your AI chat memory |
| `/health` | Bot health check |

**Reply keyboard shortcuts:** Main Hub, Ask AI, Book, Support

---

## Quick walkthrough

1. **`/start`**: See the branded welcome and Main Hub with inline buttons and reply keyboard.
2. **`/review`**: Read the guided 60-second tour of all capabilities.
3. **`/book`**: Pick a service (Consultation, Demo walkthrough, Technical review, Onboarding call) → enter a date (`YYYY-MM-DD`) → choose a time slot → receive a confirmation card with booking ID.
4. **`/support`**: Choose a category (Billing, Technical, Account, Integration, Other) → describe the issue → set priority (Low / Normal / Urgent) → receive a ticket ID.
5. **`/ai`**: Ask a product or troubleshooting question; the assistant remembers recent messages in your session.
6. **`/faq`**: Browse 8 categories or type a question (try searching for *whatsapp* or *discord* for cross-platform notes).
7. **`/about`**: Background on the demo and the patterns it showcases.

**Suggested order:** `/review` → `/book` → `/support` → `/ai`

---

## Tech stack

Python, python-telegram-bot, FastAPI, SQLite, Groq/OpenAI

---

## Author

**Zachary Hutton**: Portmore, Jamaica  
GitHub: [github.com/zacharyahutton/telegram-booking-bot](https://github.com/zacharyahutton/telegram-booking-bot)

---

Developer setup: see [DEPLOY.md](./DEPLOY.md)

## How it works

- `bot/handlers/core.py` registers the commands and runs the booking and support flows as small step-by-step conversations.
- `bot/keyboards/menus.py` builds the inline menus and the reply keyboard.
- `bot/database.py` stores users, bookings, tickets and chat memory in SQLite.
- `bot/services/llm.py` calls the LLM (Groq or OpenAI) for the chat mode, with the recent conversation as context.
- `bot/content/` holds the FAQ entries and bot copy, kept separate from the logic.
- `run_polling.py` starts the bot locally. `run_webhook.py` and `api/webhook_server.py` serve the same handlers behind FastAPI for production.

## Project structure

```
api/          FastAPI webhook server
bot/          handlers, keyboards, services, content, database, config
run_polling.py, run_webhook.py
Dockerfile, railway.toml
```

## Roadmap

- Calendar export for confirmed bookings
- Admin view for bookings and tickets
- Tests for the booking and support state machines
