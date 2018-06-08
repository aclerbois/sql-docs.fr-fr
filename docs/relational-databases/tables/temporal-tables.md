---
title: Tables temporelles | Microsoft Docs
ms.custom: ''
ms.date: 07/11/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e442303d-4de1-494e-94e4-4f66c29b5fb9
caps.latest.revision: 47
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: b7e70190afc73d0dbad741f89e7d1dfc47404c87
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="temporal-tables"></a>Tables temporelles
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  SQL Server 2016 introduit la prise en charge des tables temporelles avec version contrôlée par le système en tant que fonctionnalité de base de données, qui offre une prise en charge intégrée de la fourniture d’informations sur les données stockées dans la table à tout moment, et non pas seulement les données correctes au moment présent. La fonctionnalité temporelle est une fonctionnalité de base de données introduite dans la norme ANSI SQL 2011.  
  
 **Démarrage rapide**  
  
-   **Prise en main :**  
  
    -   [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)  
  
    -   [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)  
  
    -   [Scénarios d’utilisation de table temporelle](../../relational-databases/tables/temporal-table-usage-scenarios.md)  
  
    -   [Prise en main des tables temporelles dans Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-temporal-tables/)  
  
-   **Exemples :**  
  
    -   [Création d’une table temporelle avec versions gérées par le système](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)  
  
    -   [Utilisation des tables temporelles avec versions gérées par le système et à mémoire optimisée](../../relational-databases/tables/working-with-memory-optimized-system-versioned-temporal-tables.md)  
  
    -   [Modification des données dans une table temporelle avec version gérée par le système](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)  
  
    -   [Interrogation des données dans une table temporelle avec version gérée par le système](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)  
  
    -   **Télécharger l’exemple de base de données Adventure Works :** pour prendre en main les tables temporelles, téléchargez la [base de données AdventureWorks pour SQL Server 2016 CTP3](https://www.microsoft.com/download/details.aspx?id=49502) avec des exemples de script et suivez les instructions contenues dans le dossier « Temporel »  
  
-   **Syntaxe :**  
  
    -   [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
  
    -   [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
    -   [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)  
  
-   **Vidéo :** pour une discussion de 20 minutes sur la fonctionnalité temporelle, consultez [Temporal in SQL Server 2016](http://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).  
  
## <a name="what-is-a-system-versioned-temporal-table"></a>Qu’est-ce qu’une table temporelle avec version gérée par le système ?  
 Une table temporelle avec version contrôlée par le système est un type de table utilisateur conçu pour conserver un historique complet des modifications apportées aux données et permettre l’analyse à un point dans le temps. Ce type de table temporelle est appelée table temporelle avec version gérée par le système, car la période de validité de chaque ligne est gérée par le système (c’est-à-dire le moteur de base de données).  
  
 Chaque table temporelle contient deux colonnes définies explicitement, chacune d’elles contenant un type de données **datetime2** . Ces colonnes sont appelées colonnes de période. Ces colonnes de période sont utilisées de manière exclusive par le système pour enregistrer la période de validité de chaque ligne lorsqu’une ligne est modifiée.  
  
 En plus de ces colonnes de période, une table temporelle contient également une référence à une autre table avec un schéma en miroir. Le système utilise cette table pour stocker automatiquement la version précédente de la ligne chaque fois qu’une ligne de la table temporelle est mise à jour ou supprimée. Cette table supplémentaire est appelée table d’historique. La table principale qui stocke les versions de ligne actuelles (réelles) est appelée table actuelle ou simplement table temporelle. Lors de la création d’une table temporelle, les utilisateurs peuvent spécifier une table d’historique existante (qui doit être compatible avec le schéma) ou laisser le système créer une table d’historique par défaut.  
  
## <a name="why-temporal"></a>Pourquoi la fonctionnalité temporelle ?  
 Les sources de données réelles sont dynamiques et la plupart des décisions commerciales s’appuient sur des informations que les analystes obtiennent en observant l’évolution de données. Les tables temporelles sont utilisées notamment dans les cas suivants :  
  
-   Audit de toutes les modifications de données et analyse des données si nécessaire  
  
-   Reconstruction de l’état des données à partir d’un moment quelconque dans le passé  
  
-   Calcul des tendances dans le temps  
  
-   Maintien d’une dimension à variation lente pour les applications d’aide à la décision  
  
-   Récupération à la suite de modifications accidentelles des données et d’erreurs d’application  
  
## <a name="how-does-temporal-work"></a>Fonctionnement des tables temporelles  
 La gestion des versions d’une table est implémentée sous forme de paire de tables, une table actuelle et une table d’historique. Dans chacune de ces tables, les deux colonnes **datetime2** supplémentaires suivantes permettent de définir la période de validité de chaque ligne :  
  
-   Colonne de début de la période : le système enregistre l’heure de début associée à la ligne de cette colonne, généralement désignée comme colonne **SysStartTime** .  
  
-   Colonne de fin de la période : le système enregistre l’heure de fin associée à la ligne de cette colonne, généralement désignée comme colonne **SysEndTime** .  
  
 La table actuelle contient la valeur actuelle pour chaque ligne. La table d’historique contient la valeur précédente de chaque ligne, le cas échéant, ainsi que l’heure de début et l’heure de fin de la période pendant laquelle elle était valide.  
  
 ![Temporal-HowWorks](../../relational-databases/tables/media/temporal-howworks.PNG "Temporal-HowWorks")  
  
 L’exemple simple suivant illustre un scénario dont les informations se trouvent dans une table Employee appartenant à une base de données de ressources humaines hypothétique :  
  
```  
CREATE TABLE dbo.Employee   
(    
  [EmployeeID] int NOT NULL PRIMARY KEY CLUSTERED   
  , [Name] nvarchar(100) NOT NULL  
  , [Position] varchar(100) NOT NULL   
  , [Department] varchar(100) NOT NULL  
  , [Address] nvarchar(1024) NOT NULL  
  , [AnnualSalary] decimal (10,2) NOT NULL  
  , [ValidFrom] datetime2 (2) GENERATED ALWAYS AS ROW START  
  , [ValidTo] datetime2 (2) GENERATED ALWAYS AS ROW END  
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)  
 )    
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.EmployeeHistory));  
```  
  
 **INSERTION :** lors d’une opération **INSERT**, le système définit la valeur de la colonne **SysStartTime** sur l’heure de début de la transaction en cours (dans le fuseau horaire UTC) d’après l’horloge du système et attribue à la colonne **SysEndTime** la valeur maximale de 9999-12-31. La ligne est alors marquée comme ouverte.  
  
 **MISE À JOUR :** lors d’une opération **UPDATE**, le système stocke la valeur précédente de la ligne dans la table d’historique et définit la valeur de la colonne **SysEndTime** sur l’heure de début de la transaction en cours (dans le fuseau horaire UTC) d’après l’horloge du système. La ligne est alors marquée comme fermée, avec une période enregistrée pendant laquelle la ligne était valide. Dans la table actuelle, la ligne est mise à jour avec la nouvelle valeur et le système définit la valeur de la colonne **SysStartTime** sur l’heure de début de la transaction (dans le fuseau horaire UTC) d’après l’horloge du système. La valeur de la ligne mise à jour dans la table actuelle pour la colonne **SysEndTime** conserve la valeur maximale de 9999-12-31.  
  
 **SUPPRESSION :** lors d’une opération **DELETE**, le système stocke la valeur précédente de la ligne dans la table d’historique et définit la valeur de la colonne **SysEndTime** sur l’heure de début de la transaction en cours (dans le fuseau horaire UTC) d’après l’horloge du système. La ligne est alors marquée comme fermée et la période pendant laquelle la ligne précédente était valide est enregistrée. Dans la table actuelle, la ligne est supprimée. Les requêtes de la table actuelle ne renvoient pas cette ligne. Seules les requêtes qui traitent des données d’historique renvoient les données pour lesquelles une ligne est fermée.  
  
 **FUSION :** lors d’une opération **MERGE**, l’opération se comporte exactement comme si un maximum de trois instructions (une instruction **INSERT**, une instruction **UPDATE**et/ou une instruction **DELETE**) s’exécutait, selon ce qui est spécifié en tant qu’actions dans l’instruction **MERGE** .  
  
> [!IMPORTANT]  
>  Les heures enregistrées dans les colonnes datetime2 système sont basées sur l’heure de début de la transaction proprement dite. Par exemple, toutes les lignes insérées dans une seule transaction ont la même heure UTC enregistrée dans la colonne correspond au début de la période **SYSTEM_TIME** .  
  
## <a name="how-do-i-query-temporal-data"></a>Interrogation de données temporelles  
 La clause **FROM***\<table>* de l’instruction **SELECT** utilise une nouvelle clause **FOR SYSTEM_TIME** avec cinq sous-clauses temporelles spécifiques pour interroger les données des tables actives et d’historique. Cette nouvelle syntaxe de l’instruction **SELECT** est prise en charge directement sur une table unique, propagée par plusieurs jointures et par des vues sur plusieurs tables temporelles.  
  
 ![Temporal-Querying](../../relational-databases/tables/media/temporal-querying.PNG "Temporal-Querying")  
  
 La requête suivante recherche les versions de ligne dans la table Employee pour lesquelles EmployeeID vaut 1000 et qui ont été actives pendant au moins une partie de la période comprise entre le 1er janvier 2014 et le 1er janvier 2015 (limite supérieure comprise) :  
  
```  
SELECT * FROM Employee   
    FOR SYSTEM_TIME    
        BETWEEN '2014-01-01 00:00:00.0000000' AND '2015-01-01 00:00:00.0000000'   
            WHERE EmployeeID = 1000 ORDER BY ValidFrom;  
```  
  
> [!NOTE]  
>  **FOR SYSTEM_TIME** filtre les lignes dont la période de validité indique une durée égale à zéro (**SysStartTime** = **SysEndTime**).  
> Ces lignes sont générées si vous effectuez plusieurs mises à jour sur la même clé primaire au sein de la même transaction.  
> Dans ce cas, l’interrogation des données temporelles renvoie uniquement les versions de ligne avant les transactions et celles qui sont devenues réelles après les transactions.  
> Si vous devez inclure ces lignes dans l’analyse, interrogez la table d’historique directement.  
  
 Dans le tableau ci-dessous, SysStartTime dans la colonne Lignes qualifiées représente la valeur figurant dans la colonne **SysStartTime** de la table interrogée et **SysEndTime** représente la valeur figurant dans la colonne **SysEndTime** de la même table. Pour la syntaxe complète et des exemples, consultez [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md) et [Interrogation des données dans une table temporelle avec version gérée par le système](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md).  
  
|Expression|Lignes qualifiées|Description|  
|----------------|---------------------|-----------------|  
|**AS OF**<date_time>|SysStartTime \<= date_time AND SysEndTime > date_time|Renvoie une table avec une ligne contenant les valeurs qui étaient réelles (actuelles) au moment spécifié dans le passé. En interne, une union est effectuée entre la table temporelle et sa table d’historique. Les résultats sont filtrés de manière à renvoyer les valeurs de la ligne qui était valide au moment spécifié par le paramètre *<date_time>*. La valeur d’une ligne est considérée comme valide si la valeur *system_start_time_column_name* est inférieure ou égale à celle du paramètre *<date_time>* et si la valeur *system_end_time_column_name* est supérieure à celle du paramètre *<date_time>*.|  
|**FROM**<start_date_time>**TO**<end_date_time>|SysStartTime < end_date_time AND SysEndTime > start_date_time|Renvoie une table avec les valeurs de toutes les versions de ligne qui étaient actives pendant l’intervalle de temps spécifié, sans tenir compte du fait qu’elles soient devenues actives avant la valeur du paramètre *<start_date_time>* pour l’argument FROM ou qu’elles aient cessé d’être actives après la valeur du paramètre *<end_date_time>* pour l’argument TO. En interne, une union est effectuée entre la table temporelle et sa table d’historique. Les résultats sont filtrés de manière à renvoyer les valeurs de toutes les versions de ligne qui étaient actives à tout moment de l’intervalle spécifié. Les lignes qui ont cessé d’être actives exactement à la limite inférieure définie par le point de terminaison FROM ne sont pas incluses. Les enregistrements qui sont devenus actifs exactement à la limite supérieure définie par le point de terminaison TO ne sont pas inclus non plus.|  
|**BETWEEN**<start_date_time>**AND**<end_date_time>|SysStartTime \<= end_date_time AND SysEndTime > start_date_time|Identique à la description de **FOR SYSTEM_TIME FROM** <start_date_time>**TO** <end_date_time> ci-dessus, sauf que la table de lignes renvoyée inclut des lignes qui sont devenues actives sur la limite supérieure définie par le point de terminaison <end_date_time>.|  
|**CONTAINED IN** (<start_date_time> , <end_date_time>)|SysStartTime >= start_date_time AND SysEndTime \<= end_date_time|Renvoie une table avec les valeurs de toutes les versions de ligne qui ont été ouvertes et fermées dans l’intervalle de temps spécifié, défini par les deux valeurs datetime de l’argument CONTAINED IN. Les lignes qui sont devenues actives exactement sur la limite inférieure ou qui ont cessé d’être actives exactement sur la limite supérieure sont incluses.|  
|**ALL**|Toutes les lignes|Renvoie l’union de lignes appartenant à la table actuelle et à la table d’historique.|  
  
> [!NOTE]  
>  Vous pouvez éventuellement choisir de masquer ces colonnes de période de manière à ce que les requêtes qui n’y font pas explicitement référence ne les retournent pas (scénario **SELECT \* FROM***\<table>*). Pour renvoyer une colonne masquée, incluez simplement une référence explicite à celle-ci dans la requête. De même, les instructions **INSERT** et **BULK INSERT** vont continuer d’agir comme si ces nouvelles colonnes de période étaient absentes (et les valeurs de la colonne seront remplies automatiquement). Pour plus d’informations sur l’utilisation de la clause **HIDDEN** , consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md) et [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)   
 [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)   
 [Scénarios d’utilisation de table temporelle](../../relational-databases/tables/temporal-table-usage-scenarios.md)   
 [Considérations et limitations liées aux tables temporelles](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)   
 [Gérer la rétention des données d’historique dans les tables temporelles avec version gérée par le système](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)   
 [Partitionnement des tables temporelles](../../relational-databases/tables/partitioning-with-temporal-tables.md)   
 [Vérifications de cohérence système des tables temporelles](../../relational-databases/tables/temporal-table-system-consistency-checks.md)   
 [Sécurité de la table temporelle](../../relational-databases/tables/temporal-table-security.md)   
 [Vues et fonctions de métadonnées de table temporelle](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)  
  
  
