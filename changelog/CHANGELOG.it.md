# Registro delle modifiche

Qui sono annotate tutte le modifiche rilevanti di LanBridge.

Il formato segue [Keep a Changelog](https://keepachangelog.com/it/1.1.0/) e la
numerazione segue il [versionamento semantico](https://semver.org/lang/it/).

## [0.5.2] - 2026-09-11

### Corretto

- **Le note di versione nella finestra di aggiornamento mostravano solo il primo titolo.**
  Quando vengono estratte dal registro delle modifiche, le note sono normalizzate a
  semplici avanzamenti di riga, mentre un controllo di testo di Windows spezza le righe sul
  ritorno a capo: tutto ciò che seguiva la prima riga non veniva mai disegnato. Il corpo di
  una versione in inglese contiene già i ritorni a capo, ed è per questo che sembravano
  vuote solo le note tradotte. Ora vengono convertite prima di essere mostrate e compaiono
  in un blocco scorrevole e selezionabile.

## [0.5.1] - 2026-09-11

### Corretto

- **Le partite si vedevano ma non si riusciva a entrare, oppure non comparivano affatto.**
  Tutto ciò che rende una partita accessibile arriva *in entrata* attraverso il tunnel, e
  Windows lo blocca per intero in modo predefinito: l'annuncio inoltrato dal relè del
  compagno è UDP in entrata, ed entrare in partita è una connessione TCP in entrata. La
  versione a riga di comando apriva entrambi; l'applicazione non l'ha mai fatto, così la
  macchina rimasta senza una sua regola risultava irraggiungibile in un verso o in
  entrambi. Ora ogni sessione apre la porta di individuazione soltanto per la sottorete
  della VPN — non per tutte le reti a cui la macchina è collegata —, sposta la scheda del
  tunnel fuori dalla categoria *pubblica* che Windows le assegna, e ripristina entrambe le
  cose alla fine.
- **L'interfaccia si sfasciava con qualsiasi dimensione del testo superiore a quella
  predefinita, e riavviare non la recuperava.** Il livello ingrandito veniva prima centrato
  e poi fatto crescere dal proprio angolo in alto a sinistra, così tutto partiva più in
  basso e più a destra del dovuto e usciva dal bordo inferiore e da quello destro,
  portandosi via il pulsante delle impostazioni. Poiché la dimensione del testo viene
  ricordata, ogni riavvio ricadeva nello stesso stato, senza un modo per raggiungere
  l'impostazione che lo aveva causato.
- **Lo scaricamento di un aggiornamento non mostrava alcun avanzamento.** La finestra si
  chiudeva appena premuto *Scarica* e il trasferimento avveniva senza nulla sullo schermo,
  cosa indistinguibile da uno scaricamento mai iniziato. Le note di versione ora restano
  aperte, con una barra di avanzamento, la quantità trasferita e un *Annulla* che funziona.
- **Riattivare il controllo automatico degli aggiornamenti non faceva nulla per un massimo
  di quattro ore.** Il controllo in secondo piano rileggeva l'impostazione solo alla
  successiva esecuzione programmata; ora guarda subito.
- **La scelta sul comportamento alla chiusura sembrava scartata** se nella stessa visita
  alle impostazioni si cambiava lingua. Tradurre l'elenco sostituisce la voce selezionata e
  questo azzera la selezione: gli altri elenchi si ripristinano da soli, questo no.

### Aggiunto

- **Gli aggiornamenti vengono cercati anche mentre l'applicazione è nell'area di
  notifica**, ogni quattro ore invece che solo all'avvio. Una nuova versione viene
  annunciata con un fumetto nell'area di notifica, e il suggerimento dell'icona continua a
  segnalarla dopo che il fumetto è scomparso.
- **Un pulsante «Controlla ora» nelle impostazioni**, per quando aspettare il prossimo
  controllo programmato non ha senso.

## [0.5.0] - 2026-09-11

### Corretto

- **La VPN cadeva appena si apriva una partita.** La ricerca del processo a cui un launcher
  passa il testimone confrontava solo eseguibili con lo stesso nome, quindi un gioco che
  prosegue con un nome diverso non veniva mai trovato e il tunnel cadeva. Ora conta qualsiasi
  processo ancora in esecuzione dalla stessa cartella di installazione, e la ricerca annota
  che cosa ha cercato.
- **Arresta non faceva nulla una volta terminata la sessione.** Chiudere la pipe verso il
  processo ausiliario generava un errore quando l'altra estremità era già sparita, e
  quell'errore sfuggiva alla pulizia: l'applicazione continuava a credere che una sessione
  finita fosse ancora attiva.
- **Cambiare lingua svuotava tutti gli elenchi a discesa** invece di tradurli. Sostituire il
  contenuto di un elenco azzera la selezione, e l'associazione riscriveva quella selezione
  vuota.
- **Il registro attività non seguiva le righe nuove.** Ora scorre fino alla più recente e
  smette di seguirle non appena si scorre verso l'alto per leggere qualcosa.

### Aggiunto

- **Note di rilascio nella tua lingua.** I registri delle modifiche tradotti vengono
  pubblicati insieme alle build, e la finestra di aggiornamento mostra quella corrispondente
  alla lingua dell'interfaccia.
- **Esporta rapporto errori**: un pulsante che compare con un contrassegno non appena
  qualcosa fallisce. Raccoglie l'attività a schermo insieme ai file di log di entrambi i
  processi, così un problema si può segnalare senza sapere dove stiano i log.
- L'impostazione della dimensione del testo ora ridimensiona tutta l'interfaccia, non solo il
  registro.

## [0.4.0] - 2026-09-10

### Aggiunto

- **Controllo automatico degli aggiornamenti.** L'applicazione verifica se su GitHub esiste
  una versione più recente e mostra che cosa è cambiato prima di installarla. Attivo per
  impostazione predefinita, disattivabile dalle impostazioni.
- **Finestra delle impostazioni** dietro il pulsante a ingranaggio, con lingua, dimensione
  del testo dell'interfaccia, comportamento alla chiusura e controllo aggiornamenti: così
  una scelta memorizzata non diventa mai un vicolo cieco.
- **Dieci lingue in più**: cinese tradizionale, cinese semplificato, giapponese, coreano,
  spagnolo, francese, tedesco, portoghese e russo, oltre a inglese e italiano. Stato,
  elenchi a discesa, finestre di dialogo e menu dell'area di notifica sono tradotti.
- **Il registro attività è ora testo selezionabile**, con i pulsanti *Copia tutto* ed
  *Esporta…*.
- **Dimensione del testo dell'interfaccia regolabile** (10–22 pt), mantenuta tra un avvio e
  l'altro.
- **Icona dell'applicazione**, usata da finestra, barra delle applicazioni, area di
  notifica e Installazione applicazioni.
- **Il programma di installazione chiede dove installare** e propone il collegamento nel
  menu Start e quello sul desktop come due scelte indipendenti.

### Corretto

- **La VPN cadeva proprio quando il gioco finiva di caricare.** Warcraft III, come la
  maggior parte dei titoli con un launcher o un aggiornatore, chiude il primo processo e
  passa il testimone a un altro. Quella chiusura veniva interpretata come «il bersaglio si
  è chiuso» e il tunnel veniva smontato nel momento peggiore. Ora la sessione segue
  l'applicazione attraverso il passaggio di consegne.
- **L'icona nell'area di notifica non compariva mai.** L'handle dell'icona veniva distrutto
  prima che Windows lo usasse, e `Shell_NotifyIcon` con un handle distrutto non mostra
  nulla senza segnalare alcun errore.
- **Riaprire dall'area di notifica avviava una seconda copia** invece di ripristinare
  quella in esecuzione. Ora c'è una sola istanza per utente e un nuovo avvio riporta in
  primo piano la finestra esistente.
- **`WinDivert64.sys` restava bloccato dopo la chiusura.** Chiudere gli handle del driver
  non basta: aprirne uno registra un servizio del kernel che continua a girare, e il file
  resta bloccato finché non viene arrestato. Ora il servizio viene arrestato e rimosso alla
  fine della sessione. Chiudere la finestra arresta anche il processo ausiliario con
  privilegi, cosa che prima non avveniva.
- **Riportare la lingua su «Predefinito di sistema» non faceva nulla.** La modifica veniva
  annunciata con una sola notifica «è cambiato tutto», alla quale WinUI non reagisce in
  modo affidabile; ora ogni stringa viene annunciata per nome.
- **La disinstallazione chiedeva di riavviare.** Il programma di installazione chiude prima
  l'applicazione e il suo ausiliario, quindi non restano file in uso.
- Le righe del registro avevano la spaziatura di un elemento di elenco, che lasciava mezza
  riga vuota fra una voce e l'altra.

### Modificato

- I pulsanti del registro si sono spostati nel pannello attività a destra, invece di stare
  in fondo alla colonna di configurazione.

## [0.3.0] - 2026-09-09

### Aggiunto

- Registrazione degli errori in `%LOCALAPPDATA%\LanBridge\logs\`, un file per processo e
  per esecuzione, con le eccezioni non gestite intercettate in tre punti e annotate invece
  di far sparire l'applicazione in silenzio.
- Persistenza delle impostazioni: scritte a ogni modifica, sopravvivono a un arresto
  anomalo o a una chiusura forzata.
- Supporto dell'area di notifica, con una domanda alla chiusura fra uscire e ridurre.
- Interfaccia in cinese tradizionale oltre all'inglese.
- Programma di installazione MSI con collegamenti, informazioni di versione e voce in
  Installazione applicazioni.

### Corretto

- La build pubblicata si avviava e moriva subito dentro il runtime XAML: pubblicare
  un'applicazione WinUI non pacchettizzata non porta con sé il markup compilato, e
  `InitializeComponent` non trovava nulla da caricare.

## [0.2.0] - 2026-09-09

### Corretto

- **La finestra non compariva mai dopo la richiesta di elevazione.** WinUI 3 non può girare
  con privilegi elevati: l'attivazione WinRT fallisce e il processo termina senza mostrare
  nulla. Ora l'interfaccia gira senza privilegi e affida il lavoro privilegiato a un
  processo ausiliario separato, che chiede il consenso una volta per sessione.
  L'interfaccia non detiene alcun privilegio e l'ausiliario li mantiene solo finché la
  sessione è in corso.

## [0.1.0] - 2026-09-09

### Aggiunto

- VPN per singola applicazione: importa un profilo OpenVPN e rifiuta la route predefinita e
  il DNS inviati dal server, così solo la subnet VPN attraversa il tunnel e il resto del
  computer conserva il percorso abituale.
- Confinamento facoltativo per processo tramite WinDivert, scartando il traffico verso la
  subnet VPN proveniente da qualsiasi processo diverso dal bersaglio.
- Inoltro del rilevamento LAN per tunnel che non trasportano il broadcast, compreso il
  protocollo W3GS di Warcraft III, le cui informazioni di partita vengono inviate solo come
  risposta unicast e mai in broadcast: vanno quindi richieste al gioco locale e inoltrate.
- Inoltro generico di broadcast UDP per altri giochi, configurato per porta.
- Versione a riga di comando dello stesso motore.

[0.5.2]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.2
[0.5.1]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.1
[0.5.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.0
[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0
