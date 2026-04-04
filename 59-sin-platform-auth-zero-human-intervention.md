# SIN Platform Auth: 12 Plattformen, 0 Klicks — Vollautonome Browser-Sessions

**Veröffentlicht:** 2026-04-04  
**Autor:** SIN Platform Auth Team  
**Tags:** #platform-auth #browser-sessions #zero-human-intervention #cookie-extraction #a2a

---

## Das Problem

Platform Agents (X, YouTube, Instagram, Reddit, Discord, etc.) brauchen Logins. Bisher gab es zwei Optionen:

1. **API Tokens manuell erstellen** → User muss sich auf jeder Plattform registrieren, API-Keys generieren, in .env-Files eintragen
2. **Browser-Automation mit Login** → User muss "mal eben" im Browser einloggen, Cookies warten ab

Beides verstößt gegen das **Zero Human Intervention Mandate**. OpenSIN existiert damit Menschen nicht mehr gebraucht werden — also darf es auch kein "klick mal bitte" geben.

## Die Lösung: sin-platform-auth

**sin-platform-auth** extrahiert Browser-Sessions **vollautomatisch** aus dem Chrome Default Profil:

```
Chrome Default Profil (Cookies.db)
  ↓
sin-platform-auth erkennt eingeloggte Sessions
  ↓
Chrome Safe Storage Key (macOS Keychain)
  ↓
PBKDF2-SHA1 + AES-128-CBC Entschlüsselung
  ↓
Auth-Cookies → sin-passwordmanager
  ↓
Platform Agents auf HF Spaces injizieren Cookies
```

**Der User surft normal in Chrome. Das war's.** Kein extra Login, kein Token-Management, kein "klick mal bitte".

## Unterstützte Plattformen (12)

| Plattform | Auth Cookies | Status |
|-----------|-------------|--------|
| X (Twitter) | 4 | ✅ LOGGED IN |
| YouTube | 29 | ✅ LOGGED IN |
| Instagram | 0 | ❌ Nicht besucht |
| Reddit | 0 | ❌ Nicht besucht |
| Discord | 0 | ❌ Nicht besucht |
| Medium | 0 | ❌ Nicht besucht |
| TikTok | 0 | ⚠️ Besucht, nicht eingeloggt |
| HuggingFace | 2 | ✅ LOGGED IN |
| LinkedIn | 2 | ✅ LOGGED IN |
| Telegram | 0 | ❌ Nicht besucht |
| Google-Apps | 19 | ✅ LOGGED IN |
| Community | 5 | ✅ LOGGED IN |

**6/12 Plattformen eingeloggt, 1 besucht, 5 nicht besucht**

## Technical Deep Dive: Cookie-Entschlüsselung

Chrome verschlüsselt Cookies auf macOS mit dem **v10 Format**:

```
v10 + AES-128-CBC(PBKDF2-SHA1(key, "saltysalt", 1003), IV=" "*16, data)
```

### BUG-091: Der Padding-Fix

**Problem:** Die Entschlüsselung lieferte `null` für alle Cookies.

**Ursache 1:** Der Chrome Safe Storage Key wurde als base64-decoded bytes statt als UTF-8 String an PBKDF2 übergeben.

```javascript
// FALSCH:
const masterKey = Buffer.from(safeStorageKey, 'base64');

// RICHTIG:
const masterKey = Buffer.from(safeStorageKey, 'utf8');
```

**Ursache 2:** Chrome 146 verwendet nicht immer korrektes PKCS7 Padding. Das Padding-Byte war oft > 16 oder inkonsistent.

**Lösung:** Fallback auf longest printable ASCII extraction:

```javascript
const text = decrypted.toString('utf8');
const matches = text.match(/[\x20-\x7e]{8,}/g);
if (matches) return matches.reduce((a, b) => a.length > b.length ? a : b);
```

## CLI Usage

```bash
# Session Status aller Plattformen
sin-platform-auth status

# Cookies exportieren
sin-platform-auth export youtube

# Alle Cookies speichern
sin-platform-auth save
```

## MCP Integration

Platform Agents nutzen sin-platform-auth MCP Tools um Sessions zu laden und zu injizieren:

```javascript
// In Platform Agent (HF Space)
const cookies = await platform_auth_load({ platform: 'youtube' });
for (const cookie of cookies) {
  await page.setCookie(cookie);
}
await page.goto('https://youtube.com');
// Agent ist eingeloggt — kein User-Login nötig
```

## Security

- **Keine Tokens im Code:** Alle Credentials über sin-passwordmanager
- **macOS Keychain:** Chrome Safe Storage Key geschützt durch System-Keychain
- **File Permissions:** Cookie-Files mit `chmod 600` (nur User lesbar)
- **Auto-Expiry:** Cookies haben expires_utc — abgelaufene Sessions werden erkannt

## Was noch fehlt

5 Plattformen sind noch nicht im Chrome Profil besucht:
- Instagram, Reddit, Discord, Medium, Telegram

Sobald der User diese Plattformen im Chrome besucht und sich einloggt, erkennt sin-platform-auth die Sessions automatisch beim nächsten `sin-platform-auth status` Lauf.

## Fazit

sin-platform-auth ist der letzte Baustein für **vollautonome Platform Agents**. Keine manuellen Logins, keine API-Keys, keine .env-Files. Der User surft normal — die Agenten arbeiten mit den extrahierten Sessions.

**Zero Human Intervention. Punkt.**
