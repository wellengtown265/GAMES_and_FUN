# SNAKE_on_FIRE — GAME DESIGN MASTERFILE

## Projektübersicht

**Titel:** SNAKE_on_FIRE

**Genre:**
- Arcade
- Retro Action
- Skill-Based Survival
- Pixel / Neon Arcade

**Core Identity:**
SNAKE_on_FIRE ist eine moderne, aggressive und stylisierte Neuinterpretation des klassischen Snake-Konzepts — kombiniert mit Highspeed-Gameplay, Feuer-/Heat-Mechaniken, visueller Eskalation und Arcade-Pressure.

---

# 1. CORE GAME LOOP

## Grundprinzip
Der Spieler steuert eine Snake durch eine Arena.

Durch das Einsammeln von Objekten:
- wächst die Snake
- steigt Geschwindigkeit
- erhöht sich Heat-/Fire-Level
- verändert sich die Umgebung
- eskaliert das Risiko

Das Ziel:
- möglichst lange überleben
- Highscores erreichen
- Multiplikatoren halten
- Combos aufbauen
- maximale Heat-Stufen kontrollieren

---

# 2. DESIGN PHILOSOPHIE

## Zielgefühl
Das Spiel soll sich anfühlen wie:
- „Snake auf Adrenalin“
- visuelle Überladung ohne Chaos
- schnelle Entscheidungen
- permanenter Druck
- Flow + Risiko
- aggressive Arcade-Energie

## Inspirationsquellen
- klassische Nokia Snake
- Retro Arcade Games
- Neon-Racer
- Bullet-Hell Intensität
- Pixel-Art Klassiker
- moderne Synthwave-Ästhetik

---

# 3. VISUELLE IDENTITÄT

## Art Style
### Hauptstil
- Retro Pixel Art
- High Resolution Pixel Rendering
- Neon Glow
- starke Kontraste
- dunkle Hintergründe
- intensive Partikeleffekte

### Stilrichtung
Kombination aus:
- Retro Arcade
- Cyberpunk
- Synthwave
- Pixel-Action
- Digital Fire Aesthetic

---

## Farbpalette

### Primärfarben
| Element | Farbe |
|---|---|
| Hintergrund | Tiefschwarz / Dunkelblau |
| Feuer | Orange / Rot / Gelb |
| Neon | Cyan / Magenta |
| UI Highlights | Elektrisches Blau |
| Danger States | Tiefrot |

### Sekundärfarben
- Glühendes Weiß
- dunkles Violett
- Ember Orange
- toxisches Neon-Grün

---

# 4. SNAKE DESIGN

## Grunddesign
Die Snake soll:
- klar lesbar
- stylisiert
- aggressiv
- lebendig
- „heiß“ wirken

## Animationen
### Idle
- leichtes Pulsieren
- Glow-Flackern

### Movement
- dynamische Segmentschwankung
- Motion Trails
- Heat-Particles

### High Heat
- brennende Kanten
- Rauchpartikel
- Flammenanimation
- Lichtpulse

### Max Heat
- komplette Feuer-Silhouette
- extreme Trails
- Screen Distortion
- Arena-Reaktionen

---

# 5. FEUER / HEAT SYSTEM

## Heat-System
Zentrales Gameplay-Element.

### Heat steigt durch:
- Nahrung
- Geschwindigkeit
- Combos
- aggressive Spielweise
- Kettenreaktionen

### Heat beeinflusst:
- Geschwindigkeit
- Sichtbarkeit
- Partikelintensität
- Audiointensität
- Risiko
- Punkte-Multiplikator

---

## Heat-Stufen

### Level 1 — Warm
- leichtes Glow
- minimale Effekte

### Level 2 — Hot
- sichtbar brennend
- erhöhte Geschwindigkeit
- Musik intensiviert sich

### Level 3 — Burning
- starke Flammen
- Arena beginnt zu reagieren
- hohes Risiko

### Level 4 — Inferno
- maximale Instabilität
- visuelle Verzerrungen
- extreme Geschwindigkeit
- Highscore-Multiplikator

---

# 6. SPIELFELD / ARENA

## Arena-Design
### Grundstruktur
- klares Grid-System
- dunkle futuristische Fläche
- Neonlinien
- glühende Kanten

## Dynamische Umgebung
Mit steigender Heat:
- pulsiert die Arena
- verändern sich Farben
- entstehen Störungen
- treten Partikeleffekte auf

---

# 7. FOOD / PICKUPS

## Standard Food
- Energie-Orbs
- glühende Pixel-Früchte
- Neon-Kerne

## Spezialobjekte
### Heat Boost
- sofortige Heat-Erhöhung

### Freeze
- temporäre Verlangsamung

### Shield
- Schutz vor Fehlern

### Multiplier
- Score-Boost

### Magnet
- Pickup-Anziehung

---

# 8. GAME MODES

## Classic
Traditionelles Snake mit moderner Präsentation.

## Inferno Mode
Maximale Geschwindigkeit und eskalierende Heat.

## Survival
Wellenbasierte Arena mit steigender Schwierigkeit.

## Zen Mode
Langsamer, atmosphärischer Modus.

## Hardcore
- keine Fehler erlaubt
- maximale Geschwindigkeit
- aggressive Spawn-Mechaniken

---

# 9. UI / HUD DESIGN

## UI-Stil
- minimalistisch
- neonbasiert
- pixel-clean
- starke Lesbarkeit

