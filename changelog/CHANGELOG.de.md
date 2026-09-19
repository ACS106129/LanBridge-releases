# Änderungsprotokoll

Alle nennenswerten Änderungen an LanBridge werden hier festgehalten.

Das Format folgt [Keep a Changelog](https://keepachangelog.com/de/1.1.0/), die
Versionsnummern folgen [Semantic Versioning](https://semver.org/lang/de/).

## [0.5.29] - 2026-09-19

### Hinzugefügt

- VPN direkt von VPN Gate laden.
- Websiteliste ohne Trennen ändern.

### Geändert

- Im Tunnel nur die Sites des Ziels.
- Das Update bringt nur noch eine Laufzeit.

### Behoben

- Namensverfolgung übersteht Neuverbindung.

## [0.5.28] - 2026-09-19

### Behoben

- **Das Update ist 38,8 MB kleiner** — der Machine-Learning-Stack des Windows App SDK, onnxruntime und DirectML, wurde in einem anwendungsweisen VPN mitgeliefert, das ihn nie aufruft.
- **Eine Veröffentlichung wird erst sichtbar, wenn alle ihre Installer angehängt sind**, weshalb 0.5.27 einer chinesischen Oberfläche einen deutschen Installer anbot: sie wurde mit einer hochgeladenen Datei veröffentlicht, während der Rest noch lief, und die Anwendung bemerkt eine neue Version binnen etwa einer Minute.

## [0.5.27] - 2026-09-19

### Behoben

- **Das Ziel erreicht nichts mehr außer durch den Tunnel** — das erste Paket einer Verbindung zu einer Adresse ohne Route wird verworfen statt mit der echten Adresse dieses Rechners gesendet; das war die Ursache der wiederholten 403 und des hängenden Ladebildschirms.
- **Das verworfene Paket ist es, was die Route anlegt**, sodass die Verbindung bei der ersten Neuübertragung gelingt statt zu scheitern.
- **Ein blockiertes IPv6-Paket wird weder gezählt noch als Leck protokolliert** — weder in der Zeile im Moment selbst noch im Fazit am Ende, das einen gemessenen Lauf mit "22 von 73 Zielen gingen nicht durch den Tunnel" beschrieb, während alle 51 IPv4-Ziele es taten und die 22 der Wächter bei der Arbeit waren.
- **Einem Tunnel, der sich mit einer anderen Adresse neu verbindet, wird gefolgt**, und liegt die neue außerhalb des Subnetzes, um das der Paketfilter gebaut wurde, hört das Verweigern auf und sagt das, statt jedes Paket des Ziels in eine Verweigerung zu verwandeln.

### Geändert

- **Routen werden wieder aus dem Tunnel genommen** — eine Adresse wird freigegeben, sobald kein verfolgter Name sie mehr nennt und das Ziel keine Verbindung dorthin offen hat, statt dass die Menge die ganze Sitzung über wächst.
- **Eine übernommene Route verfällt nach fünf ungenutzten Minuten** und gibt ihren Platz im Limit der Sitzung zurück.

## [0.5.26] - 2026-09-18

### Geändert

- **Die Liste der Seiten muss nicht mehr stimmen** — sie ist nur noch ein Warmstart, und alles, was das Ziel ohne den Tunnel erreicht, bekommt eine eigene Route; ausgenommen bleiben der VPN-Server, die eigenen Netze dieses Rechners, Broadcast und IPv6.
- **Die Zusammenfassung am Ende nennt ein Ziel nicht mehr verfehlt, nachdem es geroutet wurde**, weil sie das System erneut fragt, statt die angelegten Routen zurückzulesen.

## [0.5.25] - 2026-09-17

### Behoben

- **Seiten gingen nur dann durch den Tunnel, wenn beide Seiten zufällig dieselbe Adresse nannten** — achtzehn von siebenundzwanzig Namen antworteten unterschiedlich, geroutet wurde nur die Antwort des Tunnels, und die Anwendung benutzte die lokale; jetzt werden beide geroutet.
- **Die Seiten werden eine pro Zeile in einem eigenen Fenster bearbeitet**, statt in einem Feld mit siebenundzwanzig Namen.
- **Ein vom Server abgebrochener Download wird dort fortgesetzt, wo er stehen blieb**, statt weggeworfen zu werden, mit bis zu fünf Versuchen je Teil.
- **0.5.24 schob das auf eine Zeitüberschreitung und lag falsch** — der Fehlschlag lief seit dreißig Minuten, ein Limit von fünfzehn kann ihn nicht beendet haben.

## [0.5.24] - 2026-09-17

### Behoben

- **Ein langsamer Update-Download wurde kurz vor dem Ende weggeworfen**, weil das Limit von fünfzehn Minuten auch das Lesen der Datei umfasste und nicht nur das Erreichen des Servers; ein Gesamtlimit gibt es nicht mehr.
- **Updates laden etwa dreimal schneller**, weil der Release-Host je Verbindung und nicht die Leitung begrenzt, also wird der Installer in vier Teilen gleichzeitig geholt.
- **Der Fortschritt wird alle 512 KB statt alle 80 KB gemeldet**, was ein Tempo ist, mit dem das Fenster etwas anfangen kann.

## [0.5.23] - 2026-09-16

### Hinzugefügt

- **Die Sitzung hält fest, wohin das Ziel tatsächlich gegangen ist und was davon den Tunnel verfehlt hat**, sodass ein Lauf benennt, was fehlt, statt die Namensliste eine Vermutung bleiben zu lassen.
- **Adressen werden mit dem Namen gemeldet, auf den sie hören**, weil der Randknotenname eines Content-Netzes den Ort trägt — und der Ort ist die ganze Frage.

### Behoben

- **Die eingetragenen Namen werden während der Sitzung verfolgt**, statt einmal zu Beginn festgelegt zu werden, denn sie antworten mit einer TTL von sechzig Sekunden und eine Sitzung dauert Stunden.
- **Die Suche nach einem funktionierenden Resolver endet, sobald einer geantwortet hat**, statt je Namen in eine Zeitüberschreitung zu laufen und vor dem Start Minuten zu kosten.

## [0.5.22] - 2026-09-16

### Hinzugefügt

- **Die Ausgangsadresse wird vor und nach dem Anlegen der Routen gemessen, und beide Antworten stehen im Log**, denn jede frühere Prüfung war ein Schritt dorthin und nicht das selbst — und "nicht feststellbar" wird als genau das geschrieben.

### Behoben

- **Das Log enthält jetzt die Hälfte einer Sitzung, für die man es überhaupt braucht** — die Schritte des Fensters, jede Meldung des Paketfilters und alles, was OpenVPN gesagt hat, landen in der Datei.
- **Eine Route wird nicht mehr einen Moment vor ihrem Wirken verworfen**; die Routingtabelle bekommt ein paar hundert Millisekunden, um sich zu setzen.
- **Der Paketfilter hält den Filter fest, mit dem er geöffnet wurde**, sodass eine fehlende Klausel von einer zu unterscheiden ist, die nie zutraf.

## [0.5.21] - 2026-09-14

### Behoben

- **Über das VPN geschickte Seiten liefen nicht darüber, während alles das Gegenteil behauptete** — der nächste Hop wurde als .1 des Netzes geraten, die es auf einem /30 nicht gibt; er wird jetzt aus der Adresse abgeleitet, die der Tunnel tatsächlich bekam, und jede Route wird als die geprüft, die das System wirklich nähme.
- **Ein Name, der durch den Tunnel nicht aufgelöst werden konnte, wurde als übereinstimmend mit der lokalen Antwort gemeldet**; keine Antwort ist nicht dieselbe Antwort, und das Log nennt nun Resolver und Transport.
- **Die Abfrage durch den Tunnel fällt auf TCP zurück**, denn ein Relais, das nur eines von beidem trägt, ist bei freiwillig betriebenen Servern üblich.

## [0.5.20] - 2026-09-13

### Hinzugefügt

- **Seiten, die Sie durch das VPN schicken können, ohne den ganzen Rechner zu schicken** — benannte Seiten werden durch den Tunnel aufgelöst und durch ihn geroutet, und während der Sitzung erreicht sie nur die Zielanwendung.

## [0.5.19] - 2026-09-13

### Hinzugefügt

- **Ein Ort für Benutzernamen und Kennwort**, für Server, die eine Anmeldung verlangen; der Dialog sagt offen, dass openvpn es nur aus einer Datei lesen kann und es daher unverschlüsselt im eigenen Ordner dieses Profils liegt.

### Behoben

- **Das Ziel erreichte das Internet über IPv6, vollständig am Tunnel vorbei** — ein Leck in jedem Modus, denn ein Tunnel, der IPv4 trägt, kann nicht tragen, was der Rechner über IPv6 sendet; das IPv6 des Ziels wird jetzt verworfen.
- **"Kein Update", obwohl nichts gefragt worden war** — bei erschöpftem Limit wurde die vorhandene Version gemeldet, und der Validator, der eine Prüfung kostenlos macht, wird nun zwischen Läufen behalten.

## [0.5.18] - 2026-09-13

### Behoben

- **Das Info-Fenster dankte WinDivert, ohne zu sagen, unter welchen Bedingungen es verwendet wird**, und nennt jetzt die LGPL v3, die mitgelieferte Kopie und den Ort der Quellen.
- **Die freien Plätze eines Warcraft-III-Raums änderten sich auf dem anderen Rechner nie**, weil die Ankündigung, die das Gegenüber liest, Broadcast ist und nicht abgefangen werden kann; sie wird nun aus der Anzeige abgeleitet und vor dem Senden geprüft.
- **Der Update-Download hielt das Fenster weiterhin fest** — 0.5.16 erklärte das für behoben, während niemand den Code aufrief; er läuft jetzt wirklich im Hintergrund, mit **Im Hintergrund fortsetzen** im Dialog und einem Abbruch im Hauptfenster.

## [0.5.17] - 2026-09-13

### Behoben

- **Verließ ein Spieler die Warcraft-III-Lobby, kam niemand mehr hinein**, weil ein Listener und jede darauf angenommene Verbindung sich einen Porteintrag teilten und der erste Schließvorgang ihn mitnahm; jetzt wird jeder Socket einzeln verfolgt.
- **Die Zielanwendung lief weiterhin als Administrator** — der Weg aus 0.5.16 brauchte ein Recht, das ein erhöhter Helfer nicht haben kann, also wird nun der genommen, das er hat, und im Log steht welcher.

## [0.5.16] - 2026-09-13

### Hinzugefügt

- **Ein Installer in jeder Sprache, die die Anwendung spricht**, jeweils mit der passenden ANSI-Codepage, und das Update bietet den zur Fenstersprache passenden an.
- **Das Fenster öffnet sich dort, wo Sie es verlassen haben**, sofern diese Position noch auf einem Bildschirm liegt.
- **Ein neues Symbol** — ein Pfeil, der durch die Öffnung eines Rings hinausführt, für jede Größe einzeln gezeichnet statt aus einem großen Bild verkleinert.

### Behoben

- **Start lag unterhalb des sichtbaren Bereichs**; die Schaltflächen sind jetzt unter den Karten verankert, die dahinter scrollen.
- **Die Zielanwendung lief als Administrator**, sodass eine Anmeldung im Browser ihr den Autorisierungscode nie übergeben konnte; sie wird jetzt mit dem Token der Shell gestartet.
- **Das Herunterladen eines Updates nahm das ganze Fenster in Geiselhaft** und läuft nun im Hintergrund, mit Fortschrittsbalken und einer funktionierenden Abbrechen-Schaltfläche.

## [0.5.15] - 2026-09-13

### Behoben

- **Eine einzige Ablehnung des Servers beendete den Versuch**, was für einen eigenen Server richtig und für ein öffentliches Relais falsch ist; es wird nun dreimal wiederholt, bevor gemeldet wird.
- **"EXITING auth-failure" erklärte nichts** und sagt jetzt, welcher Schritt fehlschlug und was das für Ihre Art von Server bedeutet.

## [0.5.14] - 2026-09-12

### Hinzugefügt

- **Ein importiertes Profil gehört jetzt der Anwendung** — die .ovpn und jedes Zertifikat und jeder Schlüssel, auf den sie verweist, werden in einen eigenen Ordner kopiert, sodass das Löschen des Originals nichts ändert.
- **Ein Ort, um zu sehen, was aufbewahrt wird**, mit Umbenennen, Löschen und einem Weg in den Ordner, wobei die Bestätigung in der Zeile selbst stattfindet.
- **OpenVPN, falls Sie es nicht haben**, geholt vom eigenen Download-Host von OpenVPN und alles ablehnend, dem Windows nicht traut oder das OpenVPN nicht signiert hat.
- **Tests für den Installer**, die seine Seiten in beiden Sprachen durchlaufen und bei der Zusammenfassung abbrechen, sodass ein Testlauf nichts installiert.
- **Ein Test, der auf die Pixel schaut**, und jede Erklärungszeile in Einstellungen und Info gegen ihren Hintergrund misst, in beiden Designs.

### Behoben

- **Drei Zeilen im Info-Fenster waren unsichtbar**, weil ein Pinsel aus den Ressourcen der Anwendung gegen deren eigenes Design aufgelöst wird, das eine nicht paketierte WinUI-Anwendung nach dem Start nicht mehr ändern kann.
- **Die Update-Prüfung hörte auf, höflich zu fragen** — sie liest jetzt, wann das Kontingent zurückkommt, wartet bis dahin und sagt es einmal statt sechzigmal.
- **Der Installer schrieb über sein eigenes Bildmaterial**, das nämlich der Hintergrund ist, auf den der Dialog schreibt, und keine Illustration daneben.
- **Das Update gab allen den englischen Installer** und fragt nun nach dem, der zur Fenstersprache passt.

### Geändert

- Unter jeder Einstellung steht eine Zeile, was sie ändert und wo die Einstellungen liegen.
- Das Info-Fenster nennt, wie oft nach Updates gesucht wird, und dankt dem Community-Client von OpenVPN und WinDivert.

## [0.5.13] - 2026-09-12

### Hinzugefügt

- **Ein Flügel** — Bediengeräusche über den General-MIDI-Synthesizer, den Windows ohnehin hat, auf einer pentatonischen Skala, damit je zwei Töne zusammenpassen, mit einem Kästchen zum Abschalten.
- **Bewegung dort, wo etwas geschehen ist**, und nicht überall.
- **Das Diagramm berichtet, statt zu mimen** — ohne Sitzung steht es still, und die Zelle für Blockiertes zeigt die tatsächlich verworfenen Pakete.
- **Ein Installer, der aussieht wie dieses Produkt**, mit erzeugtem Bildmaterial statt der WiX-Platzhalter.
- **Ein Installer in Ihrer Sprache**, eine MSI je Sprache, zunächst Englisch und traditionelles Chinesisch.

### Geändert

- Der Hinweis unter der prozessweisen Beschränkung erscheint jetzt, wenn das Kästchen **leer** ist, denn das ist der Zustand, vor dem zu warnen sich lohnt.

## [0.5.12] - 2026-09-12

### Behoben

- **Im Installationsordner lagen achtundachtzig Übersetzungsordner für Sprachen, die diese Anwendung nicht anbietet**, als Win32-Ressourcen mitgeliefert, die die übliche Einstellung zum Ausdünnen nicht erreicht; jetzt bleiben fünfzehn.

### Geändert

- Die prozessweise Option heißt *Nur die Zielanwendung darf mit dem VPN sprechen*, denn das tut sie; den Tunnel als Ganzes hat sie nie bestimmt.

### Hinzugefügt

- **Zehn weitere Prüfungen, die das echte Fenster bedienen**, über die Dialoge und die Auswahl darin — beim Schreiben fanden sich vier Tests, die etwas suchten, das es nie gab.

## [0.5.11] - 2026-09-12

### Behoben

- **Der dunkle Modus war weiße Schrift auf weißer Seite**, weil das Design auf ein Element innerhalb desjenigen gelegt wurde, das den Hintergrund malt.
- **Das Symbol des Zielabschnitts erschien als leeres Kästchen**, denn ein Codepunkt in der Zeichentabelle einer Schrift bedeutet nicht, dass es dafür eine Glyphe gibt.
- **Die Bedienelemente in jedem Abschnitt standen zentriert** in einer Karte, die bereits die richtige Breite hatte.
- **Anhängen wählen und ohne Programm auf Start drücken warf eine Ausnahme**; jetzt wird gesagt, welche Auswahl fehlt.

### Hinzugefügt

- **Tests, die das echte Fenster bedienen** — neunzehn Prüfungen, die den veröffentlichten Build öffnen und hinsehen, weil jeder bisher gemeldete optische Fehler sämtliche Unit-Tests des Projekts bestehen konnte.

## [0.5.10] - 2026-09-12

### Behoben

- **Die Hinweiszeile verbreiterte die Spalte, statt umzubrechen**, weil ein horizontaler Stapel seine Kinder mit unbegrenzter Breite misst.
- **Einstellungen und Anwendungsinformationen erschienen doppelt**, weil das alte Paar beim Umzug nach rechts oben stehen blieb.
- **Die Prozessauswahl bot diese Anwendung sich selbst an.**

### Geändert

- **Das Diagramm füllt den Platz, den es bekommt**, statt in fester Breite in der Ecke einer großen leeren Karte zu sitzen.
- **Der Hinweis unter der prozessweisen Beschränkung erscheint nur, solange sie an ist**, und sagt, was sie tut.
- Der Zielpfad wird beim Überfahren vollständig gezeigt, wie schmal das Feld auch ist.

## [0.5.9] - 2026-09-12

### Behoben

- **Der dunkle Modus war unbrauchbar**, weil die Seite nie einen eigenen Hintergrund malte und Dialoge das Design gesondert mitgeteilt bekommen mussten.

### Hinzugefügt

- **Die beiden Beschränkungsschalter sind jetzt ein Bild**, ein Raster aus wer sendet gegen wohin, das auf einen Blick beantwortet, woran zwei Absätze Prosa scheiterten.
- **Anhängen wählt aus einer Liste laufender Programme**, statt nach einer anderswo abgeschriebenen Prozess-ID zu fragen.

### Geändert

- Die Routing-Warnung erscheint nur, solange diese Option an ist, sagt eine Sache und sagt sie in der Farbe einer Warnung.
- Die Abschnitte haben ihre Nummerierung verloren; Schritte in einer Reihenfolge waren sie nie.
- Einstellungen und Anwendungsinformationen sind nach rechts oben gewandert, die Statuskarten darunter.

## [0.5.8] - 2026-09-12

### Behoben

- **Den Tunnel auf eine Anwendung zu beschränken wirkte nur in eine Richtung** und ließ jedes andere Programm hier vom anderen Ende aus erreichbar — die Hälfte, auf die es ankommt, wenn man nicht weiß, wer dort ist.

### Geändert

- **Die Routing-Option steht jetzt andersherum** — *Allen Verkehr durch das VPN senden*, standardmäßig aus, mit der Angabe, was das Einschalten für den Betreiber jenes Servers bedeutet.

### Hinzugefügt

- **Helles und dunkles Erscheinungsbild**, oder dem System folgend, sofort angewendet und gemerkt.
- **Symbole durch die Oberfläche hindurch** und eine Statusleuchte, die während einer Sitzung grün ist.

## [0.5.7] - 2026-09-12

### Behoben

- **Ein Download ließ sich nicht abbrechen**, weil er lief, während die Klickverzögerung des Dialogs gehalten wurde, was den Dialog seine eigenen Schaltflächen deaktivieren lässt.
- **Ein abgebrochener oder fehlgeschlagener Download ließ seine Teildatei zurück**, eine je Versuch, für immer.
- **Start zu drücken, wenn es nichts zu starten gab, tat gar nichts**; jetzt wird gesagt, welche Auswahl fehlt.

### Geändert

- **Ein Update während einer laufenden Sitzung zu installieren warnt jetzt zuerst**, mit der sicheren Antwort als Vorgabe, denn das Installieren trennt die Zielanwendung.

## [0.5.6] - 2026-09-12

### Behoben

- **Eine neue Version wird jetzt etwa eine Minute nach der Veröffentlichung bemerkt**, über eine bedingte Anfrage, die nichts kostet, solange sich nichts geändert hat.
- **Den Update-Hinweis wegzuklicken ließ keinen Weg zurück**; Einstellungen und Info-Dialog bieten nun beide *Jetzt aktualisieren*, solange eines wartet.
- **Stopp hieß *Stopp*, während es nichts mehr zu stoppen gab**, und heißt während des Wartens auf eine Übergabe *Erzwingen*.

### Geändert

- **Alles zum Thema Update sitzt jetzt im Anwendungsinformations-Dialog**, neben der Version, mit der verglichen wird.
- **Ein heruntergeladener Installer bleibt erhalten, wenn Sie *Später* wählen**, bis die zugehörige Version überholt ist.

## [0.5.5] - 2026-09-12

### Behoben

- **Automatische Update-Prüfungen waren zu selten, um automatisch zu wirken** — jetzt alle dreißig Minuten und zusätzlich, wenn das Fenster nach vorn kommt und die letzte Prüfung über fünf Minuten her ist.
- **Die beiden Beschränkungsoptionen lasen sich wie Dubletten**; jede Beschriftung nennt nun ihre eigene Achse: welche Ziele, oder welches Programm.

### Geändert

- **Die Schaltfläche im Banner heißt *Jetzt aktualisieren*** statt *Neuigkeiten*, denn installieren ist, was sie tut.
- **Auch aus den Einstellungen lässt sich ein Update starten**, nicht nur danach suchen.
- **Der leere Streifen am oberen Fensterrand ist weg.**
- **Die Zahl weitergeleiteter Anzeigen wird nur für Warcraft III gezeigt**, das einzige Protokoll, auf das sie zutrifft.

## [0.5.4] - 2026-09-12

### Behoben

- **Das Update-Fenster zeigte die Release Notes als Markdown-Quelltext**, statt sie zu setzen, was das zum Lesen Geschriebene mühsam machte.
- **Beim Aktualisieren wurde die Anwendung nicht zuerst geschlossen**, sodass der Installer Dateien ersetzen wollte, die Tunnel und Helfer noch hielten.
- **Fenster und Taskleiste behielten ein generisches Platzhaltersymbol**, denn ein nicht paketiertes Fenster nimmt das Symbol nicht von selbst aus der Exe.
- **Die chinesische Beschriftung für "den Rest des Rechners vom Tunnel fernhalten" beschrieb die andere Einstellung**, wodurch zwei orthogonale Optionen wie Dubletten aussahen.

### Geändert

- **Name und Slogan belegen nicht mehr den oberen Fensterrand**; die Titelleiste sagt bereits, was das ist.
- **Der Anwendungsinformations-Dialog wiederholt den Namen aus seinem Titel nicht mehr** und zeigt die Version groß genug, um sie auf einen Blick zu lesen.

## [0.5.3] - 2026-09-12

### Behoben

- **Ein auf einem Rechner gehostetes Spiel war vom anderen aus sichtbar, aber nicht betretbar**, weil nur der Erkennungsport offen war, während Warcraft III von 6112 bis 6119 hochwandert; jetzt ist der ganze Hostbereich offen, weiterhin nur zum VPN-Subnetz.
- **Ein Raum blieb in der Liste des anderen Spielers, nachdem der Host ihn verlassen hatte**, weil die Schließungsmeldung Broadcast ist und nie ankommt; das Relais merkt nun, dass der Host nicht mehr antwortet, und zieht ihn selbst zurück.
- **Ein Sprachwechsel leerte die Auswahllisten für Zielanwendung und LAN-Erkennung**, weil das Ersetzen des gewählten Eintrags wie dessen Verschwinden gelesen wird; Einträge behalten nun ihre Identität und nur ihr Text ändert sich.
- **Das Einstellungsfenster behielt die alte Sprache in Titel und Schaltfläche**, die nicht Teil des neu beschrifteten Inhalts sind.
- **Das Aktivitätsprotokoll folgte neuen Zeilen nicht zuverlässig**, weil es scrollte, bevor die neue Zeile gesetzt war.
- **Ein Upgrade schrieb jede Datei neu, geändert oder nicht**, weil die alte Version vollständig entfernt wurde, bevor eine einzige neue Datei geschrieben war.
- **Eine der Protokoll-Schaltflächen war angeordnet und anklickbar, wurde aber nie gezeichnet.**

### Hinzugefügt

- **Eine Schaltfläche für Anwendungsinformationen** neben der für Einstellungen, mit Version, Copyright und einem Link zu den Notes dieser Version.
- **Ein Kästchen *LanBridge starten* auf der letzten Installer-Seite**, das die Anwendung ohne Erhöhung startet — so ist sie gedacht.

### Geändert

- **Das Schließen der Zielanwendung beendet die Sitzung nur noch, wenn der Tunnel an sie gebunden ist**, da er sonst noch Verkehr für etwas anderes tragen könnte.

## [0.5.2] - 2026-09-11

### Behoben

- **Die Release Notes im Update-Fenster zeigten nur ihre erste Überschrift**, weil das Steuerelement an einem Wagenrücklauf umbricht, den die normalisierten Notes nicht mehr enthielten.

## [0.5.1] - 2026-09-11

### Behoben

- **Spiele waren sichtbar, aber nicht betretbar, oder erschienen gar nicht** — alles, was ein Spiel betretbar macht, kommt eingehend an, und Windows blockiert das standardmäßig; eine Sitzung öffnet den Erkennungsport nun allein für das VPN-Subnetz und macht am Ende alles rückgängig.
- **Die Oberfläche zerfiel bei jeder Textgröße über der Vorgabe, und ein Neustart brachte sie nie zurück**, weil die skalierte Ebene erst zentriert und dann von ihrer eigenen oberen linken Ecke aus vergrößert wurde.
- **Ein Update-Download zeigte überhaupt keinen Fortschritt**, was von einem nie gestarteten Download nicht zu unterscheiden ist.
- **Automatische Update-Prüfungen wieder einzuschalten bewirkte bis zu vier Stunden nichts.**
- **Die Schließen-Auswahl sah aus, als wäre sie verworfen worden**, wenn im selben Besuch der Einstellungen die Sprache gewechselt wurde.

### Hinzugefügt

- **Update-Prüfungen laufen auch, während die Anwendung im Infobereich sitzt**, angekündigt mit einer Sprechblase und im Tooltip des Symbols festgehalten.
- **Eine Schaltfläche *Jetzt prüfen* in den Einstellungen**, für den Fall, dass auf die nächste geplante Prüfung zu warten nicht der Punkt ist.

## [0.5.0] - 2026-09-11

### Behoben

- **Das VPN brach ab, sobald ein Spielraum geöffnet wurde**, weil die Suche nach dem Prozess, an den ein Launcher übergibt, nur gleichnamige Programmdateien traf.
- **Stopp tat nichts, wenn eine Sitzung bereits geendet hatte**, weil das Schließen der Pipe zu einem verschwundenen Helfer eine Ausnahme warf, die aus dem Aufräumpfad entkam.
- **Ein Sprachwechsel leerte jede Auswahlliste**, statt sie zu übersetzen.
- **Das Aktivitätsprotokoll folgte neuen Zeilen nicht** und hört jetzt auf zu folgen, sobald Sie nach oben scrollen.

### Hinzugefügt

- **Release Notes in Ihrer Sprache**, neben den Builds veröffentlicht und im Update-Dialog passend angezeigt.
- **Fehlerbericht exportieren**, das die Aktivität auf dem Bildschirm mit den Protokollen beider Prozesse bündelt.
- Die Textgrößen-Einstellung skaliert jetzt die ganze Oberfläche, nicht nur das Aktivitätsprotokoll.

## [0.4.0] - 2026-09-10

### Hinzugefügt

- **Automatische Update-Prüfungen**, die vor dem Installieren zeigen, was sich geändert hat; standardmäßig an.
- **Einstellungsdialog** hinter der Zahnradschaltfläche, mit Sprache, Schriftgröße, Schließverhalten und Update-Prüfung.
- **Zehn weitere Oberflächensprachen**, neben Englisch und traditionellem Chinesisch.
- **Das Aktivitätsprotokoll ist jetzt markierbarer Text**, mit *Alles kopieren* und *Exportieren…*.
- **Einstellbare Schriftgröße** (10–22 pt), zwischen Läufen gemerkt.
- **Anwendungssymbol**, verwendet von Fenster, Taskleiste, Infobereich und Programmliste.
- **Der Installer fragt jetzt nach dem Installationsort** und bietet die beiden Verknüpfungen als getrennte Optionen.

### Behoben

- **Das VPN brach ab, sobald das Spiel fertig geladen hatte**, weil das Beenden des ersten Launcher-Prozesses als Beenden des Ziels gelesen wurde; die Sitzung folgt der Übergabe nun.
- **Das Infobereich-Symbol erschien nie**, weil das Symbol-Handle zerstört wurde, bevor Windows es benutzte.
- **Erneutes Öffnen aus dem Infobereich startete eine zweite Kopie**, statt die laufende wiederherzustellen.
- **`WinDivert64.sys` blieb nach dem Schließen gesperrt**, weil das Öffnen eines Treiber-Handles einen Kernel-Dienst registriert, der weiterläuft.
- **Die Sprache auf "Systemstandard" zurückzustellen bewirkte nichts**, weil eine einzelne "alles hat sich geändert"-Meldung nicht zuverlässig verarbeitet wird.
- **Das Deinstallieren verlangte einen Neustart**, da Anwendung und Helfer noch Dateien hielten.
- Zeilen des Aktivitätsprotokolls hatten einen Listenabstand, der eine halbe Leerzeile zwischen den Einträgen ließ.

### Geändert

- Die Protokoll-Aktionen sind in das Aktivitätsfeld rechts gewandert, statt unten in der Konfigurationsspalte zu sitzen.

## [0.3.0] - 2026-09-09

### Hinzugefügt

- Fehlerprotokolle in `%LOCALAPPDATA%\LanBridge\logs\`, eine Datei je Prozess und Lauf, mit unbehandelten Ausnahmen aufgezeichnet statt stillem Ende.
- Einstellungen werden bei jeder Änderung geschrieben und überstehen so einen Absturz.
- Unterstützung für den Infobereich mit Nachfrage beim Schließen.
- Traditionelles Chinesisch neben Englisch.
- MSI-Installer mit Verknüpfungen, Versionsangaben und Eintrag in der Programmliste.

### Behoben

- Der veröffentlichte Build startete und starb dann in der XAML-Laufzeit, weil eine nicht paketierte WinUI-Anwendung ihr kompiliertes Markup beim Veröffentlichen nicht mitnimmt.

## [0.2.0] - 2026-09-09

### Behoben

- **Nach der Erhöhungsabfrage erschien nie ein Fenster**, weil WinUI 3 nicht erhöht laufen kann; die Oberfläche läuft ohne Erhöhung und übergibt privilegierte Arbeit an einen Helfer, der einmal je Sitzung um Zustimmung bittet.

## [0.1.0] - 2026-09-09

### Hinzugefügt

- Anwendungsweises VPN, das die gepushte Standardroute und DNS ablehnt, sodass nur das VPN-Subnetz den Tunnel quert.
- Optionale prozessweise Beschränkung mit WinDivert, die Verkehr ins VPN-Subnetz von jedem Prozess außer dem Ziel verwirft.
- LAN-Erkennungsrelais für Tunnel ohne Broadcast, einschließlich des W3GS-Protokolls von Warcraft III, dessen Spielinformationen nur als Unicast-Antwort gesendet werden.
- Allgemeines UDP-Broadcast-Relais für andere Spiele, über den Port konfiguriert.
- Kommandozeilenversion derselben Engine ohne Oberfläche.

[0.5.27]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.27
[0.5.26]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.26
[0.5.25]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.25
[0.5.24]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.24
[0.5.23]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.23
[0.5.22]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.22
[0.5.21]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.21
[0.5.20]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.20
[0.5.19]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.19
[0.5.18]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.18
[0.5.17]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.17
[0.5.16]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.16
[0.5.15]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.15
[0.5.14]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.14
[0.5.13]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.13
[0.5.12]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.12
[0.5.11]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.11
[0.5.10]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.10
[0.5.9]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.9
[0.5.8]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.8
[0.5.7]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.7
[0.5.6]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.6
[0.5.5]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.5
[0.5.4]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.4
[0.5.3]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.3
[0.5.2]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.2
[0.5.1]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.1
[0.5.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.0
[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0
