# Registro delle modifiche

Qui sono annotate tutte le modifiche rilevanti di LanBridge.

Il formato segue [Keep a Changelog](https://keepachangelog.com/it/1.1.0/) e la
numerazione segue il [versionamento semantico](https://semver.org/lang/it/).

## [0.5.19] - 2026-09-13

### Added

- **Un posto per nome utente e password.** Alcuni server chiedono di accedere e non c'era
  dove scriverlo. Un `auth-user-pass` senza file dopo significa "chiedi alla console", e qui
  openvpn parte senza finestra e con l'output reindirizzato: fa una domanda che nessuno sente
  e poi segnala un accesso fallito. La gestione dei profili ora ha un pulsante per ciascuno.

  La password è salvata in chiaro e la finestra lo dice invece di lasciar credere altro. Si
  trova nella cartella di quel profilo, che possono aprire solo tu, SYSTEM e gli
  amministratori, e non viene mai riletta per essere mostrata.

### Fixed

- **L'applicazione di destinazione usciva su internet via IPv6, aggirando il tunnel.**
  Trovato osservando una sessione vera: quattro minuti, quattro destinazioni, una via IPv6.
  Questa macchina ha un indirizzo IPv6 globale dell'operatore e il tunnel è IPv4.

  Era una perdita in ogni modalità, compresa quella che consegna l'intera macchina alla VPN.
  Ora l'IPv6 della destinazione viene scartato durante la sessione; quello degli altri no.

- **"Aggiornato" senza aver chiesto.** A limite orario esaurito, il controllo riportava la
  versione già installata come se avesse guardato. Ora dice che non è riuscito a controllare
  e quando riproverà.

## [0.5.18] - 2026-09-13

### Fixed

- **La finestra Informazioni ringraziava WinDivert senza dire a quali condizioni è usato.**
  È la GNU LGPL v3, che chiede al programma di dirlo, di nominare la licenza e di indicare
  la copia che distribuisce; un ringraziamento non è nessuna delle tre. Ora ci sono tutte e
  tre. Ed è indicato che OpenVPN viene scaricato da openvpn.net anziché distribuito qui.
- **I posti liberi di una sala di Warcraft III non cambiavano mai sull'altra macchina.**
  Apri un posto dove sedeva un computer e l'altro continuava a vedere la sala com'era,
  finché non usciva dall'elenco delle partite e rientrava.

  Chi ha già la sala nel proprio elenco non rilegge l'annuncio completo. Prende i numeri da
  un piccolo pacchetto che l'host trasmette ogni volta che la sala cambia, e ricostruisce la
  voce solo quando l'elenco viene riaperto. Quel pacchetto è in broadcast, e il broadcast è
  proprio ciò che qui non si riesce a intercettare: la porta su cui bisognerebbe ascoltare è
  già occupata dal gioco. Perciò ora viene ricavato dall'annuncio e inviato quando i numeri
  si muovono.

  I numeri vengono controllati prima: si leggono da una posizione fissa in fondo a un
  pacchetto la cui disposizione è stata dedotta, e una partita ha da uno a ventiquattro
  posti e non può averne di liberi più di quanti ne abbia. Altrimenti la lettura è
  sbagliata, e allora non si invia nulla.

- **Il download dell'aggiornamento teneva ancora ferma la finestra.** La 0.5.16 diceva di
  averlo corretto. Il trasferimento in background, la barra di avanzamento e il pulsante di
  annullamento erano scritti e nulla li chiamava mai.

  Ora avviene davvero in background, e all'avvio la finestra di dialogo offre **Continua in
  background**: la finestra si chiude, il trasferimento prosegue e riferisce nella barra
  della finestra principale, dove può anche essere annullato. Annullare e fallire ora si
  distinguono — prima entrambi aprivano la pagina della versione nel browser.

## [0.5.17] - 2026-09-13

### Fixed

- **Bastava che un giocatore uscisse dalla sala di Warcraft III perché non entrasse più
  nessuno.** Segnalato così: metti il posto di qualcuno su computer, aperto o chiuso e non
  riesce più a rientrare. Sono tre modi di far cadere la sua connessione, e un giocatore
  che se ne va da solo fa lo stesso.

  Un socket in ascolto e ogni connessione accettata su di esso condividono una porta
  locale. L'elenco di quali porte appartengono al gioco era tenuto per porta, così
  l'ascolto e le connessioni condividevano una voce, e la prima connessione a chiudersi se
  la portava via. Warcraft è ancora in ascolto e continua ad annunciarsi, quindi la sala
  resta nell'elenco di tutti — ma ogni pacchetto che arriva su quella porta non è più di
  nessuno agli occhi del filtro, e viene scartato. Visibile e inaccessibile, per tutti,
  finché l'host non crea un'altra partita.

  Ora ogni socket è tenuto da conto separatamente, e una porta smette di appartenere al
  gioco quando si chiude l'ultimo, non il primo.

- **L'applicazione di destinazione girava ancora come amministratore.** La 0.5.16 diceva di
  averlo corretto e non l'aveva fatto. Dare a un processo l'identità dell'utente connesso si
  può fare in due modi, che chiedono permessi diversi: quello usato richiede un privilegio
  che un amministratore con elevazione non ha e non può ottenere, quindi falliva ogni volta
  e il vecchio comportamento subentrava in silenzio. Ora usa quello il cui permesso
  l'assistente possiede davvero.

## [0.5.16] - 2026-09-13

### Aggiunto

- **Un installer in ogni lingua che l'applicazione parla.** Ne parlava undici e il suo
  installer due. Ora sono undici, ciascuno con la codepage ANSI giusta.
- **La finestra si apre dove l'hai lasciata.** Dimensione, posizione e se era ingrandita.
  Si salva la dimensione ripristinata, e una posizione che non cade più su nessuno schermo
  viene scartata.
- **Una nuova icona.** La vecchia era una barra con due punti e non diceva nulla di cosa
  faccia questo programma. Ora è una freccia che esce dall'apertura di un anello: il
  tunnel, e l'unica applicazione che lo attraversa. Disegnata separatamente a ogni
  dimensione. L'anello è aperto dal lato da cui esce la freccia, perché uno chiuso con una
  linea è il segnale di divieto.

### Corretto

- **Avvia finiva sotto la piega.** I due pulsanti erano l'ultima cosa nella colonna delle
  schede di configurazione, e quella colonna scorre. Non appena le schede bastavano a
  riempirla — e a un'altezza di finestra ordinaria bastano — l'azione principale
  dell'applicazione diventava qualcosa da cercare scorrendo. Ora i pulsanti sono fissati
  sotto le schede, e sono le schede a scorrere dietro di loro.
- **L'applicazione di destinazione girava come amministratore.** L'assistente che la avvia
  deve esserlo, e un processo figlio eredita il token del padre. Un programma elevato è
  isolato dal desktop non elevato: è così che un gioco che accede dal browser non riceve
  mai il suo codice di autorizzazione. Ora viene avviato con il token della shell, come te.
- **Scaricare un aggiornamento bloccava l'intera finestra.** Ora avviene in secondo piano,
  con l'avanzamento in una barra della finestra principale.

## [0.5.15] - 2026-09-13

### Corretto

- **Un solo rifiuto del server chiudeva il tentativo.** openvpn considera fatale un
  accesso rifiutato ed esce al primo: giusto per un server tuo, sbagliato per un relay
  pubblico, che rifiuta perché è pieno o perché il volontario che lo teneva non c'è più, e
  lo stesso profilo si connette un minuto dopo. Ora riprova, e si ferma dopo tre volte
  così che una password davvero sbagliata venga comunque segnalata.
- **"EXITING auth-failure" non spiegava nulla.** Sembra una password sbagliata, e dopo che
  un certificato è stato accettato di solito non lo è. Il messaggio ora dice quale passo è
  fallito e cosa significa.

## [0.5.14] - 2026-09-12

### Aggiunto

- **Un profilo importato ora appartiene all'applicazione.** Prima si ricordava dove fosse
  il file e lo si rileggeva a ogni avvio, il che regge finché il file non si sposta, la
  chiavetta non esce o la cartella Download non viene svuotata. Ora viene copiato in una
  cartella propria, insieme a ogni certificato e chiave a cui fa riferimento, e quei
  riferimenti vengono riscritti verso le copie.
- **Un posto dove vedere cosa è conservato.** Un pulsante Gestisci accanto a Importa: cosa
  c'è, quale è in uso, rinomina, elimina e una porta sulla cartella.
- **OpenVPN, se non ce l'hai.** Questa applicazione pilota il client community di OpenVPN;
  non lo contiene. Ora lo dice prima di partire e propone di scaricare la versione attuale
  dal server ufficiale di OpenVPN e installarla, rifiutando qualunque cosa Windows non
  accetti o che non sia firmata da OpenVPN.
- **Test per l'installer.** Cosa c'è nel pacchetto e un percorso fra le sue pagine nelle
  due lingue. Si ferma al riepilogo e annulla: eseguire la suite non installa nulla.
- **Un test che guarda i pixel.** Ogni riga di spiegazione viene fotografata in entrambi i
  temi e misurata contro ciò che ha dietro.

### Corretto

- **Tre righe della finestra Informazioni erano invisibili.** Erano dipinte con un pennello
  preso dalle risorse dell'applicazione, che si risolve sul tema dell'applicazione stessa —
  e un'applicazione WinUI non pacchettizzata non può cambiarlo dopo l'avvio, mentre le
  finestre di dialogo sono disegnate nel tema che hai scelto.
- **Il controllo aggiornamenti ha smesso di bussare.** Sessanta all'ora è esattamente il
  limite senza autenticazione. Ora legge quando torna la quota e aspetta.

- **L'installer scriveva sopra la propria grafica.** Quelle immagini non sono disegni
  accanto al testo: sono lo sfondo su cui la finestra scrive, nel suo colore scuro, e dove
  lo decide lei. Riempire tutti i 493 pixel con una sfumatura blu metteva ogni titolo scuro
  su scuro. Ora la grafica è una fascia a sinistra e un blocco a destra nel banner.
- **L'aggiornamento offriva a tutti l'installer inglese.** Una release porta un MSI per
  lingua e l'aggiornamento prendeva il primo dell'elenco, cioè quello caricato per primo.
  Ora chiede quello che corrisponde alla lingua della finestra, e ripiega sull'inglese
  quando quella lingua non ne ha uno proprio.

### Modificato

- Sotto ogni impostazione una riga che dice cosa cambia e dove viene conservata.
- Informazioni spiega come vengono cercati gli aggiornamenti e cita OpenVPN e WinDivert.

## [0.5.13] - 2026-09-12

### Aggiunto

- **Il suono.** WinUI ha un sistema sonoro dentro ogni controllo — messa a fuoco,
  attivazione, finestre di dialogo che si aprono e si chiudono — e tace finché
  un'applicazione non lo chiede. Questa non l'aveva mai chiesto: ogni pressione era muta per
  omissione e non per scelta. Ora è una scelta, è spaziale, e nelle impostazioni c'è una
  casella per chi preferisce un'utilità silenziosa.
- **Movimento dove è successo qualcosa.** Le due colonne compaiono mentre la finestra si
  monta, il testo di stato riemerge quando cambia, il conteggio inoltrato sobbalza quando
  sale, la spia respira durante una sessione, e una riga di avviso spinge le vicine invece
  di comparire dal nulla.
- **Il disegno riferisce invece di recitare.** Faceva la stessa animazione che stesse
  accadendo qualcosa o no: decorazione travestita da strumento. Senza sessione è smorzato e
  fermo, con la sessione si muove, e la cella bloccata mostra quanti pacchetti la guardia
  ha davvero scartato — un numero che all'interfaccia non era mai arrivato, perché nessuno
  aveva mai ascoltato l'evento che lo porta.
- **Un programma di installazione che somiglia a questo prodotto**, con immagini generate
  al posto dei segnaposto di WiX.
- **Un programma di installazione nella tua lingua**: un MSI per lingua invece dell'inglese
  per tutti. Inglese e cinese tradizionale per cominciare.

### Modificato

- L'avviso sul confinamento per processo compare ora quando la casella è **vuota**, che è
  lo stato che merita un avviso, e dice cosa significa quello stato invece di ripetere
  l'etichetta.

## [0.5.12] - 2026-09-12

### Corretto

- **La cartella di installazione conteneva ottantotto cartelle di traduzioni per lingue che
  questa applicazione non offre** — af-ZA, sl-SI, fil-PH e le altre. Sono le stringhe del
  Windows App SDK stesso, distribuite come risorse Win32 e non come assembly satellite
  .NET, quindi l'impostazione consueta per sfoltirle non le raggiunge. Ora restano solo
  quelle corrispondenti a una lingua che l'interfaccia parla: quindici invece di ottantotto.

### Modificato

- L'opzione per processo si chiama *Solo l'applicazione di destinazione può parlare con la
  VPN*, che è ciò che fa. Non ha mai governato l'intero tunnel.

### Aggiunto

- **Altre dieci verifiche che guidano la finestra vera**, sulle finestre di dialogo e su
  ciò che contengono. Scriverle ha trovato due cose da sé: una finestra di dialogo qui non
  è una finestra e non si chiama come sembrava, così quattro test cercavano qualcosa che
  non è mai esistito; e Avvia non è disponibile senza un profilo, invece di accettare la
  pressione e non fare nulla.

## [0.5.11] - 2026-09-12

### Corretto

- **La modalità scura era testo bianco su pagina bianca.** Il tentativo precedente dipingeva
  lo sfondo su un elemento e applicava il tema a quello interno, così il testo si risolveva
  nel tema scuro e la superficie dietro nel chiaro. Il tema ora sta sull'elemento che
  dipinge lo sfondo, dove i due concordano.
- **L'icona della sezione di destinazione veniva disegnata come un quadrato vuoto.** Che un
  code point sia nella tabella caratteri di un font non significa che il font ne abbia il
  glifo. I tre segni di sezione ora sono emoji.
- **I controlli di ogni sezione stavano centrati** in una scheda che aveva già la larghezza
  giusta. Un expander allarga sé stesso, non il proprio contenuto.
- **Scegliere il collegamento e premere Avvia senza scegliere un programma sollevava
  un'eccezione.** Il controllo aggiunto per un eseguibile mancante non copre il
  collegamento, a cui manca altro. Ora dice cosa manca, e il messaggio non chiede più di
  digitare un identificatore di processo in un controllo che è un elenco.

### Aggiunto

- **Test che guidano la finestra vera.** Tutti i difetti visivi segnalati finora
  supererebbero qualunque test unitario del progetto, perché nessuno riguarda ciò che
  restituisce un metodo. Diciannove verifiche ora aprono la build pubblicata e guardano.

## [0.5.10] - 2026-09-12

### Corretto

- **La riga di avviso allargava la colonna invece di andare a capo.** Una pila orizzontale
  misura i figli con larghezza illimitata, quindi un blocco di testo a capo automatico al
  suo interno non va mai a capo: allarga tutto ciò che ha accanto, pulsante compreso.
  Entrambe le note ora stanno in una griglia che dà al testo una larghezza vera.
- **Impostazioni e informazioni comparivano due volte.** La coppia accanto alle schede di
  stato è rimasta lì quando sono salite in alto a destra.
- **L'elenco dei processi proponeva questa stessa applicazione.** Collegare il tunnel alla
  finestra che lo configura non è nelle intenzioni di nessuno.

### Modificato

- **Il disegno usa lo spazio che gli è stato dato.** Le rotte si allungano con la finestra
  invece di restare a larghezza fissa nell'angolo di una scheda grande e vuota, e i pacchetti
  percorrono tutta quella larghezza.
- **La nota sul confinamento per processo compare solo quando è attivo**, con lo stesso segno
  di avviso di quella sull'instradamento, e dice cosa fa: i pacchetti di ogni altro programma
  di qui vengono fermati.
- Il percorso del programma di destinazione si vede per intero al passaggio del puntatore,
  per quanto stretta sia la casella.

## [0.5.9] - 2026-09-12

### Corretto

- **La modalità scura era inutilizzabile.** La pagina non dipingeva mai uno sfondo proprio,
  quindi il testo seguiva il tema e la superficie dietro no: testo chiaro su fondo chiaro.
  Anche le finestre di dialogo mantenevano l'aspetto di sistema, perché una finestra di
  dialogo è ospitata dalla radice della finestra e non dall'elemento su cui il tema è stato
  impostato, e andava avvisata a parte.

### Aggiunto

- **I due interruttori di confinamento ora sono un disegno.** Una griglia di chi invia
  contro dove, con il traffico che percorre ogni rotta: dal tunnel, dall'uscita di sempre,
  oppure fermato. Le quattro celle sono tutte le combinazioni delle due impostazioni, e
  rispondono a colpo d'occhio a ciò che due paragrafi di testo non riuscivano a spiegare.
- **Il collegamento si sceglie da un elenco di programmi in esecuzione** invece di chiedere
  un identificatore di processo cercato altrove, con un pulsante per aggiornarlo.

### Modificato

- L'avviso sull'instradamento compare solo finché quell'opzione è attiva, dice una cosa
  sola, e la dice nel colore di un avviso.
- Le sezioni hanno perso la numerazione: non sono mai stati passi da seguire in ordine.
- Impostazioni e informazioni sull'applicazione sono passate in alto a destra, con le
  schede di stato subito sotto.

## [0.5.8] - 2026-09-12

### Corretto

- **Riservare il tunnel a una sola applicazione funzionava in una direzione soltanto.**
  Scartava il traffico che gli altri programmi di qui mandavano alla VPN e non faceva nulla
  di quello che ne arrivava: tutti gli altri programmi di questo computer restavano quindi
  raggiungibili dall'altro capo, che è la metà che conta quando non si sa chi ci sia. Ora
  vale in entrambe le direzioni, e la spiegazione lo dice invece di promettere più di quanto
  facesse.

### Modificato

- **L'opzione di instradamento è ora al contrario.** Tenere il resto del computer fuori dal
  tunnel è lo stato sicuro e quello che vogliono quasi tutti, quindi non dovrebbe essere da
  attivare. La casella ora dice *Inviare tutto il traffico attraverso la VPN*, è disattivata
  per impostazione predefinita e spiega cosa comporta attivarla: tutto ciò che questo
  computer invia passa prima dal server VPN, e chi lo gestisce vede tutto. Conta soprattutto
  con un profilo che ti ha dato qualcun altro.

### Aggiunto

- **Aspetto chiaro e scuro**, o come il sistema, applicato subito e ricordato. In Aspetto,
  nelle impostazioni.
- **Icone in tutta l'interfaccia** — su ogni sezione, su Avvia e Arresta e sulle azioni del
  registro — e una spia di stato verde finché una sessione è in corso.

## [0.5.7] - 2026-09-12

### Corretto

- **Uno scaricamento non si poteva annullare.** Il pulsante diceva *Annulla* e non era
  premibile: lo scaricamento veniva eseguito trattenendo il rinvio del clic della finestra
  di dialogo, e una finestra con un rinvio in sospeso disabilita i propri pulsanti, compreso
  l'unico che avrebbe potuto fermarlo. Ora il trasferimento procede accanto alla finestra
  anziché dentro il suo gestore del clic, così il pulsante è attivo esattamente finché c'è
  qualcosa da annullare.
- **Uno scaricamento annullato o fallito lasciava il proprio file parziale**, uno per
  tentativo, per sempre. Ora il file incompleto viene scartato quando il trasferimento non
  arriva in fondo, e uno scaricamento completato ripulisce i programmi di installazione
  precedenti.
- **Premere Avvia senza nulla da avviare non faceva assolutamente niente**: nessun
  messaggio, nessuna riga di registro, nessun cambiamento. Senza profilo, o senza
  un'applicazione scelta, ora dice quale manca invece di sembrare guasto.

### Modificato

- **Installare un aggiornamento mentre una sessione è in corso ora avvisa prima**, e la
  risposta prudente è quella predefinita. Installare ferma il tunnel e disconnette
  l'applicazione di destinazione: non è una cosa da scoprire dopo.

## [0.5.6] - 2026-09-12

### Corretto

- **Una nuova versione viene notata circa un minuto dopo la pubblicazione**, invece che al
  controllo programmato successivo. Chiedere così spesso è sostenibile perché la richiesta è
  condizionale: si rimanda indietro il validatore della risposta precedente e, finché la
  versione non cambia, la risposta è «non modificato» — senza corpo e senza contare per il
  limite di richieste. Solo una versione davvero nuova consuma una richiesta. Resta
  comunque un'interrogazione e non una notifica, quindi un minuto e non un istante, ma non
  c'è nulla da premere né da riavviare.
- **Chiudere l'avviso di aggiornamento non lasciava modo di tornarci.** La chiusura valeva
  per tutta la sessione e solo un riavvio lo riportava. Ora sia la finestra delle
  informazioni sia le impostazioni offrono *Aggiorna ora* finché un aggiornamento è in
  attesa, così chiudere l'avviso chiude soltanto l'avviso.
- **Il pulsante diceva *Arresta* quando non restava più nulla da arrestare.** Quando
  l'applicazione di destinazione esce, la sessione attende fino a venti secondi per vedere
  se un launcher passa il testimone a un altro processo: in quel frattempo ciò per cui la
  sessione esiste è già morto. In quella finestra il pulsante dice *Arresto forzato*, che è
  ciò che fa davvero: chiudere subito la sessione invece di aspettare il passaggio.

### Modificato

- **Tutto ciò che riguarda l'aggiornamento è ora nella finestra delle informazioni
  sull'applicazione**, e il suo pulsante porta un contrassegno finché un aggiornamento è in
  attesa. Il controllo automatico, il controllo immediato, l'ora dell'ultimo controllo e
  l'aggiornamento stesso stanno accanto alla versione con cui si confrontano, invece di
  essere divisi fra lì e le impostazioni.
- **Il programma di installazione scaricato viene conservato se si sceglie *Più tardi*.**
  Prima, non installare subito buttava via lo scaricamento; ora lo stesso pulsante offre
  *Installa ora* finché la versione a cui appartiene non viene superata.

## [0.5.5] - 2026-09-12

### Corretto

- **I controlli automatici degli aggiornamenti erano troppo radi per sembrare automatici.**
  Quattro ore fra l'uno e l'altro volevano dire che in pratica solo un riavvio trovava
  qualcosa, e il meccanismo vero diventava un pulsante nelle impostazioni — e nessuno vuole
  premere un pulsante per sentirsi dire che non c'è niente di nuovo. Ora il controllo
  avviene ogni trenta minuti, e anche quando si porta la finestra in primo piano se
  l'ultimo risale a più di cinque minuti fa. Le impostazioni mostrano quando è avvenuto
  l'ultimo, così si vede che accade.
- **Le due opzioni di confinamento si leggevano come doppioni.** Entrambe erano formulate
  come «limitare il tunnel», senza dire che limitano cose diverse. Ora ogni etichetta nomina
  il proprio asse — *Solo gli indirizzi della VPN passano dal tunnel* contro *Solo
  l'applicazione di destinazione può usare il tunnel* — e ogni spiegazione apre dicendo a
  quale domanda risponde: quali destinazioni, o quale programma.

### Modificato

- **Il pulsante dell'avviso si chiama *Aggiorna ora***, non *Novità*. Quello che fa è
  installare l'aggiornamento; mostrare le note è ciò che fa strada facendo.
- **Le impostazioni possono avviare un aggiornamento**, non solo cercarlo.
- **La striscia vuota in cima alla finestra non c'è più.** Impostazioni e informazioni
  sull'applicazione sono scese accanto alle schede di stato, l'unica cosa che stava lassù.
- **Il conteggio degli annunci inoltrati compare solo con Warcraft III.** È l'unico
  protocollo le cui informazioni di partita vanno chieste e inoltrate; con gli altri il
  contatore resterebbe a zero per sempre, il che si legge come un guasto e non come «non
  applicabile».

## [0.5.4] - 2026-09-12

### Corretto

- **La finestra di aggiornamento mostrava le note come sorgente Markdown** — cancelletti,
  asterischi e apici inversi — invece di formattarle, rendendo faticoso leggere proprio ciò
  che è scritto per essere letto. Titoli, elenchi, enfasi e codice in linea ora sono
  formattati.
- **L'aggiornamento non chiudeva prima l'applicazione.** Il programma di installazione
  partiva mentre il tunnel e il processo ausiliario con privilegi tenevano ancora i file
  che stava per sostituire. Ora la sessione viene chiusa e questo processo termina prima
  che l'installazione parta, e il programma di installazione chiude un'istanza rimasta
  invece di lasciare che trasformi un aggiornamento in una richiesta di riavvio.
- **La finestra e la barra delle applicazioni mantenevano un'icona generica** mentre l'area
  di notifica e Installazione applicazioni mostravano quella vera. Una finestra non
  pacchettizzata non prende l'icona dall'eseguibile da sola.
- **L'etichetta cinese di «Tenere il resto del computer fuori dal tunnel» descriveva
  l'impostazione sbagliata.** Si leggeva come «tenere fuori dal tunnel il traffico delle
  altre applicazioni», che è ciò che fa il confinamento per applicazione, e faceva sembrare
  le due opzioni dei doppioni. Sono ortogonali: una limita quali destinazioni usano il
  tunnel, l'altra quale processo può usarlo.

### Modificato

- **Il nome e il sottotitolo non occupano più la parte alta della finestra.** La barra del
  titolo dice già di cosa si tratta, e i dettagli sono passati nella finestra delle
  informazioni.
- **La finestra delle informazioni non ripete più il nome che le fa da titolo** e lascia
  l'apertura della cartella dei log al pannello del registro, dove quel pulsante già stava.
  La versione, che è il motivo per cui la si apre, è ora abbastanza grande da leggersi al
  volo.

## [0.5.3] - 2026-09-12

### Corretto

- **Una partita ospitata su una macchina si vedeva dall'altra ma non si riusciva a
  entrarci.** In entrata veniva aperta solo la porta di individuazione, che non è
  necessariamente quella su cui l'host resta in ascolto: Warcraft III prende la 6112 se può
  e sale fino alla 6119 se non può, annunciando quella che ha ottenuto. Un host spinto via
  dalla 6112 risultava quindi visibile e irraggiungibile, e solo in quella direzione, il che
  faceva sembrare che il problema fosse di una delle due macchine. Ora viene aperto l'intero
  intervallo di hosting, sempre e solo verso la sottorete della VPN.
- **Una partita restava nell'elenco dell'altro giocatore dopo che l'host l'aveva
  lasciata.** Warcraft III annuncia la chiusura in broadcast, e un broadcast può uscire
  dalla scheda della VPN, dove il relè deliberatamente non ascolta: l'annuncio non veniva
  mai raccolto e l'altro capo continuava a proporre una partita che non esisteva più. Ora
  il relè si accorge che l'host ha smesso di rispondere alle sue sonde e la ritira da sé,
  usando l'ultimo annuncio inoltrato per dire di quale si trattava.
- **Cambiare lingua svuotava gli elenchi dell'applicazione di destinazione e
  dell'individuazione in rete**, e scegliere *Predefinito di sistema* svuotava l'elenco
  delle lingue stesso. Ritradurre un elenco significa sostituire le voci che contiene, e un
  elenco a discesa interpreta la sostituzione della voce selezionata come la sua scomparsa:
  azzerava la selezione, e l'associazione riscriveva quel vuoto sopra la scelta. Ora le
  voci mantengono la propria identità e cambia soltanto il loro testo, quindi non resta
  nulla da azzerare. Due tentativi precedenti rimettevano a posto la selezione dopo il
  fatto; questo elimina la causa.
- **La finestra delle impostazioni manteneva la lingua precedente nel proprio titolo e
  pulsante** quando la lingua veniva cambiata dall'interno. Tutto il contenuto della
  finestra veniva rietichettato, ma il titolo e il pulsante di chiusura non ne fanno parte.
- **Il registro attività non seguiva le righe nuove in modo affidabile.** Scorreva prima
  che la riga nuova fosse disposta, finendo dove prima stava la fine e restando sempre una
  riga indietro. Ora scorre dopo la disposizione e smette di seguire appena si sale a
  leggere qualcosa, riprendendo quando si torna in fondo.
- **L'aggiornamento riscriveva tutti i file, modificati o no.** La versione precedente
  veniva rimossa per intero prima che fosse scritto un solo file nuovo, così ogni
  aggiornamento riscriveva l'intera installazione. Ora la versione nuova viene scritta per
  prima e quella precedente rimossa dopo: il programma di installazione salta i file
  identici e resta da scrivere soltanto ciò che è davvero cambiato.
- **Uno dei pulsanti del registro era disposto e cliccabile, ma non veniva mai disegnato.**
  *Apri la cartella dei log* occupava il suo spazio e rispondeva ai clic senza mostrare
  nulla. Le azioni del registro stanno ora su una sola fila orizzontale invece che una
  colonna ciascuna, eliminando la disposizione per colonne che non funzionava.

### Aggiunto

- **Un pulsante con le informazioni sull'applicazione** accanto a quello delle
  impostazioni: quale versione è in esecuzione, il copyright e un collegamento alle sue
  note e ai download.
- **Una casella *Avvia LanBridge* nell'ultima pagina del programma di installazione**,
  selezionata per impostazione predefinita. Avvia l'applicazione senza elevazione, che è
  come LanBridge deve funzionare: il consenso viene chiesto all'avvio di una sessione, non
  prima.

### Modificato

- **Chiudere l'applicazione di destinazione termina la sessione solo quando il tunnel è
  legato a essa.** Con *Solo questa applicazione può usare la VPN* attivo, il tunnel esiste
  per quel processo e cade con lui: altrimenti resterebbe un tunnel che nulla sulla macchina
  ha il permesso di usare. Senza quell'opzione il tunnel è delimitato solo per destinazione
  e potrebbe ancora trasportare traffico di altro, quindi resta attivo finché non lo fermi.

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
