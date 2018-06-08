---
title: Description des index cluster et non-cluster | Microsoft Docs
ms.custom: ''
ms.date: 11/28/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- query optimizer [SQL Server], index usage
- index concepts [SQL Server]
ms.assetid: b7d6b323-728d-4763-a987-92e6292f6f7a
caps.latest.revision: 36
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 8780ec7ab94ae66f4ba76133fcc884cad2456522
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clustered-and-nonclustered-indexes-described"></a>Description des index cluster et non-cluster
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

 > Pour accéder au contenu relatif aux versions précédentes de SQL Server, consultez [Description des index cluster et non-cluster](https://msdn.microsoft.com/en-US/library/ms190457(SQL.120).aspx).

  Un index est une structure sur disque associée à une table ou une vue qui accélère l'extraction des lignes de la table ou de la vue. Il contient des clés créées à partir d'une ou plusieurs colonnes de la table ou de la vue. Ces clés sont stockées dans une structure (B-tree) qui permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de trouver rapidement et efficacement la ou les lignes associées aux valeurs de clé.  
  
 Une table ou une vue peut comporter les types d'index suivants :  
  
-   Cluster  
  
    -   Les index cluster trient et stockent les lignes de données de la table ou la vue en fonction de leurs valeurs de clé. Ce sont les colonnes incluses dans la définition de l'index. Il ne peut y avoir qu’un index cluster par table car les lignes de données peuvent être stockées dans un seul ordre.  
  
    -   Le seul moment où les lignes de données d'une table sont stockées dans l'ordre de tri se produit lorsque la table contient un index cluster. Lorsqu'une table possède un index cluster, elle est appelée table cluster. En l'absence de cet index, ses lignes de données sont stockées dans une structure désordonnée nommée segment.  
  
-   Non-cluster  
  
    -   Les index non-cluster possèdent une structure distincte de celle des lignes de données. Un index non-cluster contient les valeurs de clé de l'index non-cluster et chaque entrée de valeur de clé dispose d'un pointeur vers les lignes de données qui contient la valeur de clé.  
  
    -   Le pointeur situé entre une ligne d'un index non-cluster et une ligne de données est appelé un localisateur de ligne. La structure d'un localisateur de ligne varie selon que les pages de données sont stockées dans un segment de mémoire ou dans une table cluster. Pour un segment de mémoire, un localisateur de ligne est un pointeur vers la ligne. Pour une table cluster, le localisateur de ligne est la clé de l'index cluster.  
  
    -   Vous pouvez ajouter des colonnes sans clé au niveau du nœud terminal de l’index non cluster pour contourner les limites de clé d’index existantes, et exécuter des requêtes indexées totalement couvertes. Pour plus d’informations, consultez [Créer des index avec colonnes incluses](../../relational-databases/indexes/create-indexes-with-included-columns.md). Pour plus d’informations sur les limites de clé d’index, consultez [Spécifications des capacités maximales pour SQL Server](../../sql-server/maximum-capacity-specifications-for-sql-server.md). 
  
 Tant les index cluster que les index non-cluster peuvent être uniques. En d'autres termes, deux lignes ne peuvent pas avoir la même valeur de clé d'index. Dans le cas contraire, l'index n'est pas unique et plusieurs lignes peuvent partager la même valeur de clé. Pour plus d’informations, consultez [Créer des index uniques](../../relational-databases/indexes/create-unique-indexes.md).  
  
 Les index d'une table ou d'une vue sont automatiquement conservés à chaque fois que les données de la table sont modifiées.  
  
 Pour connaître d’autres types d’index destinés à des usages spéciaux, consultez [Index](../../relational-databases/indexes/indexes.md) .  
  
## <a name="indexes-and-constraints"></a>Index et contraintes  
 Des index sont automatiquement créés lorsque les contraintes PRIMARY KEY et UNIQUE sont définies sur les colonnes de la table. Par exemple, lorsque vous créez une table et déterminez une colonne particulière qui sera la clé primaire, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée automatiquement une contrainte PRIMARY KEY et un index sur cette colonne. Pour plus d’informations, consultez [Supprimer des clés primaires](../../relational-databases/tables/create-primary-keys.md) et [Créer des clés primaires](../../relational-databases/tables/create-unique-constraints.md).  
  
## <a name="how-indexes-are-used-by-the-query-optimizer"></a>Utilisation des index par l’optimiseur de requête  
 Les index bien conçus peuvent réduire les opérations d'E/S disque et consommer moins de ressources système, améliorant ainsi les performances des requêtes. Les index peuvent être utiles pour diverses requêtes qui contiennent les instructions SELECT, UPDATE, DELETE ou MERGE. Supposons la requête `SELECT Title, HireDate FROM HumanResources.Employee WHERE EmployeeID = 250` dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Lorsque cette requête est exécutée, l'optimiseur de requête évalue chaque méthode disponible d'extraction des données et opte pour la plus efficace d'entre elles. La méthode peut être une analyse de table ou l'analyse d'un ou plusieurs index s'ils existent.  
  
 Lors d'une analyse de table, l'optimiseur de requête lit toutes les lignes de la table et extrait celles qui répondent aux critères de la requête. Une analyse de table génère de nombreuses opérations d'E/S disque et peut consommer une grande quantité de ressources. Toutefois, l'analyse d'une table peut être la méthode la plus efficace si par exemple le jeu des résultats de la requête représente un pourcentage élevé de lignes de la table.  
  
 Lorsque l'optimiseur de requête utilise un index, il recherche les colonnes de clé d'index et l'emplacement de stockage des lignes visées par la requête, puis extrait les lignes correspondantes à partir de cet emplacement. En général, une recherche dans un index est beaucoup plus rapide qu'une recherche dans la table car contrairement à une table, un index contient souvent très peu de colonnes par ligne et les lignes sont triées.  
  
 L'optimiseur de requête sélectionne en général la méthode la plus efficace lors de l'exécution des requêtes. Toutefois, si aucun index n'est disponible, l'optimiseur de requête doit utiliser une analyse de table. Votre tâche consiste à concevoir et à créer les index les mieux adaptés à votre environnement, de sorte que l'optimiseur de requête possède une sélection d'index garantissant un choix efficace. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient [l’Assistant Paramétrage du moteur de base de données](../../relational-databases/performance/database-engine-tuning-advisor.md) , qui facilite l’analyse de l’environnement de votre base de données et la sélection des index appropriés.  
  
> [!IMPORTANT] 
> Pour plus d’informations sur les règles de conception d’index et les éléments internes, consultez le [Guide de conception d’index SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="related-content"></a>Contenu connexe  
 [Guide de conception d’index SQL Server](../../relational-databases/sql-server-index-design-guide.md)     
 [Créer des index cluster](../../relational-databases/indexes/create-clustered-indexes.md)  
 [Créez des index non-cluster](../../relational-databases/indexes/create-nonclustered-indexes.md)  
  
  
