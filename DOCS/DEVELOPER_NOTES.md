# SNAKE on FIRE — Developer Notes

**Leitfaden für Chefentwickler. Alles was du wissen musst um das Projekt anzufassen.**

Stand: v1.4.2 · 15. Mai 2026  
Autor: Projektentwicklung in Zusammenarbeit mit Claude Code (Anthropic)

---

## 1. PROJEKT-ÜBERSICHT

### Was ist das?
Ein vollständiges Browser-Arcade-Spiel: modernes, hyperintensives Snake. Single-File HTML/CSS/JS, keine Build-Pipeline, keine Dependencies außer Firebase SDK (optional) und Google Fonts.

### Deliverable
**Eine einzige Datei:** `snake-on-fire.html` — enthält HTML-Struktur, alle CSS-Stile, und das vollständige JavaScript-Spiel. `index.html` ist eine identische Kopie (für Hosts die `index.html` erwarten).

### Tech-Stack
| Layer | Technologie |
|-------|-------------|
| Rendering | HTML5 Canvas (768×528px, `requestAnimationFrame`) |
| Sprache | Vanilla JavaScript, kein Framework |
| Styles | CSS3 mit CSS Custom Properties |
| Font | Press Start 2P (Google Fonts CDN) |
| Leaderboard | Firebase Realtime Database (optional, `USE_FIREBASE = true`) |
| Hosting | Netlify (auto-deploy aus GitHub `main`) |
| Source | GitHub: `wellengtown265/GAMES_and_FUN` |
| Domain | https://snake-on-fire-v1-3-2.netlify.app/ |

---

## 2. DATEI-STRUKTUR

```
SNAKE_on_FIRE_2026/
├── snake-on-fire.html     ← Hauptspieldatei (SOURCE OF TRUTH)
├── index.html             ← Identische Kopie von snake-on-fire.html
├── CLAUDE.md              ← Anleitung für Claude Code (KI-Assistent)
├── DEPLOY_WORKFLOW.md     ← Schritt-für-Schritt Deploy-Anleitung
├── netlify.toml           ← Netlify: publish = "." (kein Build-Schritt)
├── database.rules.json    ← Firebase RTDB Security Rules
├── firebase.json          ← Firebase CLI Konfiguration (nur database)
├── .firebaserc            ← Firebase Projekt-Referenz (snake-on-fire)
├── .gitignore             ← .DS_Store, node_modules, etc.
├── ASSETS/
│   └── wellingtown.png    ← Wellingtown Dorf-Grill Bild (Quelldatei, 1566×1005px)
│                            (Im Spiel als Base64-JPEG eingebettet, nicht als File geladen)
└── DOCS/
    ├── VERSIONS.md        ← Diese Versionsgeschichte
    ├── DEVELOPER_NOTES.md ← Dieses Dokument
    ├── GAME_DESIGN.md     ← Ursprüngliches Design-Masterfile (Vision/Roadmap)
    ├── SECURITY_ANALYSE.md ← Firebase Security-Analyse
    ├── LEARNINGS_SESSION1.md ← Lernnotizen erste Session
    └── LEARNINGS_SHORT.md ← Kurze Lernnotizen
```

**Wichtig für Netlify:** `netlify.toml` hat `publish = "."` — der Root-Ordner wird deployed. `snake-on-fire.html` und `index.html` müssen im Root bleiben.

**Wichtig für Firebase CLI:** `database.rules.json`, `firebase.json`, `.firebaserc` müssen im Root bleiben. `firebase deploy` erwartet sie dort.

---

## 3. ARCHITEKTUR DES SPIELS

### 3.1 Datei-Aufbau (top to bottom)

