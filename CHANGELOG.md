# Änderungsverlauf

Alle nennenswerten Änderungen an diesem Projekt. Neueste Version zuerst.

## [v0.1.1] – 25.10.2026

Eine Prüfrunde gegen das Original: vier unabhängige Durchsichten haben den Code mit dem
dekompilierten Lingo, dem Programmcode des Reit-Minispiels, den Originaldateien der CD und den
Bildschirmfotos im Handbuch verglichen. Ergebnis: **18 Fehler gefunden und behoben**, die
Testsuite ist von 253 auf 315 Tests gewachsen.

### Behoben
- **Nachrichten konnten nie gelesen werden.** Der Nachrichten-Dialog meldete dem Spiel immer
  „später lesen" zurück. Dadurch brach die Schleife, die die Nachrichten abarbeitet, sofort ab und
  die Liste wuchs endlos weiter (über 500 Einträge nach 1200 Spieltagen). Jetzt erscheinen die
  beiden Knöpfe des Originals, „gelesen" löscht den Eintrag und zeigt die nächste Nachricht.

- **Spielstände hatten ein anderes Format als das Original.** Farb- und Datumswerte wurden ohne
  die inneren Leerzeichen des Originals geschrieben — `rgb(163,228,168)` statt
  `rgb( 163, 228, 168 )`. Belegt an den 46 Original-Pferdedatensätzen der CD. Nach der Korrektur
  lesen und schreiben sich 47 von 48 Originaldateien zeichengenau zurück.
- **Menüeinträge wurden nie ausgegraut.** Das Zählen von Eigenschaften verwechselte sich mit dem
  Zählen von Textbestandteilen und lieferte einen zu hohen Wert (12 betroffene Stellen beim
  Züchter und im Menü).
- **Bildlaufleisten und Namensliste.** Das Zählen von Zeichen und Zeilen eines Textfelds lieferte
  immer 0. Dadurch stimmte die Höhe der Bildlaufleisten nicht, und die Namensliste in der
  Spielerwahl füllte nur einen Eintrag.
