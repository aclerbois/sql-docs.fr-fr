---
title: DBCC CHECKCONSTRAINTS (Transact-SQL) | Microsoft Docs
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
- DBCC CHECKCONSTRAINTS
- DBCC_CHECKCONSTRAINTS_TSQL
- CHECKCONSTRAINTS
- CHECKCONSTRAINTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC CHECKCONSTRAINTS statement
- consistency [SQL Server], constraints
- checking constraint consistency
- constraints [SQL Server], consistency checks
- integrity [SQL Server], constraints
ms.assetid: da6c9cee-6687-46e8-b504-738551f9068b
caps.latest.revision: 45
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: a274fea3b1171774def99daea9248ca96cd4c365
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-checkconstraints-transact-sql"></a>DBCC CHECKCONSTRAINTS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Vérifie l'intégrité d'une contrainte spécifiée ou de toutes les contraintes sur une table spécifiée de la base de données en cours.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DBCC CHECKCONSTRAINTS  
[   
    (   
    table_name | table_id | constraint_name | constraint_id   
    )  
]  
    [ WITH   
    [ { ALL_CONSTRAINTS | ALL_ERRORMSGS } ]  
    [ , ] [ NO_INFOMSGS ]   
    ]  
```  
  
## <a name="arguments"></a>Arguments  
 *table_name* | *table_id* | *constraint_name* | *constraint_id*  
 Table ou contrainte à vérifier. Quand *table_name* ou *table_id* est spécifié, toutes les contraintes activées sur cette table sont vérifiées. Quand *constraint_name* ou *constraint_id* est spécifié, seule cette contrainte est vérifiée. Si aucun identificateur de table ni aucun identificateur de contrainte n'est spécifié, toutes les contraintes activées sur toutes les tables de la base de données actuelle sont vérifiées.  
 Un nom de contrainte identifie de manière unique la table à laquelle la contrainte appartient. Pour plus d'informations, consultez [Database Identifiers](../../relational-databases/databases/database-identifiers.md).  
  
 par  
 Permet de spécifier des options.  
  
 ALL_CONSTRAINTS  
 Vérifie toutes les contraintes activées et désactivées de la table si le nom de la table est défini, ou bien si toutes les tables sont vérifiées. Si tel n'est pas le cas, vérifie seulement la contrainte activée. ALL_CONSTRAINTS n'a pas d'effet lorsqu'un nom de contrainte est spécifié.  
  
 ALL_ERRORMSGS  
 Renvoie toutes les lignes qui violent les contraintes de la table vérifiée. Cette instruction concerne par défaut les 200 premières lignes.  
  
 NO_INFOMSGS  
 Supprime tous les messages d'information.  
  
## <a name="remarks"></a>Notes   
DBCC CHECKCONSTRAINTS crée et exécute une requête pour toutes les contraintes FOREIGN KEY et les contraintes CHECK sur une table.
  
Par exemple, une requête de clé étrangère se présente comme suit :
  
```sql
SELECT <columns>  
FROM <table_being_checked> LEFT JOIN <referenced_table>  
    ON <table_being_checked.fkey1> = <referenced_table.pkey1>   
    AND <table_being_checked.fkey2> = <referenced_table.pkey2>  
WHERE <table_being_checked.fkey1> IS NOT NULL   
    AND <referenced_table.pkey1> IS NULL  
    AND <table_being_checked.fkey2> IS NOT NULL  
    AND <referenced_table.pkey2> IS NULL  
