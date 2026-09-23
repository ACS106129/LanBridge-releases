# Journal des modifications

Toutes les modifications notables de LanBridge sont consignées ici.

Le format suit [Keep a Changelog](https://keepachangelog.com/fr/1.1.0/) et les versions
suivent le [versionnage sémantique](https://semver.org/lang/fr/).

## [0.5.31] - 2026-09-23

### Ajouté

- Une liste de sites DMM prête à l'emploi.

### Modifié

- Seule la connexion passe par le tunnel ; le jeu n'est plus ralenti.
- La session ne poursuit plus l'application dans un autre processus.
- Si l'application tourne déjà, c'est elle qui sert, pas une seconde copie.

### Corrigé

- Le tunnel ne meurt plus en silence après une reconnexion.
- L'adresse de connexion n'est plus perdue entre deux résolutions.
- Une coupure brève ne relance plus le tunnel.

## [0.5.30] - 2026-09-20

### Corrigé

- Une vérification refusée ne s’affiche plus comme à jour.
- Les profils inutilisés peuvent être supprimés pendant la connexion.
- Le mode attente ne dit plus qu’il lance l’application.
- Les sites non listés ne quittent plus le tunnel.
- Les lignes répétées à intervalle régulier restent dans le journal.

## [0.5.29] - 2026-09-19

### Ajouté

- Télécharger un VPN depuis VPN Gate.
- Modifier la liste des sites en session.

### Modifié

- Seuls les sites de la cible sont tunnelisés.
- La mise à jour n'embarque qu'un runtime.

### Corrigé

- Le suivi survit à une reconnexion openvpn.
- Les vérifications de mise à jour ne sont plus refusées.

## [0.5.28] - 2026-09-19

### Corrigé

- **La mise à jour est plus légère de 38,8 Mo** : la pile d'apprentissage automatique du Windows App SDK, onnxruntime et DirectML, voyageait dans un VPN par application qui ne l'appelle jamais.
- **Une publication n'est plus visible tant que tous ses installateurs ne sont pas joints**, ce qui explique que la 0.5.27 ait proposé un installateur allemand à une interface chinoise : elle a été publiée avec un seul fichier envoyé et le reste en cours, et l'application repère une nouvelle version en une minute environ.

## [0.5.27] - 2026-09-19

### Corrigé

- **L'application cible n'atteint plus rien en dehors du tunnel** : le premier paquet d'une connexion vers une adresse qu'aucune route ne couvre est rejeté au lieu de partir avec l'adresse réelle de cette machine, ce qui causait les 403 répétés et l'écran de chargement bloqué.
- **Rejeter ce paquet est ce qui ajoute la route**, de sorte que la connexion aboutit dès sa première retransmission au lieu d'échouer.
- **Un paquet IPv6 bloqué n'est plus compté ni consigné comme une fuite**, ni dans la ligne du moment ni dans le verdict final, qui décrivait une exécution mesurée par « 22 destinations sur 73 ne sont pas passées par le tunnel » alors que les 51 en IPv4 y étaient passées et que les 22 étaient le garde faisant son travail.
- **Un tunnel qui se reconnecte sur une autre adresse est suivi**, et si la nouvelle sort du sous-réseau autour duquel le filtre de paquets a été construit, le refus cesse et le dit, au lieu de transformer en refus chaque paquet que la cible envoie.

### Modifié

- **Les routes ressortent à nouveau du tunnel** : une adresse est libérée dès qu'aucun nom suivi ne la renvoie et que la cible n'a plus de connexion ouverte vers elle, au lieu de voir l'ensemble grossir toute la session.
- **Une route adoptée expire après cinq minutes d'inactivité** et rend sa place dans la limite de la session.

## [0.5.26] - 2026-09-18

### Modifié

- **La liste des sites n'a plus besoin d'être juste** : ce n'est plus qu'un démarrage à chaud, et tout ce que la cible atteint hors du tunnel reçoit sa propre route, sauf le serveur VPN, les réseaux propres de cette machine, la diffusion et IPv6.
- **Le récapitulatif de fin n'appelle plus manquée une destination qui a été routée**, parce qu'il interroge de nouveau le système au lieu de relire les routes ajoutées.

## [0.5.25] - 2026-09-17

### Corrigé

- **Les sites ne passaient par le tunnel que lorsque les deux côtés donnaient par hasard la même adresse** : dix-huit noms sur vingt-sept répondaient différemment, seule la réponse du tunnel était routée et l'application utilisait la locale ; les deux sont routées désormais.
- **Les sites s'éditent un par ligne, dans une fenêtre dédiée**, au lieu d'une seule zone contenant vingt-sept noms.
- **Un téléchargement interrompu par le serveur reprend là où il s'est arrêté** au lieu d'être jeté, chaque partie étant redemandée jusqu'à cinq fois.
- **0.5.24 a mis cela sur le compte d'un délai d'attente et se trompait** : l'échec durait depuis trente minutes, une limite de quinze ne peut pas l'avoir terminé.

## [0.5.24] - 2026-09-17

### Corrigé

- **Un téléchargement lent était jeté juste avant la fin**, parce que la limite de quinze minutes couvrait la lecture du fichier et pas seulement l'accès au serveur ; il n'y a plus de limite globale.
- **Les mises à jour se téléchargent environ trois fois plus vite**, car l'hôte de publication limite chaque connexion et non la ligne : l'installateur est récupéré en quatre parties simultanées.
- **La progression est signalée tous les 512 Ko au lieu de tous les 80 Ko**, un rythme que la fenêtre peut exploiter.

## [0.5.23] - 2026-09-16

### Ajouté

- **La session note où la cible est réellement allée et ce qui a manqué le tunnel**, si bien qu'une exécution nomme ce qui manque au lieu de laisser la liste de noms à l'état de conjecture.
- **Les adresses sont signalées avec le nom auquel elles répondent**, car le nom de nœud d'un réseau de diffusion porte le lieu, et le lieu est toute la question.

### Corrigé

- **Les noms configurés sont suivis pendant toute la session**, au lieu d'être figés une fois au départ, puisqu'ils répondent avec un TTL de soixante secondes et qu'une session dure des heures.
- **La recherche d'un résolveur qui fonctionne s'arrête dès qu'un a répondu**, au lieu d'expirer nom par nom et de coûter des minutes avant le démarrage.

## [0.5.22] - 2026-09-16

### Ajouté

- **L'adresse de sortie est mesurée avant et après la pose des routes, et les deux réponses vont au journal**, car toutes les vérifications précédentes étaient un pas vers cela et non cela même — et « impossible à déterminer » est écrit tel quel.

### Corrigé

- **Le journal conserve enfin la moitié de la session pour laquelle on en aurait besoin** : les étapes de la fenêtre, chaque message du filtre de paquets et tout ce qu'a dit OpenVPN vont dans le fichier.
- **Une route n'est plus rejetée un instant avant de fonctionner** ; quelques centaines de millisecondes sont laissées à la table de routage pour se stabiliser.
- **Le filtre de paquets consigne le filtre avec lequel il a été ouvert**, pour distinguer une clause absente d'une clause qui n'a jamais correspondu.

## [0.5.21] - 2026-09-14

### Corrigé

- **Les sites envoyés par le VPN n'y passaient pas alors que tout affirmait le contraire** : le saut suivant était deviné comme le .1 du réseau, qui n'existe pas sur un /30 ; il est désormais déduit de l'adresse réellement obtenue par le tunnel et chaque route est vérifiée comme celle que le système emprunterait vraiment.
- **Un nom impossible à résoudre par le tunnel était signalé comme concordant avec la réponse locale** ; aucune réponse n'est pas la même réponse, et le journal indique le résolveur et le transport.
- **L'interrogation par le tunnel bascule sur TCP**, car un relais qui transporte l'un et pas l'autre est courant chez les serveurs bénévoles.

## [0.5.20] - 2026-09-13

### Ajouté

- **Des sites que vous pouvez envoyer par le VPN sans y envoyer toute la machine** : les sites nommés sont résolus par le tunnel et routés par lui, et pendant la session seule l'application cible les atteint.

## [0.5.19] - 2026-09-13

### Ajouté

- **Un endroit pour l'identifiant et le mot de passe**, pour les serveurs qui demandent une connexion ; la boîte de dialogue dit franchement qu'openvpn ne peut le lire que dans un fichier, donc il est stocké en clair dans le dossier propre à ce profil.

### Corrigé

- **La cible atteignait Internet en IPv6, en contournant entièrement le tunnel** : une fuite dans tous les modes, puisqu'un tunnel qui transporte IPv4 ne peut pas transporter ce que la machine envoie en IPv6 ; l'IPv6 de la cible est désormais rejeté.
- **« Aucune mise à jour » alors que rien n'avait été demandé** : quota épuisé, la version déjà présente était renvoyée ; le validateur qui rend la vérification gratuite est maintenant conservé entre les exécutions.

## [0.5.18] - 2026-09-13

### Corrigé

- **La boîte À propos remerciait WinDivert sans dire à quelles conditions il est utilisé**, et nomme désormais la LGPL v3, la copie livrée avec le programme et l'emplacement des sources.
- **Les places libres d'un salon Warcraft III ne changeaient jamais sur l'autre machine**, car l'annonce lue par le pair est une diffusion impossible à capturer ; elle est maintenant déduite de l'annonce complète et vérifiée avant l'envoi.
- **Le téléchargement de la mise à jour retenait toujours la fenêtre** : 0.5.16 le disait corrigé alors que rien n'appelait ce code ; il tourne maintenant vraiment en arrière-plan, avec **Continuer en arrière-plan** dans la boîte de dialogue et une annulation dans la fenêtre principale.

## [0.5.17] - 2026-09-13

### Corrigé

- **Le départ d'un joueur fermait le salon Warcraft III pour tout le monde**, car l'écouteur et chaque connexion acceptée partageaient une seule entrée de port et la première fermeture l'emportait ; chaque socket est désormais suivi séparément.
- **L'application cible s'exécutait toujours en administrateur** : la voie de 0.5.16 exigeait un privilège qu'un assistant élevé ne peut pas détenir, donc celle qu'il détient est utilisée, et le journal dit laquelle.

## [0.5.16] - 2026-09-13

### Ajouté

- **Un installateur dans chaque langue parlée par l'application**, chacun avec la page de codes ANSI de sa culture, et la mise à jour propose celui qui correspond à la langue de la fenêtre.
- **La fenêtre s'ouvre là où vous l'avez laissée**, sauf si cette position ne tombe plus sur un écran.
- **Une nouvelle icône** : une flèche sortant par l'ouverture d'un anneau, dessinée séparément à chaque taille plutôt que réduite depuis une grande image.

### Corrigé

- **Démarrer se trouvait sous la ligne de flottaison** ; les boutons sont désormais fixés sous les cartes, qui défilent derrière eux.
- **L'application cible s'exécutait en administrateur**, si bien qu'une connexion dans le navigateur ne pouvait jamais lui transmettre le code d'autorisation ; elle est lancée avec le jeton du shell.
- **Télécharger une mise à jour prenait toute la fenêtre en otage**, et cela tourne maintenant en arrière-plan avec une barre de progression et un bouton d'annulation qui fonctionne.

## [0.5.15] - 2026-09-13

### Corrigé

- **Un seul refus du serveur mettait fin à la tentative**, ce qui est juste pour un serveur à soi et faux pour un relais public ; il réessaie trois fois avant de signaler.
- **« EXITING auth-failure » n'expliquait rien** et indique désormais quelle étape a échoué et ce que cela signifie pour votre type de serveur.

## [0.5.14] - 2026-09-12

### Ajouté

- **Un profil importé appartient désormais à l'application** : le .ovpn et chaque certificat et clé qu'il référence sont copiés dans un dossier dédié, si bien que supprimer l'original ne change rien.
- **Un endroit pour voir ce qui est conservé**, avec renommer, supprimer et un accès au dossier, la confirmation se faisant dans la ligne même.
- **OpenVPN, si vous ne l'avez pas**, récupéré depuis l'hôte de téléchargement d'OpenVPN et refusant tout ce que Windows ne reconnaît pas ou qu'OpenVPN n'a pas signé.
- **Des tests pour l'installateur**, qui parcourent ses pages dans les deux langues et annulent au récapitulatif, de sorte qu'exécuter la suite n'installe rien.
- **Un test qui regarde les pixels**, mesurant chaque ligne explicative des Paramètres et d'À propos contre son fond, dans les deux thèmes.

### Corrigé

- **Trois lignes d'À propos étaient invisibles**, car un pinceau pris dans les ressources de l'application se résout avec le thème de l'application, qu'une application WinUI non empaquetée ne peut plus changer après son démarrage.
- **La vérification des mises à jour a cessé de demander poliment** : elle lit maintenant quand le quota revient et attend, et le dit une fois au lieu de soixante.
- **L'installateur écrivait par-dessus sa propre illustration**, qui est le fond sur lequel la boîte de dialogue écrit et non une image à côté du texte.
- **La mise à jour donnait à tous l'installateur anglais**, et demande désormais celui qui correspond à la langue de la fenêtre.

### Modifié

- Sous chaque paramètre, une ligne dit ce qu'il change et où les paramètres sont conservés.
- À propos indique la fréquence des vérifications et crédite le client communautaire d'OpenVPN et WinDivert.

## [0.5.13] - 2026-09-12

### Ajouté

- **Un piano à queue** : les sons des contrôles passent par le synthétiseur General MIDI déjà présent dans Windows, sur une gamme pentatonique pour que deux notes quelconques s'accordent, avec une case pour les couper.
- **Du mouvement là où quelque chose s'est produit**, et pas partout.
- **Le schéma rend compte au lieu de mimer** : immobile sans session, et la case des paquets bloqués montre ceux que le garde a réellement rejetés.
- **Un installateur qui ressemble à ce produit**, avec des illustrations générées plutôt que les images par défaut de WiX.
- **Un installateur dans votre langue**, un MSI par langue, d'abord l'anglais et le chinois traditionnel.

### Modifié

- L'avertissement sous le confinement par processus apparaît maintenant quand la case est **décochée**, l'état qui mérite un avertissement.

## [0.5.12] - 2026-09-12

### Corrigé

- **Le dossier d'installation contenait quatre-vingt-huit dossiers de traductions pour des langues que cette application ne propose pas**, livrés en ressources Win32 que le réglage habituel de réduction n'atteint pas ; il en reste quinze.

### Modifié

- L'option par processus s'appelle *Seule l'application cible peut parler au VPN*, ce qu'elle fait ; elle n'a jamais gouverné le tunnel entier.

### Ajouté

- **Dix vérifications de plus qui pilotent la vraie fenêtre**, couvrant les boîtes de dialogue et les choix qu'elles contiennent — et leur écriture a révélé quatre tests qui cherchaient quelque chose n'ayant jamais existé.

## [0.5.11] - 2026-09-12

### Corrigé

- **Le mode sombre était du texte blanc sur une page blanche**, car le thème était appliqué à un élément situé à l'intérieur de celui qui peint le fond.
- **L'icône de la section cible s'affichait comme un carré vide**, car un point de code présent dans la table d'une police ne signifie pas qu'un glyphe existe.
- **Les contrôles de chaque section étaient centrés** dans une carte qui avait déjà la bonne largeur.
- **Choisir de s'attacher et démarrer sans programme levait une exception**, et l'application dit maintenant quel choix manque.

### Ajouté

- **Des tests qui pilotent la vraie fenêtre** : dix-neuf vérifications ouvrent la version publiée et regardent, car tous les défauts visuels signalés jusqu'ici pouvaient passer l'ensemble des tests unitaires du projet.

## [0.5.10] - 2026-09-12

### Corrigé

- **La ligne d'avertissement élargissait la colonne au lieu de se replier**, car une pile horizontale mesure ses enfants avec une largeur illimitée.
- **Les paramètres et les informations d'application apparaissaient en double**, l'ancienne paire étant restée lors du déplacement en haut à droite.
- **Le sélecteur de processus proposait cette application elle-même.**

### Modifié

- **Le schéma remplit l'espace qui lui est donné**, au lieu de rester à largeur fixe dans le coin d'une grande carte vide.
- **La note sous le confinement par processus n'apparaît que lorsqu'il est actif**, et dit ce qu'il fait.
- Le chemin de la cible s'affiche en entier au survol, si étroite que soit la zone.

## [0.5.9] - 2026-09-12

### Corrigé

- **Le mode sombre était inutilisable**, car la page ne peignait jamais son propre fond et il fallait indiquer le thème séparément aux boîtes de dialogue.

### Ajouté

- **Les deux interrupteurs de confinement sont désormais une image**, une grille de qui envoie contre vers où, qui répond d'un coup d'œil à ce que deux paragraphes n'arrivaient pas à dire.
- **L'attachement se choisit dans une liste de programmes en cours**, au lieu de demander un identifiant de processus recopié d'ailleurs.

### Modifié

- L'avertissement de routage n'apparaît que lorsque l'option est active, dit une chose, et la dit dans la couleur d'un avertissement.
- Les sections ont perdu leur numérotation ; elles n'ont jamais été des étapes à suivre dans l'ordre.
- Paramètres et informations d'application sont passés en haut à droite, les cartes d'état en dessous.

## [0.5.8] - 2026-09-12

### Corrigé

- **Confiner le tunnel à une application ne fonctionnait que dans un sens**, laissant tous les autres programmes d'ici joignables depuis l'autre bout, la moitié qui compte quand on ignore qui s'y trouve.

### Modifié

- **L'option de routage est maintenant dans l'autre sens** : *Envoyer tout le trafic par le VPN*, désactivée par défaut, en disant ce que l'activer signifie pour qui exploite ce serveur.

### Ajouté

- **Apparence claire et sombre**, ou suivant le système, appliquée aussitôt et mémorisée.
- **Des icônes dans toute l'interface** et un voyant d'état vert pendant une session.

## [0.5.7] - 2026-09-12

### Corrigé

- **Un téléchargement ne pouvait pas être annulé**, car il s'exécutait en retenant le différé de clic de la boîte de dialogue, ce qui lui fait désactiver ses propres boutons.
- **Un téléchargement annulé ou échoué laissait son fichier partiel**, un par tentative, indéfiniment.
- **Appuyer sur Démarrer sans rien à démarrer ne faisait rien du tout**, et l'application dit maintenant quel choix manque.

### Modifié

- **Installer une mise à jour pendant une session avertit d'abord**, la réponse sûre étant celle par défaut, car l'installation déconnecte l'application cible.

## [0.5.6] - 2026-09-12

### Corrigé

- **Une nouvelle version est repérée environ une minute après sa publication**, grâce à une requête conditionnelle qui ne coûte rien tant que rien n'a changé.
- **Écarter l'avis de mise à jour ne laissait aucun moyen d'y revenir** ; les paramètres et la boîte d'informations proposent désormais *Mettre à jour* tant qu'une attend.
- **Arrêter affichait *Arrêter* alors qu'il n'y avait plus rien à arrêter**, et affiche *Forcer l'arrêt* pendant l'attente d'un relais.

### Modifié

- **Tout ce qui concerne la mise à jour est dans la boîte d'informations**, à côté de la version servant de comparaison.
- **Un installateur téléchargé est conservé si vous choisissez *Plus tard***, jusqu'à ce que sa version soit dépassée.

## [0.5.5] - 2026-09-12

### Corrigé

- **Les vérifications automatiques étaient trop rares pour sembler automatiques** : toutes les trente minutes désormais, et aussi au retour de la fenêtre si la dernière date de plus de cinq minutes.
- **Les deux options de confinement se lisaient comme des doublons**, et chaque libellé nomme maintenant son propre axe : quelles destinations, ou quel programme.

### Modifié

- **Le bouton de la bannière s'appelle *Mettre à jour*** plutôt que *Nouveautés*, car installer est ce qu'il fait.
- **Les paramètres peuvent aussi lancer une mise à jour**, pas seulement en chercher une.
- **La bande vide en haut de la fenêtre a disparu.**
- **Le compteur d'annonces relayées ne s'affiche que pour Warcraft III**, le seul protocole concerné.

## [0.5.4] - 2026-09-12

### Corrigé

- **La fenêtre de mise à jour affichait les notes en Markdown brut** au lieu de les mettre en forme, rendant pénible à lire ce qui était écrit pour être lu.
- **La mise à jour ne fermait pas l'application d'abord**, si bien que l'installateur remplaçait des fichiers encore tenus par le tunnel et l'assistant.
- **La fenêtre et la barre des tâches gardaient une icône générique**, car une fenêtre non empaquetée ne prend pas l'icône de l'exécutable d'elle-même.
- **Le libellé chinois de « garder le reste de la machine hors du tunnel » décrivait l'autre réglage**, ce qui faisait passer deux options orthogonales pour des doublons.

### Modifié

- **Le nom et l'accroche n'occupent plus le haut de la fenêtre** ; la barre de titre dit déjà ce que c'est.
- **La boîte d'informations ne répète plus le nom de son propre titre**, et affiche la version assez grande pour être lue d'un coup d'œil.

## [0.5.3] - 2026-09-12

### Corrigé

- **Une partie hébergée sur une machine était visible mais impossible à rejoindre depuis l'autre**, car seul le port de découverte était ouvert alors que Warcraft III monte jusqu'à 6119 si 6112 est pris ; toute la plage est ouverte, toujours au seul sous-réseau VPN.
- **Un salon restait dans la liste de l'autre joueur après le départ de l'hôte**, car l'annonce de fermeture est une diffusion qui n'arrive jamais ; le relais constate que l'hôte ne répond plus et le retire lui-même.
- **Changer de langue vidait les listes d'application cible et de découverte LAN**, car remplacer l'entrée sélectionnée se lit comme sa disparition ; les entrées gardent maintenant leur identité et seul leur texte change.
- **La fenêtre des paramètres gardait l'ancienne langue dans son titre et son bouton**, qui ne font pas partie du contenu réétiqueté.
- **Le journal d'activité ne suivait pas fiablement les nouvelles lignes**, car il défilait avant que la nouvelle ligne soit disposée.
- **Une mise à niveau réécrivait chaque fichier, modifié ou non**, car l'ancienne version était entièrement supprimée avant qu'un seul fichier neuf soit écrit.
- **L'un des boutons du journal était disposé et cliquable mais n'était jamais dessiné.**

### Ajouté

- **Un bouton d'informations d'application** à côté de celui des paramètres, avec la version, le copyright et un lien vers les notes de cette version.
- **Une case *Lancer LanBridge* sur la dernière page de l'installateur**, qui le démarre sans élévation, comme il est prévu de fonctionner.

### Modifié

- **Fermer l'application cible ne termine la session que lorsque le tunnel lui est lié**, sinon il pourrait encore transporter du trafic pour autre chose.

## [0.5.2] - 2026-09-11

### Corrigé

- **Les notes de version n'affichaient que leur premier titre**, car le contrôle coupe les lignes sur un retour chariot que les notes normalisées ne contenaient plus.

## [0.5.1] - 2026-09-11

### Corrigé

- **Les parties étaient visibles mais impossibles à rejoindre, ou n'apparaissaient pas** : tout ce qui rend une partie joignable arrive en entrant et Windows le bloque par défaut, donc une session ouvre le port de découverte pour le seul sous-réseau VPN et défait tout à la fin.
- **L'interface s'effondrait à toute taille de texte supérieure à la valeur par défaut, et redémarrer ne la rétablissait jamais**, car la couche mise à l'échelle était d'abord centrée puis agrandie depuis son propre coin supérieur gauche.
- **Le téléchargement d'une mise à jour n'affichait aucune progression**, ce qui ne se distingue pas d'un téléchargement jamais commencé.
- **Réactiver les vérifications automatiques ne faisait rien pendant jusqu'à quatre heures.**
- **Le choix de fermeture semblait abandonné** lorsque la langue changeait durant la même visite des paramètres.

### Ajouté

- **Les vérifications continuent tant que l'application est dans la zone de notification**, annoncées par une bulle et conservées dans l'infobulle de l'icône.
- **Un bouton *Vérifier maintenant* dans les paramètres**, pour quand attendre la prochaine n'est pas le sujet.

## [0.5.0] - 2026-09-11

### Corrigé

- **Le VPN tombait dès l'ouverture d'un salon**, car la recherche du processus auquel un lanceur passe la main ne comparait que les exécutables de même nom.
- **Arrêter ne faisait rien une fois la session terminée**, car fermer le tube vers un assistant disparu levait une exception qui s'échappait du nettoyage.
- **Changer de langue vidait chaque liste déroulante** au lieu de la traduire.
- **Le journal d'activité ne suivait pas les nouvelles lignes**, et cesse maintenant de suivre dès que vous remontez.

### Ajouté

- **Des notes de version dans votre langue**, publiées à côté des builds et affichées selon la langue de l'interface.
- **Exporter un rapport d'erreur**, qui réunit l'activité à l'écran et les journaux des deux processus.
- Le réglage de taille de texte met désormais toute l'interface à l'échelle, pas seulement le journal.

## [0.4.0] - 2026-09-10

### Ajouté

- **Vérification automatique des mises à jour**, montrant ce qui a changé avant d'installer ; activée par défaut.
- **Boîte de dialogue des paramètres** derrière le bouton engrenage, avec langue, taille de police, comportement à la fermeture et vérification des mises à jour.
- **Dix langues d'interface supplémentaires**, aux côtés de l'anglais et du chinois traditionnel.
- **Le journal d'activité est du texte sélectionnable**, avec *Tout copier* et *Exporter…*.
- **Taille de police réglable** (10–22 pt), mémorisée entre les exécutions.
- **Icône de l'application**, utilisée par la fenêtre, la barre des tâches, la zone de notification et la liste des programmes.
- **L'installateur demande où installer** et propose les deux raccourcis comme choix indépendants.

### Corrigé

- **Le VPN tombait dès la fin du chargement du jeu**, car la sortie du premier processus du lanceur était lue comme la fermeture de la cible ; la session suit le relais.
- **L'icône de la barre d'état n'apparaissait jamais**, car son handle était détruit avant que Windows ne l'utilise.
- **Rouvrir depuis la barre d'état lançait une seconde copie** au lieu de restaurer celle en cours.
- **`WinDivert64.sys` restait verrouillé après la fermeture**, car ouvrir un handle de pilote enregistre un service noyau qui continue de tourner.
- **Remettre la langue sur « Valeur par défaut du système » ne faisait rien**, car une unique notification « tout a changé » n'est pas traitée de façon fiable.
- **La désinstallation demandait un redémarrage**, l'application et son assistant tenant encore des fichiers.
- Les lignes du journal avaient une marge d'élément de liste qui laissait une demi-ligne vide entre les entrées.

### Modifié

- Les actions du journal sont passées dans le panneau d'activité à droite, au lieu du bas de la colonne de configuration.

## [0.3.0] - 2026-09-09

### Ajouté

- Journalisation des erreurs dans `%LOCALAPPDATA%\LanBridge\logs\`, un fichier par processus et par exécution, les exceptions non gérées étant consignées plutôt que de finir en silence.
- Persistance des paramètres à chaque changement, de sorte qu'ils survivent à un plantage.
- Prise en charge de la zone de notification avec une question à la fermeture.
- Interface en chinois traditionnel à côté de l'anglais.
- Installateur MSI avec raccourcis, métadonnées de version et entrée dans la liste des programmes.

### Corrigé

- La version publiée démarrait puis mourait dans le moteur XAML, car une application WinUI non empaquetée n'emporte pas son balisage compilé.

## [0.2.0] - 2026-09-09

### Corrigé

- **La fenêtre n'apparaissait jamais après la demande d'élévation**, car WinUI 3 ne peut pas s'exécuter élevé ; l'interface tourne sans élévation et confie le travail privilégié à un assistant qui demande le consentement une fois par session.

## [0.1.0] - 2026-09-09

### Ajouté

- VPN par application, qui refuse la route par défaut et le DNS poussés, de sorte que seul le sous-réseau VPN traverse le tunnel.
- Confinement facultatif par processus via WinDivert, rejetant le trafic vers le sous-réseau VPN de tout processus autre que la cible.
- Relais de découverte LAN pour les tunnels qui ne transportent pas la diffusion, y compris le protocole W3GS de Warcraft III, dont les informations de partie ne sont envoyées qu'en réponse unicast.
- Relais générique de diffusion UDP pour d'autres jeux, configuré par port.
- Version en ligne de commande du même moteur, sans interface.

[0.5.31]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.31
[0.5.30]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.30
[0.5.29]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.29
[0.5.28]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.28
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
