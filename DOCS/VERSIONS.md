# Versionsgeschichte — SNAKE on FIRE

Chronologische Übersicht aller Releases und technischer Änderungen.  
`GAME_VERSION` in `snake-on-fire.html` + `index.html` immer synchron halten.

---

## v1.4.2 — Mobile-Release (15. Mai 2026)

**Fokus:** Vollständiger Mobile- und iPhone-Support. Das Spiel ist jetzt auf jedem Gerät spielbar.

**Neue Features:**
- **D-Pad Overlay:** 4 Richtungstasten erscheinen automatisch unter dem Spielfeld auf Touch-Geräten (`@media (pointer:coarse)`). Sichtbar während PLAYING/PAUSED, sonst versteckt.
- **Pause-Button (HTML):** `<button id="pauseBtn">` im Canvas-Wrap — auf Touch sichtbar, wechselt `II` ↔ `▶`.
- **Name-Entry-Button:** `<button id="nameBtn">⌨ TAP TO TYPE</button>` unter dem Canvas im NAME-State — öffnet iOS-Keyboard zuverlässig.
- **iOS-Tastatur-Fix:** `focusNameInput()` ruft `focus()` synchron **und** nach 50ms auf. `nameInput`: 4×4px statt 1×1px, `z-index:100` — behebt iOS-Bug.
- **Y-Taste als ESC-Ersatz:** In PLAYING (→ Pause), PAUSED (→ Weiter), HIGHSCORE (→ Menü), GLOBAL_HIGHSCORE (→ zurück).
- **Swipe-Navigation:** Swipe → auf HIGHSCORE → GLOBAL_HIGHSCORE. Swipe ← → zurück.
- **Wellingtown als Base64:** `const WELLINGTOWN_B64` direkt im Code, ~64KB JPEG. Kein externer File-Load.
- **`body flex-direction:column`:** D-Pad/Buttons stacken korrekt unter dem Canvas.
- **`updateMobileUI()`:** Hides/shows HTML-Elemente per State. Aufgerufen aus `update()`, gecacht mit `_prevMobileState`.

**Deploy:** `git -c http.version=HTTP/1.1 push origin main` (HTTP/2 = 400 auf diesem System).

---

## v1.4.1 (15. Mai 2026)

**Fokus:** Bugfix — Wellingtown-Hintergrund in GLOBAL_HIGHSCORE jetzt sichtbar.

- `loadGlobalBg()` Race-Condition behoben. `globalBgLoading`-Flag verhindert Mehrfach-Loads.
- Box von 548×380 auf 440×320, besser zentriert.
- Overlay `rgba(0,0,0,0.35)` für Lesbarkeit.

---

## v1.4 (15. Mai 2026)

**Fokus:** GLOBAL_HIGHSCORE-Screen mit Glassmorphism + Wellingtown-Hintergrund.

- Neuer `GLOBAL_HIGHSCORE` State, `drawGlobalHighscore()`, `loadGlobalBg()`.
- Glassmorphism-Box mit Neon-Glow-Border, Live-Firebase-Top-10.
- 🔥 Top-3-Icons, ✦ Rest. FEUERTONNEN_PRODUCTIONS Branding.
- Navigation: G-Taste → öffnet, ESC → zurück.

---

## v1.3.2 (14./15. Mai 2026)

**Fokus:** Produktionsstabilität, Firebase RTDB globale Highscores, saubere Deployments.

- Firebase `push()` statt vollständigem `highscores`-Knoten-Überschreiben.
- Ein dauerhafter `.on('value')`-Listener — kein Flackern bei Reconnects.
- `database.rules.json`: `!data.exists()`, Validierung, Index `score`.
- `.firebaserc`: Projekt `snake-on-fire` → `snake-on-fire-default-rtdb` (Europe West 1).

---

## v1.3 und früher

Vor Firebase-Stabilisierung. Listener-Instabilität, Flackern in Top-10.

| Commit | Inhalt |
|--------|--------|
| `initial release` | Erstes Repo-Setup, Basisspiel |
| `firebase live` | Firebase-Anbindung live |
| `final production deployment` | v1.3.2 Produktion |

---

## Hinweise für künftige Releases

- `GAME_VERSION` in **beiden** HTML-Dateien anheben + Eintrag hier ergänzen.
- `index.html` = Kopie von `snake-on-fire.html` — `cp snake-on-fire.html index.html`.
- Push-Befehl: `git -c http.version=HTTP/1.1 -c http.postBuffer=524288000 push origin main`

## Roadmap

- **v2.0 (geplant):** Web Audio API Musik — Chiptune-Loop, die sich mit Fire-Level intensiviert. Soundeffekte für Fressen, Tod, Eskalation. Vollständig in-file, keine externen Assets.
