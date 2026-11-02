# Dart-Zähler

Zähler für Steeldarts als einzelne HTML-Datei, ohne Build und ohne Server.

**Spielmodi:** X01 (301/501/701, Single/Double/Master In und Out, Legs), Cricket (Standard und Cut-Throat), Around the Clock, Shanghai, Killer, Highscore, Split Score.

**Funktionen:** Dart-für-Dart-Eingabe mit Bust-Erkennung und Checkout-Vorschlägen, Spielerprofile mit Halftone-Avataren, Statistiken (Average, Bestwerte, MPR, direkter Vergleich), unbegrenztes Rückgängig.

## Starten

`index.html` im Browser öffnen oder über GitHub Pages veröffentlichen (Settings → Pages → Branch `main`, Ordner `/root`).

## Daten

Ohne Einrichtung speichert die Seite Profile, Statistiken und den laufenden Spielstand im `localStorage` des Browsers, also nur auf diesem Gerät. Unter „Profile & Statistik“ lassen sich alle Daten als JSON-Datei exportieren und auf einem anderen Gerät importieren.

## Gruppen-Sync (Firebase Firestore)

Damit eine Spielrunde Profile und Statistiken auf mehreren Handys teilt, nutzt die Seite Firestore. Zugang gibt es über einen 4-stelligen Gruppencode (rund 920.000 Möglichkeiten, also nur Schutz vor Zufall, nicht vor gezieltem Durchprobieren). Wer den Code kennt, kann die Daten der Gruppe lesen und ändern. Einen Login gibt es nicht.

Einmalig einrichten:

1. In der [Firebase-Konsole](https://console.firebase.google.com) ein Projekt anlegen (Google Analytics nicht nötig).
2. Build → Firestore Database → Datenbank erstellen, Standort `eur3` oder `europe-west3`, im Produktionsmodus.
3. Firestore → Regeln: Inhalt von `firestore.rules` einfügen und veröffentlichen.
4. Projekteinstellungen → Allgemein → „Web-App hinzufügen“. Das angezeigte `firebaseConfig`-Objekt in `index.html` bei `var FIREBASE_CONFIG=null;` eintragen. Die Werte sind öffentlich und dürfen im Repository stehen.
5. Optional: Unter Authentifizierung → Einstellungen → Autorisierte Domains die GitHub-Pages-Domain eintragen (für Firestore allein nicht nötig). In der Google Cloud Console den API-Schlüssel auf die Pages-Domain beschränken.

Danach erscheint unter „Profile & Statistik“ der Bereich „Gruppe“: Gruppe anlegen, Mitspieler per Link oder Code einladen. Beim ersten Beitritt werden die lokalen Profile und Spiele des Geräts in die Gruppe übernommen. Offline gespielte Partien werden übertragen, sobald das Gerät wieder online ist.

## Schriften

- Display: PP Agrandir Grand Heavy (lizenzpflichtig, wird nur genutzt, wenn lokal installiert), sonst Unbounded über Google Fonts.
- Fließtext: Young Serif Light (SIL Open Font License 1.1), in `index.html` eingebettet.
