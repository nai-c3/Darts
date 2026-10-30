# Dart-Zähler

Zähler für Steeldarts als einzelne HTML-Datei, ohne Build und ohne Server.

**Spielmodi:** X01 (301/501/701, Single/Double/Master In und Out, Legs), Cricket (Standard und Cut-Throat), Around the Clock, Shanghai, Killer, Highscore, Split Score.

**Funktionen:** Dart-für-Dart-Eingabe mit Bust-Erkennung und Checkout-Vorschlägen, Spielerprofile mit Halftone-Avataren, Statistiken (Average, Bestwerte, MPR, direkter Vergleich), unbegrenztes Rückgängig.

## Starten

`index.html` im Browser öffnen oder über GitHub Pages veröffentlichen (Settings → Pages → Branch `main`, Ordner `/root`).

## Daten

Außerhalb von claude.ai speichert die Seite Profile, Statistiken und den laufenden Spielstand im `localStorage` des Browsers. Die Daten bleiben also auf dem Gerät und im Browser, in dem gespielt wird.

## Schriften

- Display: PP Agrandir Grand Heavy (lizenzpflichtig, wird nur genutzt, wenn lokal installiert), sonst Unbounded über Google Fonts.
- Fließtext: Young Serif Light (SIL Open Font License 1.1), in `index.html` eingebettet.
