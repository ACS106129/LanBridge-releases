# Registro delle modifiche

Tutte le modifiche rilevanti di LanBridge sono registrate qui.

Il formato segue [Keep a Changelog](https://keepachangelog.com/it/1.1.0/) e le versioni
seguono il [versionamento semantico](https://semver.org/lang/it/).

## [0.5.27] - 2026-09-19

### Corretto

- **L'applicazione di destinazione non raggiunge più nulla se non attraverso il tunnel**: il primo pacchetto di una connessione verso un indirizzo che nessuna rotta copre viene scartato anziché partire con l'indirizzo reale di questa macchina, ed era questo a causare i 403 ripetuti e la schermata di caricamento bloccata.
- **Scartare quel pacchetto è ciò che aggiunge la rotta**, così la connessione riesce alla prima ritrasmissione invece di fallire.
- **Un pacchetto IPv6 bloccato non viene più registrato come fuga**; ora il registro dice che è stato scartato perché il client ripieghi su IPv4, che era l'intento.
- **Un tunnel che si riconnette con un altro indirizzo viene seguito**, e se il nuovo cade fuori dalla sottorete attorno a cui è stato costruito il filtro dei pacchetti, il rifiuto si ferma e lo dichiara, invece di trasformare in rifiuto ogni pacchetto che la destinazione invia.

### Modificato

- **Le rotte tornano a uscire dal tunnel**: un indirizzo viene rilasciato quando nessun nome seguito lo restituisce più e la destinazione non ha connessioni aperte verso di esso, invece di far crescere l'insieme per tutta la sessione.
- **Una rotta adottata scade dopo cinque minuti di inattività** e restituisce il proprio posto nel limite della sessione.

## [0.5.26] - 2026-09-18

### Modificato

- **L'elenco dei siti non deve più essere corretto**: ora è solo un avvio a caldo, e tutto ciò che la destinazione raggiunge senza il tunnel ottiene una rotta propria, tranne il server VPN, le reti di questa macchina, il broadcast e IPv6.
- **Il riepilogo finale non chiama più mancata una destinazione dopo averla instradata**, perché interroga di nuovo il sistema anziché rileggere le rotte aggiunte.

## [0.5.25] - 2026-09-17

### Corretto

- **I siti passavano dal tunnel solo quando i due lati concordavano per caso sull'indirizzo**: diciotto nomi su ventisette rispondevano in modo diverso, veniva instradata solo la risposta del tunnel e l'applicazione usava quella locale; ora si instradano entrambe.
- **I siti si modificano uno per riga, in una finestra dedicata**, invece di un'unica casella con ventisette nomi.
- **Un download interrotto dal server riprende da dove si era fermato** anziché essere buttato, richiedendo ogni parte fino a cinque volte.
- **La 0.5.24 lo attribuì a un timeout e sbagliava**: il guasto durava da trenta minuti, quindi un limite di quindici non può averlo concluso.

## [0.5.24] - 2026-09-17

### Corretto

- **Un download lento veniva buttato poco prima di finire**, perché il limite di quindici minuti copriva la lettura del file e non solo il raggiungimento del server; ora non c'è più un limite complessivo.
- **Gli aggiornamenti scaricano circa tre volte più in fretta**, perché l'host di pubblicazione limita ogni connessione e non la linea: l'installer viene preso in quattro parti insieme.
- **L'avanzamento è segnalato ogni 512 KB anziché ogni 80 KB**, un ritmo che la finestra può sfruttare.

## [0.5.23] - 2026-09-16

### Aggiunto

- **La sessione annota dove la destinazione è andata davvero e quanto di ciò ha mancato il tunnel**, così un'esecuzione nomina ciò che manca invece di lasciare l'elenco dei nomi a un'ipotesi.
- **Gli indirizzi sono riportati con il nome a cui rispondono**, perché il nome di un nodo di una rete di distribuzione porta con sé il luogo, e il luogo è tutta la questione.

### Corretto

- **I nomi configurati sono seguiti per tutta la sessione**, invece di essere fissati una volta all'avvio, dato che rispondono con un TTL di sessanta secondi e una sessione dura ore.
- **La ricerca di un resolver funzionante si ferma appena uno risponde**, senza andare in timeout nome per nome e costare minuti prima di partire.

## [0.5.22] - 2026-09-16

### Aggiunto

- **L'indirizzo di uscita viene misurato prima e dopo aver posato le rotte, e entrambe le risposte finiscono nel registro**, perché ogni verifica precedente era un passo verso quello e non quello stesso — e «non determinabile» è scritto così com'è.

### Corretto

- **Il registro conserva finalmente la metà di sessione per cui servirebbe**: i passaggi della finestra, ogni messaggio del filtro dei pacchetti e tutto ciò che ha detto OpenVPN finiscono nel file.
- **Una rotta non viene più respinta un istante prima di funzionare**; alla tabella di routing si lasciano alcune centinaia di millisecondi per assestarsi.
- **Il filtro dei pacchetti annota il filtro con cui è stato aperto**, così una clausola assente si distingue da una che non ha mai corrisposto.

## [0.5.21] - 2026-09-14

### Corretto

- **I siti mandati nella VPN non ci passavano mentre tutto diceva di sì**: il salto successivo era indovinato come .1 della rete, che su una /30 non esiste; ora si ricava dall'indirizzo che il tunnel ha davvero ottenuto e ogni rotta è verificata come quella che il sistema userebbe realmente.
- **Un nome non risolvibile attraverso il tunnel veniva riportato come concorde con la risposta locale**; nessuna risposta non è la stessa risposta, e il registro indica quale resolver e quale trasporto.
- **L'interrogazione attraverso il tunnel ripiega su TCP**, perché un relay che porta l'uno e non l'altro è comune tra i server gestiti da volontari.

## [0.5.20] - 2026-09-13

### Aggiunto

- **Siti che potete mandare nella VPN senza mandarci l'intera macchina**: i siti indicati vengono risolti attraverso il tunnel e instradati attraverso di esso, e durante la sessione li raggiunge solo l'applicazione di destinazione.

## [0.5.19] - 2026-09-13

### Aggiunto

- **Un posto per nome utente e password**, per i server che chiedono l'accesso; la finestra dice apertamente che openvpn può leggerla solo da un file, quindi resta in chiaro nella cartella propria di quel profilo.

### Corretto

- **La destinazione raggiungeva Internet via IPv6, aggirando del tutto il tunnel**: una fuga in ogni modalità, poiché un tunnel che porta IPv4 non può portare ciò che la macchina invia su IPv6; ora l'IPv6 della destinazione viene scartato.
- **«Nessun aggiornamento» quando non era stato chiesto nulla**: a quota esaurita riportava la versione già presente, e il validatore che rende gratuita la verifica ora si conserva tra le esecuzioni.

## [0.5.18] - 2026-09-13

### Corretto

- **La finestra Informazioni ringraziava WinDivert senza dire a quali condizioni è usato**, e ora nomina la LGPL v3, la copia distribuita accanto al programma e dove sono i sorgenti.
- **I posti liberi di una stanza di Warcraft III non cambiavano mai sull'altra macchina**, perché l'annuncio che l'altro capo legge è broadcast e non si può catturare; ora è ricavato dall'inserzione e controllato prima di essere inviato.
- **Il download dell'aggiornamento continuava a tenere la finestra**: la 0.5.16 lo dava per risolto mentre nulla richiamava quel codice; ora gira davvero in secondo piano, con **Continua in secondo piano** nella finestra di dialogo e l'annullamento in quella principale.

## [0.5.17] - 2026-09-13

### Corretto

- **Se un giocatore usciva dalla lobby di Warcraft III, nessuno riusciva più a entrare**, perché il listener e ogni connessione accettata condividevano una sola voce di porta e la prima chiusura se la portava via; ora ogni socket è seguito a parte.
- **L'applicazione di destinazione girava ancora come amministratore**: la via della 0.5.16 richiedeva un privilegio che un helper elevato non può avere, quindi si usa quello che ha e il registro dice quale.

## [0.5.16] - 2026-09-13

### Aggiunto

- **Un installer in ogni lingua parlata dall'applicazione**, ciascuno con la codepage ANSI della propria cultura, e l'aggiornamento propone quello corrispondente alla lingua della finestra.
- **La finestra si apre dove l'avete lasciata**, a meno che quella posizione non cada più su uno schermo.
- **Una nuova icona**: una freccia che esce dall'apertura di un anello, disegnata separatamente a ogni dimensione anziché ridotta da un'immagine grande.

### Corretto

- **Avvia stava sotto la piega**; i pulsanti ora sono fissati sotto le schede, che scorrono dietro di essi.
- **L'applicazione di destinazione girava come amministratore**, così un accesso dal browser non poteva mai consegnarle il codice di autorizzazione; ora parte con il token della shell.
- **Scaricare un aggiornamento teneva in ostaggio l'intera finestra**, e ora gira in secondo piano con barra di avanzamento e un pulsante di annullamento che funziona.

## [0.5.15] - 2026-09-13

### Corretto

- **Un solo rifiuto del server chiudeva il tentativo**, corretto per un server proprio e sbagliato per un relay pubblico; ora riprova tre volte prima di segnalare.
- **«EXITING auth-failure» non spiegava nulla** e ora dice quale passaggio è fallito e cosa significa per il tipo di server che usate.

## [0.5.14] - 2026-09-12

### Aggiunto

- **Un profilo importato ora appartiene all'applicazione**: il .ovpn e ogni certificato e chiave a cui rimanda vengono copiati in una cartella propria, quindi cancellare l'originale non cambia nulla.
- **Un posto per vedere cosa è conservato**, con rinomina, eliminazione e un accesso alla cartella, con la conferma nella riga stessa.
- **OpenVPN, se non lo avete**, prelevato dall'host di download di OpenVPN e rifiutando tutto ciò di cui Windows non si fida o che OpenVPN non ha firmato.
- **Test per l'installer**, che ne percorrono le pagine nelle due lingue e annullano al riepilogo, così eseguire la suite non installa nulla.
- **Un test che guarda i pixel**, misurando ogni riga esplicativa di Impostazioni e Informazioni contro il proprio sfondo, in entrambi i temi.

### Corretto

- **Tre righe in Informazioni erano invisibili**, perché un pennello preso dalle risorse dell'applicazione si risolve con il tema dell'applicazione stessa, che un'app WinUI non pacchettizzata non può cambiare dopo l'avvio.
- **Il controllo aggiornamenti aveva smesso di chiedere con garbo**: ora legge quando torna la quota e aspetta, e lo dice una volta invece di sessanta.
- **L'installer scriveva sopra la propria grafica**, che è lo sfondo su cui la finestra scrive e non un'immagine accanto al testo.
- **L'aggiornamento dava a tutti l'installer inglese**, e ora chiede quello corrispondente alla lingua della finestra.

### Modificato

- Sotto ogni impostazione una riga dice cosa cambia e dove sono conservate le impostazioni.
- Informazioni indica ogni quanto si cercano gli aggiornamenti e ringrazia il client della community di OpenVPN e WinDivert.

## [0.5.13] - 2026-09-12

### Aggiunto

- **Un pianoforte a coda**: i suoni dei controlli passano dal sintetizzatore General MIDI già presente in Windows, su una scala pentatonica perché due note qualsiasi si accordino, con una casella per zittirlo.
- **Movimento dove è successo qualcosa**, e non ovunque.
- **Il diagramma riferisce invece di mimare**: fermo senza sessione, e la cella dei bloccati mostra i pacchetti che la guardia ha davvero scartato.
- **Un installer che somiglia a questo prodotto**, con grafica generata anziché i segnaposto di WiX.
- **Un installer nella vostra lingua**, un MSI per lingua, per cominciare inglese e cinese tradizionale.

### Modificato

- L'avviso sotto il confinamento per processo compare ora quando la casella è **vuota**, lo stato su cui vale la pena avvertire.

## [0.5.12] - 2026-09-12

### Corretto

- **La cartella di installazione conteneva ottantotto cartelle di traduzioni per lingue che questa applicazione non offre**, incluse come risorse Win32 che la consueta impostazione di sfoltimento non raggiunge; ora ne restano quindici.

### Modificato

- L'opzione per processo si chiama *Solo l'applicazione di destinazione può parlare con la VPN*, che è ciò che fa; non ha mai governato l'intero tunnel.

### Aggiunto

- **Altri dieci controlli che pilotano la finestra vera**, sulle finestre di dialogo e sulle scelte al loro interno — e scriverli ha trovato quattro test che cercavano qualcosa mai esistito.

## [0.5.11] - 2026-09-12

### Corretto

- **La modalità scura era testo bianco su pagina bianca**, perché il tema era applicato a un elemento interno a quello che dipinge lo sfondo.
- **L'icona della sezione destinazione appariva come un quadrato vuoto**, poiché un code point presente nella mappa di un font non significa che esista un glifo.
- **I controlli di ogni sezione erano centrati** in una scheda che aveva già la larghezza giusta.
- **Scegliere di agganciarsi e premere avvia senza scegliere un programma sollevava un'eccezione**, e ora viene detto quale scelta manca.

### Aggiunto

- **Test che pilotano la finestra vera**: diciannove controlli aprono la build pubblicata e guardano, perché ogni difetto visivo segnalato finora poteva superare tutti i test unitari del progetto.

## [0.5.10] - 2026-09-12

### Corretto

- **La riga di avviso allargava la colonna invece di andare a capo**, perché una pila orizzontale misura i figli con larghezza illimitata.
- **Impostazioni e informazioni sull'applicazione comparivano due volte**, con la vecchia coppia rimasta al suo posto quando sono passate in alto a destra.
- **Il selettore dei processi offriva questa stessa applicazione.**

### Modificato

- **Il diagramma riempie lo spazio che gli viene dato**, invece di restare a larghezza fissa nell'angolo di una scheda grande e vuota.
- **La nota sotto il confinamento per processo compare solo mentre è attivo**, e dice cosa fa.
- Il percorso della destinazione si vede per intero al passaggio del mouse, per quanto stretta sia la casella.

## [0.5.9] - 2026-09-12

### Corretto

- **La modalità scura era inutilizzabile**, perché la pagina non dipingeva mai uno sfondo proprio e alle finestre di dialogo il tema andava indicato a parte.

### Aggiunto

- **I due interruttori di confinamento ora sono un'immagine**, una griglia di chi invia contro dove, che risponde a colpo d'occhio a ciò che due paragrafi non riuscivano a dire.
- **L'aggancio si sceglie da un elenco di programmi in esecuzione**, invece di chiedere un identificatore di processo copiato da altrove.

### Modificato

- L'avviso di routing compare solo mentre quell'opzione è attiva, dice una cosa e la dice nel colore di un avviso.
- Le sezioni hanno perso la numerazione; non sono mai stati passaggi da seguire in ordine.
- Impostazioni e informazioni sono passate in alto a destra, con le schede di stato sotto.

## [0.5.8] - 2026-09-12

### Corretto

- **Confinare il tunnel a una sola applicazione funzionava solo in una direzione**, lasciando ogni altro programma qui raggiungibile dall'altro capo, che è la metà che conta quando non sapete chi ci sia.

### Modificato

- **L'opzione di routing è ora al contrario**: *Invia tutto il traffico attraverso la VPN*, disattivata per impostazione predefinita, dicendo cosa significa attivarla per chi gestisce quel server.

### Aggiunto

- **Aspetto chiaro e scuro**, o seguendo il sistema, applicato subito e ricordato.
- **Icone in tutta l'interfaccia** e una spia di stato verde mentre una sessione è in corso.

## [0.5.7] - 2026-09-12

### Corretto

- **Un download non si poteva annullare**, perché girava tenendo il differimento del clic della finestra, che le fa disattivare i propri pulsanti.
- **Un download annullato o fallito lasciava il file parziale**, uno per tentativo, per sempre.
- **Premere Avvia senza nulla da avviare non faceva proprio nulla**, e ora viene detto quale scelta manca.

### Modificato

- **Installare un aggiornamento con una sessione in corso avverte prima**, con la risposta sicura come predefinita, poiché l'installazione disconnette l'applicazione di destinazione.

## [0.5.6] - 2026-09-12

### Corretto

- **Una nuova versione viene notata circa un minuto dopo la pubblicazione**, con una richiesta condizionale che non costa nulla finché nulla è cambiato.
- **Chiudere l'avviso di aggiornamento non lasciava modo di tornarci**; impostazioni e finestra informazioni offrono ora *Aggiorna ora* finché uno è in attesa.
- **Interrompi diceva *Interrompi* quando non c'era più nulla da interrompere**, e dice *Forza interruzione* durante l'attesa di un passaggio di consegne.

### Modificato

- **Tutto ciò che riguarda l'aggiornamento sta nella finestra informazioni**, accanto alla versione con cui si confronta.
- **Un installer scaricato viene conservato se scegliete *Più tardi***, finché la sua versione non è superata.

## [0.5.5] - 2026-09-12

### Corretto

- **I controlli automatici erano troppo rari per sembrare automatici**: ora ogni trenta minuti, e anche quando la finestra torna in primo piano se l'ultimo risale a più di cinque minuti.
- **Le due opzioni di confinamento si leggevano come doppioni**, e ogni etichetta ora nomina il proprio asse: quali destinazioni, o quale programma.

### Modificato

- **Il pulsante del banner si chiama *Aggiorna ora*** anziché *Novità*, perché installare è ciò che fa.
- **Anche dalle impostazioni si può avviare un aggiornamento**, non solo cercarlo.
- **La striscia vuota in cima alla finestra è sparita.**
- **Il conteggio degli annunci inoltrati si mostra solo per Warcraft III**, l'unico protocollo a cui si applica.

## [0.5.4] - 2026-09-12

### Corretto

- **La finestra di aggiornamento mostrava le note come sorgente Markdown** invece di formattarle, rendendo faticoso da leggere ciò che era scritto per essere letto.
- **L'aggiornamento non chiudeva prima l'applicazione**, così l'installer sostituiva file ancora tenuti dal tunnel e dall'helper.
- **Finestra e barra delle applicazioni conservavano un'icona generica**, perché una finestra non pacchettizzata non prende da sola l'icona dall'eseguibile.
- **L'etichetta cinese di «tenere il resto della macchina fuori dal tunnel» descriveva l'altra impostazione**, facendo sembrare doppioni due opzioni ortogonali.

### Modificato

- **Nome e slogan non occupano più la parte alta della finestra**; la barra del titolo dice già cos'è.
- **La finestra informazioni non ripete più il nome del proprio titolo**, e mostra la versione abbastanza grande da leggersi a colpo d'occhio.

## [0.5.3] - 2026-09-12

### Corretto

- **Una partita ospitata su una macchina si vedeva ma non si poteva raggiungere dall'altra**, perché era aperta solo la porta di individuazione mentre Warcraft III sale fino a 6119 se 6112 è occupato; ora è aperto l'intero intervallo, sempre solo verso la sottorete VPN.
- **Una stanza restava nell'elenco dell'altro giocatore dopo che l'host l'aveva lasciata**, perché l'annuncio di chiusura è broadcast e non arriva mai; il relay si accorge che l'host non risponde più e la ritira da sé.
- **Cambiare lingua svuotava gli elenchi dell'applicazione di destinazione e dell'individuazione LAN**, perché sostituire la voce selezionata si legge come la sua sparizione; ora le voci mantengono la propria identità e cambia solo il testo.
- **La finestra delle impostazioni manteneva la vecchia lingua nel titolo e nel pulsante**, che non fanno parte del contenuto rietichettato.
- **Il registro attività non seguiva in modo affidabile le nuove righe**, perché scorreva prima che la nuova riga fosse disposta.
- **Un aggiornamento riscriveva ogni file, modificato o no**, perché la vecchia versione veniva rimossa per intero prima di scrivere un solo file nuovo.
- **Uno dei pulsanti del registro era disposto e cliccabile ma non veniva mai disegnato.**

### Aggiunto

- **Un pulsante di informazioni sull'applicazione** accanto a quello delle impostazioni, con versione, copyright e un collegamento alle note di quella versione.
- **Una casella *Avvia LanBridge* nell'ultima pagina dell'installer**, che lo avvia senza elevazione, come è pensato per funzionare.

### Modificato

- **Chiudere l'applicazione di destinazione termina la sessione solo quando il tunnel è legato a essa**, perché altrimenti potrebbe ancora trasportare traffico per qualcos'altro.

## [0.5.2] - 2026-09-11

### Corretto

- **Le note di versione mostravano solo il primo titolo**, perché il controllo spezza le righe su un ritorno a capo che le note normalizzate non contenevano più.

## [0.5.1] - 2026-09-11

### Corretto

- **Le partite si vedevano ma non si potevano raggiungere, o non comparivano affatto**: tutto ciò che rende raggiungibile una partita arriva in entrata e Windows lo blocca per impostazione predefinita, quindi una sessione apre la porta di individuazione solo per la sottorete VPN e ripristina tutto alla fine.
- **L'interfaccia si disfaceva a ogni dimensione di testo superiore a quella predefinita, e riavviare non la ripristinava mai**, perché il livello scalato veniva prima centrato e poi ingrandito dal proprio angolo in alto a sinistra.
- **Scaricare un aggiornamento non mostrava alcun avanzamento**, indistinguibile da un download mai iniziato.
- **Riattivare i controlli automatici non faceva nulla per un massimo di quattro ore.**
- **La scelta sulla chiusura sembrava scartata** se la lingua veniva cambiata nella stessa visita alle impostazioni.

### Aggiunto

- **I controlli proseguono mentre l'applicazione è nell'area di notifica**, annunciati con un fumetto e conservati nel suggerimento dell'icona.
- **Un pulsante *Controlla ora* nelle impostazioni**, per quando aspettare il prossimo controllo non è il punto.

## [0.5.0] - 2026-09-11

### Corretto

- **La VPN cadeva appena si apriva una stanza**, perché la ricerca del processo a cui un launcher passa il testimone confrontava solo eseguibili con lo stesso nome.
- **Interrompi non faceva nulla a sessione già conclusa**, perché chiudere la pipe verso un helper sparito sollevava un'eccezione che sfuggiva alla pulizia.
- **Cambiare lingua svuotava ogni elenco a discesa** invece di tradurlo.
- **Il registro attività non seguiva le nuove righe**, e ora smette di seguirle appena si scorre verso l'alto.

### Aggiunto

- **Note di versione nella vostra lingua**, pubblicate accanto alle build e mostrate secondo la lingua dell'interfaccia.
- **Esporta rapporto di errore**, che unisce l'attività a schermo ai registri di entrambi i processi.
- L'impostazione della dimensione del testo ora scala l'intera interfaccia, non solo il registro.

## [0.4.0] - 2026-09-10

### Aggiunto

- **Controllo automatico degli aggiornamenti**, che mostra cosa è cambiato prima di installare; attivo per impostazione predefinita.
- **Finestra delle impostazioni** dietro il pulsante a ingranaggio, con lingua, dimensione del carattere, comportamento alla chiusura e controllo aggiornamenti.
- **Altre dieci lingue dell'interfaccia**, accanto a inglese e cinese tradizionale.
- **Il registro attività è testo selezionabile**, con *Copia tutto* ed *Esporta…*.
- **Dimensione del carattere regolabile** (10–22 pt), ricordata tra le esecuzioni.
- **Icona dell'applicazione**, usata da finestra, barra delle applicazioni, area di notifica ed elenco programmi.
- **L'installer chiede dove installare** e offre i due collegamenti come scelte indipendenti.

### Corretto

- **La VPN cadeva appena il gioco finiva di caricare**, perché l'uscita del primo processo del launcher era letta come chiusura della destinazione; ora la sessione segue il passaggio.
- **L'icona nell'area di notifica non compariva mai**, perché il suo handle veniva distrutto prima che Windows lo usasse.
- **Riaprire dall'area di notifica avviava una seconda copia** invece di ripristinare quella in esecuzione.
- **`WinDivert64.sys` restava bloccato dopo la chiusura**, perché aprire un handle del driver registra un servizio kernel che continua a girare.
- **Riportare la lingua a «Predefinita di sistema» non faceva nulla**, perché un'unica notifica «è cambiato tutto» non viene gestita in modo affidabile.
- **La disinstallazione chiedeva un riavvio**, dato che applicazione e helper tenevano ancora dei file.
- Le righe del registro avevano un riempimento da elemento di elenco che lasciava mezza riga vuota tra le voci.

### Modificato

- Le azioni del registro sono passate nel pannello attività a destra, invece che in fondo alla colonna di configurazione.

## [0.3.0] - 2026-09-09

### Aggiunto

- Registrazione degli errori in `%LOCALAPPDATA%\LanBridge\logs\`, un file per processo e per esecuzione, con le eccezioni non gestite annotate invece di terminare in silenzio.
- Persistenza delle impostazioni a ogni modifica, così sopravvivono a un arresto anomalo.
- Supporto dell'area di notifica con una domanda alla chiusura.
- Interfaccia in cinese tradizionale accanto all'inglese.
- Installer MSI con collegamenti, metadati di versione e voce nell'elenco programmi.

### Corretto

- La build pubblicata si avviava e moriva nel runtime XAML, perché un'app WinUI non pacchettizzata non porta con sé il markup compilato.

## [0.2.0] - 2026-09-09

### Corretto

- **La finestra non compariva mai dopo la richiesta di elevazione**, perché WinUI 3 non può girare elevata; l'interfaccia gira senza elevazione e affida il lavoro privilegiato a un helper che chiede il consenso una volta per sessione.

## [0.1.0] - 2026-09-09

### Aggiunto

- VPN per applicazione che rifiuta la rotta predefinita e il DNS spinti, così solo la sottorete VPN attraversa il tunnel.
- Confinamento facoltativo per processo con WinDivert, che scarta il traffico verso la sottorete VPN di qualunque processo diverso dalla destinazione.
- Relay di individuazione LAN per tunnel che non trasportano broadcast, incluso il protocollo W3GS di Warcraft III, le cui informazioni di partita sono inviate solo come risposta unicast.
- Relay generico di broadcast UDP per altri giochi, configurato per porta.
- Versione a riga di comando dello stesso motore, senza interfaccia.

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
