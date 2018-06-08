---
title: Nouveautés de SSMA pour SAP ASE (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 2be0cf8d-6dbe-443a-abbd-036249922205
caps.latest.revision: 21
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 86a54f8dce44607b91437a89043ff98fdd670ad5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="whats-new-in-ssma-for-sap-ase-sybasetosql"></a>Nouveautés de SSMA pour SAP ASE (SybaseToSQL)
Cette rubrique répertorie SSMA pour SAP ASE (anciennement SSMA pour Sybase) les modifications dans chaque version. 

## <a name="ssma-v77"></a>SSMA v7.7
La version v7.7 de SSMA pour SAP ASE contient les modifications suivantes :
- SSMA pour SAP ASE a été améliorée avec des correctifs ciblés améliorent les mesures de qualité et de conversion.
- En fonction de la forte demande, la version 32 bits de SSMA pour SAP ASE est de retour. Par rapport à l’implémentation précédente (avant v7.4), il existe deux packages de programme d’installation, mais ils ne peuvent pas être installés côte à côte. Par conséquent, vous devez choisir la version la plus appropriée en fonction des composants de connectivité que vous avez. Il est toujours préférable d’utiliser la version 64 bits, si possible.

> [!IMPORTANT]
> SSMA v7.4 et versions ultérieures, .net 4.5.2 est un préalable de l’installation.

## <a name="ssma-v76"></a>SSMA v7.6
La version v7.6 de SSMA pour SAP ASE contient les modifications suivantes :
- SSMA pour SAP ASE a été améliorée avec des correctifs ciblés améliorent les mesures de qualité et de conversion et prise en charge de SQL Server 2017 (version préliminaire publique). Prise en charge de 2017 du serveur SQL sur Windows et Linux en version préliminaire publique et ne doit pas être utilisé pour les migrations de production.
- SSMA pour SAP ASE a été mis à jour pour prendre en charge pour la conversion des fonctions de Sybase.

> [!IMPORTANT]
> SSMA v7.4 et versions ultérieures, .net 4.5.2 est un préalable de l’installation, et la version 32 bits de l’outil a été supprimée.

## <a name="ssma-v75"></a>Versions 7.5 SSMA
La version de versions 7.5 de SSMA pour SAP ASE contient les modifications suivantes :
-   Amélioré avec plusieurs améliorations pour garantir une accessibilité supérieure aux personnes handicapées.
-   Mise à jour pour prendre en charge pour la syntaxe de créer ou remplacer.

> [!IMPORTANT]
> .NET 4.5.2 est requis pour l’installation de versions 7.5 SSMA. En outre, à partir de v7.4, la version 32 bits de SSMA est interrompue.  

## <a name="ssma-v74"></a>SSMA v7.4
La version v7.4 de SSMA pour Sybase contient les modifications suivantes :
- Le **délai de requête** option est désormais disponible pendant la détection des objets de schéma à la source et cible.
![option de délai d’attente de requête](../media/query-timeout_red.png)
- Les mesures de qualité et de conversion a été améliorée avec des correctifs ciblés, en fonction des commentaires des clients.

> [!IMPORTANT]
> .NET 4.5.2 est requis pour l’installation de SSMA v7.4. En outre, à partir de v7.4, la version 32 bits de SSMA est interrompue.  

## <a name="ssma-v73"></a>SSMA v7.3
La version v7.3 de SSMA pour Sybase contient les modifications suivantes :
- Métrique de qualité et de conversion améliorée avec des correctifs ciblés en fonction des commentaires des clients.
- Infrastructure d’extensibilité SSMA exposé via les éléments suivants :
  - Fonctionnalité d’exportation à un projet SQL Server Data Tools (SSDT).
    -   Vous pouvez désormais exporter des scripts de schéma à partir de SSMA pour un projet SSDT. Vous pouvez utiliser les scripts de schéma pour apporter des modifications de schéma supplémentaires et de déployer votre base de données.
![Enregistrer en tant que commande de projet SSDT](../media/export-schema-scripts_red.png)
  - Bibliothèques peuvent être consommées par SSMA pour effectuer des conversions personnalisées.
    - Vous pouvez maintenant construire le code qui peut gérer les conversions de syntaxe personnalisée et les conversions qui n’ont pas été précédemment traitées par SSMA.
      - Obtenir des instructions sur la création d’un convertisseur personnalisé sont disponibles dans ce billet de blog, [fonctions de conversion d’extension Assistant Migration SQL Server](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/).
      - Exemple de projet pour la conversion peut être le télécharger [billet de blog](https://blogs.msdn.microsoft.com/datamigration/ssmafororacleconversionsample/).

## <a name="ssma-v72"></a>SSMA v7.2
La version v7.2 de SSMA pour Sybase contient les modifications suivantes :
- Métrique de qualité et de conversion améliorée avec des correctifs ciblés en fonction des commentaires des clients.
- Améliorations de télémétrie pour fournir une meilleure points de données pour résoudre les problèmes de client et d’améliorer le taux de conversion de SSMA.

## <a name="ssma-v71"></a>SSMA v7.1
La version de v7.1 de SSMA pour Access contient les modifications suivantes :
- 2017 du serveur SQL sur Windows et Linux CTP1 est désormais une plateforme cible prise en charge pour la migration. Cette fonctionnalité est en version d’évaluation technique et prend en charge le déplacement de schéma et les données pour les serveurs SQL cible.
- SSMA prend désormais en charge les mises à jour automatiques pour télécharger la dernière version de SSMA dès qu’elles sont disponibles.
- Fichiers binaires installables de SSMA sont maintenant remis via des fichiers de package Windows installer (.msi).

**Ressources**

[Extension des capacités de SQL Server Migration Assistant conversion](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/)

[Évaluer et migrer des données à partir de plateformes de données non Microsoft pour SQL Server *(avec un exemple d’Oracle)*](https://blogs.msdn.microsoft.com/datamigration/2016/11/16/sql-server-migration-assistant-how-to-assess-and-migrate-databases-from-non-microsoft-data-platforms-to-sql-server/) 

## <a name="may-2016"></a>Mai 2016  
La version de mai 2016 de SSMA pour Sybase contient les modifications suivantes :  

-  Ajout de la prise en charge pour SQL Server 2016.
-  Supprimé vérification du programme d’installation pour .net 2.0.
-  Dépendance Extension Pack mis à jour à partir de .net 3.5 pour .net 4.0.
-  Fixe « enregistrer le projet » et « ouvrir le projet » des commandes pour la Console de SSMA.
-  Commande securepassword « fixe » pour la Console de SSMA.
-  Correction de comptage des objets pour le chargement initial.
-  Résolution du bogue dans les paramètres globaux.

## <a name="march-2016"></a>Mars 2016  
La version préliminaire de mars 2016 de SSMA pour Sybase contient les modifications suivantes :  
  
-  Prise en charge supplémentaire pour la migration vers SQL Server 2016.  
  
## <a name="january-2016"></a>Janvier 2016  
La version de maintenance de janvier 2016 de SSMA pour Sybase contient les modifications suivantes :  
  
-  Élément de Menu ajouté vue journal de SSMA (RFC 5706203).  
-  Ajout de télémétrie. 
  
## <a name="july-2014"></a>Juillet 2014  
La version de juillet 2014 de SSMA pour Sybase contient les modifications suivantes :  
  
-  Conversion de code de base de données SQL Azure amélioré.  
-  Déplacé les fonctionnalités du Pack d’extension vers le schéma pour prendre en charge de la base de données SQL Azure.  
-  Améliorations de performances ajoutés testé pour les bases de données avec plus de 10k objets.  
-  Améliorations de l’interface utilisateur ajoutées pour le traitement d’un grand nombre d’objets.  
-  Ajouté la possibilité de mettre en surbrillance les schémas LOB « connues » (afin qu’ils peuvent être ignorés lors de la conversion).  
-  Ajouté des améliorations de la vitesse de conversion.  
-  Ajouté la possibilité d’afficher le nombre d’objets dans l’interface utilisateur. 
-  Taille de rapport réduite de plus de 25 %.  
-  Messages d’erreur améliorés pour les constructions non analysées.  
  
## <a name="april-2014"></a>Avril 2014  
La version d’avril 2014 de SSMA pour Sybase contient les modifications suivantes :  
  
-   Prise en charge de Microsoft SQL Server 2014.  
-   Bogues résolus concernant la conversion vers Azure.  
-   Bogues résolus en ce qui concerne les pages du rapport invisible dans Internet Explorer 10.  
  
## <a name="january-2012"></a>Janvier 2012  
La version de janvier 2012 de SSMA pour Sybase contient les modifications suivantes :  
  
-   Prise en charge supplémentaire pour la conversion de déclencheur de restauration.  
-   Fourni un correctif pour la conversion@ROWCOUNT et @@ERROR dans la même instruction SET.  
  
## <a name="july-2011"></a>Juillet 2011  
La version de juillet 2011 de SSMA pour Sybase contient les modifications suivantes :  
  
-   Amélioration signalement d’erreurs pendant la migration des données.  
  
## <a name="april-2011"></a>Avril 2011  
La version d’avril 2011 de SSMA pour Sybase contient les modifications suivantes :  
  
-   Produit consolidé « De SSMA pour Sybase », qui prend en charge [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] « Denali » et SQL Azure.  
-   Prise en charge pour la connexion et la migration vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] « Denali ».  
-   Ajout d’une nouvelle fonctionnalité pour convertir et migrer des bases de données Sybase vers SQL Azure.  
-   Moteur de migration améliorée des données côté client, prise en charge parallèle migration des données.  
-   Performances de migration de données améliorées avec Simple et en bloc connecté des modes de récupération.  
-   Ajouté la possibilité de convertir correctement et de migrer des bases de données Sybase respectant la casse à la casse de SQL Server.  
-   Prise en charge pour la conversion des instructions de jointure Non ANSI Sybase ASE aux instructions de jointure ANSI du serveur SQL a été étendue pour les instructions DELETE et UPDATE.  
-   Fournir des options de connectivité supplémentaire pour se connecter aux serveurs de Sybase ASE à l’aide du fournisseur de Sybase ASE ODBC et les fournisseurs de Sybase ASE ADO.Net.  
-   Supprimer la dépendance sur une base de données distinct appelé **SysDB**, qui contient les fonctions d’émulation de Sybase (installées en tant que partie du Pack d’Extension).  
-   Permet également d’installer SSMA pour Sybase Extension Pack sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] clusters.  
-   Ajouté la compatibilité descendante des projets créés dans les versions antérieures de SSMA (version 4.0 et v4.2).  
-   Ajouté la possibilité d’installer SSMA pour Sybase v5.0 produit côte-à-côte (à côte SxS) avec les versions antérieures de SSMA (version 4.0 et v4.2).  
  
## <a name="july-2010"></a>Juillet 2010  
La version de juillet 2010 de SSMA pour Sybase contient les modifications suivantes :  
  
-   Prise en charge supplémentaire pour la migration vers SQL Server 2008 R2.  
-   Ajouter une nouvelle application de Console de SSMA pour l’exécution de ligne de commande.  
-   Prise en charge supplémentaire pour la Migration de données à l’aide à la fois les moteurs de la Migration des données côté Client et côté serveur.  
-   Prise en charge supplémentaire pour l’instruction de « SELECT personnalisée » dans la migration des données.  
-   Prise en charge supplémentaire pour la migration à partir de Sybase ASE 15.0.3 et 15.5.  
  
## <a name="june-2008"></a>Juin 2008  
La version de juin 2008 de SSMA pour Sybase contient les modifications suivantes :  
  
-   Ajouté SSMA Tester, qui teste automatiquement la conversion des objets de base de données et la migration des données effectuées par SSMA. Une fois que toutes les étapes de migration de SSMA sont terminées, utilisez SSMA testeur pour vérifier que les objets convertis fonctionnent de manière identique et que toutes les données a été transféré correctement.  
-   Conversion de versions antérieures à SQL ajoutée. Utilisateur peut désormais spécifier une table temporaire (et tout autre objet) déclarations pour chaque procédure source à utiliser lors de la conversion.  
-   Améliorations apportées dans la conversion de l’objet :  
    -   Joint la conversion révisée.  
    -   Agrégats et des non-agrégats sans avaient/groupe par clauses.  
    -   La fonction IDENTITY avec une instruction SELECT INTO.  
    -   Contraintes en cluster et des index sur les données uniquement verrouillé.  
    -   Tables temporaires créées par SELECT INTO.  
    -   Contraintes / d’index pour les tables temporaires.  
    -   Nouvelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008 datetime pris en charge.  
    -   Sybase 15.0 connectivité et des types de données prise en charge.  
  
## <a name="may-2007"></a>Mai 2007  
La version de mai 2007 de SSMA pour Sybase contient les modifications suivantes :  
  
-   Ajouté la possibilité de charger le contenu de la base de données plus rapide lors de l’enregistrement d’un projet.  
-   Prise en charge supplémentaire pour les commentaires d’entré par l’utilisateur dans SQL Server au format mode SQL.  
-   Améliorations apportées dans la conversion de l’objet.  
  
Le fichier d’aide n’est pas mis à jour pour cette version. Pour plus d’informations, consultez la section Notes de la Documentation plus loin dans cette rubrique.  
  
## <a name="november-2006"></a>Novembre 2006  
La version de novembre 2006 de SSMA pour Sybase contient les modifications suivantes :  
  
-   Ajouter de nouveaux paramètres globaux :  
    -   Vous pouvez choisir d’afficher les numéros de ligne dans les fenêtres de l’éditeur.  
    -   Vous pouvez configurer SSMA pour inviter à remplacer les objets en double ou jamais, ou toujours remplacer les objets en double lors de la conversion de schéma.  
-   Ajouter une nouvelle option de conversion qui vous permet de configurer comment SSMA gère les situations suivantes :  
    -   Une instruction CAST ou CONVERT qui contient une chaîne binaire.  
    -   Recherche de valeurs null dans des expressions d’égalité.  
    -   Tables de proxy.  
    -   Numéros d’erreur message utilisateur pour RAISERROR.  
    -   Mettre à jour les instructions qui contiennent des identificateurs non résolus.  
-   Une nouvelle option de migration qui vous permet de spécifier comment SSMA doit gérer les dates qui sont en dehors de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] plage de dates.  
-   Ajouté une **SQL de mise en forme** définition sur le **SQL** onglet, qui met en forme le code pour une meilleure lisibilité.  
-   Correctifs de bogues qui incluent les éléments suivants :  
    -   SSMA convertit maintenant le verrou de TABLE *table* IN {SHARED | Instructions en MODE exclusif} en ajoutant un indicateur TABLOCK ou TABLOCKX à la requête SELECT suivante sur la table.  
    -   Les conversions nécessaires sont maintenant ajoutées quand des types binaires sont utilisés dans les expressions de caractères.  
    -   Améliorations des performances et de mémoire.  
  
## <a name="july-2006"></a>Juillet 2006  
La version de juillet 2006 de SSMA pour Sybase était la version initiale.  
  
## <a name="see-also"></a>Voir aussi  
[Prise en main de SSMA pour Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/getting-started-with-ssma-for-sybase-sybasetosql.md)
