# 10000er – installierbare iOS-PWA

Eine responsive, vollständig clientseitige Progressive Web App für iPhone, iPad und Desktop. Keine Python-Laufzeit auf dem iPhone nötig. Enthält die 19 bereitgestellten Original-JSON-Dateien, 20 Spielrunden, Ranglisten, direkte Spielerduelle, Rekorde, SVG-Diagramme, Rundenverläufe und eine Prüfung der Abschlussflags.

## Installation auf iPhone/iPad

**Wichtig:** Eine PWA lässt sich nicht direkt aus einer ZIP-Datei oder über `file://` installieren. Die entpackten Dateien müssen unter einer **HTTPS-Adresse** erreichbar sein (z. B. GitHub Pages, Netlify, eigener HTTPS-Webserver). Die HTTPS-Adresse in **Safari** öffnen, über **Teilen → Zum Home-Bildschirm** hinzufügen und anschließend die App vom Home-Bildschirm starten. Die Service-Worker-Installation speichert die App und alle 19 Ausgangsdateien für Offline-Nutzung. Nach erfolgreichem Erstladen funktioniert die App ohne Netzwerk.

### Einfaches Hosting mit GitHub Pages
1. Repository anlegen, den **Inhalt** des ZIP-Ordners (nicht den Ordner selbst) hochladen.
2. GitHub → Settings → Pages → Deploy from branch → `main` / `(root)`.
3. Veröffentlichte `https://...github.io/.../`-Adresse in Safari öffnen.
4. Über Teilen zum Home-Bildschirm hinzufügen.

**Datenschutz:** Wer die App mit den enthaltenen 19 JSON-Dateien auf einem **öffentlichen** Hosting-Dienst veröffentlicht, macht diese Dateien dort öffentlich zugänglich! Für private Spieldaten deshalb einen zugriffsgeschützten HTTPS-Server nutzen oder vor Veröffentlichung die JSON-Dateien entfernen und später lokal importieren. GitHub Pages ist **nicht** privat, auch wenn das Repository privat ist.

### Lokal testen
```bash
cd 10000er_iOS_PWA
python3 -m http.server 8080
```
Im Browser auf dem selben Rechner `http://localhost:8080` öffnen. Auf dem iPhone ist `http://192.168.x.x:8080` **keine** sichere HTTPS-Adresse; die PWA-Offline-Installation ist dort nicht zuverlässig möglich. Zum Testen der Oberfläche im Heimnetz geht HTTP, für Installation/Offline auf iOS HTTPS verwenden.

## Funktionen
- Übersicht mit Anzahl Dateien, Runden, Wertungen und abgeschlossenen Runden
- Ranglisten nach Gesamtpunkten, Punktedurchschnitt und Rundenführungen
- Spielerduelle über gemeinsame Spielrunden, mit Gleichständen
- Rekorde und Nullwertungsquoten
- Spielrundenliste mit Detailansicht und Statusprüfung
- Mehrfach-JSON-Import aus der Dateien-App (ersetzt Dateien gleicher Match-ID)
- Lokale IndexedDB-Datenbank, Export aller gespeicherten Matches als JSON-Array, selektives Löschen und Rücksetzen
- SVG-Diagramme ohne externe Bibliotheken
- Service Worker mit Offline-Cache inklusive 19 Ausgangsdateien

## Datenmodell und Interpretation
Die Dateien sind originale Exporte des Spielstands. Die App summiert das Feld `punkte0` für aktive Spieler und ordnet Wertungen per `spielerId` oder `refid` zu. Ein Match kann mehrere Spielrunden enthalten. Nur `beendet: true` zählt als bestätigter Abschluss; eine Punkteführung ist **kein** offiziell bestätigter Sieg. Nullwertungen können auch leere Eingaben darstellen. Bei identischen Namen in unterschiedlichen Matches wird von derselben Person ausgegangen. Die App nutzt die im Export vorhandenen aktiven Spielerpositionen und speichert die Originaldateien unverändert in IndexedDB.

## Datensicherheit
IndexedDB ist an die Domain gebunden. Browser-Datenlöschung, Neuinstallation oder Domainwechsel können Daten entfernen. Nach neuen Imports über **Daten → Daten exportieren** sichern. Der Export ist ein JSON-Array und kann als eine Datei wieder importiert werden. Auf öffentlichen Hosts sind die mitgelieferten JSON-Dateien abrufbar; lokale spätere Imports werden dagegen nicht hochgeladen.

## Dateien
`index.html`, `styles.css`, `app.js`, `sw.js`, `manifest.webmanifest`, `icons/`, `data/`.

Keine Build-Schritte, kein Node.js und kein Xcode nötig. Der lokale Python-Webserver ist nur ein optionales Testwerkzeug.
