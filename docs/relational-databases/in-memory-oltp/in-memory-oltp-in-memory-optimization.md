---
title: OLTP en mémoire (optimisation en mémoire) | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- In-Memory OLTP
- memory-optimized tables
ms.assetid: e1d03d74-2572-4a55-afd6-7edf0bc28bdb
caps.latest.revision: 106
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 54fa82bd4f225847214211cf3549858bf9bf3182
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34707757"
---
# <a name="in-memory-oltp-in-memory-optimization"></a>OLTP en mémoire (optimisation en mémoire)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] permet d’améliorer considérablement les performances du traitement transactionnel, de l’intégration et du chargement des données, et des scénarios de données temporaires.  Pour accéder au code et aux connaissances de base indispensables pour tester rapidement votre propre table optimisée en mémoire et votre procédure stockée compilée en mode natif, consultez
 -  [Démarrage rapide 1 : technologies OLTP en mémoire pour accélérer les performances Transact-SQL](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md).  
 
Une vidéo de 17 minutes expliquant la fonction OLTP en mémoire et montrant ses avantages en matière de performances :

-  [In-Memory OLTP in SQL Server 2016](https://www.youtube.com/watch?v=l5l5eophmK4)(OLTP en mémoire dans SQL Server 2016)

Pour télécharger la démonstration des performances pour l’OLTP en mémoire utilisé dans la vidéo : 

- [In-Memory OLTP Performance Demo v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

Pour une présentation plus détaillée de l’OLTP en mémoire et un examen des scénarios où l’utilisation de cette technologie offre des avantages en matière de performances :

- [Vue d’ensemble et scénarios d’utilisation](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)
 
 Notez que [!INCLUDE[hek_2](../../includes/hek-2-md.md)] est la technologie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permettant d’améliorer les performances du traitement des transactions. Pour découvrir la technologie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui améliore les performances des requêtes de création de rapports et analytiques, consultez le [Guide des index columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md).
  
 Plusieurs améliorations ont récemment été apportées à la fonction OLTP en mémoire dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], ainsi que dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. La surface d’exposition Transact-SQL a été augmentée pour simplifier la migration des applications de base de données. Les opérations ALTER sur les tables optimisées en mémoire et les procédures stockées compilées en mode natif sont désormais prises en charge afin de simplifier la gestion des applications. Pour obtenir des informations sur les nouvelles fonctionnalités de [!INCLUDE[hek_2](../../includes/hek-2-md.md)], consultez [Index columnstore - Nouveautés](../../relational-databases/indexes/columnstore-indexes-what-s-new.md).  
  
> [!NOTE]  
>  **À votre tour d’essayer**  
>   
>  L’OLTP en mémoire est disponible dans les pools élastiques et les bases de données SQL Azure des niveaux Premium et Critique pour l’entreprise. Pour prendre en main l’OLTP en mémoire et Columnstore dans Azure SQL Database, consultez [Optimize Performance using In-Memory Technologies in SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-in-memory/)(Optimiser les performances à l’aide des technologies en mémoire dans SQL Database).  
  

## <a name="in-this-section"></a>Contenu de cette section  
 Cette section contient les rubriques suivantes :  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Démarrage rapide 1 : technologies OLTP en mémoire pour accélérer les performances Transact-SQL](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)|Plongez directement au cœur de l’OLTP en mémoire.|
