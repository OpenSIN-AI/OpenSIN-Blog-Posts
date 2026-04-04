---
title: "Fleet Monitoring Deep Dive: Alles im Blick"
description: "Wie du 100+ OpenSIN-Agenten monitorst. Metriken, Alerts und Dashboards."
date: 2026-04-04
author: OpenSIN Team
tags: ['monitoring', 'fleet', 'opensin', 'devops', 'tools']
category: Vergleich
readTime: "8 Minuten"
---

# Fleet Monitoring Deep Dive: Alles im Blick

**Veröffentlicht:** 03.04.2026  
**Autor:** OpenSIN Team  
**Lesezeit:** 12 Minuten

---

## 147 Repos. 108 A2A-Agenten. 9 Teams. Null Menschen die nachts aufwachen.

Das ist kein Flex. Das ist eine architektonische Notwendigkeit.

Wenn du 108 autonome Agenten betreibst, die rund um die Uhr arbeiten — auf HuggingFace Spaces, Oracle Cloud Compute, lokalen Macs — dann hast du zwei Möglichkeiten:

**Entweder du baust ein Monitoring-System das so autonom ist wie die Agenten selbst. Oder du schläfst nie wieder.**

Wir haben uns für Option 1 entschieden. Und zwar radikal.

---

## Warum Herkömmliches Monitoring Hier Versagt

Prometheus allein reicht nicht. Grafana allein reicht nicht. PagerDuty allein reicht nicht.

Das Problem: Herkömmliche Monitoring-Tools gehen davon aus, dass da **jemand** ist der die Alerts liest. Bei uns ist das nicht der Fall. Unser Fleet arbeitet um 3:47 Uhr morgens genauso hart wie um 14:00 Uhr. Und es erwartet nicht dass ein Mensch aufwacht wenn Agent `sin-twitter-03` crasht.

**Das System muss sich selbst überwachen. Sich selbst diagnostizieren. Sich selbst alarmieren. Und im Idealfall: Sich selbst reparieren.**

Genau dafür haben wir das `@opensin/fleet-monitor` Package gebaut.

---

## Die Architektur: Vier Schichten. Keine Kompromisse.

```
┌─────────────────────────────────────────────────────────────────┐
│                    LAYER 1: MONITOR ENGINE                       │
│  checkAllAgents() → parallele Health-Checks für alle 108+ Agenten │
│  Timeout: max(3x expectedLatency, 5000ms)                       │
│  Consecutive-Failure-Tracking pro Agent                         │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    LAYER 2: METRICS COLLECTOR                    │
│  CPU, Memory, Tokens, Kosten, Latency (avg/p50/p95/p99)         │
│  Request-Counts (success/failed/error-rate)                     │
│  Cost-per-Hour Tracking                                         │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    LAYER 3: ALERT MANAGER                        │
│  7 Alert-Typen mit Schwellwerten                                 │
│  Cooldown-Mechanismus (5 Min) gegen Alert-Spam                  │
│  Dual-Channel: Telegram + Discord                               │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                    LAYER 4: STORAGE + VISUALIZATION              │
│  SQLite (WAL-Mode) für Time-Series                              │
│  Prometheus /metrics Endpoint                                   │
│  Grafana Dashboards (auto-provisioned)                          │
│  REST API für Dashboard-Integration                             │
└─────────────────────────────────────────────────────────────────┘
```

---

## Das Fleet-Monitor Package: Was Drin Steckt

### Monitor Engine (`src/monitor.ts`)

Das Herzstück. Eine `MonitorEngine` Klasse die alle 30 Sekunden (konfigurierbar) jeden einzelnen Agenten anpingt — parallel, nicht sequentiell.

```typescript
// Jeder Agent wird parallel geprüft — nicht nacheinander
const checks = agents.map(async (agent) => {
  const result = await this.checkAgent(agent);
  const metrics = await this.collectMetrics(agent);
  this.alertManager.evaluate(result, agent);
  return result;
});
await Promise.allSettled(checks);
```

**Warum parallel? Weil 108+ Agenten × 200ms sequentiell = 21.6 Sekunden wären. Parallel: unter 2 Sekunden.**

Jeder Health-Check trackt:
- HTTP Status Code
- Latenz in Millisekunden
- Uptime des Agents
- Version des Agents
- Consecutive Failures