```  
  
Les données de la requête sont stockées dans une table temporaire. Une fois que toutes les tables ou les contraintes demandées ont été vérifiées, le jeu de résultats est renvoyé.
DBCC CHECKCONSTRAINTS vérifie l'intégrité des contraintes FOREIGN KEY et CHECK, mais ne vérifie pas celle des structures de données sur disque d'une table. Ces vérifications de structures de données peuvent être effectuées à l’aide de [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) et de [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).
  
**S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]
  
Si *table_name* ou *table_id* est spécifié et activé pour la gestion système des versions, DBCC CHECKCONSTRAINTS effectue également des vérifications de cohérence des données temporelles sur la table spécifiée. Quand *NO_INFOMSGS* n’est pas spécifié, cette commande retourne chaque violation de cohérence dans la sortie sur une ligne distincte. Le format de la sortie sera ([pkcol1], [pkcol2]..) = (\<pkcol1_value>, \<pkcol2_value>…) ET \<le problème avec l’enregistrement de la table temporelle>.
  
|Vérifier|Informations supplémentaires dans la sortie en cas d’échec de la vérification|  
|-----------|-----------------------------------------------|  
|PeriodEndColumn ≥ PeriodStartColumn (en cours)|[sys_end] = '{0}' AND MAX(DATETIME2) = '9999-12-31 23:59:59.99999'|  
|PeriodEndColumn ≥ PeriodStartColumn (en cours, historique)|[sys_start] = '{0}' AND [sys_end] = '{1}'|  
|PeriodStartColumn < current_utc_time (en cours)|[sys_start] = '{0}' AND SYSUTCTIME|  
|PeriodEndColumn < current_utc_time (historique)|[sys_end] = '{0}' AND SYSUTCTIME|  
|Chevauchements|(sys_start1, sys_end1), (sys_start2, sys_end2) pour deux enregistrements qui se chevauchent.<br /><br /> S’il existe plus de 2 enregistrements qui se chevauchent, la sortie affiche plusieurs lignes indiquant chacune une paire de chevauchements.|  
  
Il n’existe aucun moyen de spécifier constraint_name ni constraint_id afin d’exécuter uniquement des vérifications de cohérence temporelle.
  
## <a name="result-sets"></a>Jeux de résultats  
DBCC CHECKCONSTRAINTS retourne un ensemble de lignes comportant les colonnes suivantes.
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|Nom de la table|**varchar**|Nom de la table.|  
|Constraint Name (Nom de la contrainte)|**varchar**|Nom de la contrainte qui n'est pas respectée.|  
|Où|**varchar**|Affectations des valeurs de la colonne qui identifient la ou les lignes ne respectant pas la contrainte.<br /><br /> La valeur de cette colonne peut être utilisée dans la clause WHERE d'une instruction SELECT qui demande les lignes qui ne respectent pas la contrainte.|  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’appartenance au rôle de serveur fixe **sysadmin** ou au rôle de base de données fixe **db_owner** .
  
## <a name="examples"></a>Exemples  
  
### <a name="a-checking-a-table"></a>A. Vérification d'une table  
L'exemple suivant vérifie l'intégrité des contraintes de la table `Table1` de la base de données `AdventureWorks`.
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TABLE Table1 (Col1 int, Col2 char (30));  
GO  
INSERT INTO Table1 VALUES (100, 'Hello');  
GO  
ALTER TABLE Table1 WITH NOCHECK ADD CONSTRAINT chkTab1 CHECK (Col1 > 100);  
GO  
DBCC CHECKCONSTRAINTS(Table1);  
GO  
```  
  
### <a name="b-checking-a-specific-constraint"></a>B. Vérification d'une contrainte spécifique  
L'exemple suivant vérifie l'intégrité de la contrainte `CK_ProductCostHistory_EndDate`.
  
```sql  
USE AdventureWorks2012;  
GO  
DBCC CHECKCONSTRAINTS ('Production.CK_ProductCostHistory_EndDate');  
GO  
```  
  
### <a name="c-checking-all-enabled-and-disabled-constraints-on-all-tables"></a>C. Vérification de toutes les contraintes activées et désactivées de toutes les tables  
 L'exemple suivant vérifie l'intégrité de toutes les contraintes activées et désactivées de toutes les tables de la base de données actuelle.  
  
```sql  
DBCC CHECKCONSTRAINTS WITH ALL_CONSTRAINTS;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
[DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[DBCC CHECKTABLE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)
  
  