```
<head>
  Viewport-Meta, PWA-Meta-Tags, Google Fonts, Firebase SDK CDN
  <style> CSS-Stile </style>
</head>
<body>
  <div id="wrap"> Canvas + Overlay-Divs + pauseBtn </div>
  <div id="dpad"> 4 Richtungstasten (Mobile) </div>
  <button id="nameBtn"> Keyboard-Trigger (Mobile) </button>
  <input id="nameInput"> Verstecktes Text-Input (iOS Keyboard) </input>
  <script>
    Firebase-Initialisierung
    WELLINGTOWN_B64-Konstante (Base64-JPEG)
    Spielkonstanten (W, H, CELL, COLS, ROWS, STATES)
    State-Variablen (state, snake, dir, score, etc.)
    Game-Logic (resetGame, spawnFood, tickGame, etc.)
    Render-Funktionen (drawBackground, drawSnake, drawMenu, etc.)
    Input-Handler (keydown, touchstart/move/end)
    Mobile Name-Entry (nameInputEl, focusNameInput)
    routeKey() — State-Machine Input-Router
    Mobile UI (updateMobileUI, D-Pad-Listener)
    Main Loop (update, render, loop)
  </script>
</body>
```

### 3.2 State Machine

Das Herzstück des Spiels. `state` enthält immer einen der folgenden Werte:

```
STATES = {
  INTRO           → Startbildschirm mit Animation
  MENU            → Hauptmenü (NEW GAME / HIGHSCORE)
  PCOUNT          → Spieleranzahl wählen (1-4)
  NAME            → Name(n) eingeben
  COUNTDOWN       → 3-2-1 Countdown
  PLAYING         → Aktives Gameplay
  PAUSED          → Pause-Screen
  GAMEOVER        → Tod-Animation
  RESULTS         → Runden-Ergebnis nach allen Spielern
  HIGHSCORE       → Lokale + Global Top 10 Übersicht
  GLOBAL_HIGHSCORE → Wellingtown-Hintergrund + Firebase Top 10
}
```

**Drei Touchpoints beim Hinzufügen eines neuen States:**
1. `STATES`-Enum erweitern
2. `draw<State>()` Funktion schreiben + in `render()`-Switch registrieren
3. Eingabe-Handling in `routeKey()` Switch ergänzen

### 3.3 Game Loop Entkopplung

Render-Tick und Logik-Tick sind **getrennt**:
- `update(dt)` läuft jeden Frame — akkumuliert `tickTimer`
- Wenn `tickTimer >= tickInterval` → `tickGame()` (ein Logik-Schritt)
- `render()` läuft jeden Frame — **interpoliert** Snake-Position mit `progress = tickTimer / tickInterval`

Das ergibt flüssige Bewegung auf beliebiger Bildwiederholrate trotz diskretem Grid.

**Kritisch:** Snake nie außerhalb von `tickGame()` mutieren, sonst springt die Interpolation.

### 3.4 Input-Pipeline

```
keydown / touchend → routeKey(e)
                  → state-switch → direkte State-Änderung
                     oder
                  → setDir(d) → dirQueue (max 2 Einträge)
                                → tickGame() konsumiert je einen pro Tick
```

`setDir()` verhindert 180°-Umkehr (= Selbst-Biss). Die Queue mit max. 2 Einträgen erlaubt schnelle Voraus-Eingaben.

### 3.5 Fire-Level System

`fireLevel()` gibt 0-6 zurück basierend auf `foodEaten`:
- 0: normal (< 10 gegessen)
- 1: warm (≥ 10)
- 2: heiß (≥ 20)
- 3: brennend (≥ 30)
- 4: ESKALATION blau/rot (≥ 40)
- 5: MASSIVE ESKALATION (≥ 50)
- 6: SUPER ESKALATION rainbow (≥ 60)

Beeinflusst: Snake-Farben, Partikel-Count/Typ, HUD-Label, Hellfire-Overlay.

### 3.6 Firebase Integration

```javascript
USE_FIREBASE = true  // oben in <script>

// Listener (einmalig bei initFirebase):
db.ref('highscores').orderByChild('score').limitToLast(10).on('value', snap => {
  applyGlobalHighscoresSnapshot(snap);  // → globalTop10 Array
});

// Upload nach Spielende:
uploadScore(name, score)  // → scoreUploadQueue → flushScoreUploadQueue()
```

Score-Upload ist **queue-basiert** mit Retry-Logik. Falls Firebase offline → Queue hält Scores bis Reconnect.

**Firebase Projekt:** `snake-on-fire` → URL: `snake-on-fire-default-rtdb.europe-west1.firebasedatabase.app`  
**Achtung:** Nicht verwechseln mit dem Firebase-Console-Projekt `snake-on-fire-57e9f` (hat eigene RTDB, andere URL).