|[Vue d’ensemble et scénarios d’utilisation](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)|Vue d’ensemble de l’OLTP en mémoire et des scénarios où l’utilisation de cette technologie offre des avantages en matière de performances.|
|[Conditions requises pour l’utilisation des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)|Décrit les configurations matérielle et logicielle requises et fournit des instructions pour l'utilisation des tables optimisées en mémoire.|  
|[Exemples de code OLTP en mémoire](../../relational-databases/in-memory-oltp/in-memory-oltp-code-samples.md)|Contient des exemples de code qui montrent comment créer et utiliser une table optimisée en mémoire.|  
|[Tables optimisées en mémoire](../../relational-databases/in-memory-oltp/memory-optimized-tables.md)|Présente les tables optimisées en mémoire.|  
|[Variables de table mémoire optimisée](http://msdn.microsoft.com/library/bd102e95-53e2-4da6-9b8b-0e4f02d286d3)|L'exemple de code illustre comment utiliser une variable de table optimisée en mémoire plutôt qu'une variable de table traditionnelle pour réduire l'utilisation de tempdb.|  
|[Index sur des tables optimisées en mémoire](http://msdn.microsoft.com/library/86805eeb-6972-45d8-8369-16ededc535c7)|Présente les index optimisés en mémoire.|  
|[Procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md)|Présente les procédures stockées compilées en mode natif.|  
|[Gestion de la mémoire pour l’OLTP en mémoire](http://msdn.microsoft.com/library/d82f21fa-6be1-4723-a72e-f2526fafd1b6)|Comprendre et gérer l'utilisation de la mémoire sur votre système.|  
|[Création et gestion du stockage des objets mémoire optimisés](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)|Traite des fichiers de données et delta, qui stockent les informations sur les transactions dans les tables optimisées en mémoire.|  
|[Sauvegarder, restaurer et récupérer des tables optimisées en mémoire](http://msdn.microsoft.com/library/3f083347-0fbb-4b19-a6fb-1818d545e281)|Décrit la sauvegarde, la restauration et la récupération des tables optimisées en mémoire.|  
|[Prise en charge d’OLTP en mémoire par Transact-SQL](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)|Décrit la prise en charge [!INCLUDE[tsql](../../includes/tsql-md.md)] pour l' [!INCLUDE[hek_2](../../includes/hek-2-md.md)].|  
|[Prise en charge de la haute disponibilité pour les bases de données OLTP en mémoire](../../relational-databases/in-memory-oltp/high-availability-support-for-in-memory-oltp-databases.md)|Décrit les groupes de disponibilité et le clustering de basculement dans l' [!INCLUDE[hek_2](../../includes/hek-2-md.md)].|  
|[Prise en charge d’OLTP en mémoire par SQL Server](../../relational-databases/in-memory-oltp/sql-server-support-for-in-memory-oltp.md)|Répertorie les nouveautés et les mises à jour en matière de syntaxe et de fonctionnalités prenant en charge les tables optimisées en mémoire.|  
|[Migration vers OLTP en mémoire](../../relational-databases/in-memory-oltp/migrating-to-in-memory-oltp.md)|Explique comment migrer les tables sur disque vers des tables optimisées en mémoire.|  
  
 Des informations supplémentaires sur l' [!INCLUDE[hek_2](../../includes/hek-2-md.md)] sont disponibles dans :  

- [Vidéo expliquant la fonction OLTP en mémoire et montrant ses avantages en termes de performances](https://www.youtube.com/watch?v=l5l5eophmK4).

- [In-Memory OLTP Performance Demo v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

-   [Livre blanc technique sur les mécanismes internes de la fonction OLTP en mémoire SQL Server](https://msdn.microsoft.com/library/mt764316.aspx)  

-   [Comparaison des fonctionnalités de Columnstore et d’OLTP en mémoire SQL Server](http://download.microsoft.com/download/D/0/0/D0075580-6D72-403D-8B4D-C3BD88D58CE4/SQL_Server_2016_In_Memory_OLTP_and_Columnstore_Comparison_White_Paper.pdf)

-   Nouveautés d’OLTP en mémoire dans SQL Server 2016, [partie 1](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2015/11/12/in-memory-oltp-whats-new-in-sql2016-ctp3/) et [partie 2](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/25/whats-new-for-in-memory-oltp-in-sql-server-2016-since-ctp3/)
  
-   [OLTP en mémoire – Modèles de charge de travail courants et considérations relatives à la migration](http://msdn.microsoft.com/library/dn673538.aspx)  
  
-   [Blog OLTP en mémoire](http://go.microsoft.com/fwlink/?LinkId=311696)  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalités de base de données](../../relational-databases/database-features.md)  
  
  