### Metrics Collector (`src/metrics.ts`)

Sieben Metrik-Kategorien die wir sammeln — nicht weil es cool aussieht, sondern weil jede einzelne eine echte Entscheidung ermöglicht:

| Kategorie | Metriken | Wofür wir es nutzen |
|---|---|---|
| **Health** | Status, Latency, Uptime, Version | Ist der Agent alive? |
| **CPU** | Usage % | Braucht er mehr Ressourcen? |
| **Memory** | Used MB, Total MB, Usage % | Memory Leak? |
| **Tokens** | Input/Output/Total, Tokens/Min | Kosten-Optimierung |
| **Cost** | Cost/Hour, Cost/Token, Daily Total | Budget-Alarme |
| **Latency** | Avg, P50, P95, P99 | Performance-Degradation |
| **Requests** | Total, Success, Failed, Error-Rate | Qualität der Arbeit |

### Alert Manager (`src/alerts.ts`)

Hier wird es ernst. Der Alert Manager evaluiert **sieben** verschiedene Alert-Typen mit unterschiedlichen Severity-Leveln:

| Alert-Typ | Warning | Critical |
|---|---|---|
| **Health Check Failed** | ≥ 3 consecutive failures | Critical Agent + 1 failure |
| **High Latency** | > 2x expected | > 5x expected |
| **High CPU** | > 85% | > 95% |
| **High Memory** | > 90% | > 95% |
| **High Error Rate** | > 5% | > 20% |
| **High Cost** | > $10/hr | — |
| **High Token Rate** | > 100k/min | — |

**Und das Wichtigste: Ein Cooldown von 5 Minuten verhindert dass derselbe Alert 50 Mal pro Minute feuert.**

### Notification Channels

**Telegram:** Formatierter Markdown mit Severity-Emoji (🔴 critical, 🟡 warning, 🔵 info)

**Discord:** Color-coded Embeds mit allen relevanten Metriken

Beide Channels feuern parallel via `Promise.allSettled` — wenn einer ausfällt, kommt der Alert trotzdem durch.

---

## Die 9 Teams — 108+ Agenten im Detail

| Team | Repos | HF Spaces | Zweck |
|---|---|---|---|
| **Infrastructure** | 6 | sin-server, sin-cloudflare, sin-supabase, sin-storage, sin-authenticator, sin-passwordmanager | A2A Fleet Control Plane, Credentials, DNS, DB |
| **Teams** | 8 | sin-team-social, sin-team-orchestrator, sin-team-google-apps, sin-team-shop, sin-team-company, sin-team-worker, sin-team-creator, sin-team-marketing | Team-Koordination und Orchestrierung |
| **Apple** | 14 | sin-imessage, sin-apple-device-control, sin-apple-notifications, sin-apple-calendar-contacts, sin-apple-notes, sin-apple-mobile, sin-apple-facetime, sin-apple-mail, sin-apple-reminders, sin-apple-photos-files, sin-apple-safari-webkit, sin-apple-system-settings, sin-apple-shortcuts, sin-team-apple-apps | macOS Systemintegration via AppleScript |
| **Security** | 17 | sin-bugbounty, sin-hacker-recon, sin-hacker-fuzz, sin-hacker-exploit, sin-hacker-audit, sin-hacker-web, sin-hacker-network, sin-hacker-mobile, sin-hacker-auth, sin-hacker-crypto, sin-hacker-social, sin-hacker-cloud, sin-hacker-ai, sin-hacker-malware, sin-hacker-iot, sin-hacker-forensics, sin-hacker-redteam | Bug Bounty, Recon, Fuzzing, Exploit, Audit |
| **Code** | 6 | sin-code-security, sin-code-devops, sin-code-datascience, sin-code-ai, sin-code-database, sin-code-integration | DevOps, Data Science, AI Code-Gen, DB |
| **Social & Marketing** | 20 | sin-reddit, sin-discord, sin-youtube, sin-tiktok, sin-medium, sin-instagram, sin-x-twitter, sin-community, sin-linkedin, sin-telegram, sin-google-apps, sin-devto, sin-hackernews, sin-producthunt, sin-stackoverflow, sin-quora, sin-indiehackers, sin-lobsters, sin-slashdot, sin-tiktok-shop | Reddit, Discord, YouTube, TikTok, X, Instagram, Medium, LinkedIn, Telegram + 10 mehr |
| **Messaging** | 16 | WhatsApp, Signal, Email, Google Chat, Teams, SMS, IRC, Matrix, WebChat, Feishu, WeChat, LINE, Nostr, Beeper, BlueBubbles, Chatroom | Multi-Platform Messaging |
| **Legal & Finance** | 9 | sin-tax, sin-contract, sin-compliance, sin-evidence, sin-damages, sin-claimwriter, sin-paragraph, sin-summary, sin-team-lawyer | Steuer, Verträge, Compliance, Schadensberechnung |
| **Specialized** | 12 | sin-research, sin-mindrift, sin-stripe, sin-github-action, sin-shop-logistic, sin-shop-finance, sin-ci-cd, sin-oraclecloud-mcp, sin-xbox, sin-playstation, sin-nintendo, sin-zoom | Research, Payment, CI/CD, Gaming, Cloud |

