# SNAKE_on_FIRE_2026 — Deployment & Firebase Learnings

## Ziel

Das lokale Browsergame:
- sauber über GitHub versionieren
- öffentlich spielbar machen
- globale Highscores aktivieren
- anonymisiert deployen
- reproduzierbaren Workflow aufbauen

---

# Finales System

## Infrastruktur

| Bereich | Tool |
|---|---|
| Versionskontrolle | Git |
| Repository | GitHub |
| Hosting | Netlify |
| Datenbank / Highscores | Firebase Realtime Database |
| Lokale Entwicklung | macOS Terminal |

---

# Wichtigste Learnings

## 1. Git arbeitet immer ordnerbasiert

Fehler:
```bash
fatal: not a git repository
```

Bedeutung:
- falscher Ordner
- kein `.git` vorhanden

Wichtiger Workflow:

```bash
cd DEIN_PROJEKTORDNER
pwd
git status
```

---

# 2. GitHub != lokales Git

GitHub Repo alleine reicht nicht.

Lokal muss ebenfalls:
- `git init`
- Branch
- Commit
- Remote

existieren.

---

# 3. Clean Reset eines Repos

Wichtigster Lernschritt:

```bash
rm -rf .git
```

Löscht:
- Commit-Historie
- Branches
- Git-Konfiguration

Löscht NICHT:
- Spielcode
- HTML-Dateien
- Assets

Danach:

```bash
git init
git branch -M main
git add .
git commit -m "initial release"
```

---

# 4. GitHub anonymisieren

Ziel:
- kein echter Name
- keine private Mail

Konfiguration:

```bash
git config --global user.name "wellengtown265"
git config --global user.email "284475823+wellengtown265@users.noreply.github.com"
```

Prüfen:

```bash
git config --global --list
```

---

# 5. GitHub Push Workflow

Finaler Standardworkflow:

```bash
git add .
git commit -m "update"
git push
```

Das synchronisiert:
- lokale Änderungen
- GitHub Repo
- Netlify Deploy

---

# 6. Unterschied main vs master

Problem:
- GitHub verwendete `main`
- lokales Git verwendete `master`

Fix:

```bash
git branch -M main
```

Dann:

```bash
git push -u origin main
```

---

# 7. Netlify Deployment

Warum Netlify:
- extrem schnell
- ideal für HTML Games
- Auto-Deploy über GitHub
- einfacher als GitHub Pages

Workflow:

1. Netlify Account
2. GitHub verbinden
3. Repo importieren
4. Build Settings leer lassen
5. Deploy

Finale URL:

https://snake-on-fire-v1-3.netlify.app/

---

# 8. Firebase Realtime Database

Ziel:
- globale Highscores
- persistente Speicherung

---

## Firebase CLI Installation

macOS Permission Fix:

```bash
sudo npm install -g firebase-tools
```

Dann:

```bash
firebase login
```

---

# 9. Firebase Projekt

Erstellt:
- `snake-on-fire`
- Projekt-ID:
  `snake-on-fire-57e9f`

---

# 10. Realtime Database

Aktiviert:
- Realtime Database
- Europe West Region
- Test Mode

Database URL:

```text
https://snake-on-fire-57e9f-default-rtdb.europe-west1.firebasedatabase.app/
```

---

# 11. Firebase lokal verbinden

Projektliste:

```bash
firebase projects:list
```

Projekt verbinden:

```bash
firebase use --add
```

Dann:

```bash
firebase init database
```

Rules-Datei:

```text
database.rules.json
```

Deploy:

```bash
firebase deploy --only database
```

---

# 12. Finaler Produktionsworkflow

## Änderungen am Spiel

```bash
git add .
git commit -m "update"
git push
```

Automatisch passiert:
- GitHub updated
- Netlify deployed
- Spiel-Link bleibt identisch
- Friends sehen Updates sofort

---

# 13. Architekturübersicht

```text
LOKALER CODE
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

# 14. Wichtigste Erkenntnisse

## Terminal-Arbeit ist reproduzierbar

Fast alles bestand aus:
- klaren Befehlen
- sauberer Reihenfolge
- kleinen Debug-Schritten

---

## Git ist kein Hosting

Git:
- speichert Versionen

Netlify:
- hostet

Firebase:
- speichert Live-Daten

---

## Der wichtigste Skill

Nicht:
- alles auswendig kennen

Sondern:
- verstehen welche Systeme verbunden sind
- reproduzierbare Pipelines bauen

---

# Finaler Status

## Live-Spiel
https://snake-on-fire-v1-3.netlify.app/

## GitHub
https://github.com/wellengtown265/GAMES_and_FUN

## Features
- öffentlich spielbar
- globale Highscores
- automatischer Deploy
- anonymisierte Git-Identität
- reproduzierbarer Workflow
