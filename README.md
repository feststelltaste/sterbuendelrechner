# 🪵 Sterbündelrechner

Eine kleine, eigenständige Web-App (eine einzelne `index.html`, kein Build,
keine Abhängigkeiten) zur Berechnung von Volumen und Verkaufspreis von
Brennholzbündeln in runden Bündelgestellen – z. B. für den Verkauf mehrerer
Bündel unterschiedlicher Größe an der Straße oder ab Hof.

## Funktionen

- **Volumenberechnung** je Bündel anhand von Innendurchmesser des
  Gestells und Scheitlänge:
  - **Ster / Raummeter (RM)** – geometrisches Volumen sowie ein Wert
    inklusive Kompressionsfaktor
  - **Schüttraummeter (SRM)** – Ster × 1,45 (Aufmaß für lose geschüttetes Holz)
  - **Festmeter (FM)** – Ster × 0,70 (reines Holzvolumen ohne Zwischenräume)
- **Kompressionsfaktor** (0–10 %, Standard 5 %): Ein Bündel ist enger
  gepackt als lose gestapeltes Scheitholz und enthält dadurch mehr Holz,
  als das geometrische Volumen des Gestells vermuten lässt – der Faktor
  gibt an, wie viel mehr Ster das lose gestapelt wären, und erhöht das
  für Menge und Preis angesetzte Volumen entsprechend
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
- **Presets** für gängige Gestellgrößen (0,25 / 0,5 / 0,75 / 1,0 Ster) und
  Scheitlängen (25 / 33 / 50 / 100 cm)
- **Live-Vorschau** als SVG-Grafik des gepackten Bündels inkl. Spanngurten,
  die sich automatisch an Gurtanzahl und -umschalter anpasst
- **Polterliste**: mehrere Bündel-Posten mit unterschiedlichen Maßen,
  Mengen und Preisen erfassen, einzeln bearbeiten (inkl. Wiederherstellung
  von Holzart und Spanngurt-Einstellung) oder löschen und als Gesamtsumme
  (Menge, Ster, SRM, Gurte, Kosten) auswerten
- **PDF-Export** der Stückliste inkl. Preisberechnung über den nativen
  Druckdialog des Browsers ("Als PDF speichern")
- **Heller Modus als Standard** (besser ablesbar bei Tageslicht/im
  Freien), per Schalter oben rechts auf einen dunklen Modus umschaltbar

## Verwendung

1. `index.html` in einem beliebigen Browser öffnen (kein Server nötig).
2. Bezeichnung ggf. individuell anpassen, Holzart wählen, Innendurchmesser
   und Scheitlänge über Slider/Eingabefeld oder per Preset-Buttons
   einstellen.
3. Bei Bedarf den Kompressionsfaktor anpassen (Standard: 5 %).
4. Anzahl der Bündel, Holzpreis pro Ster (wird je nach Holzart
   vorbelegt) sowie – falls verwendet – Anzahl und Preis der Spanngurte
   eintragen.
5. Ergebnis (Ster, SRM, FM, Gurte, Gesamtpreis) wird live berechnet.
6. Mit **„+ Zur Messungsliste hinzufügen“** den aktuellen Posten in die
   Polterliste übernehmen – dort werden alle Posten summiert.
7. Über ✎ einzelne Posten zur Bearbeitung zurück ins Formular laden, über
   ✕ entfernen oder über **„Liste leeren“** die gesamte Polterliste
   zurücksetzen.
8. Mit **„📄 Als PDF exportieren“** die Stückliste als PDF sichern
   (Browser-Druckdialog, Ziel „Als PDF speichern“).

## Berechnungsgrundlage

Das geometrische Volumen eines Bündels wird als Zylinder aus
Innendurchmesser $d$ und Scheitlänge $L$ berechnet:

$$
\text{Ster}_{\text{geometrisch}} = \frac{\pi \cdot d^2}{4} \cdot L
$$

Da ein Bündel enger gepackt ist als lose gestapeltes Scheitholz, wird für
Mengen- und Preisberechnung zusätzlich der Kompressionsfaktor $k$
(0–10 %, Standard 5 %) angesetzt – er gibt an, wie viel Ster das im
Bündel enthaltene Holz lose gestapelt zusätzlich einnehmen würde:

$$
\text{Ster} = \text{Ster}_{\text{geometrisch}} \cdot \left(1 + \frac{k}{100}\right)
$$

Die Faktoren für Schüttraummeter (× 1,45) und Festmeter (× 0,70) sind
gängige Näherungswerte für dicht gepacktes Scheitholz mit einem geringen
Rundholzanteil (< 10 %).

## Technik

Reines HTML/CSS/JavaScript ohne externe Bibliotheken oder Frameworks –
alles läuft clientseitig im Browser, es werden keine Daten gespeichert
oder übertragen. Der PDF-Export nutzt die native Druckfunktion des
Browsers, das Theme wird per CSS-Variablen umgeschaltet und lokal im
Browser (`localStorage`) gemerkt.
