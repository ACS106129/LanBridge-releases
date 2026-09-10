# Änderungsprotokoll

Alle nennenswerten Änderungen an LanBridge werden hier festgehalten.

Das Format folgt [Keep a Changelog](https://keepachangelog.com/de/1.1.0/), die
Versionierung folgt [Semantic Versioning](https://semver.org/lang/de/).

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

[0.5.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.0
[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0
