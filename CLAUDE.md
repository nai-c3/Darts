# Dart-Zähler – Kontext für Claude Code

Steeldarts-Zähler als **eine einzige Datei**: `index.html` (HTML, CSS, JS inline, kein Build, keine Abhängigkeiten außer Google Fonts „Unbounded“). Veröffentlicht über GitHub Pages aus `main` / root. Zuvor in claude.ai entstanden und dort zusätzlich als Artefakt gehostet.

## Arbeitsweise mit dem Nutzer
- Antworten auf Deutsch. Rolle: Berater, nicht Assistent. Schwächen und Risiken zuerst nennen, keine Floskeln oder Lob.
- Kernaussagen mit [Certain] / [Likely] / [Guessing] markieren.
- Bei Widerspruch nur mit neuen Fakten umschwenken.

## Funktionen
- Spielmodi: X01 (301/501/701, Single/Double/Master In und Out, Legs „First to“, Bust nach Steeldarts-Regeln, Checkout-Vorschläge), Cricket (Standard, Cut-Throat), Around the Clock (optional mit Bull, D/T springen), Shanghai, Killer, Highscore, Split Score (Start 40 oder Einwurfrunde, Halbieren abgerundet).
- Eingabe Dart für Dart: Double/Triple antippen, dann Zahl. Unbegrenztes Rückgängig (Snapshot-Stack).
- Spielerprofile mit Farbe und Foto. Das Foto wird nur als 20×20-Helligkeitsraster (Base36-String) gespeichert und als Halftone-SVG gezeichnet. Gäste ohne Statistik.
- Statistik pro Profil: Spiele, Siege, Siegquote (nur Mehrspieler), X01-Average, bestes Spiel-Ø, höchste Aufnahme, höchstes Finish, 180er, 100+, Legs, Average-Verlauf (letzte 12), Cricket-MPR, Bestwerte anderer Modi, direkter Vergleich, letzte Spiele.
- Partie wird bei Spielende gespeichert. Wird der Siegwurf rückgängig gemacht, wird sie wieder gelöscht.

## Speicherung
- `localStorage`: `dartzaehler-v1` (Setup, laufendes Spiel, Undo-Stack), `dartzaehler-data-v1` (Profile, Partien).
- Gruppen-Sync: Firebase Firestore (SDK 10.14.1 per dynamischem `import()` von gstatic), Konstante `FIREBASE_CONFIG` in `index.html` (solange `null`, ist der Sync ausgeblendet). Pfade `groups/<code>/profiles|matches`, 4-stelliger Code (ältere Gruppen 12-stellig, weiter gültig) aus `23456789abcdefghjkmnpqrstuvwxyz`, Anzeige in Großbuchstaben, beim Anlegen wird geprüft, dass die Gruppe leer ist; keine Anmeldung, Regeln in `firestore.rules`. Gruppe in `localStorage` `dartzaehler-group-v1` ({code, merged}). Lokale Daten werden nur beim ersten Beitritt hochgeladen, danach ist die Gruppe maßgeblich (sonst kämen gelöschte Einträge zurück). Einladung per `#g=<code>`.
- Export/Import als JSON (`{app:'dartzaehler',v:1,profiles,matches}`), Import ergänzt nur neue IDs. Letzte Sicherung in `dartzaehler-backup-v1`.
- Nur auf claude.ai (und nur ohne Gruppe): zusätzlich `window.claude.use('db')` + `use('user')`, Pfad `data/users/<id>/dz/profiles` und `/dz/matches`. Außerhalb von claude.ai fehlt `window.claude`, die Seite fällt automatisch auf `localStorage` zurück.

## Design: System „Linkedin-Niklas“ (eigenes Design-System des Nutzers)
- Farben: ultramarine #3F37FF, cobalt #023FA5, midnight #240D57, violet #592795, raspberry #EA465C, signal #ED292A, sun #FBC507, rust #CE4000, magenta #ED40B5, paper #ECEDDF, white.
- Bewusst **nur heller Modus** (Nutzerwunsch): Grund paper, Karten weiß, aktiver Spieler ultramarine mit sun-Akzent. Tastenfeld weiß, Tasten paper; Double = sun, Triple = signal, Ziel = ultramarine.
- Schrift: Display „PP Agrandir Grand Heavy“ (lizenzpflichtig, nur lokal), Fallback Unbounded 800, Versalien, Laufweite −0,05em. Fließtext Young Serif Light (OFL, als Base64 eingebettet).
- Keine Schatten, Verläufe, Rahmen um Text oder Emojis. Farbflächen eckig, Bedienelemente Radius 12px, Badges 8px, Avatare rund.
- Raster: Abstände 4/8/12/16/20/24/32px; Schriftgrößen 11/13/15/17/19/24/30/44px plus clamp-Werte für große Zahlen.
- Header: nur Illustration ohne sichtbaren Text; Titel als `.sr-only`-h1. Scheibe dreht (48 s pro Umdrehung), Pfeil fliegt alle 7 s ein mit Treffer-Ring. Weil `render()` den Header neu zeichnet, setzt `heroSvg()` eine negative `animation-delay` seit Seitenstart, damit nichts neu beginnt.
- Animationen: Halbieren (Split Score) und „Raus“ (Killer) als Vollbild-Effekt über `playFx()` außerhalb von `#app` (überlebt `render()`, Antippen schließt). Spielende: Konfetti, Sieger-Headline, Verlierer-Zeile „X verliert.“ schwingt, Verlierer-Zeilen sacken ab; nur beim ersten Anzeigen (`ui.endPlayed`), bei laufendem Effekt um `--late` verzögert. Alles respektiert prefers-reduced-motion (dann Toast statt Effekt).
- Split Score: eigenes Tastenfeld je Ziel (Zahl: Single/Double/Triple als große Tasten; Doppel/Triple: 1–20 vorbelegt; Bull: 25/50; immer „Daneben“), Rundenleiste über den Karten, kein Average.
- Spielmenü (Topbar „Menü“): Weiterspielen, Zum Hauptmenü (Spiel bleibt offen), Spiel abbrechen (zweistufig, verwirft ohne Statistik).
- Markenentscheidungen im CSS sind mit `unslop-ignore` markiert.

## Offene Ideen (vom Nutzer noch nicht beauftragt)
- Summen-Eingabe pro Aufnahme (statt Dart für Dart) für schnelleres Zählen.
- Checkout-Quote (Versuche aufs Doppel erfassen).
- Optionale Rundung beim Split-Score-Halbieren.
