# Journal des modifications

Toutes les modifications notables de LanBridge sont consignées ici.

Le format suit [Keep a Changelog](https://keepachangelog.com/fr/1.1.0/) et la
numérotation suit [le versionnage sémantique](https://semver.org/lang/fr/).

## [0.5.14] - 2026-09-12

### Ajouté

- **Un profil importé appartient désormais à l'application.** Avant, on mémorisait
  l'emplacement du fichier et on le relisait à chaque démarrage, ce qui tient jusqu'à ce
  que le fichier bouge, que la clé USB sorte ou que les Téléchargements soient vidés.
  Il est maintenant copié dans son propre dossier, avec chaque certificat et chaque clé
  qu'il référence, et ces références sont réécrites vers les copies.
- **Un endroit pour voir ce qui est conservé.** Un bouton Gérer à côté d'Importer :
  ce qui est stocké, ce qui sert, renommer, supprimer, ouvrir le dossier.
- **OpenVPN, si vous ne l'avez pas.** Cette application pilote le client communautaire
  OpenVPN, elle ne l'embarque pas. Elle le dit maintenant avant de démarrer et propose de
  récupérer la version actuelle depuis le serveur officiel d'OpenVPN et de l'installer,
  en refusant tout ce que Windows n'accepte pas ou qui n'est pas signé par OpenVPN.
- **Des tests pour l'installateur.** Le contenu du paquet, et un parcours de ses pages
  dans les deux langues. Il s'arrête au récapitulatif et annule : lancer la suite
  n'installe rien.
- **Un test qui regarde les pixels.** Chaque ligne d'explication est photographiée dans
  les deux thèmes et mesurée contre ce qu'elle a derrière elle.

### Corrigé

- **Trois lignes d'À propos étaient invisibles.** Elles étaient peintes avec un pinceau
  pris dans les ressources de l'application, qui se résout selon le thème de
  l'application — qu'une application WinUI non empaquetée ne peut plus changer après le
  démarrage — alors que les boîtes de dialogue sont dessinées dans le thème choisi.
- **La vérification des mises à jour a cessé d'insister.** Soixante par heure, c'est
  exactement la limite sans authentification. Elle lit maintenant quand le quota revient
  et attend.

- **L'installateur écrivait par-dessus sa propre illustration.** Ces images ne sont pas des
  dessins à côté du texte : ce sont le fond sur lequel la boîte de dialogue écrit, dans sa
  propre couleur sombre, et c'est elle qui choisit où. Remplir les 493 pixels d'un dégradé
  bleu mettait chaque titre en sombre sur sombre. L'illustration est désormais une bande à
  gauche et un bloc à droite de la bannière, le reste restant blanc.
### Modifié

- Chaque réglage porte une ligne disant ce qu'il change et où il est conservé.
- À propos explique comment les mises à jour sont cherchées et crédite OpenVPN et
  WinDivert.

## [0.5.13] - 2026-09-12

### Ajouté

- **Le son.** WinUI intègre un système sonore à chaque contrôle — focus, activation,
  ouverture et fermeture des boîtes de dialogue — et il se tait tant qu'une application ne
  le demande pas. Celle-ci ne l'avait jamais demandé : chaque clic était donc silencieux
  par omission et non par choix. C'est un choix désormais, c'est spatialisé, et une case
  dans les paramètres reste là pour qui préfère un utilitaire muet.
- **Du mouvement là où quelque chose s'est produit.** Les deux colonnes apparaissent tandis
  que la fenêtre s'assemble, le texte d'état remonte quand il change, le compteur relayé
  tressaille quand il monte, le voyant respire pendant une session, et une ligne
  d'avertissement pousse ses voisines au lieu de surgir de nulle part.
- **Le dessin rend compte au lieu de mimer.** Il jouait la même animation qu'il se passe
  quelque chose ou non — de la décoration déguisée en instrument. Il est terne et immobile
  sans session, il s'anime avec elle, et la case bloquée affiche le nombre de paquets
  réellement écartés : un chiffre qui n'était jamais parvenu à l'interface, faute d'avoir
  jamais été écouté.
- **Un installateur qui ressemble à ce produit**, avec des images générées plutôt que
  celles fournies par défaut par WiX.
- **Un installateur dans votre langue** : un MSI par langue plutôt que l'anglais pour tous.
  Anglais et chinois traditionnel pour commencer.

### Modifié

- L'avertissement du confinement par processus apparaît maintenant quand la case est
  **décochée**, l'état qui mérite un avertissement, et dit ce que cet état signifie au lieu
  de répéter l'étiquette. Les deux avertissements portent aussi des marques différentes.

## [0.5.12] - 2026-09-12

### Corrigé

- **Le dossier d'installation contenait quatre-vingt-huit dossiers de traductions pour des
  langues que cette application ne propose pas** — af-ZA, sl-SI, fil-PH et les autres. Ce
  sont les textes du Windows App SDK lui-même, livrés en ressources Win32 et non en
  assemblages satellites .NET : le réglage habituel pour les élaguer ne les atteint donc
  pas. Seules celles correspondant à une langue que l'interface parle sont conservées :
  quinze au lieu de quatre-vingt-huit.

### Modifié

- L'option par processus s'appelle « Seule l'application cible peut dialoguer avec le
  VPN », ce qu'elle fait réellement. Elle n'a jamais régi le tunnel entier.

### Ajouté

- **Dix vérifications de plus qui pilotent la vraie fenêtre**, portant sur les boîtes de
  dialogue et leur contenu. Les écrire a trouvé deux choses : une boîte de dialogue ici
  n'est pas une fenêtre et ne porte pas le nom qu'elle semblait porter, si bien que quatre
  tests cherchaient quelque chose qui n'a jamais existé ; et Démarrer est indisponible sans
  profil plutôt que d'accepter le clic sans rien faire.

## [0.5.11] - 2026-09-12

### Corrigé

- **Le mode sombre affichait du texte blanc sur une page blanche.** La tentative précédente
  peignait le fond sur un élément et appliquait le thème à celui qu'il contenait : le texte
  se résolvait donc dans le thème sombre et la surface derrière lui dans le clair. Le thème
  est désormais posé sur l'élément qui peint le fond, là où les deux s'accordent.
- **L'icône de la section cible se dessinait en carré vide.** Qu'un point de code figure
  dans la table de caractères d'une police ne veut pas dire que la police en possède le
  glyphe. Les trois marques de section sont des emoji.
- **Les contrôles de chaque section étaient centrés** dans une carte qui avait déjà la
  bonne largeur. Un expandeur s'étire lui-même, pas son contenu.
- **Choisir l'attachement et démarrer sans sélectionner de programme levait une
  exception.** La vérification ajoutée pour un exécutable manquant ne couvre pas
  l'attachement, à qui il manque autre chose. Elle dit maintenant ce qui manque, et le
  message ne demande plus de saisir un identifiant de processus dans une liste.

### Ajouté

- **Des tests qui pilotent la vraie fenêtre.** Tous les défauts visuels signalés jusqu'ici
  passeraient n'importe quel test unitaire du projet, car aucun ne porte sur ce que
  retourne une méthode. Dix-neuf vérifications ouvrent désormais la version publiée et
  regardent.

## [0.5.10] - 2026-09-12

### Corrigé

- **La ligne d'avertissement élargissait la colonne au lieu de se replier.** Une pile
  horizontale mesure ses enfants avec une largeur illimitée : un bloc de texte à retour à
  la ligne placé dedans ne se replie donc jamais, il élargit tout ce qui l'entoure, bouton
  compris. Les deux notes sont maintenant dans une grille qui donne au texte une largeur
  réelle.
- **Les paramètres et les informations apparaissaient deux fois.** La paire posée à côté
  des cartes d'état est restée là quand elles sont montées en haut à droite.
- **Le sélecteur de processus proposait cette application elle-même.** Attacher le tunnel à
  la fenêtre qui le configure n'est l'intention de personne.

### Modifié

- **Le dessin occupe l'espace qui lui est alloué.** Les routes s'étirent avec la fenêtre au
  lieu de garder une largeur fixe dans le coin d'une grande carte vide, et les paquets
  parcourent toute cette largeur.
- **La note du confinement par processus n'apparaît que lorsqu'il est actif**, avec la même
  marque d'avertissement que celle du routage, et dit ce qu'il fait : les paquets de tous
  les autres programmes d'ici sont arrêtés.
- Le chemin du programme cible s'affiche en entier au survol, si étroite que soit la
  zone.

## [0.5.9] - 2026-09-12

### Corrigé

- **Le mode sombre était inutilisable.** La page ne peignait jamais son propre fond : le
  texte suivait donc le thème et la surface derrière lui non — texte clair sur fond clair.
  Les boîtes de dialogue conservaient elles aussi l'apparence du système, car une boîte de
  dialogue est hébergée par la racine de la fenêtre et non par l'élément sur lequel le
  thème a été posé ; il fallait le lui dire séparément.

### Ajouté

- **Les deux commutateurs de confinement sont maintenant un dessin.** Une grille de qui
  envoie face à vers où, avec du trafic parcourant chaque route : par le tunnel, par la
  sortie habituelle, ou arrêté. Les quatre cases sont toutes les combinaisons des deux
  réglages, et répondent d'un coup d'œil à ce que deux paragraphes n'arrivaient pas à
  expliquer.
- **L'attachement se choisit dans une liste de programmes en cours** au lieu de demander un
  identifiant de processus trouvé ailleurs, avec un bouton pour l'actualiser.

### Modifié

- L'avertissement de routage n'apparaît que lorsque cette option est active, ne dit qu'une
  chose, et la dit dans la couleur d'un avertissement.
- Les sections ont perdu leur numérotation ; ce n'étaient jamais des étapes à suivre dans
  l'ordre.
- Les paramètres et les informations sur l'application sont passés en haut à droite, avec
  les cartes d'état juste en dessous.

## [0.5.8] - 2026-09-12

### Corrigé

- **Réserver le tunnel à une seule application ne fonctionnait que dans un sens.** Le trafic
  que les autres programmes d'ici envoyaient au VPN était rejeté, mais rien n'était fait de
  celui qui en arrivait : tous les autres programmes de cette machine restaient donc
  joignables depuis l'autre bout — c'est-à-dire la moitié qui compte quand on ignore qui s'y
  trouve. Cela s'applique désormais dans les deux sens, et l'explication le dit au lieu de
  promettre plus qu'elle ne tenait.

### Modifié

- **L'option de routage est maintenant à l'envers.** Garder le reste de la machine hors du
  tunnel est l'état sûr et celui que presque tout le monde veut : il ne devrait donc pas
  falloir l'activer. La case indique désormais « Faire passer tout le trafic par le VPN »,
  est décochée par défaut et explique ce que cocher implique : tout ce que cette machine
  envoie passe d'abord par le serveur VPN, et quiconque l'administre voit tout. Cela compte
  surtout avec un profil fourni par quelqu'un d'autre.

### Ajouté

- **Apparence claire et sombre**, ou suivant le système, appliquée immédiatement et
  mémorisée. Sous Apparence, dans les paramètres.
- **Des icônes dans toute l'interface** — sur chaque section, sur Démarrer et Arrêter et sur
  les actions du journal — et un voyant d'état vert tant qu'une session tourne.

## [0.5.7] - 2026-09-12

### Corrigé

- **Un téléchargement ne pouvait pas être annulé.** Le bouton affichait « Annuler » et
  restait impossible à cliquer : le téléchargement s'exécutait en conservant le report de
  clic de la boîte de dialogue, et une boîte de dialogue dont un report est en cours
  désactive ses propres boutons — y compris le seul qui aurait pu l'arrêter. Le transfert
  s'exécute désormais à côté de la boîte de dialogue et non dans son gestionnaire de clic,
  si bien que le bouton est actif exactement tant qu'il y a quelque chose à annuler.
- **Un téléchargement annulé ou échoué laissait son fichier partiel**, un par tentative,
  pour toujours. Le fichier incomplet est maintenant supprimé quand le transfert ne va pas
  au bout, et un téléchargement terminé fait le ménage des installateurs précédents.
- **Appuyer sur Démarrer sans rien à démarrer ne faisait strictement rien** : aucun message,
  aucune ligne de journal, aucun changement. Sans profil, ou sans application choisie, il
  indique désormais ce qui manque au lieu d'avoir l'air en panne.

### Modifié

- **Installer une mise à jour pendant qu'une session tourne prévient d'abord**, et la
  réponse prudente est celle par défaut. L'installation arrête le tunnel et déconnecte
  l'application cible : ce n'est pas une chose à découvrir après coup.

## [0.5.6] - 2026-09-12

### Corrigé

- **Une nouvelle version est repérée environ une minute après sa publication**, et non à la
  prochaine vérification programmée. Demander aussi souvent ne coûte rien parce que la
  requête est conditionnelle : le validateur de la réponse précédente est renvoyé et, tant
  que la version ne change pas, la réponse est « non modifié » — sans corps et sans compter
  dans la limite de requêtes. Seule une version réellement nouvelle consomme une requête.
  Cela reste une consultation et non une notification : c'est donc une minute et non
  l'instant même, mais il n'y a rien à cliquer ni à redémarrer.
- **Écarter l'avis de mise à jour ne laissait aucun moyen d'y revenir.** La fermeture valait
  pour toute la session et seul un redémarrage le ramenait. La fenêtre d'informations et
  les paramètres proposent désormais « Mettre à jour » tant qu'une mise à jour attend :
  écarter l'avis n'écarte donc que l'avis.
- **Le bouton disait « Arrêter » alors qu'il n'y avait plus rien à arrêter.** À la fermeture
  de l'application cible, la session attend jusqu'à vingt secondes pour voir si un lanceur
  passe la main à un autre processus — pendant ce temps, ce pour quoi la session existe est
  déjà mort. Durant cette fenêtre le bouton affiche « Arrêt forcé », ce qu'il fait
  réellement : terminer la session maintenant plutôt qu'attendre le relais.

### Modifié

- **Tout ce qui touche aux mises à jour se trouve désormais dans la fenêtre d'informations
  sur l'application**, dont le bouton porte une pastille tant qu'une mise à jour attend. La
  vérification automatique, la vérification immédiate, la date de la dernière vérification
  et la mise à jour elle-même côtoient la version à laquelle elles se comparent, au lieu
  d'être réparties entre cette fenêtre et les paramètres.
- **L'installateur téléchargé est conservé si vous choisissez « Plus tard ».** Auparavant,
  ne pas installer tout de suite jetait le téléchargement ; désormais le même bouton
  propose « Installer maintenant » jusqu'à ce que la version concernée soit dépassée.

## [0.5.5] - 2026-09-12

### Corrigé

- **Les vérifications automatiques de mise à jour étaient trop rares pour paraître
  automatiques.** Quatre heures d'intervalle faisaient qu'en pratique seul un redémarrage
  semblait trouver quelque chose, laissant un bouton des paramètres comme véritable
  mécanisme — et personne n'a envie d'appuyer sur un bouton pour s'entendre dire qu'il n'y
  a rien de neuf. La vérification a désormais lieu toutes les trente minutes, et ramener la
  fenêtre au premier plan vérifie aussi lorsque la dernière date de plus de cinq minutes.
  Les paramètres affichent l'heure de la dernière vérification, pour qu'on voie qu'elle a
  lieu.
- **Les deux options de confinement se lisaient comme des doublons.** Toutes deux étaient
  formulées comme limitant le tunnel, sans dire qu'elles ne limitent pas la même chose.
  Chaque étiquette nomme maintenant son axe — « Seules les adresses du VPN passent par le
  tunnel » face à « Seule l'application cible peut utiliser le tunnel » — et chaque
  explication commence par la question à laquelle elle répond : quelles destinations, ou
  quel programme.

### Modifié

- **Le bouton de la bannière s'appelle « Mettre à jour »**, et non « Nouveautés ». Ce qu'il
  fait, c'est installer la mise à jour ; afficher les notes est ce qu'il fait en chemin.
- **Les paramètres peuvent lancer une mise à jour**, pas seulement en chercher une.
- **La bande vide en haut de la fenêtre a disparu.** Les paramètres et les informations sur
  l'application sont descendus à côté des cartes d'état, la seule chose qui s'y trouvait.
- **Le compteur d'annonces relayées ne s'affiche que pour Warcraft III.** C'est le seul
  protocole dont les informations de partie doivent être demandées puis transmises ; pour
  les autres le compteur resterait à zéro pour toujours, ce qui se lit comme une panne et
  non comme « sans objet ».

## [0.5.4] - 2026-09-12

### Corrigé

- **La fenêtre de mise à jour affichait les notes sous forme de source Markdown** — dièses,
  astérisques et accents graves — au lieu de les mettre en forme, rendant pénible la
  lecture de ce qui est écrit pour être lu. Titres, puces, emphase et code en ligne sont
  désormais formatés.
- **La mise à jour ne fermait pas l'application au préalable.** L'installateur démarrait
  alors que le tunnel et l'assistant privilégié retenaient encore les fichiers qu'il allait
  remplacer. La session est maintenant arrêtée et ce processus se termine avant que
  l'installateur ne s'exécute ; l'installateur met fin à une instance restante plutôt que
  de la laisser transformer une mise à jour en demande de redémarrage.
- **La fenêtre et la barre des tâches gardaient une icône générique** tandis que la zone de
  notification et Ajout/Suppression de programmes affichaient la vraie. Une fenêtre non
  empaquetée ne prend pas l'icône de l'exécutable d'elle-même.
- **L'étiquette chinoise de « Garder le reste de la machine hors du tunnel » décrivait le
  mauvais réglage.** Elle se lisait « garder le trafic des autres applications hors du
  tunnel », ce que fait le confinement par application, si bien que les deux options
  semblaient faire double emploi. Elles sont orthogonales : l'une limite les destinations
  qui empruntent le tunnel, l'autre le processus autorisé à l'emprunter.

### Modifié

- **Le nom et la accroche n'occupent plus le haut de la fenêtre.** La barre de titre dit
  déjà de quoi il s'agit, et les détails sont passés dans la fenêtre d'informations.
- **La fenêtre d'informations ne répète plus le nom qui lui sert de titre** et laisse
  l'ouverture du dossier des journaux au panneau du journal, où ce bouton se trouvait déjà.
  La version, qui est la raison d'ouvrir cette fenêtre, s'affiche désormais assez grande
  pour être lue d'un coup d'œil.

## [0.5.3] - 2026-09-12

### Corrigé

- **Une partie hébergée sur une machine était visible depuis l'autre mais impossible à
  rejoindre.** Seul le port de découverte était ouvert en entrée, or ce n'est pas forcément
  celui sur lequel l'hôte écoute : Warcraft III prend le 6112 s'il le peut et monte jusqu'au
  6119 sinon, en annonçant celui qu'il a obtenu. Un hôte chassé du 6112 était donc visible
  et inaccessible — et dans ce sens seulement, ce qui donnait l'impression qu'une des deux
  machines était en cause. Toute la plage d'hébergement est désormais ouverte, toujours au
  seul sous-réseau du VPN.
- **Une partie restait dans la liste de l'autre joueur après que l'hôte l'a fermée.**
  Warcraft III annonce la fermeture par diffusion, et une diffusion peut sortir par
  l'adaptateur du VPN, où le relais n'écoute délibérément pas : l'annonce n'était donc
  jamais recueillie et le pair continuait de proposer une partie qui n'existait plus. Le
  relais remarque désormais que l'hôte ne répond plus à ses sondes et la retire lui-même,
  en s'appuyant sur la dernière annonce relayée pour dire laquelle.
- **Changer de langue vidait les listes déroulantes de l'application cible et de la
  découverte réseau**, et choisir « Par défaut du système » vidait la liste des langues
  elle-même. Retraduire une liste revient à remplacer les entrées qu'elle contient, et une
  liste déroulante considère le remplacement de l'entrée sélectionnée comme sa
  disparition : elle effaçait la sélection, et la liaison réécrivait ce vide par-dessus le
  choix. Les entrées conservent maintenant leur identité et seul leur texte change, il n'y
  a donc plus rien à effacer. Deux tentatives précédentes rétablissaient la sélection après
  coup ; celle-ci supprime la cause.
- **La fenêtre des paramètres gardait l'ancienne langue dans son titre et son bouton**
  lorsque la langue était changée depuis l'intérieur. Tout le contenu de la fenêtre était
  réétiqueté, mais le titre et le bouton de fermeture n'en font pas partie.
- **Le journal d'activité ne suivait pas les nouvelles lignes de façon fiable.** Il
  défilait avant que la nouvelle ligne ne soit mise en page, allant donc là où se trouvait
  l'ancienne fin et restant toujours une ligne en retard. Il défile désormais après la mise
  en page et cesse de suivre dès que vous remontez pour lire, reprenant au retour en bas.
- **Chaque mise à niveau réécrivait tous les fichiers, modifiés ou non.** L'ancienne
  version était entièrement supprimée avant l'écriture du moindre fichier neuf, si bien que
  chaque mise à niveau réécrivait toute l'installation. La nouvelle version est désormais
  écrite d'abord et l'ancienne supprimée ensuite, ce qui permet à l'installateur d'ignorer
  les fichiers identiques et ne laisse à écrire que ce qui a réellement changé.
- **L'un des boutons du journal était disposé et cliquable, mais n'était jamais dessiné.**
  *Ouvrir le dossier des journaux* occupait sa place et répondait aux clics sans rien
  afficher du tout. Les actions du journal tiennent désormais sur une seule rangée
  horizontale au lieu d'une colonne chacune, ce qui supprime la disposition par colonne
  qui échouait.

### Ajouté

- **Un bouton d'informations sur l'application** à côté de celui des paramètres : la
  version en cours d'exécution, le copyright, et un lien vers ses notes et téléchargements.
- **Une case « Lancer LanBridge » sur la dernière page de l'installateur**, cochée par
  défaut. Elle démarre l'application sans élévation, comme LanBridge doit s'exécuter : le
  consentement est demandé au démarrage d'une session, pas avant.

### Modifié

- **Fermer l'application cible ne met fin à la session que si le tunnel lui est lié.**
  Avec « Seule cette application peut utiliser le VPN » activé, le tunnel existe pour ce
  seul processus et disparaît avec lui ; sinon il resterait un tunnel qu'aucun programme de
  la machine n'a le droit d'utiliser. Sans cette option, le tunnel n'est délimité que par
  destination et peut encore transporter le trafic d'autre chose : il reste donc en place
  jusqu'à ce que vous l'arrêtiez.

## [0.5.2] - 2026-09-11

### Corrigé

- **Les notes de version dans la fenêtre de mise à jour n'affichaient que leur premier
  titre.** Extraites du journal des modifications, les notes sont normalisées en simples
  sauts de ligne, alors qu'un contrôle de texte Windows coupe les lignes sur le retour
  chariot : tout ce qui suivait la première ligne n'était jamais dessiné. Le corps d'une
  version en anglais contient déjà des retours chariot, d'où l'impression que seules les
  notes traduites étaient vides. Elles sont désormais converties avant affichage et
  présentées dans un bloc défilable et sélectionnable.

## [0.5.1] - 2026-09-11

### Corrigé

- **Les parties étaient visibles mais impossibles à rejoindre, ou n'apparaissaient pas du
  tout.** Tout ce qui rend une partie rejoignable arrive en *entrée* par le tunnel, et
  Windows bloque l'ensemble par défaut : l'annonce relayée par le pair est de l'UDP
  entrant, et rejoindre une partie est une connexion TCP entrante. La version en ligne de
  commande ouvrait les deux ; l'application ne l'a jamais fait, si bien que la machine
  qui ne gardait aucune règle héritée d'elle restait injoignable dans un sens ou dans les
  deux. Une session ouvre désormais le port de découverte pour le seul sous-réseau du VPN
  — et non pour tous les réseaux auxquels la machine est raccordée —, sort l'adaptateur
  du tunnel de la catégorie « public » que Windows lui attribue, et rétablit les deux à
  la fin.
- **L'interface se disloquait dès que la taille du texte dépassait la valeur par défaut,
  et un redémarrage n'y changeait rien.** La couche agrandie était d'abord centrée, puis
  grandissait depuis son propre coin supérieur gauche : tout commençait donc plus bas et
  plus à droite que prévu et débordait en bas et à droite, emportant le bouton des
  paramètres. Comme la taille du texte est mémorisée, chaque redémarrage retombait dans le
  même état, sans moyen d'atteindre le réglage responsable.
- **Le téléchargement d'une mise à jour n'affichait aucune progression.** La fenêtre se
  fermait dès l'appui sur *Télécharger* et le transfert se faisait sans rien à l'écran, ce
  qui ne se distingue pas d'un téléchargement qui n'a jamais commencé. Les notes de version
  restent maintenant ouvertes, avec une barre de progression, la quantité transférée et une
  *Annulation* qui fonctionne.
- **Réactiver la vérification automatique des mises à jour restait sans effet pendant
  jusqu'à quatre heures.** La vérification en arrière-plan ne relisait le réglage qu'à son
  prochain passage ; elle regarde désormais tout de suite.
- **Le choix du comportement à la fermeture semblait perdu** lorsque la langue était
  changée au cours de la même visite dans les paramètres. Traduire la liste remplace
  l'entrée sélectionnée, ce qui efface la sélection : les autres listes se rétablissent
  seules, pas celle-ci.

### Ajouté

- **Les mises à jour sont vérifiées même lorsque l'application est dans la zone de
  notification**, toutes les quatre heures au lieu du seul démarrage. Une nouvelle version
  est annoncée par une bulle dans la zone de notification, et l'info-bulle de l'icône
  continue de le signaler une fois la bulle disparue.
- **Un bouton « Vérifier maintenant » dans les paramètres**, pour quand attendre la
  prochaine vérification programmée n'a pas d'intérêt.

## [0.5.0] - 2026-09-11

### Corrigé

- **Le VPN se coupait dès l'ouverture d'une partie.** La recherche du processus auquel un
  lanceur passe la main ne comparait que les exécutables portant le même nom : un jeu qui
  continue sous un autre nom n'était jamais trouvé et le tunnel tombait. Tout processus
  encore actif depuis le même dossier d'installation compte désormais, et la recherche
  consigne ce qu'elle a cherché.
- **Arrêter ne faisait rien une fois la session terminée.** Fermer le tube vers le processus
  auxiliaire levait une erreur quand l'autre extrémité avait disparu, et cette erreur
  s'échappait du nettoyage — l'application continuait donc de croire qu'une session
  terminée tournait encore.
- **Changer de langue vidait toutes les listes déroulantes** au lieu de les traduire.
  Remplacer le contenu d'une liste efface la sélection, et la liaison réécrivait cette
  sélection vide.
- **Le journal d'activité ne suivait pas les nouvelles lignes.** Il défile maintenant
  jusqu'à la plus récente et cesse de suivre dès que vous remontez pour lire.

### Ajouté

- **Notes de version dans votre langue.** Les journaux traduits sont publiés avec les
  binaires, et la boîte de dialogue de mise à jour affiche celui qui correspond à la langue
  de l'interface.
- **Exporter le rapport d'erreur** : un bouton qui apparaît avec un repère dès que quelque
  chose échoue. Il rassemble l'activité affichée et les fichiers journaux des deux
  processus, pour signaler un problème sans savoir où sont rangés les journaux.
- Le réglage de taille du texte agit désormais sur toute l'interface, pas seulement sur le
  journal.

## [0.4.0] - 2026-09-10

### Ajouté

- **Recherche automatique de mises à jour.** L'application vérifie s'il existe une version
  plus récente sur GitHub et affiche ce qui a changé avant l'installation. Activée par
  défaut ; désactivable dans les paramètres.
- **Boîte de dialogue des paramètres** derrière le bouton en forme de roue dentée :
  langue, taille du texte de l'interface, comportement à la fermeture et recherche de
  mises à jour — pour qu'un choix mémorisé ne soit jamais une impasse.
- **Dix langues supplémentaires** : chinois traditionnel, chinois simplifié, japonais,
  coréen, espagnol, allemand, portugais, russe et italien, en plus de l'anglais et du
  français. L'état, les listes déroulantes, les boîtes de dialogue et le menu de la zone de
  notification sont traduits.
- **Le journal d'activité est désormais du texte sélectionnable**, avec les boutons
  *Tout copier* et *Exporter…*.
- **Taille du texte de l'interface réglable** (10 à 22 pt), conservée d'une session à l'autre.
- **Icône d'application**, utilisée par la fenêtre, la barre des tâches, la zone de
  notification et Ajout/Suppression de programmes.
- **L'installateur demande où installer** et propose le raccourci du menu Démarrer et celui
  du bureau comme deux choix indépendants.

### Corrigé

- **Le VPN se coupait au moment précis où le jeu finissait de charger.** Warcraft III,
  comme la plupart des titres dotés d'un lanceur ou d'un utilitaire de mise à jour, quitte
  son premier processus et passe la main à un autre. Cette sortie était interprétée comme
  « la cible s'est fermée » et le tunnel était démonté au pire moment. La session suit
  désormais l'application à travers ce relais.
- **L'icône de la zone de notification n'apparaissait jamais.** Son handle était détruit
  avant que Windows ne s'en serve, et `Shell_NotifyIcon` avec un handle détruit n'affiche
  rien sans signaler d'erreur.
- **Rouvrir depuis la zone de notification lançait une seconde copie** au lieu de restaurer
  celle en cours. Une seule instance s'exécute désormais par utilisateur, et relancer
  ramène la fenêtre existante au premier plan.
- **`WinDivert64.sys` restait verrouillé après la fermeture.** Fermer les handles du pilote
  ne suffit pas : en ouvrir un enregistre un service noyau qui continue de tourner, et le
  fichier reste verrouillé jusqu'à son arrêt. Le service est maintenant arrêté et supprimé
  à la fin de la session. Fermer la fenêtre arrête également le processus auxiliaire
  privilégié, ce qui n'était pas le cas auparavant.
- **Revenir à « Par défaut du système » pour la langue ne faisait rien.** Le changement
  était annoncé par une seule notification « tout a changé », que WinUI ne traite pas de
  façon fiable ; chaque chaîne est désormais annoncée nommément.
- **La désinstallation demandait un redémarrage.** L'installateur ferme d'abord
  l'application et son auxiliaire, donc aucun fichier ne reste en cours d'utilisation.
- Les lignes du journal portaient la marge d'un élément de liste, laissant une demi-ligne
  vide entre les entrées.

### Modifié

- Les actions liées au journal ont été déplacées dans le panneau d'activité à droite, au
  lieu de figurer en bas de la colonne de configuration.

## [0.3.0] - 2026-09-09

### Ajouté

- Journalisation des erreurs dans `%LOCALAPPDATA%\LanBridge\logs\`, un fichier par
  processus et par exécution, les exceptions non gérées étant interceptées à trois endroits
  et consignées au lieu de faire disparaître l'application en silence.
- Persistance des paramètres : écrits à chaque modification, ils survivent à un plantage ou
  à un arrêt forcé.
- Prise en charge de la zone de notification, avec une question à la fermeture proposant de
  quitter ou de réduire.
- Interface en chinois traditionnel en plus de l'anglais.
- Installateur MSI avec raccourcis, informations de version et entrée dans
  Ajout/Suppression de programmes.

### Corrigé

- La version publiée démarrait puis mourait aussitôt dans le moteur XAML : publier une
  application WinUI non empaquetée n'embarque pas son balisage compilé, et
  `InitializeComponent` n'avait donc rien à charger.

## [0.2.0] - 2026-09-09

### Corrigé

- **La fenêtre n'apparaissait jamais après l'invite d'élévation.** WinUI 3 ne peut pas
  s'exécuter avec des privilèges élevés : l'activation WinRT échoue et le processus se
  termine sans rien afficher. L'interface s'exécute désormais sans privilèges et confie le
  travail privilégié à un processus auxiliaire distinct, qui demande le consentement une
  fois par session. L'interface ne détient aucun privilège et l'auxiliaire ne les conserve
  que pendant la session.

## [0.1.0] - 2026-09-09

### Ajouté

- VPN par application : importe un profil OpenVPN et refuse la route par défaut et le DNS
  poussés par le serveur, de sorte que seul le sous-réseau VPN traverse le tunnel et que
  le reste de la machine conserve son chemin habituel.
- Confinement facultatif par processus via WinDivert, rejetant le trafic vers le
  sous-réseau VPN venant de tout autre processus que la cible.
- Relais de découverte LAN pour les tunnels incapables de porter la diffusion, y compris le
  protocole W3GS de Warcraft III, dont les informations de partie ne sont envoyées qu'en
  réponse unicast et jamais diffusées : il faut donc les demander au jeu local puis les
  relayer.
- Relais générique de diffusion UDP pour d'autres jeux, configuré par port.
- Version en ligne de commande du même moteur.

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