---

## 4. MOBILE SUPPORT (v1.4.2)

### 4.1 Layout

```css
body { display:flex; flex-direction:column; align-items:center; justify-content:center; }
#wrap { width: min(96vw, calc((100dvh - 64px) * (768/528))); aspect-ratio: 768/528; }
```

- Landscape: Canvas füllt ~80-90% der Höhe
- Portrait: Canvas skaliert auf Breite, D-Pad erscheint darunter
- `touch-action:none` auf Canvas + Body verhindert Browser-Scroll während Spielens

### 4.2 D-Pad

4 HTML-Buttons (`#dUp`, `#dDown`, `#dLeft`, `#dRight`) in `<div id="dpad">`.  
Sichtbar: `@media (pointer:coarse)` + State PLAYING/PAUSED.  
Events: `touchstart` (sofort, `passive:false`) + `click` (Fallback für Maus-Tests).

```javascript
updateMobileUI()  // gecacht, wird jeden Frame aus update() aufgerufen
// Vergleicht state mit _prevMobileState → DOM nur bei State-Change anfassen
```

### 4.3 iOS Keyboard Fix

Problem: iOS öffnet Soft-Keyboard nur bei `focus()` **direkt in einem User-Gesture-Handler**.

Lösung in `focusNameInput()`:
```javascript
try { nameInputEl.focus(); } catch(_) {}       // 1. Synchron (im Handler)
setTimeout(() => { nameInputEl.focus(); }, 50); // 2. Fallback nach 50ms
```

Zusätzlich: `nameInput` CSS ist 4×4px (nicht 1×1px) und `z-index:100` — iOS ignoriert zu kleine Elemente manchmal.

`<button id="nameBtn">⌨ TAP TO TYPE</button>` ist ein echter HTML-Button unter dem Canvas — sein `touchstart`-Handler ruft `focusNameInput()` auf. Echter Button = garantierter User-Gesture-Kontext auf iOS.

### 4.4 Touch-Swipe (Gameplay)

Während PLAYING: Swipe in 4 Richtungen = `setDir()` (gleicher Code-Pfad wie Tastatur).  
Threshold: `SWIPE_MIN_DIST=30px`, `SWIPE_MAX_TIME=500ms`.  
Swipe-Hint zeichnet eine Linie vom Start zum aktuellen Finger-Punkt während des Swipes.

Auf Screens (nicht PLAYING):
- Swipe → auf HIGHSCORE → GLOBAL_HIGHSCORE öffnen (= G-Taste)
- Swipe ← auf GLOBAL_HIGHSCORE → zurück zu HIGHSCORE
- Tap (< 30px, < 500ms) → `tapAsEnter()` → entspricht ENTER-Taste im aktuellen State

---

## 5. WELLINGTOWN HINTERGRUND

### Technische Umsetzung

**v1.4:** Geladen per `img.src = './wellingtown.png'` — funktionierte lokal aber nicht immer auf Netlify (Pfad-Problem) und hatte Race-Condition beim ersten Render.

**v1.4.2:** Direkt als Base64-JPEG eingebettet:
```javascript
const WELLINGTOWN_B64 = 'data:image/jpeg;base64,/9j/4AAQSkZ...(64KB)...';
// In loadGlobalBg():
img.src = WELLINGTOWN_B64;  // kein HTTP-Request, sofort verfügbar
```

**Optimierung:** Quelldatei (`ASSETS/wellingtown.png`) ist 1566×1005px, 2.3MB.  
Für Base64 verkleinert auf 640×410px, JPEG quality 65 → ~64KB.

**Python-Befehl zum Neu-Generieren bei Bildänderungen:**
```python
from PIL import Image
import io, base64
img = Image.open('ASSETS/wellingtown.png').convert('RGB')
img = img.resize((640, int(640 * img.size[1] / img.size[0])), Image.LANCZOS)
buf = io.BytesIO()
img.save(buf, 'JPEG', quality=65, optimize=True)
b64 = 'data:image/jpeg;base64,' + base64.b64encode(buf.getvalue()).decode()
# → b64 in WELLINGTOWN_B64 Konstante einfügen
```

