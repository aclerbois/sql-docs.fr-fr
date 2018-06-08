---
title: Notes de publication Microsoft SQL Studio Operations (version préliminaire) | Documents Microsoft
description: Notes de publication Microsoft SQL Studio Operations (version préliminaire)
ms.custom: tools|sos
ms.date: 05/08/2018
ms.prod: sql
ms.reviewer: alayu; sstein
ms.suite: sql
ms.prod_service: sql-tools
ms.component: sos
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 3f461b78c3d76f7e6b848b83d8a2333dffe5de3c
ms.sourcegitcommit: a9da0abd3e17fbcd6339980d7331d0418cdada53
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2018
---
# <a name="sql-operations-studio-preview-release-notes"></a>Notes de publication SQL opérations Studio (version préliminaire)

**[Télécharger la version préliminaire publique de mai](download.md)**


## <a name="may-2018-may-public-preview"></a>Mai 2018 (version préliminaire publique de mai)

date de publication : le 7 mai 2018  
version : 0.29.3

Le *peut Public Preview* porte sur les correctifs de bogues et de stabilisation. Cette version contient les caractéristiques suivantes :  

- Annonce Redgate SQL recherche les extensions disponibles dans le Gestionnaire d’extensions.
- Localisation de communauté disponible pour 10 langues : allemand, espagnol, Français, italien, japonais, coréen, portugais, russe, chinois simplifié et chinois traditionnel.
- Collection de télémétrie réduite, annulations améliore l’expérience et les liens dans le produit à la déclaration de confidentialité.
- Gestionnaire d’extensions a amélioré Marketplace d’expérience pour découvrir facilement des extensions de la Communauté.
- Travaux d’extension de l’Agent SQL et de l’historique des travaux du mode d’amélioration du produit.
- Met à jour des extensions pour whoisactive et le serveur de rapports.
- Améliorer le défilement de gérer les propriétés du tableau de bord.
- Résoudre les problèmes de GitHub :
   - Corriger [émettre 703](https://github.com/Microsoft/sqlopsstudio/issues/703): l’entrée de texte HTML dans les données de modification entraîne la valeur à afficher incorrectement jusqu'à ce que l’actualisation
   - Corriger [émettre 821](https://github.com/Microsoft/sqlopsstudio/issues/821): dépendance de package sqlopsstudio.deb
   - Corriger [émettre 1260](https://github.com/Microsoft/sqlopsstudio/issues/1260): mot clé 'distinct' pas mis en surbrillance
   - Corriger [émettre 1332](https://github.com/Microsoft/sqlopsstudio/issues/1332): restaurer des données de modification ligne ne fonctionne pas
   - Corriger [émettre 1215](https://github.com/Microsoft/sqlopsstudio/issues/1215): extension de l’Agent SQL et de la barre d’état
   - Corriger [émettre 1316](https://github.com/Microsoft/sqlopsstudio/issues/1316): ne pas de l’Agent SQL redimensionner après modifier la taille de windows


Pour plus d’informations, consultez la [journal des modifications](https://github.com/Microsoft/sqlopsstudio/blob/master/CHANGELOG.md), et [versions](https://github.com/Microsoft/sqlopsstudio/releases).



## <a name="april-2018-april-public-preview"></a>Avril 2018 (avril Public Preview)

date de publication : le 25 avril 2018  
version : 0.28.6

Le *avril Public Preview* contient des correctifs de bogues et améliorations. 

- Améliorations apportées à l’extension de l’aperçu de l’Agent SQL.
- Prise en charge des fichiers volumineux et protégés pour l’enregistrement Admin protégé améliorée et > fichiers 256M Studio des opérations SQL.
- Terminal intégré de fractionnement pour travailler avec plusieurs terminaux ouverts à la fois.
- Installation réduit un fichier sur le disque nombre pied d’impression pour des installations plus rapides et les temps de démarrage.
- Continue à résoudre les problèmes de GitHub :
   - Corriger [émettre 37](https://github.com/Microsoft/sqlopsstudio/issues/37): lors de la visionneuse graphique génère une erreur, un comportement inattendu se produit.
   - Corriger [émettre 462](https://github.com/Microsoft/sqlopsstudio/issues/462): demande de fonctionnalité : Option pour les groupes de serveurs à être développé par défaut.
   - Corriger [émettre 606](https://github.com/Microsoft/sqlopsstudio/issues/606): intellisense - suggestion incorrecte pour la commande « update ».
   - Corriger [émettre 967](https://github.com/Microsoft/sqlopsstudio/issues/967): attendez le plan de requête lorsque vous sélectionnez showplan XML dans la grille de résultats.
   - Corriger [émettre 1023](https://github.com/Microsoft/sqlopsstudio/issues/1023): ajouter des crochets pour l’appel de ms_foreachdb de flyfishingdba.
   - Corriger [émettre 1048](https://github.com/Microsoft/sqlopsstudio/issues/1048): erreur de négociation de préconnexion SSL/TLS.
   - Corriger [émettre 1050](https://github.com/Microsoft/sqlopsstudio/issues/1050): effacer insights afficher avant l’affichage d’erreur.
   - Corriger [émettre 1057](https://github.com/Microsoft/sqlopsstudio/issues/1057): nouvelles actions de requête dans le widget d’explorer et de restauration sont interrompues.
   - Corriger [émettre 1068](https://github.com/Microsoft/sqlopsstudio/issues/1068): sortie de tableau de bord pop-up windows avec le message d’erreur pour la base de données SQL Azure.
   - Corriger [émettre 1069](https://github.com/Microsoft/sqlopsstudio/issues/1069): boîte de dialogue Connexion affiche une erreur de serveur requis lors de l’affichage initial.
   - Corriger [émettre 1070](https://github.com/Microsoft/sqlopsstudio/issues/1070): un double-clic pour développer exigent que les groupes de serveurs.
   - Corriger [émettre 1072](https://github.com/Microsoft/sqlopsstudio/issues/1072): arrière-plan du contrôle de sélection est semi-transparent.
   - Corriger [émettre 1115](https://github.com/Microsoft/sqlopsstudio/issues/1115): corriger contraste élevé tous les problèmes d’accessibilité dans Studio des opérations SQL.
   - Corriger [émettre 1101](https://github.com/Microsoft/sqlopsstudio/issues/1101): Échec de l’Extension au lien de mise à niveau « télécharger manuellement » accède à un emplacement incorrect.
   - Corriger [émettre 1103](https://github.com/Microsoft/sqlopsstudio/issues/1103): défilement V ne fonctionne ne pas dans l’onglet Accueil.
   - Corriger [émettre 1104](https://github.com/Microsoft/sqlopsstudio/issues/1104): onglets de l’extension SQL a cessé de fonctionner.


Une mise en surbrillance significatif pour la version préliminaire publique d’avril est l’actualisation de code source la plateforme 1.21 de Code Visual Studio. Cela permet de bénéficier de plusieurs mises à jour à l’éditeur de base et le banc d’essai à partir du point de 1.19 synchronisation précédente. Voici quelques exemples :

- [Nouvelle interface utilisateur de Notifications](https://code.visualstudio.com/updates/v1_21#_new-notifications-ui) - facilement gérer et passez en revue les notifications Studio des opérations SQL.
- [Intégré fractionnement Terminal](https://code.visualstudio.com/updates/v1_21#_split-terminals) -travailler avec plusieurs terminaux ouverts à la fois.
- [Enregistrer des fichiers volumineux et protégés](https://code.visualstudio.com/updates/v1_20#_save-files-that-need-admin-privileges) - enregistrement Admin protégés et > fichiers 256 M Studio des opérations SQL.
- [Meilleure prise en charge d’un fichier volumineux](https://code.visualstudio.com/updates/v1_21#_text-buffer-improvements) -optimisations de mémoire tampon de texte pour les fichiers volumineux.
- [Recherche de paramètres améliorée](https://code.visualstudio.com/updates/v1_20#_settings-search) - facilement rechercher le paramètre de droite à utiliser la recherche en langage naturel.
- [Extraits de code globales](https://code.visualstudio.com/updates/v1_20#_global-snippets) -créer des extraits de code que vous pouvez utiliser tous les types de fichier.
- [Les sélections multiples Explorer](https://code.visualstudio.com/updates/v1_20#_multi-select-in-the-explorer) -effectuer des actions sur plusieurs fichiers à la fois.
- [Erreurs et avertissements dans l’Explorateur de](https://code.visualstudio.com/updates/v1_20#_error-indicators-in-the-explorer) - rapidement accéder aux erreurs dans votre base de code.
- [Faites glisser et supprimer, copier et coller entre des fenêtres](https://code.visualstudio.com/updates/v1_21#_better-drag-and-drop-support) -déplacer des fichiers entre les fenêtres Studio des opérations SQL.
- [Prise en charge du sous-module GIT](https://code.visualstudio.com/updates/v1_20#_git-submodules) -Git d’effectuer des opérations sur les référentiels Git imbriquées.
- [Prise en charge de l’écran de terminal du lecteur](https://code.visualstudio.com/updates/v1_20#_screen-reader-support) -Terminal Server intégré a désormais le mode de « Écran lecteur optimisé ».
- [Mise en page de l’éditeur centré](https://code.visualstudio.com/updates/v1_21#_centered-editor-layout) -optimiser votre écran d’affichage du code.
- [Résultats de la recherche horizontal (version préliminaire)](https://code.visualstudio.com/updates/v1_21#_horizontal-search) -vous pouvez maintenant afficher les résultats de recherche dans un volet horizontal.

Pour plus d’informations, extraction le [Notes de publication Visual Studio Code février](https://code.visualstudio.com/updates/v1_21)et le [Notes de publication Visual Studio Code janvier](https://code.visualstudio.com/updates/v1_20).

Pour plus d’informations, consultez la [journal des modifications](https://github.com/Microsoft/sqlopsstudio/blob/master/CHANGELOG.md).

## <a name="march-2018-march-public-preview"></a>Mars 2018 (mars version préliminaire publique)

date de publication : le 28 mars 2018  
version : 0.27.3

Le *mars Public Preview* continue à résoudre les problèmes de GitHub supérieur et se concentre sur l’amélioration de notre histoire d’extensibilité. Plus précisément l’activation du Gestionnaire d’extensions, améliorer la gestion du tableau de bord et en fournissant l’Agent SQL et les extensions insights. Cette version inclut les améliorations suivantes :

- Améliorer le modèle d’extensibilité du tableau de bord pour prendre en charge les analyses à onglets et volets de configuration.
   - Gestionnaire d’extensions permet simple acquisition d’extensions.
   - Extensions de tableau de bord pour sp_whoisactive de [whoisactive.com](http://www.whoisactive.com).
   - Pour plus d’informations, consultez [étendre les fonctionnalités des opérations de SQL Studio](extensions.md).
- Ajouter d’autres [API d’extensibilité pour la connexion et l’objet explorer](https://github.com/Microsoft/sqlopsstudio/wiki/Extensibility-API) management.
- Corrigez les clients importants ayant un impact sur [GitHub problèmes](https://github.com/Microsoft/sqlopsstudio/issues).


## <a name="february-2018-february-public-preview"></a>Février 2018 (février version préliminaire publique)

date de publication : le 15 février 2018  
version : 0.26.7

Le *Public Preview de février* inclut des suggestions de fonctionnalités et les correctifs de bogues de priorité élevée. Cette version inclut les améliorations suivantes :

- Présentation d’Installation de la mise à jour automatique, qui fournit une notification lorsqu’une nouvelle version est disponible en téléchargement 
- La boîte de dialogue de connexion 'Database' champ est désormais une liste déroulante remplie dynamiquement qui contient la liste des bases de données rempli à partir du serveur spécifié.
- Corriger [émettre 6](https://github.com/Microsoft/sqlopsstudio/issues/6): conserver la connexion et la base de données sélectionnée lors de l’ouverture de nouveaux onglets de requête.
- Corriger [numéro 22](https://github.com/Microsoft/sqlopsstudio/issues/22): 'Nom_serveur' et 'Nom de la base de données' - peut ces zones de liste déroulante au lieu de zones de texte ?
- Corriger [émettre 549](https://github.com/Microsoft/sqlopsstudio/issues/549): installation sans assistance en mode silencieux/très entraîne l’application qui ouvre après l’installation.
- Corriger [émettre 481](https://github.com/Microsoft/sqlopsstudio/issues/481): ajouter l’option de « Vérifier les mises à jour ».
- Éditeur SQL colorisation et la saisie semi-automatique des correctifs :
   - Corriger [émettre 584](https://github.com/Microsoft/sqlopsstudio/issues/584): mot clé « Complet » pas mis en surbrillance par IntelliSense.
   - Corriger [émettre 345](https://github.com/Microsoft/sqlopsstudio/issues/345): fonctions SQL de colorisation dans l’éditeur.
   - Corriger [émettre 300](https://github.com/Microsoft/sqlopsstudio/issues/300): [#tempData] dernières «] » affiche la couleur verte.
   - Corriger [émettre 225](https://github.com/Microsoft/sqlopsstudio/issues/225): incompatibilité de couleur de mot clé.
   - Corriger [émettre 60](https://github.com/Microsoft/sqlopsstudio/issues/60): sql non valide couleur mise en évidence lors de l’utilisation d’une table temporaire dans la clause from.
- Introduire des API d’extensibilité de connexion.
- Intégration de 1.19 de l’éditeur de Code Visual Studio.
- Mettre à jour les composants JustinPealing/html-plan de requête utilisent plusieurs améliorations de l’Observateur de Plan de requête.


## <a name="january-2018-january-public-preview"></a>Janvier 2018 (janvier version préliminaire publique)

date de publication : le 17 janvier 2018  
version : 0.25.4

Le *préliminaire de janvier* inclut des suggestions de fonctionnalités et les correctifs de bogues de priorité élevée. Cette version inclut les améliorations suivantes :

- Les connexions serveur enregistrées sont disponibles dans la boîte de dialogue de connexion.
- Activer la sortie à chaud. Sortie à chaud est désactivée par défaut, pour permettre de voir [paramètre de sortie à chaud](settings.md#hot-exit).
- La coloration d’onglet basée sur le groupe de serveurs. La coloration d’onglet est désactivé par défaut, pour permettre de voir [onglet paramètre des couleurs](settings.md#tab-color).
- Modification *nom du serveur* à *Server* dans la boîte de dialogue de connexion.
- Résoudre interrompu *exécuter la requête en cours* commande.
- Corrigez la rupture de glisser-déposer bogue de script.
- Correctif incorrect épinglé icône du Menu Démarrer.
- Corrigez le compte Azure manquant icône de marque.


## <a name="december-2017-december-public-preview"></a>Décembre 2017 (préliminaire de décembre)

date de publication : 19 décembre 2017  
version : 0.24.1

Le *préliminaire de décembre* comprend plusieurs correctifs de bogues sur toutes les zones de fonctionnalité, ainsi que les améliorations suivantes :

- Créer boîte de dialogue règle de pare-feu est désormais disponible pour faciliter la connexion à la base de données SQL Azure et Azure SQL Data Warehouse.
- Ajout d’installation de Windows et Linux DEB et RPM packages d’installation.
- Gérer l’éditeur de présentation visuelle du tableau de bord.
- *Script en tant que Alter* et *Script en tant qu’exécutez* commandes.
- *Exécuter la requête en cours avec le Plan réel* commande.
- Intégrer la plateforme de l’éditeur VS Code 1.18.1.
- Activer les fichiers de chargement de version test d’une Extension VSIX.
- Prend en charge la syntaxe d’itération lot « GO N ».


## <a name="november-2017"></a>Novembre 2017

date de publication : 15 novembre 2017  
version : 0.23.6

- Version d’initiale [!INCLUDE[name-sos](../includes/name-sos-short.md)].


## <a name="next-steps"></a>Étapes suivantes

Consultez une des Démarrages rapides suivants pour commencer :
- [Se connecter et de requêtes SQL Server](quickstart-sql-server.md)
- [Se connecter et interroger la base de données SQL Azure](quickstart-sql-database.md)
- [Connecter la re & quête d’entrepôt de données Azure](quickstart-sql-dw.md)

Contribuer à [!INCLUDE[name-sos](../includes/name-sos-short.md)]:
- [https://github.com/Microsoft/sqlopsstudio](https://github.com/Microsoft/sqlopsstudio)
