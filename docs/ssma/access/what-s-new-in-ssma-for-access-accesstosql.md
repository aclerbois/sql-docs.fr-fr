---
title: Nouveautés de SSMA pour Access(AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
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
ms.assetid: a24d3fc0-6911-4bfa-828a-197abf222e02
caps.latest.revision: 37
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: cdd8f3f3898dcfd2f3233132ba7ecb4ea7861ab3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="whats-new-in-ssma-for-access-accesstosql"></a>Nouveautés de SSMA pour Access (AccessToSQL)
Cette rubrique répertorie SSMA pour les modifications d’accès dans chaque version.  

## <a name="ssma-v77"></a>SSMA v7.7
La version v7.7 de SSMA pour Access contient les modifications suivantes :
- SSMA pour l’accès a été améliorée avec des correctifs ciblés améliorent les mesures de qualité et de conversion.
- En fonction de la forte demande, la version 32 bits de SSMA pour Access est de retour. Par rapport à l’implémentation précédente (avant v7.4), il existe deux packages de programme d’installation, mais ils ne peuvent pas être installés côte à côte. Par conséquent, vous devez choisir la version la plus appropriée en fonction des composants de connectivité que vous avez. Il est toujours préférable d’utiliser la version 64 bits, si possible.

> [!IMPORTANT]
> SSMA v7.4 et versions ultérieures, .net 4.5.2 est un préalable de l’installation.

## <a name="ssma-v76"></a>SSMA v7.6
La version de v7.6 de SSMA pour l’accès a été améliorée avec des correctifs ciblés améliorent les mesures de qualité et de conversion et prise en charge de SQL Server 2017 (version préliminaire publique). Prise en charge de 2017 du serveur SQL sur Windows et Linux en version préliminaire publique et ne doit pas être utilisé pour les migrations de production.

> [!IMPORTANT]
> SSMA v7.4 et versions ultérieures, .net 4.5.2 est un préalable de l’installation, et la version 32 bits de l’outil a été supprimée.

## <a name="ssma-v75"></a>Versions 7.5 SSMA
La version de versions 7.5 de SSMA pour l’accès a été améliorée avec plusieurs améliorations pour garantir une accessibilité supérieure aux personnes handicapées.

> [!IMPORTANT]
> .NET 4.5.2 est requis pour l’installation de versions 7.5 SSMA. En outre, à partir de v7.4, la version 32 bits de SSMA est interrompue.

## <a name="ssma-v74"></a>SSMA v7.4
La version v7.4 de SSMA pour Access contient les modifications suivantes :
- Le **délai de requête** option est désormais disponible pendant la détection des objets de schéma à la source et cible.
![option de délai d’attente de requête](../media/query-timeout_red.png)

- Les mesures de qualité et de conversion a été améliorée avec des correctifs ciblés, en fonction des commentaires des clients.

> [!IMPORTANT]
> .NET 4.5.2 est requis pour l’installation de SSMA v7.4. En outre, à partir de v7.4, la version 32 bits de SSMA est interrompue.

## <a name="ssma-v73"></a>SSMA v7.3
La version v7.3 de SSMA pour Access contient les modifications suivantes :
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
La version de v7.2 de SSMA pour Access contient les modifications suivantes :
- Métrique de qualité et de conversion améliorée avec des correctifs ciblés en fonction des commentaires des clients.
- Améliorations de télémétrie pour fournir une meilleure points de données pour résoudre les problèmes de client et d’améliorer le taux de conversion de SSMA.

## <a name="ssma-v71"></a>SSMA v7.1
La version de v7.1 de SSMA pour Access contient les modifications suivantes :
- 2017 du serveur SQL sur Windows et Linux CTP1 est désormais une plateforme cible prise en charge pour la migration. Cette fonctionnalité est en version d’évaluation technique et prend en charge le déplacement de schéma et les données pour les serveurs SQL cible.
- SSMA prend désormais en charge les mises à jour automatiques pour télécharger la dernière version de SSMA dès qu’elles sont disponibles.
- Fichiers binaires installables de SSMA sont maintenant remis via des fichiers de package Windows installer (.msi).

**Ressources**

[Extension des capacités de SQL Server Migration Assistant conversion](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/)
  
## <a name="may-2016"></a>Mai 2016  
La version de mai 2016 de SSMA pour Access contient les modifications suivantes :  
  
-  Nouvelle prise en charge officielle pour SQL Server 2016
-  Supprimé vérification du programme d’installation pour .net 2.0.
-  Fixe « enregistrer le projet » et « ouvrir le projet » des commandes pour la Console de SSMA.
-  Commande securepassword « fixe » pour la Console de SSMA.
-  Correction de comptage des objets pour le chargement initial.
-  Chargement de données tables fixe pour les onglets de l’interface utilisateur pour l’accès.
-  Résolution du bogue dans les paramètres globaux. 
   
## <a name="march-2016"></a>Mars 2016  
La version préliminaire de mars 2016 de SSMA pour Access contient les modifications suivantes :  
  
-  Prise en charge supplémentaire pour la migration vers SQL Server 2016.  
   
## <a name="january-2016"></a>Janvier 2016  
La version de maintenance de janvier 2016 de SSMA pour Access contient les modifications suivantes :  
  
-  Fixe de fonction non valide pour la valeur par défaut d’un champ GUID (RFC 3894811).  
-  Blocage fixe sur l’importation d’enregistrements à la base de données SQL (Azure) (RFC 4919573).  
-  Élément de Menu ajouté vue journal de SSMA (RFC 5706203).  
-  Ajout de télémétrie.  
  
## <a name="july-2014"></a>Juillet 2014  
La version de juillet 2014 de SSMA pour Access contient les modifications suivantes :  
  
-   Conversion de code de base de données SQL Azure amélioré.  
-   Déplacé les fonctionnalités du Pack d’extension vers le schéma pour prendre en charge de la base de données SQL Azure.  
-   Améliorations des performances testées pour les bases de données avec plus de 10 Ko d’objets.  
-   Améliorations de l’interface utilisateur ajoutées pour le traitement d’un grand nombre d’objets.  
-   Prise en charge supplémentaire pour mettre en surbrillance des schémas « connues » de LOB (afin qu’ils peuvent être ignorés lors de la conversion).  
-   Ajouté des améliorations de la vitesse de conversion.
-   Compte de la prise en charge ajoutée pour afficher des objets dans l’interface utilisateur.
-   Taille de rapport réduite de plus de 25 %.
-   Messages d’erreur améliorés pour les constructions non analysées.  
  
## <a name="april-2014"></a>Avril 2014  
La version d’avril 2014 de SSMA pour Access contient les modifications suivantes :  
  
-   Prise en charge supplémentaire pour MS SQL Server 2014.  
-   Bogues résolus concernant la conversion vers Azure.  
-   Bogues résolus en ce qui concerne les pages du rapport invisible dans Internet Explorer 10.  
  
## <a name="january-2012"></a>Janvier 2012  
La version de janvier 2012 de SSMA pour Access contient les modifications suivantes :  
  
-   Fourni à l’option de ne conserver pas le nom d’utilisateur et mot de passe pour les tables liées de MS Access après la migration.  
-   Définir les actions cascade des références circulaires No Action.  
-   Condition des messages appropriés indiquant les actions en cascade pour les références circulaires ont été définis sur No Action.  
  
## <a name="july-2011"></a>Juillet 2011  
La version de juillet 2011 de SSMA pour Access contient les modifications suivantes :  
  
-   Amélioration signalement d’erreurs pendant la migration des données.  
  
## <a name="april-2011"></a>Avril 2011  
La version d’avril 2011 de SSMA pour Access contient les modifications suivantes :  
  
-   Ajouté une seule installation de « SSMA pour Access », qui prend en charge [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] « Denali » et SQL Azure.  
-   Ajouté la possibilité de connecter [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] « Denali ».  
-   SSMA ajouté pour la version de la Console d’accès prend en charge pour la compatibilité descendante. Vous pouvez ouvrir les projets créés avec les versions antérieures à SSMA v5.0.
-   Ajouté la possibilité d’installer le produit SSMA v5.0 côte à côte (SxS) avec les versions antérieures du produit de SSMA.  
  
## <a name="july-2010"></a>Juillet 2010  
La version de juillet 2010 de SSMA pour Access contient les modifications suivantes :  
  
-   Prise en charge supplémentaire pour la migration vers SQL Server 2008 R2 et SQL Azure.
-   Ajouter une connexion sécurisée à SQL Server et SQL Azure.  
-   Prise en charge supplémentaire pour les bases de données Access 2010.
-   Ajouter une nouvelle application de Console de SSMA pour l’exécution de ligne de commande.
-   Prise en charge supplémentaire pour le type de données DateTime2 SQL Server.
  
## <a name="june-2008"></a>Juin 2008  
La version de juin 2008 de SSMA pour Access contient les modifications suivantes :  
  
-   Prise en charge supplémentaire pour les bases de données Access 2007.  
  
## <a name="may-2007"></a>Mai 2007  
La version de mai 2007 de SSMA pour Access contient les modifications suivantes :  
  
-   Prise en charge supplémentaire pour les bases de données Access qui utilisent des stratégies de groupe de travail.  
-   Fourni la possibilité de supprimer les objets convertis à partir de l’Explorateur de métadonnées SQL Server.  
-   Prise en charge supplémentaire pour les commentaires d’entré par l’utilisateur dans SQL Server au format mode SQL.  
-   Améliorations apportées dans la conversion de l’objet.  
  
## <a name="november-2006"></a>Novembre 2006  
La version de novembre 2006 de SSMA pour Access contient les modifications suivantes :  
  
-   Ajouter un nouvel Assistant de Migration de base de données qui vous guide à travers de la migration d’une base de données à partir de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
-   Ajouter une nouvelle conversion, charge, et commande de migration qui convertit les bases de données Access, charge les objets convertis dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], et migre les données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] tout en une seule étape.  
-   Migration de requêtes amélioré. Migration de requête maintenant convertit sélectionner plus de requêtes à des vues. Pour plus d’informations, consultez [convertir des objets de base de données Access](http://msdn.microsoft.com/en-us/e0ef67bf-80a6-4e6c-a82d-5d46e0623c6c).  
-   Ajouté la possibilité de modifier des propriétés de table et d’index sur la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **Table** onglet.  
-   Ajouter de nouveaux paramètres globaux :  
    -   Vous pouvez choisir d’afficher les numéros de ligne dans les fenêtres de l’éditeur.  
    -   Vous pouvez configurer SSMA pour inviter à remplacer les objets en double ou jamais, ou toujours remplacer les objets en double lors de la conversion de schéma.  
-   Ajouter une nouvelle option de conversion qui vous permet de spécifier si SSMA affiche un avertissement lorsqu’une requête complexe contient un caractère générique.  
  
## <a name="july-2006"></a>Juillet 2006  
La version de juillet 2006 de SSMA pour Access était la version initiale.