---

## 6. DEPLOY-WORKFLOW (Kurzfassung)

```bash
# 1. Änderungen stagen + committen
git add snake-on-fire.html index.html
git commit -m "vX.Y.Z: beschreibung"

# 2. Push zu GitHub (HTTP/1.1 Pflicht!)
git -c http.version=HTTP/1.1 -c http.postBuffer=524288000 push origin main

# 3. Firebase Rules deployen (nur wenn database.rules.json geändert)
firebase deploy --only database

# 4. Netlify baut automatisch (1-2 Min) aus GitHub main
```

**Warum HTTP/1.1?** git push mit HTTP/2 gibt auf diesem System `curl 22: HTTP 400`. Ursache unklar (vermutlich Netzwerk-Proxy oder libcurl-HTTP/2-Bug). HTTP/1.1 funktioniert zuverlässig.

**GitHub CLI Auth:** `gh` ist als `wellengtown265` eingeloggt (keyring). `gh auth setup-git` konfiguriert git-Credential-Helper.

---

## 7. BEKANNTE PATTERNS UND FALLSTRICKE

### Pixel-Art Rendering
```javascript
image-rendering: pixelated; /* Canvas CSS */
ctx.fillRect(x, y, w, h);  /* Alles mit fillRect, kein drawImage mit Anti-Aliasing */
```
`drawBlockPx(px, py, fill, edge, shine)` ist der Haupt-Helper für 3-Ton-Pixel-Blöcke.

### Partikel-System
- `particles[]`: Spiel-Partikel (Feuer, Eis, Bonus)
- `ambient[]`: Hintergrund-Partikel (immer aktiv)
- Beide als einfache Arrays, kein Pool. Partikel mit `lifetime` werden in `updateParticles()` entfernt.

### Multi-Player
`players = [{name, score}]`, `currentPlayer` Index. `advanceAfterDeath()` wechselt zum nächsten Spieler oder geht zu RESULTS.

### LocalStorage Highscore
Key: `snakeOnFire_hs`. Array von `{name, score, date}`. Top 10 werden gespeichert. Reset: Browser DevTools → Application → Local Storage → löschen.

### Kein Build-Step
Kein Webpack, kein TypeScript, kein Bundler. Öffne `snake-on-fire.html` direkt im Browser. Für Firebase-Features braucht man einen lokalen HTTP-Server (kein `file://`-Protokoll erlaubt):
```bash
python3 -m http.server 8888
# → http://localhost:8888/snake-on-fire.html
```

---

## 8. GELERNTE LEKTIONEN (HEUTE — 15. Mai 2026)

### Firebase
- **Zwei verschiedene Projekte können beide `snake-on-fire` heißen** aber unterschiedliche RTDB-URLs haben. Immer die `databaseURL` aus `firebaseConfig` mit der RTDB-URL in der Firebase Console vergleichen.
- **Ein dauerhafter `.on('value')`-Listener** ist besser als mehrere bei Reconnect. Mehrfache Listener = doppelte/dreifache Events = Flackern.
- **`push()` statt `set()`** für Highscore-Einträge. `set()` würde alle anderen Einträge überschreiben.
- **Security Rules** müssen `!data.exists()` prüfen wenn nur neue Einträge erlaubt sein sollen.

### Canvas / Game Development
- **Render- und Logik-Tick trennen** ermöglicht flüssige Animation unabhängig von der Spielgeschwindigkeit.
- **prevSnake Snapshot** muss VOR der Snake-Mutation gemacht werden, nicht danach.
- **Base64-eingebettete Bilder** lösen Pfadprobleme bei verschiedenen Deploy-Umgebungen komplett. Nachteil: Datei wird größer.
- **Image async loading** braucht explizites State-Management (`globalBgLoading`-Flag), sonst können mehrere Loads gleichzeitig starten.
- **Canvas-Koordinaten** für Text mit `Press Start 2P` immer testen — der Font hat ungewöhnliche Metriken.