**108+ Agenten. 147 Repos. Jeder einzelne wird alle 30 Sekunden geprüft.**

---

## Prometheus Integration: Die /metrics Endpoint

Der Fleet Monitor exponiert einen vollständigen Prometheus-kompatiblen Endpoint unter `/metrics`:

```
# HELP opensin_fleet_healthy_agents Number of healthy agents
# TYPE opensin_fleet_healthy_agents gauge
opensin_fleet_healthy_agents 104

# HELP opensin_fleet_unhealthy_agents Number of unhealthy agents
# TYPE opensin_fleet_unhealthy_agents gauge
opensin_fleet_unhealthy_agents 4

# HELP opensin_agent_latency_ms Latest latency per agent
# TYPE opensin_agent_latency_ms gauge
opensin_agent_latency_ms{agent_id="agent-orchestrator",team_id="team-core"} 142
opensin_agent_latency_ms{agent_id="agent-router",team_id="team-core"} 87
opensin_agent_latency_ms{agent_id="agent-tokenizer",team_id="team-nlp"} 43

# HELP opensin_agent_cpu_percent CPU usage per agent
# TYPE opensin_agent_cpu_percent gauge
opensin_agent_cpu_percent{agent_id="agent-orchestrator",team_id="team-core"} 67.3

# HELP opensin_agent_memory_percent Memory usage per agent
# TYPE opensin_agent_memory_percent gauge
opensin_agent_memory_percent{agent_id="agent-orchestrator",team_id="team-core"} 72.1

# HELP opensin_agent_cost_per_hour Cost per hour per agent
# TYPE opensin_agent_cost_per_hour gauge
opensin_agent_cost_per_hour{agent_id="agent-orchestrator",team_id="team-core"} 2.45
```

Prometheus scraped alle 30 Sekunden. Retention: 30 Tage. Max 50GB.

---

## Grafana Dashboards: Was Wir Wirklich Sehen

Grafana wird per Docker Compose auto-provisioned mit Prometheus als Datasource. Unsere Dashboards zeigen:

### Dashboard 1: Fleet Overview
- **Gesamtstatus:** Healthy / Degraded / Unhealthy counts
- **Team-Heatmap:** Welche Teams haben Probleme?
- **Zeitserien:** Agent-Verfügbarkeit über 24h
- **Top-Latenzen:** Die 10 langsamsten Agenten
- **Kosten-Übersicht:** Total Cost Today, Cost/Hour pro Team

### Dashboard 2: Agent Detail
- **Einzelner Agent:** Alle Metriken im Zeitverlauf
- **CPU/Memory:** Ressourcennutzung mit Threshold-Linien
- **Token-Verbrauch:** Input/Output Tokens über Zeit
- **Error-Rate:** Failed vs Successful Requests
- **Health-History:** Wann war der Agent offline?

### Dashboard 3: Alert History
- **Aktive Alerts:** Was brennt gerade?
- **Alert-Frequenz:** Welche Agenten alerten am meisten?
- **Severity-Verteilung:** Critical vs Warning vs Info
- **MTTR:** Mean Time To Resolution

---

## Die Monitoring Stack: Docker Compose

Unsere komplette Monitoring-Infrastruktur läuft als eigener Stack:

