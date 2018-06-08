---
title: Nouveautés de SSMA pour MySQL (MySQLToSql) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
ms.custom: ''
ms.date: 03/01/2018
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 1451a0b0-6713-4d0c-954f-ea3d8fce1d31
caps.latest.revision: 21
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 6e245e45ceec31cfbff4033c71b1f609f578ff58
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="whats-new-in-ssma-for-mysql-mysqltosql"></a>Nouveautés de SSMA pour MySQL (MySQLToSql)
Cette rubrique répertorie SSMA pour que les modifications de MySQL dans chaque version. 

## <a name="ssma-v77"></a>SSMA v7.7
La version v7.7 de SSMA pour MySQL contient les modifications suivantes :
- SSMA pour MySQL a été améliorée avec des correctifs ciblés améliorent les mesures de qualité et de conversion.
- En fonction de la forte demande, la version 32 bits de SSMA pour MySQL est de retour. Par rapport à l’implémentation précédente (avant v7.4), il existe deux packages de programme d’installation, mais ils ne peuvent pas être installés côte à côte. Par conséquent, vous devez choisir la version la plus appropriée en fonction des composants de connectivité que vous avez. Il est toujours préférable d’utiliser la version 64 bits, si possible.
- SSMA pour MySQL est maintenant en mode de connexion de chaîne de connexion ODBC, qui vous permet d’utiliser des pilotes ODBC tiers compatible avec MySQL.

> [!IMPORTANT]
> SSMA v7.4 et versions ultérieures, .net 4.5.2 est un préalable de l’installation.

## <a name="ssma-v76"></a>SSMA v7.6
La version v7.6 de SSMA pour MySQL a été améliorée avec des correctifs ciblés améliorent les mesures de qualité et de conversion et prise en charge de SQL Server 2017 (version préliminaire publique). Prise en charge de 2017 du serveur SQL sur Windows et Linux en version préliminaire publique et ne doit pas être utilisé pour les migrations de production.

> [!IMPORTANT]
> SSMA v7.4 et versions ultérieures, .net 4.5.2 est un préalable de l’installation, et la version 32 bits de l’outil a été supprimée.

## <a name="ssma-v75"></a>Versions 7.5 SSMA
La version de versions 7.5 de SSMA pour MySQL a été améliorée avec plusieurs améliorations pour garantir une accessibilité supérieure aux personnes handicapées.

> [!IMPORTANT]
> .NET 4.5.2 est requis pour l’installation de versions 7.5 SSMA. En outre, à partir de v7.4, la version 32 bits de SSMA est interrompue.

## <a name="ssma-v74"></a>SSMA v7.4
La version v7.4 de SSMA pour MySQL contient les modifications suivantes :
- Le **délai de requête** option est désormais disponible pendant la détection des objets de schéma à la source et cible.
![option de délai d’attente de requête](../media/query-timeout_red.png)
- Les mesures de qualité et de conversion a été améliorée avec des correctifs ciblés, en fonction des commentaires des clients.

> [!IMPORTANT]
> .NET 4.5.2 est requis pour l’installation de SSMA v7.4. En outre, à partir de v7.4, la version 32 bits de SSMA est interrompue. 

## <a name="ssma-v73"></a>SSMA v7.3
La version v7.3 de SSMA pour MySQL contient les modifications suivantes :
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
La version v7.2 de SSMA pour MySQL contient les modifications suivantes :
- Métrique de qualité et de conversion améliorée avec des correctifs ciblés en fonction des commentaires des clients.
- Améliorations de télémétrie pour fournir une meilleure points de données pour résoudre les problèmes de client et d’améliorer le taux de conversion de SSMA.

## <a name="ssma-v71"></a>SSMA v7.1
La version de v7.1 de SSMA pour Access contient les modifications suivantes :
- 2017 du serveur SQL sur Windows et Linux CTP1 est désormais une plateforme cible prise en charge pour la migration. Cette fonctionnalité est dans technical preview et permet le déplacement de schéma et les données pour les serveurs de SQL Server cible.
- SSMA prend désormais en charge les mises à jour automatiques pour télécharger la dernière version de SSMA dès qu’elles sont disponibles.
- Fichiers binaires installables de SSMA sont maintenant remis via des fichiers de package Windows installer (.msi).

**Ressources**

[Extension des capacités de SQL Server Migration Assistant conversion](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/)

