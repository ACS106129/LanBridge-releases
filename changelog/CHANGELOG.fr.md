# Journal des modifications

Toutes les modifications notables de LanBridge sont consignées ici.

Le format suit [Keep a Changelog](https://keepachangelog.com/fr/1.1.0/) et la
numérotation suit [le versionnage sémantique](https://semver.org/lang/fr/).

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

[0.5.3]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.3
[0.5.2]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.2
[0.5.1]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.1
[0.5.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.0
[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0