### iOS / Mobile
- **iOS `focus()` in setTimeout** kann die Keyboard öffnen, aber nur wenn der Timeout kurz genug ist und vom User-Gesture-Chain ausgelöst wurde. Seit iOS 17 strenger: synchroner Aufruf + Fallback-Timeout ist robuster.
- **`touch-action:none`** auf Canvas und Body ist essentiell — sonst scrollt die Seite beim Swipen.
- **`@media (pointer:coarse)`** ist der korrekte Weg um Touch-Geräte zu erkennen (nicht User-Agent-Sniffing).
- **D-Pad als echte HTML-Elemente** (nicht Canvas-gezeichnet) sind zuverlässiger für Touch-Events als Canvas-Tap-Zonen.

### Git / Deployment
- **HTTP/2 Push-Bug:** `git push` gibt auf diesem System HTTP 400. Fix: `-c http.version=HTTP/1.1 -c http.postBuffer=524288000`.
- **`gh auth setup-git`** konfiguriert den git-Credential-Helper für GitHub CLI-Token.
- **Netlify auto-deploy** aus GitHub `main` funktioniert ohne manuellen Trigger. 1-2 Minuten nach Push ist die neue Version live.
- **`firebase deploy --only database`** deployed nur die RTDB Rules (kein Hosting in diesem Setup).

---

## 9. ROADMAP / NÄCHSTE SCHRITTE

### v2.0 — Musik (Geplant)

**Empfohlener Ansatz: Web Audio API (Chiptune, kein externes Asset)**

Warum: Keine Dateigrößen-Probleme, vollständig in-file, extrem passend zum Retro-Arcade-Feeling.

Konzept:
```javascript
// Adaptiver Soundtrack: Loop intensiviert sich mit Fire-Level
// Basis: Square/Sawtooth-Wave Chiptune bei ~120 BPM
// Level 0-2: Ruhige Melodie
// Level 3-4: Schnelleres Tempo, Bass-Boost
// Level 5-6: Chaotische Arpeggios, Distortion

const audioCtx = new AudioContext();
function playNote(freq, duration, type='square') { ... }
function startGameMusic() { ... }  // Loop mit requestAnimationFrame oder setInterval
```

Soundeffekte:
- Fressen: kurzer Aufwärts-Chirp
- Tod: Abwärts-Glissando
- Eskalation: Power-Up-Sound
- Bonus: kurze Jingle-Phrase

**Bekannte Herausforderung:** `AudioContext` muss durch User-Gesture gestartet werden (iOS/Chrome-Policy). Am besten bei erstem Tap auf INTRO oder MENU starten.

### Weitere Ideen aus GAME_DESIGN.md
- Boss-Modus / Wellen-System
- Daily Challenges (Seeded Random per Tag)
- Cosmetic Unlocks (Snake-Skins)
- Controller-Support (Gamepad API)
- Replay-System (Record dirQueue + Timestamps)

---

## 10. QUICK REFERENCE

### Häufige Aufgaben

**GAME_VERSION erhöhen:**
```bash
# In snake-on-fire.html:
const GAME_VERSION = 'X.Y.Z';
# Dann: cp snake-on-fire.html index.html
```

**Neues Wellingtown-Bild einbetten:**
```bash
python3 -c "
from PIL import Image; import io, base64
img = Image.open('ASSETS/wellingtown.png').convert('RGB')
img = img.resize((640, int(640 * img.size[1] / img.size[0])))
buf = io.BytesIO(); img.save(buf, 'JPEG', quality=65, optimize=True)
print('data:image/jpeg;base64,' + base64.b64encode(buf.getvalue()).decode())
"
# → Output als WELLINGTOWN_B64 Konstante in snake-on-fire.html einfügen
```

**Lokaler Test-Server:**
```bash
python3 -m http.server 8888
```

**Deploy:**
```bash
git add snake-on-fire.html index.html
git commit -m "vX.Y.Z: was wurde geändert"
git -c http.version=HTTP/1.1 -c http.postBuffer=524288000 push origin main
firebase deploy --only database  # nur wenn Rules geändert
```

**Firebase Rules manuell checken:**  
→ Firebase Console → Projekt `snake-on-fire` → Realtime Database → Rules

**Netlify Build-Status checken:**  
```bash
curl -sI "https://snake-on-fire-v1-3-2.netlify.app/" | head -3
```
