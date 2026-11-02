# Dart-Zähler

Zähler für Steeldarts als einzelne HTML-Datei, ohne Build und ohne Server.

**Spielmodi:** X01 (301/501/701, Single/Double/Master In und Out, Legs), Cricket (Standard und Cut-Throat), Around the Clock, Shanghai, Killer, Highscore, Split Score.

**Funktionen:** Dart-für-Dart-Eingabe mit Bust-Erkennung und Checkout-Vorschlägen, Spielerprofile mit Halftone-Avataren, Statistiken (Average, Bestwerte, MPR, direkter Vergleich), unbegrenztes Rückgängig.

## Starten

`index.html` im Browser öffnen oder über GitHub Pages veröffentlichen (Settings → Pages → Branch `main`, Ordner `/root`).

## Daten

Ohne Einrichtung speichert die Seite Profile, Statistiken und den laufenden Spielstand im `localStorage` des Browsers, also nur auf diesem Gerät. Unter „Profile & Statistik“ lassen sich alle Daten als JSON-Datei exportieren und auf einem anderen Gerät importieren.

## Gemeinsame Daten (Firebase Firestore)

Profile und Statistiken liegen online in Firestore und sind auf allen Geräten gleich, die die Seite öffnen. Es gibt keinen Code und keine Anmeldung: Wer die Adresse kennt, kann alles sehen, ändern und löschen. Deshalb ab und zu unter „Profile & Statistik“ eine Sicherung exportieren.

Einrichtung (einmalig, bereits erledigt für das Projekt `darts-d796d`):

1. In der [Firebase-Konsole](https://console.firebase.google.com) ein Projekt anlegen.
2. Firestore-Datenbank erstellen (Standort `europe-west3`, Produktionsmodus).
3. Inhalt von `firestore.rules` unter Firestore → Regeln einfügen und veröffentlichen.
4. Web-App hinzufügen und das `firebaseConfig`-Objekt in `index.html` bei `var FIREBASE_CONFIG=` eintragen. Die Werte sind öffentlich.

Beim ersten Öffnen lädt jedes Gerät seine lokalen Profile und Spiele einmalig hoch. Offline gespielte Partien werden übertragen, sobald das Gerät wieder online ist.

## Schriften

- Display: PP Agrandir Grand Heavy (lizenzpflichtig, wird nur genutzt, wenn lokal installiert), sonst Unbounded über Google Fonts.
- Fließtext: Young Serif Light (SIL Open Font License 1.1), in `index.html` eingebettet.
