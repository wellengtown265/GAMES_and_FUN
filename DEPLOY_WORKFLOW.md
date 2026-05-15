# Deploy workflow — SNAKE on FIRE

Nach jedem **abgeschlossenen Production-Fix** diese Reihenfolge ausführen.  
Netlify deployt **automatisch** aus dem GitHub-Branch **`main`** (kein manueller Netlify-Upload nötig, sobald die Site mit dem Repo verbunden ist).

---

## Referenz (Stand dokumentiert)

| Was | Wert |
|-----|------|
| **Produktions-URL (Netlify)** | https://snake-on-fire-v1-3-2.netlify.app/ |
| **GitHub-Repo** | https://github.com/wellengtown265/GAMES_and_FUN |
| **Deploy-Branch** | `main` |
| **Firebase (CLI)** | Ziel aus **`.firebaserc`** (`default`); in der Konsole oft als Projekt **`snake-on-fire-57e9f`** sichtbar — **Rules** müssen dieselbe Realtime-Database treffen wie `databaseURL` im HTML. |
| **Realtime Database Rules** | `database.rules.json` |

Die **Web-App** (`snake-on-fire.html` / `index.html`) enthält die **`firebaseConfig`** inkl. `databaseURL`. Nach einem Rules-Deploy kurz prüfen, ob Schreiben/Lesen in der **gleichen** Datenbank-Instanz ankommen wie in der Konsole (siehe Verify unten).

---

## Schritte

### 1. Änderungen stagen

```bash
git add .
```

### 2. Commit (kurze, sinnvolle Message)

```bash
git commit -m "short meaningful message"
```

**Falls** `git commit` bei dir mit `unknown option trailer` o. Ä. fehlschlägt (Hook/Tooling), alternativ:

```bash
printf '%s\n' 'short meaningful message' > /tmp/deploy-msg.txt
git commit --no-verify -F /tmp/deploy-msg.txt
```

### 3. Push zu GitHub

```bash
git -c http.version=HTTP/1.1 -c http.postBuffer=524288000 push origin main
```

**Wichtig:** Das `-c http.version=HTTP/1.1` ist nötig — normales `git push` scheitert mit HTTP 400 auf diesem Rechner (HTTP/2-Inkompatibilität mit GitHub). Einmalig global setzen falls gewünscht: `git config --global http.version HTTP/1.1`

### 4. Firebase deployen

```bash
firebase deploy
```

**Hinweis zu diesem Repo:** In `firebase.json` ist aktuell nur **`database`** (Rules) konfiguriert — **kein Firebase Hosting**. `firebase deploy` deployt damit in der Praxis die **Realtime Database Rules**. Wenn du später Firebase Hosting nutzen willst, `hosting`-Block in `firebase.json` ergänzen.

Nur Rules (explizit):

```bash
firebase deploy --only database
```

### 5. Verify (Checkliste)

Nach dem Push (Netlify braucht meist 1–2 Minuten für den Build):

- [ ] **GitHub:** Neuer Commit auf `main` sichtbar (`https://github.com/wellengtown265/GAMES_and_FUN`).
- [ ] **Netlify:** Letzter Deploy „Published“, keine Build-Fehler; Produktions-URL erreichbar.
- [ ] **Firebase:** `firebase deploy` ohne Fehler; in der [Firebase Console](https://console.firebase.google.com/project/snake-on-fire-57e9f/overview) unter **Realtime Database → Rules** der erwartete Stand.
- [ ] **Live highscores:** Spiel unter der Produktions-URL öffnen, eine Runde zu Ende spielen, **GLOBAL TOP 10** prüfen (mehrere Einträge, kein Flackern nach Refresh); optional zweites Gerät/Browser-Tab.

---

## Optional: nur schnell prüfen

```bash
git status
git log -1 --oneline
curl -sI "https://snake-on-fire-v1-3-2.netlify.app/" | head -3
```

---

*Dokument angelegt und mit dem Repo versioniert — bei Prozess-Änderungen hier anpassen.*
