---
title: "OpenSIN Security Scanner: Code sicher machen"
description: "Der integrierte Security Scanner findet Schwachstellen, bevor sie live gehen."
date: 2026-04-04
author: OpenSIN Team
tags: ['security', 'scanner', 'opensin', 'code', 'sicherheit']
category: Vergleich
readTime: "8 Minuten"
---

# OpenSIN Security Scanner: Code sicher machen

**Veröffentlicht:** 02.04.2026  
**Autor:** OpenSIN Team  
**Lesezeit:** 7 Minuten

---

## Das Problem das niemand anspricht

Jeder AI-Agent führt Code aus. Jeder Agent hat Zugriff auf Dateien, Netzwerke, APIs.
Aber kaum jemand prüft, was der Agent **wirklich** tut.

**Hardcoded API Keys. Prompt Injection. Unrestricted HTTP. Eval().**
Das sind keine theoretischen Risiken. Das ist der Alltag.

---

## Unser Scanner findet 15 Arten von Sicherheitslücken

| Kategorie | Rules | Severity |
|---|---|---|
| Prompt Injection | 2 | 🔴 Critical |
| Data Exfiltration | 2 | 🔴/🟠 Critical/High |
| Code Execution | 3 | 🔴/🟠/🟡 Critical/High/Medium |
| Network Security | 2 | 🟠 High |
| Secret Exposure | 4 | 🔴/🟠 Critical/High |
| Config Issues | 4 | 🟡/🔵/🟠 Medium/Low/High |
| Obfuscation | 1 | 🔴 Critical |

---

## So benutzt du ihn

```bash
npx opensin-scan ./agent --format json
npx opensin-scan ./agent --categories injection,secrets
npx opensin-scan ./agent --format markdown
```

---

## Was die anderen machen

Andere Agent-Plattformen: **Kein Security Scanner.**
OpenSIN: **15 Rules, 6 Scanner, JSON/Markdown/Text Reports.**

**Das ist der Unterschied zwischen "funktioniert" und "ist sicher".**

---

## Fazit

Ein Agent ohne Security Scanner ist wie ein Server ohne Firewall.
Es funktioniert — bis es nicht mehr funktioniert.

-> [Scanner auf GitHub](https://github.com/OpenSIN-AI/OpenSIN/tree/main/packages/agent-scanner)
