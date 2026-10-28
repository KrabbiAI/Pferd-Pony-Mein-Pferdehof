# Änderungsverlauf

Alle nennenswerten Änderungen an diesem Projekt. Neueste Version zuerst.

## [v0.1.5] – 28.10.2026

Die ersten beiden Fehler, die beim **wirklichen Spielen** aufgefallen sind. Beide Programmteile liefen in
der Abdeckungsmessung und standen auf keiner Liste ungeprüfter Pfade — sie verhielten sich nur falsch.
Abdeckung misst, ob Code *ausgeführt* wird, nicht ob er sich *richtig* verhält.

### Behoben
- **Das Hinweisfenster „Du hast noch den Reitdress an" ließ sich nicht wegklicken.** Der Klick auf OK
  wirkte, das Fenster ging nur sofort wieder auf — zwölfmal in zwölf Bildern. Der Schieberegler setzt beim
  Drücken `Flag` und zeigt die Warnung in *jedem* `exitFrame`, solange sie steht; beendet wird das allein
  durch das Loslassen. Unser Hinweisfenster schluckte die Ereignisse, die währenddessen eintrafen, auch
  das Loslassen — das Spiel bekam es nie und zeigte die Warnung im nächsten Bild erneut. Im Original ist
  das eine eigene, modale Form: solange sie steht, läuft der Film nicht, und danach stellt Director dem
  Bildelement sein `mouseUp` zu. Genau das tun wir jetzt auch.
- **Jeder Rollkasten des Spiels war eine Zeile zu kurz.** Die Textbox im Spielerfenster ließ sich um 14 von
  29 nötigen Pixeln rollen, die letzte Zeile blieb hinter der Unterkante. `GetMaxScroll` nimmt für einen
  Textdarsteller `charPosToLoc(Darsteller, letztesZeichen)` als Texthöhe; wir gaben den *oberen* Rand der
  letzten Zeile zurück statt ihren unteren. Den Beleg liefert das Spiel selbst: Im selben Handler steht für
  ein Feld `textHeight = member.height`, also die volle Höhe — beide Zweige meinen dasselbe. Betroffen waren
  alle Rollkästen: Apotheke, Händler, Züchter, Spielerfenster.

### Geprüft
- Zwei neue Tests in `tests/test_played.py`, die beide Fehler nachstellen: Der Regler wird gedrückt
  *gehalten* und die Warnungen gezählt, der Rollbalken gedrückt gehalten, bis er nicht weiter kann. Ohne
  die Behebungen schlagen sie fehl (12 Warnungen statt einer; 14 statt 29 Pixel Rollweg).

## [v0.1.4] – 28.10.2026

Eine vierte Prüfrunde, die genau eine Frage abarbeitet: Steckt hinter den 63 erreichbaren
Programmteilen des Originals, die noch kein Test berührt hatte, eine echte Lücke? Jeder einzelne
wurde gegen den Originalcode gelesen und entweder über die Oberfläche angefahren oder am Code als
unerreichbar belegt.

**Das Ergebnis:** Von den **900 erreichbaren Programmteilen** des Originals laufen **899
nachweislich** (99,9 %). 34 der 63 haben einen Test bekommen, 28 haben sich als im Original
unerreichbar erwiesen, einer bleibt offen. Die Bezugsgröße ist um 30 kleiner als vorher, weil ein
Unerreichbarkeitsbeweis den Programmteil aus dem Nenner nimmt.

### Behoben
- **Jeder Ausritt lief mit der Hofmusik im Rücken.** `updateStage` führte einen anstehenden
  Bildsprung nicht aus. Das Spiel braucht das an genau einer Stelle — der einzigen im ganzen Spiel,
  an der ein Sprung unmittelbar von `updateStage` gefolgt wird: Vor der Übergabe an das
  Reit-Minispiel stellt das Original den Abspielkopf auf ein Wartebild, dessen Bildskript Musik und
  Umgebungsgeräusche abschaltet. Dieses Bildskript ist nie gelaufen.

