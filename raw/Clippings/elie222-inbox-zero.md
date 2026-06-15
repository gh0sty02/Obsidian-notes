---
title: "elie222/inbox-zero: The world's best AI personal assistant for email. Open source app to help you reach inbox zero fast."
source: "https://github.com/elie222/inbox-zero"
author:
published:
created: 2026-06-16
description: "The world's best AI personal assistant for email. Open source app to help you reach inbox zero fast. - elie222/inbox-zero"
tags:
  - "clippings"
---
[![](https://github.com/elie222/inbox-zero/raw/main/apps/web/app/opengraph-image.jpg)](https://www.getinboxzero.com/)

## Inbox Zero - your 24/7 AI email assistant

Organizes your inbox, pre-drafts replies, manages your calendar, and organizes attachments. Chat with it from Slack or Telegram to manage your inbox on the go. Open source alternative to Fyxer, but more customizable and secure.  
[Website](https://www.getinboxzero.com/) · [Discord](https://www.getinboxzero.com/discord) · [Issues](https://github.com/elie222/inbox-zero/issues)

[![elie222%2Finbox-zero | Trendshift](https://camo.githubusercontent.com/05f8a42321b85c943a82b0f2c1c5cd84473e16e6b1090034d2c899039bd9eedf/68747470733a2f2f7472656e6473686966742e696f2f6170692f62616467652f7265706f7369746f726965732f36343030)](https://trendshift.io/repositories/6400)

## Mission

To help you spend less time in your inbox, so you can focus on what matters most.

## Features

- **AI Personal Assistant:** Organizes your inbox and pre-drafts replies in your tone and style.
- **AI Rules for email:** Explain in plain English how your AI should handle your inbox.
- **Reply Zero:** Track emails to reply to and those awaiting responses.
- **Bulk Unsubscriber:** One-click unsubscribe and archive emails you never read.
- **Bulk Archiver:** Clean up your inbox by bulk archiving old emails.
- **Cold Email Blocker:** Auto‑block cold emails.
- **Email Analytics:** Track your activity and trends over time.
- **Meeting Briefs:** Get personalized briefings before every meeting, pulling context from your email and calendar.
- **Smart Filing:** Automatically save email attachments to Google Drive or OneDrive.
- **Slack & Telegram Integration:** Chat with your AI assistant from Slack or Telegram to manage your inbox without leaving the apps you already use.

Learn more in our [docs](https://docs.getinboxzero.com/).

## Feature Screenshots

| [![AI Assistant](https://github.com/elie222/inbox-zero/raw/main/.github/screenshots/email-assistant.png)](https://github.com/elie222/inbox-zero/blob/main/.github/screenshots/email-assistant.png) | [![Reply Zero](https://github.com/elie222/inbox-zero/raw/main/.github/screenshots/reply-zero.png)](https://github.com/elie222/inbox-zero/blob/main/.github/screenshots/reply-zero.png) |
| --- | --- |
| *AI Assistant* | *Reply Zero* |
| [![Gmail Client](https://github.com/elie222/inbox-zero/raw/main/.github/screenshots/email-client.png)](https://github.com/elie222/inbox-zero/blob/main/.github/screenshots/email-client.png) | [![Bulk Unsubscriber](https://github.com/elie222/inbox-zero/raw/main/.github/screenshots/bulk-unsubscriber.png)](https://github.com/elie222/inbox-zero/blob/main/.github/screenshots/bulk-unsubscriber.png) |
| *Gmail client* | *Bulk Unsubscriber* |

## Demo Video

[![Inbox Zero demo](https://camo.githubusercontent.com/e977b26575a60776a9969fe7fbdfe66911f1965f76507834705933bbb52dfba9/68747470733a2f2f696d672e796f75747562652e636f6d2f76692f5575736e76654c4b77574d2f6d617872657364656661756c742e6a7067)](https://youtu.be/UusnveLKwWM)

## Built with

- [Next.js](https://nextjs.org/)
- [Tailwind CSS](https://tailwindcss.com/)
- [shadcn/ui](https://ui.shadcn.com/)
- [Prisma](https://www.prisma.io/)
- [Upstash](https://upstash.com/)
- [Turborepo](https://turbo.build/)
- [Popsy Illustrations](https://popsy.co/)

## Star History

[![Star History Chart](https://camo.githubusercontent.com/d7298247cda85c1e8043e1b98b831e5432ec0741992f27e5f05ddc3b5ee7f961/68747470733a2f2f6170692e737461722d686973746f72792e636f6d2f7376673f7265706f733d656c69653232322f696e626f782d7a65726f26747970653d44617465)](https://www.star-history.com/#elie222/inbox-zero&Date)

## Feature Requests

To request a feature open a [GitHub issue](https://github.com/elie222/inbox-zero/issues), or join our [Discord](https://www.getinboxzero.com/discord).

## Getting Started

We offer a hosted version of Inbox Zero at [getinboxzero.com](https://www.getinboxzero.com/).

### Self-Hosting

The fastest way to self-host Inbox Zero is with the CLI:

> **Prerequisites**: [Docker](https://docs.docker.com/engine/install/) and [Node.js](https://nodejs.org/) v24+

```
npx @inbox-zero/cli setup      # One-time setup wizard
npx @inbox-zero/cli start      # Start containers
```

Open [http://localhost:3000](http://localhost:3000/)

For complete self-hosting instructions, production deployment, OAuth setup, and configuration options, see our **[Self-Hosting Docs](https://docs.getinboxzero.com/hosting/quick-start)**.

### Local Development

> **Prerequisites**: [Docker](https://docs.docker.com/engine/install/), [Node.js](https://nodejs.org/) v24+, and [pnpm](https://pnpm.io/) v10+

```
git clone https://github.com/elie222/inbox-zero.git
cd inbox-zero
docker compose -f docker-compose.dev.yml up -d   # Postgres + Redis
pnpm install
npm run setup                                     # Interactive env setup
cd apps/web && pnpm prisma migrate dev && cd ../..
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000/)

After `pnpm install`, if you want to use the local Google emulator, start it with:

```
docker compose -f docker-compose.dev.yml --profile google-emulator up -d
```

Then point `apps/web/.env` at it with:

```
GOOGLE_BASE_URL=http://localhost:4002
GOOGLE_CLIENT_ID=emulate-google-client.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=emulate-google-secret
```

If you want to use the local Microsoft emulator, start it with:

```
docker compose -f docker-compose.dev.yml --profile microsoft-emulator up -d
```

Then point `apps/web/.env` at it with:

```
MICROSOFT_BASE_URL=http://localhost:4003
MICROSOFT_CLIENT_ID=emulate-microsoft-client-id
MICROSOFT_CLIENT_SECRET=emulate-microsoft-secret
```

See the **[Contributing Guide](https://docs.getinboxzero.com/contributing)** for more details including devcontainer setup.

## Contributing

View open tasks in [GitHub Issues](https://github.com/elie222/inbox-zero/issues) and join our [Discord](https://www.getinboxzero.com/discord) to discuss what's being worked on.

Docker images are automatically built on every push to `main` and tagged with the commit SHA (e.g., `elie222/inbox-zero:abc1234`). The `latest` tag always points to the most recent main build. Formal releases use version tags (e.g., `v2.26.0`).