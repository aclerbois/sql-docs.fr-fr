---
title: DBCC DROPCLEANBUFFERS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DROPCLEANBUFFERS
- DBCC_DROPCLEANBUFFERS_TSQL
- DROPCLEANBUFFERS_TSQL
- DBCC DROPCLEANBUFFERS
dev_langs:
- TSQL
helpviewer_keywords:
- clean buffers
- cold buffer cache
- buffer pools [SQL Server]
- dropping buffers
- removing buffers
- DBCC DROPCLEANBUFFERS statement
ms.assetid: a4121927-f2ce-4926-aa2c-9b1519dac048
caps.latest.revision: 35
author: uc-msft
ms.author: umajay
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 4cb3b80a79799dfabe811183e34143bd919f809f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-dropcleanbuffers-transact-sql"></a>DBCC DROPCLEANBUFFERS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)]

Supprime toutes les mémoires tampons propres du pool de mémoires tampons et les objets columnstore du pool d’objets columnstore.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe
Syntaxe de SQL Server : 

```sql
DBCC DROPCLEANBUFFERS [ WITH NO_INFOMSGS ]  
```  
Syntaxe d’Azure SQL Warehouse et Parallel Data Warehouse :

```sql  
DBCC DROPCLEANBUFFERS ( COMPUTE | ALL ) [ WITH NO_INFOMSGS ]  
```  
  
## <a name="arguments"></a>Arguments  
 WITH NO_INFOMSGS  
 Supprime tous les messages d'information. Les messages d’informations sont toujours supprimés sur [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 COMPUTE  
 Videz le cache de données en mémoire sur chaque nœud de calcul.  
  
 ALL  
 Videz le cache de données en mémoire sur chaque nœud de calcul et sur le nœud de contrôle. Il s’agit de la valeur par défaut si vous ne spécifiez aucune valeur.  
  
## <a name="remarks"></a>Notes   
Utilisez l'instruction DBCC DROPCLEANBUFFERS pour vérifier les requêtes avec un cache de tampons à froid sans arrêter et redémarrer le serveur.
Pour supprimer les mémoires tampons propres du pool de mémoires tampons et les objets columnstore du pool d’objets columnstore, utilisez d'abord CHECKPOINT pour générer un cache de tampons à froid. Cette opération permet d'écrire sur disque toutes les pages incorrectes de la base de données active et de nettoyer les mémoires tampons. Ensuite, vous pouvez émettre la commande DBCC DROPCLEANBUFFERS pour supprimer toutes les mémoires tampons du pool de mémoires tampons.
  
## <a name="result-sets"></a>Jeux de résultats  
DBCC DROPCLEANBUFFERS sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne :
  
```sql
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Autorisations  

S’applique à : SQL Server, Parallel Data Warehouse 

- Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  

S’applique à : Azure SQL Data Warehouse

- Nécessite l'appartenance au rôle serveur fixe DB_OWNER.  
  
## <a name="see-also"></a> Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)  
  
  