### Geprüft
- **34 weitere Programmteile des Originals haben einen Test** — und zwar einen, der beweist, dass er
  sein Ziel erreicht: Ein Mitschnitt vor dem Handleraufruf lässt den Test fehlschlagen, wenn er den
  Originalteil nicht mehr anfährt. Darunter das Wartebild und die Ritt-Steuerung (nur über den Cheat
  erreichbar, der nach „idkfa" auf Tabulator und F liegt), der Kartenhintergrund, die grasenden
  Pferde auf der Koppel, die Bank der Stadt, die Schimmelflecken von Stute und Fohlen in der Box,
  das Schließen des Pferdebuchs und die Diagnoseliste der Tierärztin.
- **Ein bestehender Test belegte nur scheinbar etwas:** Der Klick auf den Kartenhintergrund traf
  immer ein darüberliegendes Gebäude, weil die Klickhilfe nur in der Bildmitte nach undurchsichtigen
  Punkten sucht.
- Die 28 unerreichbaren Programmteile wurden zusätzlich von Hand gegen unsere Maschine gelesen und,
  wo sie keinen eigenen Zustand brauchen, direkt aufgerufen: Keiner benutzt einen Lingo-Befehl, einen
  Erweiterungsaufruf oder eine Bildelement-Eigenschaft, die wir nicht umsetzen.

### Im Original gefunden
- **Eine Tippfehler-Leiche im Menüsystem.** Die Schaltflächen-Weiche fragt `#subVerify` ab, die
  beiden Menüeinträge, die eine Prüfung mitbringen, schreiben aber `#_subVerify`. Der einzige
  Prüf-Rückruf, den die Hofkarte dem Menüsystem anbietet, läuft nie.
- **Der Film der Tierärztin verlässt Bild 1 nie.** Die einzige Bildnavigation im ganzen Film ist
  `go to frame the frame` im Bildskript von Bild 1 — alles, was auf den 647 weiteren Bildern liegt,
  ist unerreichbar.
- **Die Pferdeliste im Pferdebuch ist abgeschaltet.** Ihre fünf Knöpfe sind in jedem Bild
  unsichtbar, weil der einzige Aufrufer die nötige Kennung nicht mitgibt.
- **Die Tonkanal-Reparatur des Menü-Casts wurde vor der Auslieferung abgeschaltet**: Beide Zeilen,
  die sie benutzen würden, sind im ausgelieferten Objekt auskommentiert.

## [v0.1.3] – 25.10.2026

Eine dritte Prüfrunde, diesmal auf **Vollständigkeit** statt Richtigkeit: Ist überhaupt alles da,
was das Original kann? Dafür wurde das Handbuch Funktion für Funktion abgearbeitet, gemessen welche
Originalskripte beim Testen je ausgeführt werden, jedes einzelne Medium durchgerechnet und das
Speichern und Laden auf Herz und Nieren geprüft.

**Zwei Zahlen fassen das Ergebnis zusammen:**
- Das Handbuch beschreibt **214 Spielfunktionen; 210 laufen nachweislich** (98,1 %). Ohne Beleg
  bleiben vier, darunter die beiden Beenden-Knöpfe, deren Klick den Testlauf selbst beenden würde.
- Von den **930 erreichbaren Programmteilen** des Originals laufen **867 nachweislich** (93,2 %).
  Kein einziger erreichbarer Programmteil ist defekt — die übrigen sind nur ungetestet.

### Behoben
- **Pferde ließen sich gar nicht pflegen.** Fell, Mähne und Hufe — zwei ganze Handbuchkapitel und
  der Kern des Spiels — waren über die Oberfläche unerreichbar. Die Pflegehand, die dem Mauszeiger
  folgt, fing jeden Klick ab, obwohl sie ihn gar nicht verarbeitet; die Bürsten bekamen ihn nie zu
  sehen. Es stieg lediglich das Vertrauen. Jetzt fängt ein Bildelement einen Klick nur noch dann,
  wenn seine Skripte ihn auch verarbeiten.
- **163 Bilder hatten die falsche Transparenz**, darunter alle Menüecken und sämtliche Vögel. Die
  Alphaebene liegt unkomprimiert vor, wenn sie genau der Ebenengröße entspricht — wir haben blind
  entpackt. Betroffen waren 259 Bilder, 96 davon zusätzlich mit falscher Länge.
- **Ein abgebrochener Schreibvorgang konnte den Spielstand zerstören.** Es wurde direkt in die
  Zieldatei geschrieben; bei vollem Datenträger oder Absturz war der alte Stand weg. Jetzt wird
  erst in eine Zwischendatei geschrieben und dann umbenannt.
- **Die Pferdeliste im Pferdeeditor war unsichtbar** und die Zuchtlisten gesperrt: Von vier
  Größenangaben im Textkopf wurde die falsche als Zeilenhöhe gelesen — 588 Pixel je Zeile.
- Das Laden stürzte bei unlesbaren Zeichen im Spielstand ab.
- Zehn Texte standen linksbündig statt zentriert oder rechtsbündig.
- **Die Rundenanzeige im Reit-Minispiel zeigte eine Runde zu viel**, etwa „6/5" statt „5/5". Das
  Original schreibt den Text im Ziel ein zweites Mal und setzt dabei beide Zahlen gleich.

### Geprüft
- **Alle 4817 Medien einzeln** dekodiert und gezeichnet, dazu alle 8624 Einzelbilder der
  Animationen: kein Absturz, keine leere Dekodierung, keine falsche Größe, kein stummer Ton.
- **57 Funktionen des Handbuchs waren umgesetzt, aber von keinem Test berührt** — sie haben jetzt
  einen. Dazu Tests für 71 nie ausgeführte Programmteile des Originals.
- **Spielstände**: Ein weit fortgeschrittener Stand wird gespeichert, in einem frischen Prozess
  geladen und Feld für Feld zeichengenau verglichen. Dazu Grenzfälle wie beschädigte Dateien,
  voller Datenträger, gleichzeitiges Schreiben und Namen mit Umlauten.
- Ein Test war ein Glücksspiel: Der Zufallsklicker seedete nur sich selbst, nicht den Zufall des
  Spiels. Jetzt beides.
- Ein zweiter Test war es ebenfalls: Nach einem harten Ausritt steigt die Krankheitsgefahr des
  Pferdes — es sei denn, die Auslosung direkt danach trifft das Pferd wirklich, dann wird die
  Gefahr auf null gesetzt und die Krankheit stattdessen wahr. Der Test erwartete nur den ersten
  Ausgang und schlug ab und zu fehl; jetzt lässt er beide zu.

### Im Original gefunden
- **„Pferd ohne Box einstellen" ist kaputt.** Das Original baut dort einen Aufruf mit hängendem
  Komma zusammen, der sich nicht übersetzen lässt: Der Programmteil stirbt, das Fenster bleibt
  offen. Der zugehörige Zielhandler ist dadurch toter Code.
- **812 der 1742 Programmteile sind toter Code**: veraltete Kopien, nie verlinkte Filme wie der
  Entwickler-Pferdeeditor, Verhaltensweisen ohne Bildelement, Reste der Autorenumgebung.

## [v0.1.2] – 25.10.2026

Eine zweite Prüfrunde gegen das Original, diesmal auf Bereiche, die bisher Lücken hatten:
Laufzeitverhalten statt Standbilder, Ton statt nur „der Mischer öffnet", und erstmals der
Maschinencode der Erweiterungen statt nur ihrer beobachteten Nutzung. **20 weitere Fehler behoben**,
die Testsuite ist auf 393 Tests gewachsen.

Bemerkenswert: Die drei schwerwiegendsten Funde — stehende Animationen, tote Zufallsmusik und
falsche Symbole — fallen beim Spielen sofort auf, wurden aber von über 300 automatischen Tests
nicht bemerkt. Tests prüften Zustände, nicht den Verlauf über die Zeit.

### Behoben
- **Drei Bildschirme zeigten falsche Bilder.** Weist die Spiellogik einem Bildelement ein neues Bild
  zu und wechselt danach die Szene, so behält das Original diese Zuweisung bei — unsere Umsetzung
  verwarf sie. Betroffen waren die Werkzeugleiste in der Stadt (ein Symbol erschien viermal statt
  Händler, Ausrüstung, Züchter, Tierärztin, Schmied und Arena), die Stallbox (zeigte immer die
  vierte Ausbaustufe) und die Hofkarte (zeigte Platzhalter statt der stufenabhängigen Gebäude).
- **Alle Animationen standen still.** Die 62 Film-Loops zeigten dauerhaft ihr erstes Bild, weil die
  Bildnummer im Loop aus dem Ablauf abgeleitet wurde — das Spiel parkt aber überall auf einem Bild
  und schleift dort. Betroffen waren unter anderem die 24 Animationen im Stall, die Ladeanimation
  und das Flugzeug über der Hofkarte.
- **Der Abspann lief dreimal zu lang** und sprang am Ende an den Anfang: Die Sprünge zum nächsten
  Abschnitt gingen um ein Bild weiter statt zur nächsten Marke.
- **Die Zufallsmusik war faktisch tot.** Die Hintergrundmusik lief als Dauerschleife, dadurch wurde
  der Tonkanal nie frei. Genau daran hängt im Original aber das Verhalten, das das nächste Stück
  zieht — es feuerte nie, und es lief immer dasselbe Stück endlos.
- **Der Hinweiston für neue Nachrichten ging immer verloren**, weil das Einreihen eines Tons ihn
  bei belegtem Kanal verwarf.
- **Die Balance zwischen links und rechts** nutzte einen dreimal zu großen Wertebereich.
- **Eine laufende Lautstärke-Überblendung wurde vom Abspielen gelöscht.**
- Zwei Director-Eigenschaften für Stummschaltung und Gesamtlautstärke waren wirkungslos.
- **Die Zeitanzeige des Reit-Minispiels hatte ein Stundenfeld zu viel.** Das Original rechnet die
  Stunden aus und verwirft sie wieder: aus 3725 Sekunden wird „02:05.0", nicht „01:02:05".
- **Zehn Abweichungen in den Spielerweiterungen**, gefunden durch Dekompilierung ihres
  Maschinencodes: Verzeichnisabfrage, Vergabe freier Dateinamen, Verschlüsselung der Spielstände,
  Rückgabe der Reitergebnisse, Löschen von Dateien, acht Funktionen mit falschem Rückgabewert und
  die Bildschirmabfrage. Die beiden bisher nur angenommenen Verschlüsselungskonstanten sind damit
  nachgewiesen.

### Geändert
- **Der Turniersieg im Abschlusstest wird nicht mehr durchgängig erzwungen.** Mit einem voll
  austrainierten Pferd und einem Reiter, der zwischen Galopp und Kanter wechselt, werden die
  Turniere der Klassen L und M jetzt wirklich erritten; der Test prüft, dass die gerittene Zeit
  unter der Siegerzeit liegt. Nur beim Championat bleibt die Nachhilfe, weil der Skript-Reiter
  dort vier Sprünge verpatzt — seine Ungeschicklichkeit, nicht die Physik.
- Neue Tests ohne Ausnahmeliste für: Schmied und Arzt auf den Hof rufen, Stall-Apotheke und
  Medikamente, Zufallsereignisse, drei Spieler nebeneinander, schwarzes Fohlen über Speichern
  und Laden.
- Der Stammbaum-Fehler ist unabhängig als Fehler des Originals bestätigt.
- **Offene Fragen aus der ersten Runde beantwortet:** der genaue Pfad des Datenordners, das
  Dateiformat der Spielstände, die Rückgabewerte der Atom-Funktionen und wie das Spiel seine
  Objekte erzeugt. Eine Schleife im Original stellte sich dabei als toter Code heraus.

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
