---
title: "OpenSIN VS Code Extension v0.1.0 — Dein Editor wird zum Agenten-Command-Center"
date: 2026-04-04
author: OpenSIN Team
tags: [opensin, ai-agent, a2a-protocol]
category: News
readTime: "8 Minuten"
---

# OpenSIN VS Code Extension v0.1.0 — Dein Editor wird zum Agenten-Command-Center

**Veröffentlicht:** 24.04.2026  
**Autor:** OpenSIN Team  
**Lesezeit:** 10 Minuten

---

## Der Editor der alles verändert

Stell dir vor du öffnest VS Code — und statt nur Code zu schreiben, hast du ein **ganzes Team spezialisierter KI-Agenten direkt in deinem Editor.**

Nicht einen Chatbot in der Seitenleiste. Nicht einen glorifizierten Autovervollständiger.

Sondern: **Architekten die planen. Coder die implementieren. Debugger die Fehler jagen. Und das alles — parallel.**

Das ist die OpenSIN VS Code Extension v0.1.0. Und sie ist ab jetzt verfügbar.

**[→ Jetzt im VS Code Marketplace installieren](https://marketplace.visualstudio.com/items?itemName=opensin.opensin-vscode)**

---

## Was ist OpenSIN VS Code?

OpenSIN VS Code ist die erste **agentic AI Coding Extension** die nicht nur Code vorschlägt — sondern **selbstständig arbeitet.**

Sie verbindet deinen Editor direkt mit dem OpenSIN Ökosystem:

```
VS Code Extension
├── opencode CLI (lokal) → LLM-Orchestrierung
├── simone-mcp → LSP-Diagnostik & semantischer Kontext
├── Swarm Coordinator → Parallele Agenten-Dispatching
├── 5 spezialisierte Modi → Architect, Code, Debug, Ask, Proactive
└── BUDDY Gamification → Dein Coding-Begleiter im Statusbar
```

**Das Ergebnis:** Du schreibst nicht mehr Code allein. Du **koordinierst ein Team.**

---

## Phase 1-3: Was in v0.1.0 steckt

### Phase 1: Modus-basierte Agenten 🎯

Die Extension bringt **5 spezialisierte Modi** mit — inspiriert von den besten Konzepten aus Kilo Code und Claude Code, aber mit dem entscheidenden Unterschied: **Jeder Modus ist ein spezialisierter Agent, kein generischer Chat.**

| Modus | Icon | Was er macht |
|-------|------|-------------|
| **Architect** | 🏗️ | Systemarchitektur planen, Projekt-Roadmaps erstellen, Design-Entscheidungen treffen |
| **Code** | 💻 | Implementierung, Refactoring, Production-ready Code schreiben |
| **Debug** | 🐛 | Issues trace, Error Logs lesen, Fixes vorschlagen |
| **Ask** | ❓ | Codebases erklären und durchsuchen — OHNE Änderungen |
| **Proactive** | ⚡ | Immer-an Modus — Hintergrundanalyse bei jedem File Save |

Der **Proactive Mode** ist dabei unser Killer-Feature: Die Extension analysiert im Hintergrund deine Codebasis bei jedem Speichern und gibt dir proaktive Vorschläge — bevor du überhaupt fragst.

### Phase 2: Swarm Coordinator 🐝

Hier wird es richtig spannend. Der **Swarm Coordinator** dispatcht Tasks an spezialisierte Sub-Agenten — **parallel:**

- **Explore** — Durchsucht Codebase-Patterns, Dateistrukturen, AST-Grep Analysen
- **Librarian** — Findet offizielle Dokumentation, GitHub-Beispiele, Remote-Repo-Kontext
- **Oracle** — Architektur-Debugging, komplexe Logik-Analyse
- **Artistry** — Unkonventionelle Problemlösungen, alternative Ansätze

**Warum das wichtig ist:** Statt einen Agenten 10 Minuten warten zu lassen, arbeiten 4 Agenten gleichzeitig an verschiedenen Aspekten deines Problems.

### Phase 3: Inline Chat, Code Actions & Marketplace 🔌

**Inline Chat:** Code direkt im Editor diskutieren — ohne die Sidebar. Markiere Code, stelle eine Frage, bekomme eine Antwort im Kontext.

**Code Actions:** Rechtsklick auf Code → "Erkläre diesen Block", "Refaktor diese Funktion", "Finde Bugs" — alles direkt aus dem Kontextmenü.

**Marketplace-Integration:** Die Extension ist nahtlos mit dem OpenSIN Marketplace verbunden. Neue Agenten, neue Modi, neue Fähigkeiten — alles installierbar wie eine normale Extension.

---

## Was wir von den Besten geklaut haben (und besser gemacht)

Seien wir ehrlich: **Claude Code und Kilo Code haben großartige Ideen.** Aber sie haben auch fundamentale Schwächen.

### Von Claude Code genommen:
- ✅ **Inline Chat** — Code im Kontext diskutieren
- ✅ **Code Actions** — Rechtsklick-Integration
- ✅ **Proaktive Vorschläge** — Hintergrundanalyse

**Aber Claude Code kann nur:** EIN Modell. EIN Agent. EINS zu EINS.

### Von Kilo Code genommen:
- ✅ **Modus-System** — Spezialisierte Arbeitsmodi
- ✅ **Architect/Code/Debug Split** — Trennung der Concerns
- ✅ **Gamification** — BUDDY als Statusbar-Begleiter

**Aber Kilo Code kann nur:** Lokal arbeiten. Keine Swarm-Orchestrierung. Keine parallele Ausführung.

### Was OpenSIN anders macht:

```
Claude Code:  1 Prompt → 1 Antwort → Du machst weiter
Kilo Code:    1 Modus → 1 Agent → Du wartest
OpenSIN:      1 Prompt → 4 Agenten PARALLEL → Du bekommst ERGEBNISSE
```

**Der Unterschied ist nicht marginal. Er ist fundamental.**

OpenSIN VS Code ist kein besserer Chatbot im Editor. Es ist ein **Command-Center für eine ganze KI-Flotte.**

---

## Von KAIROS zu OpenSIN: Die Geschichte dahinter

Was heute als **OpenSIN VS Code** im Marketplace steht, hatte einen anderen Anfang.

Das Projekt startete als **KAIROS VS Code Extension** — benannt nach dem griechischen Wort für den "richtigen Moment." Die Idee war: Ein Editor-Plugin das den richtigen Zeitpunkt erkennt, wann es hilft und wann es sich raushält.

Aber dann passierte etwas: **KAIROS wurde Teil von etwas Größerem.**

Als das Projekt in **OpenSIN** umbenannt wurde — von einem einzelnen Tool zu einem kompletten agentic AI Ökosystem — wurde klar: Die VS Code Extension ist nicht nur ein Plugin. Sie ist das **Tor zum gesamten OpenSIN Universum.**

Jeder Modus, jeder Swarm-Agent, jede Code Action — all das verbindet sich mit den 42+ A2A-Agenten die im Hintergrund arbeiten. Die Extension ist nicht das Produkt. Sie ist das **Interface.**

**KAIROS war der Funke. OpenSIN ist das Feuer.**

---

## Installation in 60 Sekunden

### Option 1: VS Code Marketplace (Empfohlen)

1. Öffne VS Code
2. Drücke `Cmd+Shift+X` (Mac) oder `Ctrl+Shift+X` (Windows/Linux)
3. Suche nach **"OpenSIN"**
4. Klicke auf **Install**
5. Fertig. 🎉

**[→ Direkt zum Marketplace](https://marketplace.visualstudio.com/items?itemName=opensin.opensin-vscode)**

### Option 2: From Source

```bash
# Repository klonen
git clone git@github.com:OpenSIN-AI/OpenSIN-Code.git
cd OpenSIN-Code/sincode-vscode

# Dependencies installieren
npm install

# Kompilieren
npm run compile

# Als .vsix paketieren
npx vsce package

# Installieren
code --install-extension sincode-vscode-0.1.0.vsix
```

### Option 3: Development Mode

```bash
git clone git@github.com:OpenSIN-AI/OpenSIN-Code.git
cd OpenSIN-Code/sincode-vscode
npm install

# In VS Code öffnen und F5 drücken
code --extensionDevelopmentPath=$PWD
```

### Voraussetzungen

- **VS Code 1.85.0+**
- **`opencode` CLI** installiert und im PATH verfügbar
- **(Optional) `simone-mcp`** für LSP-Diagnostik

Die Extension nutzt deine globale OpenCode-Konfiguration unter `~/.config/opencode/`. Verfügbare Modelle werden automatisch aus deiner Provider-Konfiguration geladen.

---

## Commands auf einen Blick

| Command | Beschreibung |
|---------|-------------|
| `SIN Code: Start` | OpenSIN Sidebar öffnen |
| `SIN Code: Select Mode` | Zwischen Agenten-Modi wechseln |
| `SIN Code: Select Model` | Unterliegendes LLM auswählen |
| `SIN Code: Dispatch Agent` | Swarm Coordinator Panel öffnen |
| `SIN Code: Add File to Context` | Aktuelle Datei zum Kontext hinzufügen |

---

## BUDDY: Dein Coding-Begleiter 🐾

Ja, es gibt ein **virtuelles Haustier** in deiner Statusbar. Und nein, das ist kein Gimmick.

**BUDDY** ist ein gamifizierter Begleiter der:
- **Levelt auf** bei jedem Commit (+25 XP)
- **Feiert** bestandene Tests (+15 XP)
- **Reagiert** auf Fehler mit Stimmungswechseln
- **Zeigt** XP, Level und letzte Aktion beim Hover

Warum? Weil Coding Spaß machen soll. Und weil ein kleiner digitaler Begleiter dich daran erinnert: **Du bist nicht allein. Du hast ein ganzes Team.**

---

## Memory Consolidation: Dein Kontext. Automatisch.

Die Extension lädt automatisch Kontext aus:

- `AGENTS.md` — Projekt-spezifische Agenten-Regeln
- `SIN-MEMORY.md` — OpenSIN Memory-Dateien
- `CLAUDE.md` — Kompatibilität mit Claude Code Projekten
- `.sincode-memory.md` — Custom Memory-Dateien

**Kein manuelles Kontext-Kopieren. Kein "hier ist mein Codebase."** Die Extension weiß was sie wissen muss.

---

## LSP Integration: Semantischer Kontext in Echtzeit

Über `simone-mcp` bekommt die Extension:

- **Echtzeit-Diagnostik** — Errors, Warnings, Hints direkt vom Language Server
- **Symbol-Extraktion** — Semantische Kontext-Informationen aus der aktuellen Datei
- **Cursor-aware Context** — Der Agent weiß genau wo du bist und was du brauchst

Das ist kein oberflächliches Syntax-Highlighting. Das ist **tiefes semantisches Verständnis** deines Codes.

---

## Was als nächstes kommt

v0.1.0 ist erst der Anfang. Die Roadmap:

- **Phase 4:** Multi-Repo Swarm — Agenten die über mehrere Repositories gleichzeitig arbeiten
- **Phase 5:** Custom Agent Builder — Eigene spezialisierte Agenten direkt im Editor erstellen
- **Phase 6:** Voice Commands — Sprachsteuerung für alle OpenSIN Funktionen
- **Phase 7:** Team Collaboration — Geteilte Swarm-Sessions für ganze Dev-Teams

---

## Open Source. Immer.

OpenSIN VS Code ist **Apache 2.0 lizenziert.** Das bedeutet:

- ✅ Kostenlos für immer
- ✅ Selbst hostbar
- ✅ Forkbar und modifizierbar
- ✅ Kommerziell nutzbar

**[→ GitHub Repository](https://github.com/OpenSIN-AI/OpenSIN-Code)**

---

## Fazit: Der Editor der für dich arbeitet

Die meisten KI-Coding-Tools machen dich zum **Prompt-Engineer.**

OpenSIN VS Code macht dich zum **Team-Lead.**

Du gibst die Richtung vor. Die Agenten machen die Arbeit. Und am Ende des Tages hast du nicht nur Code geschrieben — du hast **Ergebnisse geliefert.**

**[→ Jetzt installieren: OpenSIN VS Code Extension v0.1.0](https://marketplace.visualstudio.com/items?itemName=opensin.opensin-vscode)**

**[→ GitHub: OpenSIN-Code Repository](https://github.com/OpenSIN-AI/OpenSIN-Code)**

---

*OpenSIN — Zero Human Intervention. Immer.*
