---
title: Graphique de traitement avec SQL Server et la base de données SQL Azure | Documents Microsoft
ms.custom: ''
ms.date: 07/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: graphs
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- SQL graph
- SQL graph, overview
ms.assetid: ''
caps.latest.revision: 1
author: shkale-msft
ms.author: shkale
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2017 || = sqlallproducts-allversions
ms.openlocfilehash: 8c2ad7f5b31a97de5d0bfb22074b55bd61bb825b
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707047"
---
# <a name="graph-processing-with-sql-server-and-azure-sql-database"></a>Graphique de traitement avec SQL Server et la base de données SQL Azure
[!INCLUDE[tsql-appliesto-ss2017-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-asdb-xxxx-xxx-md.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] offre des fonctionnalités de base de données de graphique pour modéliser des relations plusieurs-à-plusieurs. Les relations de graphique sont intégrées dans [!INCLUDE[tsql-md](../../includes/tsql-md.md)] et recevoir les avantages de l’utilisation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en tant que le système de gestion de base de la base de données.


## <a name="what-is-a-graph-database"></a>Qu’est une base de données de graphique ?  
Une base de données de graphique est une collection de nœuds (ou les sommets) et de bords (ou relations). Un nœud représente une entité (par exemple, une personne ou organisation) et un bord représente une relation entre les deux nœuds, il se connecte (par exemple, composées ou amis). Les nœuds et les bords peuvent avoir des propriétés qui s’y rapportent. Voici certaines fonctionnalités qui rendent une base de données de graphique unique :  
-   Bords ou les relations sont des entités de première classes dans une base de données du graphique, et peuvent avoir attributs ou les propriétés associées. 
-   Un contour unique peut avec souplesse connecter plusieurs nœuds dans une base de données de graphique.
-   Vous pouvez exprimer facilement des critères spéciaux et les requêtes de navigation des sauts multiples.
-   Vous pouvez facilement exprimer fermeture transitive et requêtes polymorphes.

## <a name="when-to-use-a-graph-database"></a>Quand utiliser une base de données de graphique

Rien n’est qu'une base de données du graphique peut atteindre, qui ne peut pas être obtenue à l’aide de la base de données relationnelle. Toutefois, une base de données du graphique peut rendre plus facile exprimer certains types de requêtes. En outre, avec les optimisations spécifiques, certaines requêtes peuvent plus performantes. Votre décision pour choisir une par rapport à l’autre peut être basée sur des facteurs suivants :  
-   Votre application a des données hiérarchiques. Le type de données HierarchyID peut être utilisé pour implémenter des hiérarchies, mais il présente certaines limitations. Par exemple, il n’autorise pas vous permet de stocker plusieurs parents d’un nœud.
-   Votre application a des relations plusieurs-à-plusieurs complexes ; évolution de l’application, des relations sont ajoutées.
-   Vous devez analyser les relations et interconnectés.

## <a name="graph-features-introduced-in-includesssqlv14includessssqlv14-mdmd"></a>Fonctionnalités de graphique introduites dans [!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] 
Nous avons commencé à ajouter des extensions de graphique à SQL Server, afin de faciliter le stockage et interrogation des données de graphique. Les fonctionnalités suivantes sont introduites dans la première version. 


### <a name="create-graph-objects"></a>Créer des objets de graphique
[!INCLUDE[tsql-md](../../includes/tsql-md.md)] extensions permettra aux utilisateurs de créer des tableaux de bord ou de nœud. Les nœuds et les bords peuvent avoir des propriétés qui leur sont associées. Depuis, nœuds et bords sont stockées en tant que tables, toutes les opérations qui sont pris en charge sur les tables relationnelles sont pris en charge sur le tableau de bord ou le nœud. Par exemple :  

```   
CREATE TABLE Person (ID INTEGER PRIMARY KEY, Name VARCHAR(100), Age INT) AS NODE;
CREATE TABLE friends (StartDate date) AS EDGE;
```   

![tables de vos amis personne](../../relational-databases/graphs/media/person-friends-tables.png "nœud Person et vos amis tableaux de bord")  
Nœuds et bords sont stockés sous forme de tables  

### <a name="query-language-extensions"></a>Extensions de langage de requête  
Nouvelle `MATCH` clause est introduite pour prendre en charge la recherche de correspondance et de navigation cascade via le graphique. Le `MATCH` fonction utilise la syntaxe de style pointe ASCII pour les critères spéciaux. Par exemple :  

```   
-- Find friends of John
SELECT Person2.Name 
FROM Person Person1, Friends, Person Person2
WHERE MATCH(Person1-(Friends)->Person2)
AND Person1.Name = 'John';
```   
 
### <a name="fully-integrated-in-includessnoversionincludesssnoversion-mdmd-engine"></a>Entièrement intégré dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] moteur 
Extensions de graphique sont entièrement intégrées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] moteur. Nous utilisons le même moteur de stockage, les métadonnées, processeur de requêtes, etc. pour stocker et interroger des données de graphique. Cela permet aux utilisateurs d’interroger les leurs graphique et les données relationnelles dans une requête unique. Les utilisateurs peuvent également bénéficier de combinaison de fonctionnalités de graphique avec d’autres [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] technologies telles que columnstore, haute disponibilité, R services, etc. Base de données du graphique SQL prend également en charge toutes les sécurité et conformité fonctionnalités disponibles avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
 
### <a name="tooling-and-ecosystem"></a>Outils et l’écosystème  
Les utilisateurs bénéficient d’outils existants et l’écosystème qui [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] offre. Outils de sauvegarde et de restauration, importer et exporter, fonctionnement BCP prêtes à l’emploi. Autres outils ou des services, comme SSIS, SSRS ou Power BI fonctionnera avec les tables de graphique, la façon de travailler avec des tables relationnelles.
 
 ## <a name="next-steps"></a>Étapes suivantes  
Lecture du [base de données SQL graphique - Architecture](./sql-graph-architecture.md)
   

