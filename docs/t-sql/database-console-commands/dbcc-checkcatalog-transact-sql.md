---
title: DBCC CHECKCATALOG (Transact-SQL) | Microsoft Docs
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
- DBCC_CHECKCATALOG_TSQL
- DBCC CHECKCATALOG
- CHECKCATALOG_TSQL
- CHECKCATALOG
dev_langs:
- TSQL
helpviewer_keywords:
- catalogs [SQL Server], consistency checks
- checking catalog consistency
- DBCC CHECKCATALOG statement
- integrity [SQL Server], catalogs
- consistency [SQL Server], catalogs
ms.assetid: 8076eb4e-f049-44bf-9a35-45cdd6ef0105
caps.latest.revision: 51
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: b71c8011ebf2830409f886dc61a3b9e93a00df92
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-checkcatalog-transact-sql"></a>DBCC CHECKCATALOG (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Vérifie la cohérence du catalogue dans la base de données spécifiée. La base de données doit être en ligne.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
DBCC CHECKCATALOG   
[   
    (   
    database_name | database_id | 0  
    )  
]  
    [ WITH NO_INFOMSGS ]   
```  
  
## <a name="arguments"></a>Arguments  
 *database_name* | *database_id* | 0  
 Nom ou ID de la base de données dont vous souhaitez vérifier la cohérence du catalogue. En l'absence de spécification, ou si 0 est spécifié, la base de données actuelle est utilisée. Les noms de base de données doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 WITH NO_INFOMSGS  
 Supprime tous les messages d'information.  
  
## <a name="remarks"></a>Notes   
Une fois la commande DBCC CATALOG terminée, un message est enregistré dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si la commande DBCC est correctement exécutée, le message indique que l'exécution a réussi, ainsi que la durée d'exécution de la commande. Si la commande DBCC est interrompue avant la fin de la vérification en raison d’une erreur, le message indique que la commande n’a pas abouti et précise une valeur d’état ainsi que la durée d’exécution de la commande. Le tableau suivant répertorie et décrit les valeurs d'état pouvant être incluses dans le message.
  
|État|Description|  
|-----------|-----------------|  
|0|Erreur numéro 8930 générée. Ceci indique une corruption des métadonnées qui a provoqué l'arrêt de la commande DBCC.|  
| 1|Erreur numéro 8967 générée. Une erreur DBCC interne s'est produite.|  
|2|Une erreur s'est produite lors de la réparation de la base de données en mode urgence.|  
|3|Ceci indique une corruption des métadonnées qui a provoqué l'arrêt de la commande DBCC.|  
|4|Une assertion ou une violation d'accès a été détectée.|  
|5|Une erreur inconnue s'est produite et a arrêté la commande DBCC.|  
  
DBCC CHECKCATALOG effectue différentes vérifications de cohérence entre les tables de métadonnées système. DBCC CHECKCATALOG utilise un instantané de base de données interne pour fournir la cohérence transactionnelle nécessaire à la réalisation de ces vérifications. Pour plus d’informations, consultez [Afficher la taille du fichier partiellement alloué d’un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) et la section « Utilisation de l’instantané de base de données interne DBCC » dans [DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md).
Si un instantané ne peut pas être créé, DBCC CHECKCATALOG acquiert un verrou de base de données exclusif pour obtenir la cohérence requise. Si des incohérences sont détectées, elles ne peuvent pas être réparées et la base de données doit être restaurée à partir d'une sauvegarde.
  
> [!NOTE]  
> L’exécution de DBCC CHECKCATALOG sur **tempdb** n’effectue aucune vérification. En effet, pour des raisons liées aux performances, les instantanés de base de données ne sont pas disponibles sur **tempdb**. Cela signifie que la cohérence transactionnelle requise ne peut pas être obtenue. Recyclez le serveur pour résoudre les problèmes liés aux métadonnées de **tempdb**.  
  
> [!NOTE]  
> DBCC CHECKCATALOG ne vérifie pas les données FILESTREAM. FILESTREAM stocke les objets BLOB dans le système de fichiers.  
  
En outre, DBCC CHECKCATALOG est exécuté dans le cadre de [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md).
  
## <a name="result-sets"></a>Jeux de résultats  
Si aucune base de données n'est spécifiée, DBCC CHECKCATALOG retourne :
  
```
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
Si [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] est spécifié en tant que nom de base de données, DBCC CHECKCATALOG retourne :
  
```
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance au rôle serveur fixe **sysadmin** ou au rôle de base de données fixe **db_owner**.  
  
## <a name="examples"></a>Exemples  
L'exemple suivant vérifie l'intégrité du catalogue dans la base de données active et dans la base de données `AdventureWorks`.
  
```sql
-- Check the current database.  
DBCC CHECKCATALOG;  
GO  
-- Check the AdventureWorks2012 database.  
DBCC CHECKCATALOG (AdventureWorks2012);  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)
  