## HUD Elemente
- Score
- Highscore
- Heat Meter
- Combo Counter
- Geschwindigkeit
- Multiplikator

---

## Heat Meter
Visuell zentral.

Soll:
- pulsieren
- glühen
- bei Gefahr warnen
- audio-visuelles Feedback liefern

---

# 10. AUDIO DESIGN

## Musikstil
- Synthwave
- Dark Electronic
- Retro Arcade
- Industrial Pulse

## Dynamische Musik
Musik intensiviert sich abhängig von:
- Heat
- Geschwindigkeit
- Combo-Level
- Gefahrensituationen

---

## Sound Effects
### Snake
- digitale Bewegungs-Sounds
- leichte elektrische Impulse

### Pickups
- klare Arcade-Sounds
- hohe Wiedererkennbarkeit

### Heat States
- Feuerrauschen
- Distortion
- Alarm-Sounds

---

# 11. PARTICLE EFFECTS

## Effektarten
- Feuer
- Rauch
- Neon Trails
- Glitches
- Funken
- Pixel-Explosionen

## Intensität
Skaliert dynamisch mit:
- Geschwindigkeit
- Heat
- Combos
- Spielsituation

---

# 12. CAMERA / SCREEN EFFECTS

## Effekte
- Screen Shake
- Bloom
- Chromatic Aberration
- Heat Distortion
- Motion Blur
- Light Pulses

## Einsatz
Nur situativ.
Lesbarkeit darf niemals verloren gehen.

---

# 13. SCORE SYSTEM

## Punkte entstehen durch
- Nahrung
- Geschwindigkeit
- Risiko
- Heat-Level
- Combos
- Überlebenszeit

## Multiplikatoren
Multiplikatoren steigen durch:
- aggressive Spielweise
- fehlerfreies Spielen
- hohe Heat-Level
- schnelle Kettenaktionen

---

# 14. SCHWIERIGKEITSKURVE

## Frühe Phase
- kontrolliert
- lesbar
- Einstieg

## Midgame
- steigender Druck
- komplexere Entscheidungen

## Endgame
- Hochgeschwindigkeits-Chaos
- visuelle Eskalation
- maximale Konzentration

---

# 15. RETRO PIXEL IDENTITÄT

## Pixel-Regeln
- scharfe Pixelkanten
- keine verwaschenen Texturen
- bewusste Low-Res-Elemente
- moderne Beleuchtung auf Retro-Basis

## Ziel
Retro-Gefühl mit moderner visueller Qualität.

---

# 16. LOGO DESIGN

## Stil
- Arcade
- Neon
- Feuer
- Pixel-Retro

## Elemente
- brennende Typografie
- harte Kontraste
- Glow-Effekte
- eventuell Snake-Integration

---

# 17. MENÜ DESIGN

## Hauptmenü
- animierter Hintergrund
- brennende Neon-Elemente
- pixelige Übergänge
- aggressive Audioatmosphäre

## Buttons
- starke Lesbarkeit
- Neon-Hover
- Pixel-Effekte
- schnelles UI-Feedback

---

# 18. TECHNISCHE DESIGNZIELE

## Performance
- extrem flüssig
- niedrige Input-Latenz
- stabile FPS
- schnelle Reaktionszeiten

## Plattformen
- Browser
- Desktop
- potenziell Mobile

---

# 19. VIBES / KEYWORDS

## Hauptbegriffe
- Retro
- Fire
- Neon
- Arcade
- Fast
- Aggressive
- Highscore
- Heat
- Flow
- Chaos
- Pixel
- Synthwave

---

# 20. VISUELLE REFERENZRICHTUNG

## Referenzstimmung
- dunkler Hintergrund
- intensive Neon-Kontraste
- Feuer + Glitch + Pixel
- futuristische Arcade-Hölle

## Nicht erwünscht
- realistische Grafik
- weiche Mobile-Casual-Optik
- sterile UI
- flache Farben
- überladene Menüs

---

# 21. MÖGLICHE ERWEITERUNGEN

## Zukünftige Systeme
- Boss-Modus
- Arena-Veränderungen
- Online Leaderboards
- Daily Challenges
- Skill-System
- Cosmetic Unlocks
- Replay-System
- Spectator Mode

---

# 22. BRANDING

## Zielwirkung
SNAKE_on_FIRE soll wirken wie:
- ein verlorener Arcade-Klassiker aus der Zukunft
- extrem stylisiert
- sofort wiedererkennbar
- social-media-tauglich
- gif-/clip-freundlich

---

# 23. FINALER DESIGNKERN

## Kernsatz
„Klassisches Snake — transformiert in ein hyperintensives Neon-Feuer-Arcade-Erlebnis.“

---

# 24. OFFENE DESIGNBEREICHE

## Noch zu definieren
- finale Snake-Form
- endgültige Farbpalette
- UI-Typografie
- Audio-Library
- genaue Heat-Mathematik
- Partikelintensität
- Spawn-Systeme
- Mobile Controls
- Controller Support

---

# 25. PRODUKTIONSNOTIZEN

## Prioritäten
1. Spielgefühl
2. Lesbarkeit
3. Performance
4. Heat-System
5. audiovisuelle Eskalation
6. Stil-Konsistenz

## Wichtigster Punkt
Das Spiel darf trotz Eskalation niemals unfair oder unlesbar werden.

