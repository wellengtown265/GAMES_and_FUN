# SECURITY_ANALYSE — SNAKE_on_FIRE_2026

# Überblick

Analyse der aktuellen Sicherheits-, Privacy- und Infrastruktur-Situation des Projekts:

- GitHub
- Netlify
- Firebase
- lokales macOS-System
- Git-Workflows
- öffentliche Browsergame-Architektur

---

# 1. Risiken für das Game / den Code

## 1.1 Manipulation der Highscores

Aktueller Zustand:
- Firebase Realtime Database läuft im Testmodus.

Dadurch kann theoretisch jeder:
- Scores schreiben
- Fake-Daten senden
- Datenbank spammen

Beispiel:

```json
{
  "name": "HACKER",
  "score": 999999999
}
```

### Risiko-Level
MITTEL

### Konsequenzen
- kaputte Ranglisten
- Spam
- Datenbankmüll

### Kein Risiko für:
- lokalen Mac
- GitHub-Account
- persönliche Dateien

---

## 1.2 Code-Kopien

Browsergames sind grundsätzlich öffentlich.

Jeder Besucher kann:
- HTML ansehen
- JavaScript lesen
- Assets herunterladen

Das ist normales Verhalten im Web.

### Risiko-Level
NIEDRIG

### Konsequenz
- Spielcode kann kopiert werden

---

## 1.3 Manipulation von GitHub oder Netlify

Nur möglich bei kompromittierten Accounts:
- schwaches Passwort
- geleakter Token
- fehlende 2FA

### Risiko-Level
MITTEL

### Schutzmaßnahmen
- starke Passwörter
- 2FA
- minimale Token-Rechte

---

# 2. Risiken für persönliche oder private Daten

## 2.1 Git-Leaks

Sehr häufige Entwicklergefahr.

Mögliche Leaks:
- echter Name
- private Mail
- API-Keys
- Tokens
- lokale Pfade

### Bereits umgesetzt
- GitHub No-Reply-Mail
- anonymisierte Commits
- cleanes neues Repository

---

## 2.2 Versehentliche Uploads

Gefährlichster realistischer Fehler.

Problem:
```bash
git add .
```

Nimmt ALLES im Projektordner.

Mögliche versehentliche Uploads:
- Screenshots
- Passwörter
- `.env`
- private Dokumente

### Risiko-Level
HOCH

### Wichtigste Schutzmaßnahme
`.gitignore`

---

## 2.3 macOS Username sichtbar

Teilweise sichtbar in:
- Terminal
- Screenshots
- Videos

Beispiel:
```text
name@RP-MacBook-Pro
```

### Risiko-Level
NIEDRIG

---

# 3. Risiken für das lokale System

## 3.1 Schädliche npm-Pakete

Die größte reale Entwicklergefahr.

Problem:
```bash
npm install irgendwas
```

Ein Paket könnte:
- Malware enthalten
- Tokens stehlen
- Dateien lesen

### Risiko-Level
MITTEL BIS HOCH

### Schutz
- nur bekannte Pakete
- wenige Dependencies
- Pakete prüfen

---

## 3.2 Gefährliche Terminal-Befehle

Terminal arbeitet direkt auf macOS.

Gefährliche Beispiele:
```bash
rm -rf /
```

oder versteckte Shell-Kommandos.

### Risiko-Level
HOCH

### Schutz
- niemals blind Bash-Code kopieren
- Befehle verstehen

---

## 3.3 Browser Extensions

Unterschätztes Risiko.

Extensions können teilweise:
- Webseiten lesen
- Sessions lesen
- Tastatureingaben überwachen

### Risiko-Level
MITTEL

### Schutz
- wenige Extensions
- nur bekannte Anbieter
- Rechte prüfen

---

# 4. Was NICHT passieren kann

Die aktuelle Architektur ist relativ sicher.

Das Browsergame kann NICHT:
- lokale Dateien lesen
- Finder öffnen
- Desktop durchsuchen
- den Mac fernsteuern

Grund:
- Browser Sandbox
- statisches Hosting
- kein lokaler Server

---

# 5. Sicherheitsbewertung der Infrastruktur

| Bereich | Bewertung |
|---|---|
| GitHub Privacy | Sehr gut |
| No-Reply-Mail | Sehr gut |
| Öffentliche Identität | Gut anonymisiert |
| Netlify Hosting | Gut |
| HTTPS | Gut |
| Firebase aktuell | Nur Testbetrieb |
| Lokale Angriffsfläche | Klein |
| Professionelles Hardening | Noch nicht vorhanden |

---

# 6. Wichtigste Maßnahmen

## 6.1 GitHub 2FA aktivieren

https://github.com/settings/security

---

## 6.2 .gitignore konsequent nutzen

Nie committen:
```text
.env
private
tokens
node_modules
passwords
```

---

## 6.3 Keine unbekannten npm-Pakete installieren

---

## 6.4 Keine sensiblen Screenshots veröffentlichen

Terminal kann zeigen:
- Namen
- Pfade
- Mails
- Tokens

---

# 7. Gesamtbewertung

Die aktuelle Architektur ist:
- modern
- relativ sicher
- low-maintenance
- ideal für kleine Browsergames

Die größte reale Gefahr aktuell:
- versehentliche Datenleaks
- nicht externe Hacker

---

# 8. Finaler Infrastruktur-Flow

```text
LOKALER MAC
     ↓
    GIT
     ↓
  GITHUB
     ↓
  NETLIFY
     ↓
 LIVE GAME
     ↓
 FIREBASE
(HIGHSCORES)
```

---

# 9. Finales Fazit

Für:
- kleine öffentliche Browsergames
- Friends
- Lernprojekte
- erste Deployments

ist die aktuelle Lösung:
- sinnvoll
- modern
- ausreichend sicher

Nicht geeignet ohne zusätzliche Absicherung für:
- große Userzahlen
- Echtgeld
- Accounts
- kritische Daten
- kommerzielle Plattformen
