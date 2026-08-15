# 🪵 Sterbündelrechner

Eine kleine, eigenständige Web-App (im Kern eine einzelne `index.html`, kein
Build, keine Abhängigkeiten) zur Berechnung von Volumen und Verkaufspreis von
Brennholzbündeln in runden Bündelgestellen – z. B. für den Verkauf mehrerer
Bündel unterschiedlicher Größe an der Straße oder ab Hof. Für die
Installierbarkeit als App (siehe unten) kommen noch ein Manifest, ein
Service Worker und zwei Icon-Dateien hinzu.

## Funktionen

- **Volumenberechnung** je Bündel anhand von gemessenem Innendurchmesser des
  Gestells und Scheitlänge – die App berechnet daraus das entsprechende
  äquivalente Ster-Volumen:
  - **Ster / Raummeter (RM)** – geometrisches Volumen des Gestells
    (Zylinder aus Innendurchmesser × Scheitlänge)
  - **Schüttraummeter (SRM)** – Ster × 1,45 (Aufmaß für lose geschüttetes Holz)
  - **Festmeter (FM)** – Ster × 0,70 (reines Holzvolumen ohne Zwischenräume)
- **Holzart-Auswahl** (Buche, Eiche, Fichte oder freier Text) mit
  hinterlegten Standard-Holzpreisen je Ster, die sich automatisch setzen,
  solange der Preis nicht von Hand geändert wurde
- **Bezeichnung** wird automatisch aus berechnetem Ster-Wert, Holzart und
  Scheitlänge gebildet, solange sie nicht von Hand geändert wurde
- **Preiskalkulation**
  - Holzpreis pro Ster (holzartabhängig vorbelegt)
  - Spanngurte pro Bündel inkl. Preis je Gurt – per Umschalter auch
    komplett ohne Spanngurte kalkulierbar
  - Gesamtpreis je Posten (Holz + Gurte)
- **Presets** für gängige Gestellgrößen (0,25 / 0,5 / 0,75 / 1,0 Ster – die
  Innendurchmesser sind so gewählt, dass das berechnete Ster-Volumen exakt
  dem jeweiligen Label entspricht) und Scheitlängen (25 / 33 / 50 / 100 cm)
- **Live-Vorschau** als SVG-Grafik des gepackten Bündels inkl. Spanngurten,
  die sich automatisch an Gurtanzahl und -umschalter anpasst, sowie eine
  aufklappbare **Zusammenfassung** der Ergebniswerte (Ster, SRM, FM, Gurte,
  Gesamtpreis) – am Desktop standardmäßig aufgeklappt, auf Tablet/Smartphone
  platzsparend eingeklappt und jederzeit per Klick auf „Zusammenfassung“
  ein-/ausblendbar
- **Responsives Layout**, optimiert für Smartphone, Tablet/11-Zoll-Notebook
  sowie große Desktop-Monitore (FullHD/QHD)
- **Polterliste**: mehrere Bündel-Posten mit unterschiedlichen Maßen,
  Mengen und Preisen erfassen, einzeln bearbeiten (inkl. Wiederherstellung
  von Holzart und Spanngurt-Einstellung) oder löschen und als Gesamtsumme
  (Menge, Ster, SRM, Gurte, Kosten) auswerten
- **PDF-Export** der Stückliste inkl. Preisberechnung über den nativen
  Druckdialog des Browsers ("Als PDF speichern")
- **Heller Modus als Standard** (besser ablesbar bei Tageslicht/im
  Freien), per Schalter oben rechts auf einen dunklen Modus umschaltbar
- **Installierbar als App (PWA)**: über den Browser (z. B. Chrome auf
  Android: „Zum Startbildschirm hinzufügen“/„App installieren“) als
  eigenständige App-Kachel installierbar, inkl. Offline-Nutzung durch
  einen Service Worker

## Verwendung

1. `index.html` in einem beliebigen Browser öffnen (kein Server nötig; für
   die Installation als App muss die Datei allerdings über `http(s)://`
   ausgeliefert werden, siehe Abschnitt „Technik“).
2. Bezeichnung ggf. individuell anpassen, Holzart wählen, Innendurchmesser
   und Scheitlänge über Slider/Eingabefeld oder per Preset-Buttons
   einstellen.
3. Anzahl der Bündel, Holzpreis pro Ster (wird je nach Holzart
   vorbelegt) sowie – falls verwendet – Anzahl und Preis der Spanngurte
   eintragen.
4. Ergebnis (Ster, SRM, FM, Gurte, Gesamtpreis) wird live berechnet.
5. Mit **„+ Zur Messungsliste hinzufügen“** den aktuellen Posten in die
   Polterliste übernehmen – dort werden alle Posten summiert.
6. Über ✎ einzelne Posten zur Bearbeitung zurück ins Formular laden, über
   ✕ entfernen oder über **„Liste leeren“** die gesamte Polterliste
   zurücksetzen.
7. Mit **„📄 Als PDF exportieren“** die Stückliste als PDF sichern
   (Browser-Druckdialog, Ziel „Als PDF speichern“).

## Berechnungsgrundlage

Das Ster-Volumen eines Bündels wird direkt aus dem gemessenen
Innendurchmesser $d$ und der Scheitlänge $L$ des Gestells als Zylinder
berechnet:

$$
\text{Ster} = \frac{\pi \cdot d^2}{4} \cdot L
$$

Die Presets sind so kalibriert, dass diese Formel für ihren jeweiligen
Innendurchmesser exakt den im Label genannten Ster-Wert ergibt (z. B.
Ø 98 cm bei 100 cm Scheitlänge → exakt 0,75 Ster).

Die Faktoren für Schüttraummeter (× 1,45) und Festmeter (× 0,70) sind
gängige Näherungswerte für dicht gepacktes Scheitholz mit einem geringen
Rundholzanteil (< 10 %).

## Technik

Reines HTML/CSS/JavaScript ohne externe Bibliotheken oder Frameworks –
alles läuft clientseitig im Browser, es werden keine Berechnungsdaten
(Bündelmaße, Polterliste) gespeichert oder übertragen. Der PDF-Export
nutzt die native Druckfunktion des Browsers, das Theme wird per
CSS-Variablen umgeschaltet und lediglich die Theme-Einstellung lokal im
Browser (`localStorage`) gemerkt.

Für die Installierbarkeit als **Progressive Web App** kommen drei weitere,
ebenfalls abhängigkeitsfreie Dateien hinzu:

- `manifest.webmanifest` – Name, Icons, Start-URL und Darstellung
  (`display: standalone`) für die Installation
- `sw.js` – Service Worker, der `index.html`, das Manifest und die Icons
  cacht (Stale-while-Revalidate) und damit Offline-Nutzung ermöglicht
- `icons/icon-192.png` und `icons/icon-512.png` – App-Icons (inkl.
  „maskable“-Sicherheitszone für adaptive Android-Icons)

Da `index.html` diese Dateien relativ zu ihrem eigenen Pfad referenziert,
muss die App dafür über `http://` oder `https://` (z. B. via
`python3 -m http.server` oder ein GitHub-Pages-Deployment) ausgeliefert
werden – Service Worker funktionieren nicht beim direkten Öffnen per
`file://`. Ohne Server bleibt die App weiterhin per Doppelklick auf
`index.html` nutzbar, nur eben nicht installierbar.
