# Änderungsverlauf

Alle nennenswerten Änderungen an diesem Projekt. Neueste Version zuerst.

## [Unveröffentlicht] – v0.1.1 in Arbeit

### Behoben
- **Nachrichten konnten nie gelesen werden.** Der Nachrichten-Dialog meldete dem Spiel immer
  „später lesen" zurück. Dadurch brach die Schleife, die die Nachrichten abarbeitet, sofort ab und
  die Liste wuchs endlos weiter (über 500 Einträge nach 1200 Spieltagen). Jetzt erscheinen die
  beiden Knöpfe des Originals, „gelesen" löscht den Eintrag und zeigt die nächste Nachricht.

### Untersucht
- **Turnierwertung des Originals entschlüsselt** (siehe `analysis/TOURNAMENT.md`): Die sechs
  Gegner würfeln ihre Zeiten gleichverteilt zwischen Sieger- und Verliererzeit; man gewinnt, wenn
  man schneller ist als alle sechs. Die Siegerzeit ist also keine harte Schwelle.
- **Reitgeschwindigkeit wird geprüft.** Ein maximal ausgebildetes Pferd braucht bei uns im Kanter
  138 Sekunden für das leichte Turnier, was nur etwa 2 Prozent Siegchance je Versuch ergibt. Im
  Galopp ist dasselbe Pferd nach 40 Sekunden erschöpft. Ob das dem Original entspricht, wird gerade
  gegen den Programmcode des Original-Minispiels geprüft.

### Geändert
- Screenshots entstehen jetzt mit einem Werkzeug, das gezielt nur ein einzelnes Fenster aufnimmt,
  statt den gesamten Bildschirm.

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
