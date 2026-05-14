# Versionsgeschichte — SNAKE on FIRE

Chronologische Übersicht relevanter Releases und technischer Änderungen.  
Quelle im Repo: `GAME_VERSION` in `snake-on-fire.html` / `index.html`, Git-Commits, Deployment-Kontext.

---

## v1.3.2 (aktuell)

**Stand im Code:** `GAME_VERSION = '1.3.2'`

**Fokus:** Produktionsstabilität, globale Highscores (Firebase Realtime Database), saubere Deployments.

**Technik / Inhalt (Auszug):**

- Firebase: Highscores nur noch per **`push()`** mit Einträgen `{ name, score, timestamp }`; keine Überschreibung des gesamten `highscores`-Knotens.
- **Eine** dauerhafte Listener-Registrierung für die Top-10-Abfrage (`orderByChild('score')`, `limitToLast(10)`), keine erneuten `.on('value')`-Anmeldungen bei jedem Reconnect (behebt flackernde / falsche Top-10-Anzeige).
- Client-seitige **stabile Sortierung** nach Score, Tie-Break per `timestamp`.
- **`database.rules.json`:** Lesen öffentlich, Schreiben nur bei neuem Kind (`!data.exists()`), Validierung inkl. `timestamp`, Index `score`.
- **`.firebaserc`:** Default-Projekt **`snake-on-fire`** — identisch zur `databaseURL` im Web-Client (`snake-on-fire-default-rtdb…`). **Wichtig:** Ein anderes Projekt **`snake-on-fire-57e9f`** nutzt eine **eigene, leere** RTDB-Instanz (`…57e9f-default-rtdb…`); Rules-Deployments dorthin betreffen **nicht** die Live-Highscores des Spiels. Immer dieselbe Projekt-ID wie in der Firebase-Webapp-Konfiguration verwenden.
- **`.gitignore`:** u. a. `.DS_Store` ergänzt.
- Release-Commit (Beispiel): `final production deployment` → GitHub `main`.

**Hosting (Ziel / Hinweis):**

- Netlify: neuer Produktionspfad z. B. `snake-on-fire-v1-3-2.netlify.app` (Site-Name in Netlify ändern; Deploy aus GitHub `main`).
- Firebase: primär **Realtime Database Rules** (kein Hosting-Pfad im Repo-`firebase.json`; statisches Spiel liegt typischerweise bei Netlify/GitHub Pages).

---

## v1.3 (Vorgänger)

**Kontext:** Version vor der finalen Highscore-/Deployment-Runde; im Spiel zuvor als `1.3` ausgewiesen.

**Bekannte Themen (behoben in 1.3.2):**

- Globale Top 10 konnte instabil wirken (u. a. durch mehrfache Listener bei Firebase-Reconnects).
- Abgleich CLI-Projekt vs. Client-`databaseURL` war teils unklar und musste für konsistente Rules-Deployments geklärt werden.

**Hosting-Hinweis:** ältere Netlify-URL-Generation z. B. `snake-on-fire-v1-3.netlify.app` — nicht mehr als primäre Produktions-URL vorgesehen, sobald v1.3.2 live ist.

---

## Frühere Meilensteine (Repo-Git, grob)

| Commit (Kurz) | Inhalt (Kurzbeschreibung) |
|----------------|---------------------------|
| `initial release` | Erstes öffentliches Repo-Setup, Basisspiel. |
| `firebase live` | Firebase-Anbindung / globale Highscores live geschaltet. |
| `final production deployment` | v1.3.2, Rules, Ignores, Firebaserc-Abgleich, dokumentierte Release-Linie. |

---

## Hinweise für künftige Einträge

- Bei jeder sichtbaren Version: **`GAME_VERSION`** in beiden HTML-Dateien anheben und hier einen Abschnitt ergänzen.
- Kurz notieren: **was** (Spiel / UI / Backend), **wo** deployed (Netlify-URL, Firebase nur DB o. ä.), **Git-Tags** optional (`git tag v1.3.2`).

---

*Dokument angelegt zur Nachverfolgbarkeit; Spiel- und UI-Branding unverändert dokumentiert.*