- **Reit-Minispiel: fünf Abweichungen vom Original behoben.** Nach einer zweiten, vollständigen
  Durchsicht des Original-Programmcodes: die Steiggeschwindigkeit im Zielzustand, die Frames der
  Landegeräusche, die Ermittlung der Bodenhöhe, Weggabelungen mit unbekanntem Ebenennamen und die
  Zeitanzeige, die falsch rundete (aus 1,6 Sekunden wurde „00:02.600").
- **Gesperrte Bedienelemente reagierten trotzdem.** Das Abbrechen eines Ereignisses hielt nichts
  auf: Geschwisterelemente desselben Sprites und die übergeordneten Skripte bekamen es weiterhin.
  Genau so sperrt das Spiel seine Regler bei Schmied, Händler, Arzt und Gut — gesperrte Regler
  spielten deshalb Töne ab und zeigten ihre Warnung bei jedem Bild erneut.
- **Ein Absturz der Reit-Engine wurde als erfolgreicher, sturzfreier Ritt gemeldet.**
- **Stille Fehlschläge melden sich jetzt**, statt unbemerkt zu bleiben: fehlende Zuchtpferde,
  Member-Skripte, Verhaltensparameter von Frame-Skripten und unlesbare Ablaufdaten.
- **Textfarben und Schriften.** Die Farbangaben im Textformat des Originals werden jetzt gelesen:
  Die Hinweisbox erscheint in Navy, Preis- und Kostenangaben in Rot. Die Schriftzuordnung je
  Abschnitt war verschoben, dadurch stimmten Schriftart und Fettung nicht. Zeilenhöhe und
  Grundlinie kommen jetzt aus der Originaldatei statt aus einer Schätzung.
- **Die Beschriftung „Menü" im Stall saß 26 Pixel daneben**, weil Text-Sprites im veralteten
  Ablaufrechteck statt in ihrer eigenen Box gezeichnet wurden.
- **Die Finanzbalken im Gutshofbüro** wuchsen in die falsche Richtung: Größenänderungen eines
  Sprites skalieren jetzt um den Ankerpunkt, sodass die Balken von der Nulllinie weg wachsen und
  die Vektorlinie treffen.
- **Fensterrahmen fraßen die äußerste Pixelreihe** des Fensterinhalts.
- Die Vordergrundfarbe eines Sprites galt fälschlich auch für Text-, Feld- und Schaltflächen und
  übermalte deren eigene Farben.

### Untersucht
- **Turnierwertung des Originals entschlüsselt** (siehe `analysis/TOURNAMENT.md`): Die sechs
  Gegner würfeln ihre Zeiten gleichverteilt zwischen Sieger- und Verliererzeit; man gewinnt, wenn
  man schneller ist als alle sechs. Die Siegerzeit ist also keine harte Schwelle.
- **Turniertempo bestätigt.** Das Original verlangt den Wechsel zwischen Galopp und Kanter — wer
  eine Gangart durchhält, verliert zwangsläufig. Mit richtiger Einteilung erreicht ein gut
  ausgebildetes Pferd das leichte Turnier in 110,7 statt der nötigen 122 Sekunden und das
  mittlere in 154,7 statt 168 Sekunden. Die Spielziele sind damit regulär erreichbar.
- Außerdem belegt statt geraten: Byteordnung der Fellfarbe, Zeichenreihenfolge bei gleicher Tiefe,
  Pixelgenauigkeit der Hindernis-Kollision und der Bildtakt.
- **Spielregeln von Hand nachgerechnet** und gegen Handbuch und Originalskripte geprüft:
  Verkaufspreis, Deckgebühr, Fellfarben-Vererbung, Mendelsche Regeln, Reitbarkeit, Fütterung,
  Krankheitsverlauf, Trächtigkeit, Spielzeit, Gebäudepreise und Zielprämien. Alle korrekt;
  abgesichert durch 33 neue Tests.

### Geändert
- **Neuer Strikt-Modus für Tests:** ohne jede Ausnahmeliste; jeder Skriptfehler lässt den Test an
  Ort und Stelle fehlschlagen. Ein vollständiger Durchlauf durch alle Gebäude, Pflege, Reiten,
  Zucht, Wirtschaft, Turnier sowie Speichern und Laden ergibt null Skriptfehler.
- **Eine falsche Annahme richtiggestellt:** Das Spiel setzt keinen eigenen Fehlerabfang. Frühere
  Notizen, das Original habe Skriptfehler selbst verschluckt, waren falsch — entsprechend wurden
  Kommentare und Testbegründungen korrigiert. Jede verbleibende Unterdrückung ist in
  `analysis/SUPPRESSED.md` einzeln aufgeführt und begründet.
- Drei Tests waren von der Ausführungsreihenfolge abhängig und sind jetzt beständig.
- Screenshots entstehen jetzt mit einem Werkzeug, das gezielt nur ein einzelnes Fenster aufnimmt,
  statt den gesamten Bildschirm.

### Bekannte Einschränkungen
- Die Werkzeugleiste in der Stadt zeigt einen anderen Symbolsatz als das Handbuch. Die Symbole
  werden korrekt gezeichnet, das Menü wird aber anders befüllt — noch ungeklärt.
- Glyphenbreiten weichen gegenüber dem Original um etwa ein Pixel ab (andere Schriftdarstellung).
- Ein bekannter Fehler des Originals bleibt erhalten: Ein unbekannter Vorfahre im Stammbaum löst
  eine Fehlermeldung aus. Das Spiel läuft weiter.

## [v0.1.0] – 25.10.2026

Erste Veröffentlichung.

### Hinzugefügt
- **Eigenständiger Player** für das Director-8.5-Spiel: liest die Originaldateien der CD und führt
  die originale Spiellogik (Lingo-Bytecode) in einer eigens gebauten Maschine direkt aus.
- **Alle Spielbereiche:** Hofkarte, Stall mit Fütterung, Pflege und Hufpflege, Schmied, Arzt,
  Händler, Zucht, Zuchtweide, Gutshofbüro mit Finanzen, Statistik, Personal, Werbung und
  Gebäudeausbau, Stammbaum, Pferde- und Spielereditor, Optionen.
- **Reit-Minispiel** mit allen 15 Strecken, Gangarten, Sprüngen, Ausdauer, Weggabelungen und
  Turnieren; aus dem Programmcode des Originals portiert.
- **Speichern und Laden** im Format des Originals, Spielstände unter `%APPDATA%\PferdeHof`.
- **Daten-Assistent:** Beim ersten Start fragt das Programm in einfachem Deutsch nach den
  Spieldateien und bereitet sie selbst vor — aus einem Paket (`.7z`, `.zip`), einem CD-Abbild
  (`.cue`, `.bin`, `.iso`), einem Ordner oder direkt vom CD-Laufwerk.
- **Eigenständige Windows-Anwendung** ohne Python-Installation und ohne DirectX-Runtime.
- Fenster- und Vollbildbetrieb (F11), skalierbar, 800×600 bei 30 Bildern je Sekunde.

### Technisches
- Eigener Parser für Director-Filme (`.dxr`, `.cxt`), alle 63 Dateien der CD fehlerfrei gelesen.
- Decompiler für Lingo-Bytecode: 487 Skripte, 1742 Handler, keine ungelösten Befehle.
- Über 250 automatische Tests, ein Langzeitlauf über 1200 Spieltage ohne Fehler.
- Der Kopierschutz der Original-`Starter.exe` wird weder umgangen noch benötigt.

### Bekannte Einschränkungen
- Rich-Text ist nicht in jedem Fall pixelgenau wie im Original.
- Ausführliches Spielen über Stunden durch echte Spieler steht noch aus.
- Fellfarbe und Hindernis-Kollisionen im Reit-Minispiel sind angenähert.
