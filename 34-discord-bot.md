---
title: "OpenSIN Discord Bot: Community managen"
description: "Der OpenSIN Discord Bot managed deine Community autonom. Moderation, Support, Info."
date: 2026-04-04
author: OpenSIN Team
tags: ['discord', 'bot', 'opensin', 'community', 'automation']
category: Vergleich
readTime: "8 Minuten"
---

# OpenSIN Discord Bot: Community managen

**Veröffentlicht:** 02.04.2026  
**Autor:** OpenSIN Team  
**Lesezeit:** 5 Minuten

---

## Warum Discord?

Weil dein Team nicht ständig Dashboards offen hat.
Aber Discord? **Das läuft immer.**

---

## Commands

| Command | Was es macht |
|---|---|
| `!status` | Fleet Overview (Agents, Teams, Uptime, RPM) |
| `!teams` | Alle 18 Teams mit Agent Counts |
| `!alerts` | Letzte 10 Alerts mit Severity |
| `!agent <name>` | Detail-View eines Agents |
| `!metrics` | CPU, Memory, Tokens, Cost, Error Rate |
| `!help` | Alle Commands |

---

## Setup

```bash
DISCORD_BOT_TOKEN=xxx OPENSIN_API_URL=http://localhost:8765 OPENSIN_API_KEY=xxx node src/index.js
```

3 Umgebungsvariablen. **Fertig.**

---

## Das Besondere

Der Bot ist nicht nur ein Dashboard im Chat.
Er ist die **Schnittstelle zwischen deinem Team und dem Fleet.**

Wenn ein Agent crasht, weißt du es bevor du es googeln musst.
Wenn die Kosten steigen, siehst du es bevor die Rechnung kommt.

**Proaktiv statt reaktiv.**

-> [Discord Bot auf GitHub](https://github.com/OpenSIN-AI/OpenSIN/tree/main/packages/discord-bot)
