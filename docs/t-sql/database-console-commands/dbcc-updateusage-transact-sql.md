---
title: DBCC UPDATEUSAGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- UPDATEUSAGE
- UPDATEUSAGE_TSQL
- DBCC_UPDATEUSAGE_TSQL
- DBCC UPDATEUSAGE
dev_langs:
- TSQL
helpviewer_keywords:
- inaccurate page or row counts [SQL Server]
- space [SQL Server], usage reports
- updating space usage information
- updating row counts
- disk space [SQL Server], inaccurate counts
- counting pages
- reporting count inaccuracies
- updating page counts
- synchronization [SQL Server], inaccurate counts
- incorrect space usage reports [SQL Server]
- DBCC UPDATEUSAGE statement
- integrity [SQL Server], database objects
- counting rows
- row count accuracy [SQL Server]
- page count accuracy [SQL Server]
ms.assetid: b8752ecc-db45-4e23-aee7-13b8bc3cbae2
caps.latest.revision: 56
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: 0ef83c4b349e44f7a5f399a7bf84c2be9cd2ca01
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-updateusage-transact-sql"></a>DBCC UPDATEUSAGE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Indique et corrige le nombre de pages et de lignes inexact dans les vues de catalogue. En raison de ces inexactitudes, la procédure stockée système sp_spaceused peut retourner des rapports incorrects en matière d'utilisation de l'espace.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DBCC UPDATEUSAGE   
(   { database_name | database_id | 0 }   
    [ , { table_name | table_id | view_name | view_id }   
    [ , { index_name | index_id } ] ]   
) [ WITH [ NO_INFOMSGS ] [ , ] [ COUNT_ROWS ] ]   
```  
  
## <a name="arguments"></a>Arguments  
*database_name* | *database_id* | 0  
Nom ou identificateur de la base de données pour laquelle les statistiques d'utilisation de l'espace doivent être consignées et corrigées. Si 0 est spécifié, la base de données active est utilisée. Les noms de base de données doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
*table_name* | *table_id* | *view_name* | *view_id*  
Nom ou identificateur de la table ou de la vue indexée dont les statistiques d'utilisation de l'espace doivent être consignées et corrigées. Les noms des tables et des vues doivent suivre les règles applicables aux identificateurs.  
  
*index_id* | *index_name*  
Identificateur ou nom de l'index à utiliser. Si aucun index n'est spécifié, l'instruction traite tous les index pour la table ou la vue indiquée.  
  
par  
Permet d'indiquer des options.  
  
NO_INFOMSGS  
Supprime tous les messages d'information.  
  
COUNT_ROWS  
Spécifie que la colonne row_count est mise à jour à l'aide du nombre actuel de lignes dans la table ou la vue.  
  
## <a name="remarks"></a>Notes   
DBCC UPDATEUSAGE corrige le nombre de lignes, de pages utilisées, de pages réservées, de pages de feuilles et de pages de données pour chaque partition d'une table ou d'un index. S'il n'y a pas d'imprécisions dans les tables système, DBCC UPDATEUSAGE ne retourne aucune donnée. Si des imprécisions sont trouvées et corrigées et si l'option WITH NO_INFOMSGS n'est pas utilisée, DBCC UPDATEUSAGE retourne les lignes et les colonnes mises à jour dans les tables système.
  
DBCC CHECKDB a été amélioré pour détecter le moment où les nombres de pages ou de lignes deviennent négatifs. Une fois cette détection effectuée, la sortie de DBCC CHECKDB contient un avertissement et une recommandation relatifs à l'exécution de DBCC UPDATEUSAGE afin de régler le problème.
  
## <a name="best-practices"></a>Bonnes pratiques  
Nous recommandons ce qui suit :
-   N'exécutez pas DBCC UPDATEUSAGE de manière régulière. Dans la mesure où DBCC UPDATEUSAGE peut prendre un certain temps pour s'exécuter sur les tables ou bases de données de grande taille, ne l'utilisez que si vous pensez que des valeurs incorrectes sont retournées par sp_spaceused.
-   Envisagez d'exécuter DBCC UPDATEUSAGE de manière régulière (chaque semaine, par exemple) uniquement si la base de données subit de fréquentes modifications du langage de définition de données (DDL), par exemple avec les instructions CREATE, ALTER ou DROP.  
  
## <a name="result-sets"></a>Jeux de résultats  
DBCC UPDATEUSAGE retourne le résultat suivant (les valeurs peuvent varier) :
  
`DBCC execution completed. If DBCC printed error messages, contact your system administrator.`
  
## <a name="permissions"></a>Autorisations  
Nécessite l’appartenance au rôle de serveur fixe **sysadmin** ou au rôle de base de données fixe **db_owner** .
  
## <a name="examples"></a>Exemples  
  
### <a name="a-updating-page-or-row-counts-or-both-for-all-objects-in-the-current-database"></a>A. Mise à jour du nombre de pages ou de lignes, ou les deux, pour tous les objets de la base de données active  
L'exemple suivant donne la valeur `0` au nom de la base de données et `DBCC UPDATEUSAGE` renvoie des informations mises à jour sur le nombre de pages ou de lignes dans la base de données active.
  
```sql
DBCC UPDATEUSAGE (0);  
GO  
```  
  
### <a name="b-updating-page-or-row-counts-or-both-for-adventureworks-and-suppressing-informational-messages"></a>B. Mise à jour du nombre de pages ou de lignes, ou les deux, pour AdventureWorks et suppression des messages d'information  
L'exemple suivant spécifie [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en tant que nom de la base de données et supprime tous les messages d'information.
  
```sql
DBCC UPDATEUSAGE (AdventureWorks2012) WITH NO_INFOMSGS;   
GO  
```  
  
### <a name="c-updating-page-or-row-counts-or-both-for-the-employee-table"></a>C. Mise à jour du nombre de pages ou de lignes, ou les deux, pour la table Employee  
L’exemple suivant retourne des informations mises à jour sur le nombre de pages ou de lignes dans la table `Employee`de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql
DBCC UPDATEUSAGE (AdventureWorks2012,'HumanResources.Employee');  
GO  
```  
  
### <a name="d-updating-page-or-row-counts-or-both-for-a-specific-index-in-a-table"></a>D. Mise à jour du nombre de pages ou de lignes, ou les deux, pour un index précis d'une table  
 L'exemple suivant spécifie `IX_Employee_ManagerID` en tant que nom d'index.  
  
```sql
DBCC UPDATEUSAGE (AdventureWorks2012, 'HumanResources.Employee', IX_Employee_OrganizationLevel_OrganizationNode);  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[sp_spaceused &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)  
[UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)
  
  
