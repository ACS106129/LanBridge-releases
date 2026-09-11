# Änderungsprotokoll

Alle nennenswerten Änderungen an LanBridge werden hier festgehalten.

Das Format folgt [Keep a Changelog](https://keepachangelog.com/de/1.1.0/), die
Versionierung folgt [Semantic Versioning](https://semver.org/lang/de/).

## [0.5.4] - 2026-09-12

### Behoben

- **Das Aktualisierungsfenster zeigte die Versionshinweise als Markdown-Quelltext** –
  Rauten, Sternchen und Backticks – statt sie zu formatieren, was ausgerechnet das
  mühsam zu lesen machte, was zum Lesen geschrieben ist. Überschriften, Aufzählungen,
  Hervorhebungen und Inline-Code werden jetzt formatiert.
- **Die Aktualisierung schloss die Anwendung nicht vorher.** Das Installationsprogramm
  startete, während Tunnel und privilegierter Helfer noch die Dateien hielten, die es
  ersetzen wollte. Jetzt wird die Sitzung beendet und dieser Prozess läuft aus, bevor das
  Installationsprogramm startet; dieses beendet eine übrig gebliebene Instanz, statt sie
  aus einer Aktualisierung eine Neustartaufforderung machen zu lassen.
- **Fenster und Taskleiste behielten ein allgemeines Platzhaltersymbol**, während
  Infobereich und „Apps & Features“ das richtige zeigten. Ein nicht paketiertes Fenster
  übernimmt das Symbol nicht von selbst aus der ausführbaren Datei.
- **Die chinesische Beschriftung von „Den Rest des Rechners aus dem Tunnel halten“ beschrieb
  die falsche Einstellung.** Sie las sich als „den Verkehr anderer Anwendungen aus dem
  Tunnel halten“, was die anwendungsbezogene Beschränkung tut — die beiden Optionen wirkten
  dadurch wie Dubletten. Sie stehen quer zueinander: die eine begrenzt, welche Ziele den
  Tunnel nutzen, die andere, welcher Prozess ihn nutzen darf.

### Geändert

- **Name und Untertitel belegen nicht länger den oberen Fensterrand.** Die Titelleiste sagt
  bereits, worum es geht, und die Angaben sind ins Informationsfenster gewandert.
- **Das Informationsfenster wiederholt nicht länger den Namen, mit dem es überschrieben
  ist**, und überlässt das Öffnen des Protokollordners der Protokollleiste, wo diese
  Schaltfläche ohnehin sitzt. Die Version, derentwegen man es öffnet, steht jetzt groß
  genug, um sie auf einen Blick zu lesen.

## [0.5.3] - 2026-09-12

### Behoben

- **Ein auf dem einen Rechner gehostetes Spiel war vom anderen aus sichtbar, aber nicht zu
  betreten.** Eingehend war nur der Suchport geöffnet, und das ist nicht zwingend der Port,
  auf dem ein Gastgeber lauscht: Warcraft III nimmt 6112, wenn es kann, und geht sonst bis
  6119 hinauf — angekündigt wird der, den es bekommen hat. Ein von 6112 verdrängter
  Gastgeber war damit sichtbar und unerreichbar, und das nur in dieser einen Richtung, was
  wie ein Fehler eines der beiden Rechner aussah. Jetzt wird der gesamte Hosting-Bereich
  geöffnet, weiterhin nur für das VPN-Subnetz.
- **Ein Spiel blieb in der Liste des anderen Spielers, nachdem der Gastgeber es verlassen
  hatte.** Warcraft III kündigt ein geschlossenes Spiel per Broadcast an, und ein Broadcast
  kann über den VPN-Adapter hinausgehen, wo das Relais bewusst nicht lauscht — die
  Ankündigung wurde also nie aufgegriffen, und die Gegenstelle bot weiter ein Spiel an, das
  es nicht mehr gab. Das Relais bemerkt jetzt, dass der Gastgeber nicht mehr auf seine
  Anfragen antwortet, und nimmt das Spiel selbst zurück — anhand der zuletzt
  weitergeleiteten Ankündigung, die sagt, um welches es ging.
- **Ein Sprachwechsel leerte die Auswahllisten für Zielanwendung und LAN-Suche**, und die
  Wahl von „Systemstandard“ leerte die Sprachliste selbst. Eine Liste neu zu übersetzen
  heißt, ihre Einträge zu ersetzen, und ein Auswahlfeld deutet das Ersetzen des gewählten
  Eintrags als dessen Verschwinden: Es löschte die Auswahl, und die Bindung schrieb diese
  Leere über die getroffene Wahl. Die Einträge behalten jetzt ihre Identität und nur ihr
  Text ändert sich, also bleibt nichts mehr zu löschen. Zwei frühere Anläufe stellten die
  Auswahl nachträglich wieder her; dieser beseitigt die Ursache.
- **Das Einstellungsfenster behielt in Titel und Schaltfläche die alte Sprache**, wenn die
  Sprache darin gewechselt wurde. Der gesamte Inhalt des Fensters wurde neu beschriftet,
  aber Titel und Schließen-Schaltfläche gehören nicht dazu.
- **Das Aktivitätsprotokoll folgte neuen Zeilen nicht zuverlässig.** Es scrollte, bevor die
  neue Zeile gesetzt war, landete also dort, wo das Ende vorher lag, und blieb dauerhaft
  eine Zeile zurück. Jetzt wird nach dem Setzen gescrollt, und das Folgen endet, sobald Sie
  zum Lesen nach oben scrollen — und setzt wieder ein, wenn Sie nach unten zurückkehren.
- **Jede Aktualisierung schrieb sämtliche Dateien neu, geändert oder nicht.** Die alte
  Version wurde vollständig entfernt, bevor auch nur eine neue Datei geschrieben war, sodass
  jede Aktualisierung die gesamte Installation neu schrieb. Jetzt wird zuerst die neue
  Version geschrieben und die alte danach entfernt. Dadurch überspringt das
  Installationsprogramm identische Dateien, und zu schreiben bleibt nur, was sich
  tatsächlich geändert hat.
- **Eine der Protokoll-Schaltflächen war angeordnet und anklickbar, wurde aber nie
  gezeichnet.** „Protokollordner öffnen“ belegte ihren Platz und reagierte auf Klicks, ohne
  irgendetwas anzuzeigen. Die Protokollaktionen stehen jetzt in einer einzigen waagerechten
  Reihe statt in je einer Spalte, womit die fehlerhafte spaltenweise Anordnung entfällt.

### Hinzugefügt

- **Eine Schaltfläche mit Programminformationen** neben der für die Einstellungen: welche
  Version läuft, das Copyright und ein Link zu deren Hinweisen und Downloads.
- **Ein Kontrollkästchen „LanBridge starten“ auf der letzten Seite des
  Installationsprogramms**, standardmäßig aktiviert. Es startet die Anwendung ohne erhöhte
  Rechte – so, wie LanBridge laufen soll: Die Zustimmung wird beim Start einer Sitzung
  erfragt, nicht vorher.

### Geändert

- **Das Schließen der Zielanwendung beendet die Sitzung nur noch, wenn der Tunnel an sie
  gebunden ist.** Ist „Nur diese Anwendung darf das VPN verwenden“ aktiv, besteht der Tunnel
  für genau diesen Prozess und endet mit ihm — sonst bliebe ein Tunnel übrig, den nichts auf
  dem Rechner benutzen darf. Ohne die Option ist der Tunnel nur nach Ziel eingegrenzt und
  trägt womöglich noch anderen Verkehr, bleibt also bestehen, bis Sie ihn anhalten.

## [0.5.2] - 2026-09-11

### Behoben

- **Die Versionshinweise im Aktualisierungsfenster zeigten nur ihre erste Überschrift.**
  Beim Herauslösen aus dem Änderungsprotokoll werden die Hinweise auf einfache
  Zeilenvorschübe vereinheitlicht, während ein Windows-Textsteuerelement Zeilen am
  Wagenrücklauf umbricht – alles nach der ersten Zeile wurde also nie gezeichnet. Ein
  englischer Versionstext bringt Wagenrückläufe bereits mit, weshalb nur die übersetzten
  Hinweise leer wirkten. Sie werden jetzt vor der Anzeige umgewandelt und in einem
  scrollbaren, markierbaren Block dargestellt.

## [0.5.1] - 2026-09-11

### Behoben

- **Spiele waren sichtbar, ließen sich aber nicht betreten, oder tauchten gar nicht erst
  auf.** Alles, was ein Spiel betretbar macht, kommt *eingehend* durch den Tunnel, und
  Windows blockiert das standardmäßig vollständig: Die Ankündigung, die das Relais der
  Gegenstelle weiterleitet, ist eingehendes UDP, und das Beitreten ist eine eingehende
  TCP-Verbindung. Die Kommandozeilenfassung öffnete beides; die Anwendung hat das nie
  übernommen, weshalb der Rechner ohne übrig gebliebene Regel in einer oder in beiden
  Richtungen nicht erreichbar war. Eine Sitzung öffnet den Suchport jetzt nur für das
  VPN-Subnetz – nicht für jedes Netz, mit dem der Rechner verbunden ist –, holt den
  Tunneladapter aus der Kategorie „öffentlich“, die Windows ihm gibt, und nimmt beides am
  Ende wieder zurück.
- **Die Oberfläche zerfiel bei jeder Textgröße oberhalb der Voreinstellung, und ein
  Neustart half nicht.** Die skalierte Ebene wurde erst zentriert und dann von ihrer
  eigenen linken oberen Ecke aus vergrößert. Dadurch begann die Oberfläche weiter unten
  und weiter rechts als vorgesehen und lief unten und rechts aus dem Fenster – samt der
  Schaltfläche für die Einstellungen. Da die Textgröße gespeichert wird, landete jeder
  Neustart im selben Zustand, ohne einen Weg zurück zu der Einstellung, die ihn verursacht
  hat.
- **Beim Herunterladen einer Aktualisierung war kein Fortschritt zu sehen.** Das Fenster
  schloss sich, sobald *Herunterladen* gedrückt wurde, und die Übertragung lief ohne jede
  Anzeige – von einem Download, der nie begonnen hat, nicht zu unterscheiden. Die
  Versionshinweise bleiben jetzt offen und zeigen einen Fortschrittsbalken, die übertragene
  Menge und ein *Abbrechen*, das wirkt.
- **Die automatische Aktualisierungsprüfung wieder einzuschalten blieb bis zu vier Stunden
  wirkungslos.** Die Prüfung im Hintergrund sah sich die Einstellung erst beim nächsten
  geplanten Durchlauf wieder an; jetzt schaut sie sofort nach.
- **Die Auswahl für das Schließen des Fensters wirkte verworfen**, wenn beim selben Besuch
  der Einstellungen die Sprache gewechselt wurde. Das Übersetzen der Liste ersetzt den
  ausgewählten Eintrag und löscht damit die Auswahl – die übrigen Listen stellen sich
  selbst wieder her, diese nicht.

### Hinzugefügt

- **Nach Aktualisierungen wird auch dann gesucht, wenn die Anwendung im Infobereich
  liegt**, alle vier Stunden statt nur beim Start. Eine neue Version wird mit einer
  Sprechblase im Infobereich angekündigt, und der Tooltip des Symbols weist weiter darauf
  hin, wenn die Sprechblase verschwunden ist.
- **Eine Schaltfläche *Jetzt suchen* in den Einstellungen**, für den Fall, dass es keinen
  Grund gibt, auf die nächste geplante Prüfung zu warten.

## [0.5.0] - 2026-09-11

### Behoben

- **Das VPN brach ab, sobald ein Spiel eröffnet wurde.** Die Suche nach dem Prozess, an den
  ein Starter übergibt, verglich nur Programme mit demselben Namen — ein Spiel, das unter
  anderem Namen weiterläuft, wurde nie gefunden, und der Tunnel fiel. Jetzt zählt jeder
  Prozess, der noch aus demselben Installationsordner läuft, und die Suche hält fest, wonach
  sie gesucht hat.
- **Anhalten bewirkte nach dem Ende einer Sitzung nichts.** Das Schließen der Verbindung zum
  Hilfsprozess warf einen Fehler, wenn die Gegenseite bereits weg war, und dieser Fehler
  entkam der Aufräumroutine — die Anwendung hielt eine beendete Sitzung weiter für aktiv.
- **Ein Sprachwechsel leerte alle Auswahllisten**, statt sie zu übersetzen. Das Ersetzen des
  Listeninhalts löscht die Auswahl, und die Bindung schrieb diese leere Auswahl zurück.
- **Das Aktivitätsprotokoll folgte neuen Zeilen nicht.** Es scrollt nun zum neuesten Eintrag
  und hört damit auf, sobald Sie zum Lesen nach oben scrollen.

### Hinzugefügt

- **Versionshinweise in Ihrer Sprache.** Übersetzte Änderungsprotokolle werden zusammen mit
  den Builds veröffentlicht, und der Updatedialog zeigt das zur Oberflächensprache passende.
- **Fehlerbericht exportieren**: eine Schaltfläche, die mit einer Markierung erscheint,
  sobald etwas fehlgeschlagen ist. Sie bündelt die angezeigte Aktivität mit den
  Protokolldateien beider Prozesse, sodass sich ein Problem melden lässt, ohne zu wissen, wo
  die Protokolle liegen.
- Die Einstellung der Textgröße skaliert jetzt die gesamte Oberfläche, nicht nur das
  Protokoll.

## [0.4.0] - 2026-09-10

### Hinzugefügt

- **Automatische Updateprüfung.** Die Anwendung sieht auf GitHub nach neueren Versionen und
  zeigt vor der Installation, was sich geändert hat. Standardmäßig aktiv, in den
  Einstellungen abschaltbar.
- **Einstellungsdialog** hinter der Zahnradschaltfläche, mit Sprache, Schriftgröße der
  Oberfläche, Verhalten beim Schließen und Updateprüfung — damit eine gemerkte Auswahl nie
  in eine Sackgasse führt.
- **Zehn weitere Sprachen**: Chinesisch (traditionell und vereinfacht), Japanisch,
  Koreanisch, Spanisch, Französisch, Portugiesisch, Russisch und Italienisch, neben
  Englisch und Deutsch. Statustexte, Auswahllisten, Dialoge und das Menü im Infobereich
  sind übersetzt.
- **Das Aktivitätsprotokoll ist jetzt markierbarer Text**, mit *Alles kopieren* und
  *Exportieren…*.
- **Einstellbare Schriftgröße der Oberfläche** (10–22 pt), die zwischen Sitzungen erhalten
  bleibt.
- **Anwendungssymbol** für Fenster, Taskleiste, Infobereich und Programme hinzufügen oder
  entfernen.
- **Das Installationsprogramm fragt nach dem Installationsort** und bietet die Verknüpfung
  im Startmenü und die auf dem Desktop als zwei getrennte Optionen an.

### Behoben

- **Das VPN brach genau dann ab, wenn das Spiel fertig geladen hatte.** Warcraft III
  beendet — wie die meisten Titel mit Starter oder Updater — seinen ersten Prozess und
  übergibt an einen anderen. Dieses Beenden wurde als „Ziel geschlossen" gewertet und der
  Tunnel im ungünstigsten Moment abgebaut. Die Sitzung folgt der Anwendung nun über die
  Übergabe hinweg.
- **Das Symbol im Infobereich erschien nie.** Sein Handle wurde zerstört, bevor Windows es
  verwendete, und `Shell_NotifyIcon` zeigt mit einem zerstörten Handle nichts an, ohne
  einen Fehler zu melden.
- **Erneutes Öffnen aus dem Infobereich startete eine zweite Kopie**, statt die laufende
  wiederherzustellen. Nun läuft je Benutzer nur eine Instanz, und ein erneuter Start holt
  das vorhandene Fenster nach vorn.
- **`WinDivert64.sys` blieb nach dem Beenden gesperrt.** Das Schließen der Treiber-Handles
  genügt nicht: Beim Öffnen wird ein Kerneldienst registriert, der weiterläuft, und die
  Datei bleibt gesperrt, bis er gestoppt wird. Der Dienst wird jetzt am Sitzungsende
  gestoppt und entfernt. Das Schließen des Fensters beendet außerdem den privilegierten
  Hilfsprozess, was zuvor nicht geschah.
- **Das Zurückstellen der Sprache auf „Systemstandard" bewirkte nichts.** Die Änderung
  wurde mit einer einzigen „alles geändert"-Benachrichtigung gemeldet, auf die WinUI nicht
  zuverlässig reagiert; jetzt wird jede Zeichenfolge namentlich gemeldet.
- **Die Deinstallation verlangte einen Neustart.** Das Installationsprogramm schließt nun
  zuerst die Anwendung und ihren Hilfsprozess, sodass keine Dateien in Benutzung bleiben.
- Die Protokollzeilen hatten den Innenabstand eines Listenelements, wodurch zwischen den
  Einträgen eine halbe Leerzeile stand.

### Geändert

- Die Protokollschaltflächen sind in den Aktivitätsbereich rechts gewandert, statt unten in
  der Konfigurationsspalte zu stehen.

## [0.3.0] - 2026-09-09

### Hinzugefügt

- Fehlerprotokollierung in `%LOCALAPPDATA%\LanBridge\logs\`, eine Datei je Prozess und
  Ausführung; unbehandelte Ausnahmen werden an drei Stellen abgefangen und aufgezeichnet,
  statt die Anwendung stillschweigend zu beenden.
- Dauerhafte Einstellungen: Sie werden bei jeder Änderung geschrieben und überstehen damit
  einen Absturz oder ein erzwungenes Beenden.
- Unterstützung für den Infobereich mit einer Rückfrage beim Schließen.
- Oberfläche auf Chinesisch (traditionell) zusätzlich zu Englisch.
- MSI-Installationsprogramm mit Verknüpfungen, Versionsangaben und einem Eintrag unter
  Programme hinzufügen oder entfernen.

### Behoben

- Der veröffentlichte Build startete und starb sofort in der XAML-Laufzeit: Beim
  Veröffentlichen einer nicht paketierten WinUI-Anwendung wird ihr kompiliertes Markup
  nicht mitgeliefert, sodass `InitializeComponent` nichts zu laden fand.

## [0.2.0] - 2026-09-09

### Behoben

- **Nach der Rechteabfrage erschien nie ein Fenster.** WinUI 3 kann nicht mit erhöhten
  Rechten laufen: Die WinRT-Aktivierung schlägt fehl und der Prozess endet, ohne etwas
  anzuzeigen. Die Oberfläche läuft jetzt ohne erhöhte Rechte und übergibt die
  privilegierte Arbeit an einen eigenen Hilfsprozess, der einmal je Sitzung um Zustimmung
  bittet. Die Oberfläche besitzt keinerlei Rechte, der Hilfsprozess nur solange eine
  Sitzung läuft.

## [0.1.0] - 2026-09-09

### Hinzugefügt

- VPN je Anwendung: importiert ein OpenVPN-Profil und lehnt die vom Server gesendete
  Standardroute und DNS ab, sodass nur das VPN-Subnetz durch den Tunnel geht und der Rest
  des Rechners seinen gewohnten Weg behält.
- Optionale Begrenzung auf einen Prozess über WinDivert: Verkehr anderer Prozesse in das
  VPN-Subnetz wird verworfen.
- LAN-Erkennungsweiterleitung für Tunnel, die keinen Broadcast tragen, einschließlich des
  W3GS-Protokolls von Warcraft III, dessen Spielinformationen nur als Unicast-Antwort
  verschickt und nie per Broadcast verteilt werden — sie müssen beim lokalen Spiel
  angefordert und dann weitergeleitet werden.
- Allgemeine UDP-Broadcast-Weiterleitung für andere Spiele, über den Port konfiguriert.
- Kommandozeilenversion derselben Engine.

[0.5.4]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.4
[0.5.3]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.3
[0.5.2]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.2
[0.5.1]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.1
[0.5.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.0
[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0
