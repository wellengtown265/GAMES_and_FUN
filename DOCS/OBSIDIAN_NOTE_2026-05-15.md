# 📅 2026-05-15 — Arbeitstag: SNAKE on FIRE Development

**Tags:** #development #snake-on-fire #firebase #mobile #canvas #javascript #deployment

---

## 🎮 Projekt: SNAKE on FIRE

**Repository:** `wellengtown265/GAMES_and_FUN` (GitHub)  
**Live-URL:** https://snake-on-fire-v1-3-2.netlify.app/  
**Version am Ende des Tages:** `v1.4.2`

---

## ✅ Was wurde gemacht

### Feature: GLOBAL_HIGHSCORE Screen (v1.4)
- Neuen Game-State `GLOBAL_HIGHSCORE` in die State-Machine integriert
- Vollständige `drawGlobalHighscore()` Render-Funktion geschrieben
- **Wellingtown Dorf-Grill** Bild als Fullscreen-Hintergrund (Glassmorphism-Design)
- Live-Firebase-Top-10-Tabelle mit 🔥 / ✦ / 🐍 Dekorationen
- Navigation: G-Taste auf HIGHSCORE öffnet die neue Seite, ESC geht zurück
- **FEUERTONNEN PRODUCTIONS** Branding unten links auf dem Global-Highscore-Screen

### Fix: Wellingtown-Hintergrund Sichtbarkeit (v1.4.1)
- Race-Condition in `loadGlobalBg()` behoben: Flag `globalBgLoading` verhindert Mehrfach-Loads
- Darkungs-Overlay (`rgba(0,0,0,0.35)`) für Text-Lesbarkeit über dem Bild
- Box-Größe auf 440×320px reduziert und perfekt zentriert

### Feature: Vollständiger Mobile-Support (v1.4.2)
- **D-Pad Overlay:** 4 HTML-Buttons (▲ ▼ ◄ ►) erscheinen automatisch auf Touch-Geräten
- **pauseBtn:** Echter HTML-Button oben rechts im Canvas-Wrap
- **nameBtn "⌨ TAP TO TYPE":** Öffnet zuverlässig iOS-Keyboard bei Namenseingabe
- **iOS Keyboard-Fix:** `focus()` synchron + nach 50ms, 4×4px Input-Element
- **Y-Taste** als ESC-Alternative in allen relevanten States
- **Swipe-Navigation:** → auf Highscore → Global, ← zurück
- **Wellingtown als Base64:** 64KB JPEG direkt im Code — kein externer File-Load

### Deployment
- **GitHub Push-Bug gelöst:** HTTP/2 = 400-Fehler. Fix: `-c http.version=HTTP/1.1`
- Alle 3 Plattformen erfolgreich deployed: GitHub, Netlify (auto), Firebase Rules
- DEPLOY_WORKFLOW.md mit HTTP/1.1-Fix aktualisiert

### Ordner-Reorganisation
- `DOCS/` Ordner angelegt, alle Dokumentations-Dateien sortiert verschoben
- `ASSETS/` Ordner für Mediendateien (wellingtown.png)
- `DEVELOPER_NOTES.md` als umfassende technische Referenz erstellt
- `VERSIONS.md` mit allen neuen Versionen aktualisiert
- `.bak`-Datei gelöscht

---

## 🧠 Was wurde gelernt

### Firebase
> **Problem:** Zwei Projekte können identisch heißen aber verschiedene RTDB-Instanzen haben.
> **Lösung:** `databaseURL` in `firebaseConfig` vs. Firebase Console immer vergleichen.

> **Problem:** Mehrfache `.on('value')`-Listener bei Reconnects → Daten flackern.
> **Lösung:** Einen einzigen permanenten Listener registrieren (nie erneut `.on()` bei Reconnect).

> **Regel:** `push()` statt `set()` für Leaderboard-Einträge. `set()` überschreibt den ganzen Knoten.

### Canvas & Game Loop
> **Entkopplung von Render- und Logik-Tick** = flüssige Bewegung bei beliebiger FPS.  
> `prevSnake`-Snapshot IMMER vor Mutation. Interpolation berechnet Position zwischen zwei Logik-Ticks.

> **Base64-eingebettete Bilder** lösen Pfadprobleme auf verschiedenen Deployment-Umgebungen.  
> Trade-off: Datei wird größer (~64KB), aber kein HTTP-Request, keine Race-Condition.

### iOS / Mobile Development
> **iOS `focus()` Regel:** Muss direkt aus User-Gesture-Handler aufgerufen werden.  
> Best Practice: Synchron + setTimeout-Fallback. Input-Element mindestens 4×4px.

> **`@media (pointer:coarse)`** = korrekte Touch-Device-Erkennung (nicht User-Agent).

> **Echte HTML-Elemente** für Touch-Controls sind zuverlässiger als Canvas-interne Tap-Zonen.

> **`touch-action:none`** auf Canvas und Body verhindert Browser-Scroll während Spielens.

### Git & Deployment
> **HTTP/2 Push-Bug:** `git push` = HTTP 400 auf diesem System.  
> Fix: `git -c http.version=HTTP/1.1 -c http.postBuffer=524288000 push origin main`

> **Netlify** deployed automatisch 1-2 Minuten nach GitHub-Push. Kein manueller Trigger nötig.

> **`gh auth status`** + `gh auth setup-git` = GitHub CLI konfiguriert automatisch git-Credentials.

---

## 💡 Ideen & Roadmap

### v2.0 — Musik (nächste große Session)
- **Web Audio API** — Chiptune-Musik, vollständig in-file
- Adaptiver Soundtrack: intensiviert sich mit Fire-Level
- Soundeffekte: Fressen, Tod, Eskalation, Bonus
- Herausforderung: `AudioContext` muss durch User-Gesture gestartet werden (iOS-Policy)

---

## 📊 Technischer Status am Tagesende

| Check | Status |
|-------|--------|
| JS Syntax-Check (node --check) | ✅ OK |
| snake-on-fire.html == index.html | ✅ Identisch |
| Firebase Rules deployed | ✅ |
| GitHub main up-to-date | ✅ (11 Commits) |
| Netlify Live | ✅ HTTP 200 |
| Mobile D-Pad implementiert | ✅ |
| iOS Keyboard-Fix implementiert | ✅ |
| Wellingtown Base64 eingebettet | ✅ 64KB |
| DEVELOPER_NOTES.md erstellt | ✅ |
| Ordner aufgeräumt | ✅ |

---

## 🔗 Wichtige Links

- **Live Spiel:** https://snake-on-fire-v1-3-2.netlify.app/
- **GitHub Repo:** https://github.com/wellengtown265/GAMES_and_FUN
- **Firebase Console:** https://console.firebase.google.com/project/snake-on-fire/overview
- **Firebase RTDB:** https://snake-on-fire-default-rtdb.europe-west1.firebasedatabase.app

---

*Erstellt am 15. Mai 2026 · SNAKE on FIRE v1.4.2 · FEUERTONNEN PRODUCTIONS*