```yaml
# docker-compose.monitoring.yml
services:
  room-25-prometheus:      # Metrics Collection & Storage
  room-26-grafana:         # Visualization & Dashboards
  room-27-alertmanager:    # Alert Routing & Management
  room-28-loki:            # Log Aggregation
  room-28-promtail:        # Log Collection Agent
  room-29-jaeger:          # Distributed Tracing
  room-30-node-exporter:   # System Metrics
  room-31-cadvisor:        # Container Metrics
  room-32-blackbox-exporter: # Endpoint Monitoring
  room-33-statsd-exporter:   # StatsD to Prometheus Bridge
  room-34-pushgateway:       # Batch Metrics Push
```

**11 Services. Ein Command. Vollständige Observability.**

Zugriff:
- Grafana: `http://grafana.delqhi.com:3001`
- Prometheus: `http://localhost:9090`
- AlertManager: `http://alertmanager.delqhi.com:9093`
- Jaeger: `http://localhost:16686`

---

## REST API: Das Dashboard Backend

Der Fleet Monitor exponiert eine vollständige REST API die unser Dashboard unter `a2a.delqhi.com` antreibt:

```
GET  /api/fleet/status              # Status aller 9 Teams
GET  /api/dashboard/summary         # Fleet-wide Summary
GET  /api/agents                    # Alle Agenten auflisten
GET  /api/agents/:agentId           # Details + neueste Metriken
GET  /api/agents/:agentId/metrics   # Time-Series (CPU, Memory, etc.)
GET  /api/agents/:agentId/health    # Health History
GET  /api/teams                     # Alle Teams
GET  /api/teams/:teamId             # Team Details + Agent Status
GET  /api/alerts                    # Alert History
GET  /api/alerts/active             # Aktive Alerts
POST /api/alerts/:alertId/resolve   # Alert auflösen
GET  /api/costs/summary             # Kosten-Zusammenfassung
GET  /api/metrics/timeseries        # Flexible Time-Series Queries
GET  /metrics                       # Prometheus Metrics
```

---

## Self-Healing: Wenn Monitoring Zum Action Wird

Monitoring allein ist nutzlos wenn niemand handelt. Deshalb ist unser Fleet Monitor direkt mit dem Self-Healing-System verdrahtet:

```
Agent crasht
    ↓
Fleet Monitor erkennt Ausfall (innerhalb 30s)
    ↓
Consecutive Failures ≥ 3
    ↓
Alert feuert an Telegram + Discord
    ↓
GitHub Issue wird erstellt (via A2A-SIN-GitHub-Issues)
    ↓
SIN-Hermes delegiert an Team Coder
    ↓
Auto-Healing: Neue VM-Instanz, Session-Recovery, Re-Registration
    ↓
Agent ist zurück (innerhalb 90s)
    ↓
Fleet Monitor: "healthy" — Alert wird resolved
```

**Kein Mensch. Kein PagerDuty. Keine 3am-Anrufe. Das System heilt sich selbst.**

---

## Die Kosten: Monitoring für Pennies

| Komponente | Monatliche Kosten |
|---|---|
| **Fleet Monitor VM** | ~€0 (HF Free Tier) |
| **Prometheus Storage** | ~€5 (50GB, 30 Tage) |
| **Grafana Hosting** | ~€0 (selber gehostet) |
| **Telegram Bot API** | €0 |
| **Discord Webhooks** | €0 |
| **GESAMT** | **~€5/Monat** |

**108+ Agenten überwachen. Für weniger als ein Kaffee. Pro Monat.**

---

## Fazit

Wer 108+ Agenten betreibt ohne ein Monitoring-System das so autonom ist wie die Agenten selbst, der hat kein Fleet. Der hat ein Chaos das darauf wartet zu eskalieren.

Unser Fleet Monitor ist kein Add-On. Er ist die zentrale Nervenbahn die alles zusammenhält.

**108+ Agenten. 9 Teams. 147 Repos. 30 Sekunden Poll-Intervall. 7 Alert-Typen. 2 Notification-Channels. 0 Menschen die nachts aufwachen.**

Das ist kein Marketing. Das ist Architektur.

---

> **OpenSIN. Nicht ein Agent. Eine Zivilisation.**
>
> 🌐 https://opensin.ai · 🐙 https://github.com/OpenSIN-AI