[Évaluer et migrer des données à partir de plateformes de données non Microsoft pour SQL Server *(avec un exemple d’Oracle)*](https://blogs.msdn.microsoft.com/datamigration/2016/11/16/sql-server-migration-assistant-how-to-assess-and-migrate-databases-from-non-microsoft-data-platforms-to-sql-server/) 

## <a name="may-2016"></a>Mai 2016  
La version de mai 2016 de SSMA pour MySQL contient les modifications suivantes :.

-  Ajout de la prise en charge pour SQL Server 2016.
-  Analyseur amélioré et le programme de résolution.
-  Supprimé vérification du programme d’installation pour .net 2.0.
-  Dépendance Extension Pack mis à jour à partir de .net 3.5 pour .net 4.0.
-  Par défaut fixe typemapping de BigInt pour MySql.
-  Fixe « enregistrer le projet » et « ouvrir le projet » des commandes pour la Console de SSMA.
-  Commande securepassword « fixe » pour la Console de SSMA.
-  Correction de comptage des objets pour le chargement initial.
-  Objets MsSql fixes du chargement.
-  Résolution du bogue dans les paramètres globaux.
 
## <a name="march-2016"></a>Mars 2016  
La version préliminaire de mars 2016 de SSMA pour MySQL contient les modifications suivantes :  
  
-  Prise en charge supplémentaire pour la migration vers SQL Server 2016. 
  
## <a name="january-2016"></a>Janvier 2016  
La version de maintenance de janvier 2016 de SSMA pour MySQL contient les modifications suivantes :  
  
-  Élément de Menu ajouté vue journal de SSMA (RFC 5706203).  
-  Ajout de télémétrie.  
  
## <a name="july-2014"></a>Juillet 2014  
La version de juillet 2014 de SSMA pour MySQL contient les modifications suivantes :  
  
-  Conversion de code de base de données SQL Azure amélioré. 
-  Fonctionnalités du Pack d’extension est déplacé vers le schéma pour prendre en charge de la base de données SQL Azure.  
-  Améliorations des performances testées pour les bases de données avec plus de 10k objets.  
-  Améliorations de l’interface utilisateur pour le traitement d’un grand nombre d’objets.  
-  Mise en surbrillance des schémas LOB « connues » (afin qu’ils peuvent être ignorés lors de la conversion).  
-  Améliorations de la vitesse de conversion.  
-  Afficher les nombres de l’objet dans l’interface utilisateur.  
-  Réduction de taille de rapport en plus de 25 %.  
-  Messages d’erreur améliorés pour les constructions non analysées.  
  
## <a name="april-2014"></a>Avril 2014  
La version de juillet 2011 de SSMA pour MySQL contient les modifications suivantes :  
  
-  Prise en charge de Microsoft SQL Server 2014.  
-  Bogues résolus concernant la conversion vers Azure  
-  Bogues résolus en ce qui concerne les pages du rapport invisible dans Internet Explorer 10.  
  
## <a name="july-2011"></a>Juillet 2011  
La version de juillet 2011 de SSMA pour MySQL contient les modifications suivantes :  
  
-   Prise en charge pour la conversion de limite à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] décalage de « Denali ».  
-   Amélioration signalement d’erreurs pendant la migration des données.  
  
## <a name="april-2011"></a>Avril 2011  
La version d’avril 2011 de SSMA pour MySQL contient les modifications suivantes :  
  
-   Installable de « SSMA pour MySQL », qui prend en charge un seul [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] « Denali » et SQL Azure.  
-   La possibilité de connecter [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] « Denali ».  
-   Moteur de migration améliorée des données côté client, prise en charge parallèle migration des données.  
-   Performances de migration de données améliorées avec Simple et en bloc connecté des modes de récupération.  
-   SSMA pour la version de la Console de MySQL prend en charge la compatibilité descendante. Vous pouvez ouvrir les projets créés avec les versions antérieures à SSMA v5.0.  
-   SSMA pour MySQL v5.0 produit peut être installé côte à côte (SxS) avec les versions antérieures du produit de SSMA.  
  
## <a name="july-2010"></a>Juillet 2010  
La version de juillet 2010 de SSMA pour MySQL contient les fonctionnalités suivantes :  
  
1.  **Améliorations de l’Interface utilisateur :**  
  
    -   Onglet « Modes SQL » pour les objets de base de données MySQL  
    -   Onglet « Paramètres » pour les objets de base de données MySQL  
    -   Onglet de « Données » pour les Tables de MySQL  
    -   Paramètres de projet mis à jour dans les Pages de Migration et de Conversion  
    -   « Paramètres de Migration de données » au niveau Table  
  
2.  **Améliorations apportées à se connecter à SQL Server et MySQL :**  
  
    -   Connectivité SSL dans MySQL  
    -   Connectivité cryptée dans SQL Server  
  
3.  **Améliorations apportées à l’Explorateur de la métabase de MySQL :**  
  
    -   Lors du chargement de tous les objets de base de données MySQL et leurs onglets respectifs.  
  
4.  **Améliorations de la Conversion de l’objet :**  
  
    -   Conversion des objets de la métabase de MySQL – procédures, fonctions, vues, déclencheurs et les instructions.  
    -   Prise en charge limitée pour les Types de données spatiales dans les Tables.  
    -   Option permettant de convertir les fonctions de MySQL pour les procédures stockées SQL Server  
    -   Option pour appliquer le mappage de Modes SQL et le jeu de caractères lors de la Conversion de l’objet  
  
5.  **Améliorations de la Migration des données :**  
  
    -   Prise en charge la Migration des données à l’aide à la fois les moteurs de la Migration des données côté Client et côté serveur  
    -   Prise en charge pour la Migration des données spatiales  
    -   SQL personnalisé pour la Migration des données pour les Tables  
  
6.  **SSMA pour MySQL Console :**  
  
    -   Fonctionnalité prise en charge de la Console de SSMA pour MySQL  
    -   Prise en charge pour l’interface de niveau de Script  
  
## <a name="january-2010"></a>Janvier 2010  
La version de janvier 2010 de SSMA pour MySQL était la version initiale. Il contenue les fonctionnalités suivantes :  
  
-   Prise en charge pour la migration vers les deux local SQL Server et SQL Azure.  
  
-   **Instantané de la fonctionnalité :** migration des schémas et des données de Tables/index/contraintes MySQL.
