---
title: "KAIROS VS Code Sidebar: Coding neu gedacht"
description: "Die KAIROS Sidebar bringt OpenSIN-Agenten direkt in VS Code. Coding war nie schneller."
date: 2026-04-04
author: OpenSIN Team
tags: ['vscode', 'kairos', 'coding', 'opensin', 'ide']
category: Vergleich
readTime: "8 Minuten"
---

# KAIROS VS Code Sidebar: Coding neu gedacht

**Veröffentlicht:** 02.04.2026  
**Autor:** OpenSIN Team  
**Lesezeit:** 6 Minuten

---

## Die meisten VS Code Extensions sind nur hübsche Terminals

Sie sehen modern aus. Sie haben einen Chat-Tab. Vielleicht noch einen Model-Selector.

Aber unter der Haube passiert oft nichts anderes als:

1. User tippt Text ein
2. Extension startet einen frischen CLI-Prozess
3. Antwort kommt zurück
4. Alles ist wieder vergessen

**Das ist keine Arbeitsumgebung. Das ist ein Wegwerf-Subprozess mit UI.**

---

## Was wir gebaut haben

Mit **KAIROS for VS Code** haben wir unsere Extension auf eine andere Ebene gezogen:

- **KAIROS Sidebar** direkt in VS Code
- **Mode Selector**: `Architect`, `Code`, `Debug`, `Ask`
- **Model Selector** mit Qwen 3.6 Free Lanes
- **Proactive Mode** bei File-Saves
- **BUDDY Score** als leichte Gamification-Layer
- **Simone Buttons** für Symbol- und Reference-Analysen
- **Persistente Session-Steuerung** statt frischer CLI-Calls

Das ist der eigentliche Unterschied.

---

## Der große Shift: von `opencode run` zu echter Session-Steuerung

Früher wäre die Sidebar nur ein schöner Trigger für:

```bash
opencode run "mach X"
```

Jetzt läuft sie über eine **echte lokale Session**:

```text
VS Code Sidebar
    -> lokales opencode --port ...
    -> POST /session
    -> POST /session/:id/message
    -> persistente Antworten im selben Kontext
```

Das bedeutet:

- der Verlauf bleibt erhalten
- der Arbeitskontext bleibt stabil
- mehrere Sidebar-Aktionen bauen auf derselben Session auf
- KAIROS ist kein Einmal-Chat mehr, sondern ein laufender Co-Pilot-Zustand

**Genau so muss eine IDE-Integration aussehen.**

---

## Simone macht den Unterschied

Wir haben KAIROS nicht einfach nur an ein Modell gehängt. Wir koppeln es an **Simone**.

Das heißt:

- aktives Symbol finden
- Referenzen analysieren
- Symbol-Kontext in `Architect` und `Debug` automatisch anreichern

Das Ergebnis:

Wenn du im Editor auf einer Funktion, Klasse oder Selection sitzt, bekommt KAIROS nicht nur deinen Text-Prompt, sondern kann den tatsächlichen Code-Kontext viel gezielter auflösen.

**Andere Extensions fragen dich „Was meinst du genau?“. Simone findet es.**

---

## Warum Proactive Mode wichtig ist

Die meisten Coding-Assistants warten passiv auf Eingaben.

KAIROS kann jetzt auf Save-Events reagieren und im Hintergrund eine knappe Review fahren:

- offensichtliche Bugs
- Regressionen
- fehlende Tests

Nicht als nervige Dauerunterbrechung. Sondern als **gezielte zweite Ebene von Aufmerksamkeit**.

Das ist der Anfang von dem, was moderne Agent-Umgebungen eigentlich werden müssen: **proaktiv, aber nicht störend.**

---

## Release: VS Code Extension v1.3.15

Die neue offizielle VSIX ist jetzt veröffentlicht.

### Enthalten in `v1.3.15`

- KAIROS Sidebar
- Mode Selector
- Model Selector
- Proactive Mode
- BUDDY Score
- Simone Symbol/Refs Actions
- Persistente Session-Steuerung über die lokale Session API

Release:

-> [OpenSIN-Code VS Code Extension v1.3.15](https://github.com/OpenSIN-AI/OpenSIN-Code/releases)

---

## Das größere Bild

Die meisten Teams bauen „AI in VS Code“ als Feature.

Wir bauen **eine Arbeitsumgebung für Agenten**.

Der Unterschied ist gewaltig:

- nicht nur Modell-Auswahl
- nicht nur Chat im Editor
- nicht nur ein hübscher Button

Sondern:

- Session-Zustand
- agentische Modi
- proaktive Reviews
- Symbol-/Reference-Intelligenz
- tiefe lokale Steuerung

**KAIROS ist nicht nur eine Extension. Es ist die IDE-Lane für OpenSIN-Code.**

---

## Jetzt ansehen

-> [OpenSIN-Code auf GitHub](https://github.com/OpenSIN-AI/OpenSIN-Code)  
-> [VS Code Release herunterladen](https://github.com/OpenSIN-AI/OpenSIN-Code/releases)  
-> [OpenSIN auf GitHub](https://github.com/OpenSIN-AI/OpenSIN)

---

*KAIROS ist der Anfang. Der nächste Schritt ist vollständige ACP-/Session-Kontrolle, nicht nur Sidebar-Komfort.*
