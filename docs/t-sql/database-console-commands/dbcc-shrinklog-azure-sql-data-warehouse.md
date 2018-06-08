---
title: DBCC SHRINKLOG (Parallel Data Warehouse) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2018
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
caps.latest.revision: 11
author: edmacauley
ms.author: umajay
manager: craigg
monikerRange: '>= aps-pdw-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 75fe950d32310a38a75660bccaca0f5b98689572
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="dbcc-shrinklog-parallel-data-warehouse"></a>DBCC SHRINKLOG (Parallel Data Warehouse)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md)]

Réduit la taille du journal des transactions *sur l’appliance* pour la base de données [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] actuelle. Les données sont défragmentées afin de réduire le journal des transactions. Avec le temps, le journal des transactions de la base de données peut être fragmenté et inefficace. Utilisez DBCC SHRINKLOG pour réduire la fragmentation et la taille du journal.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DBCC SHRINKLOG   
    [ ( SIZE = { target_size [ MB | GB | TB ]  } | DEFAULT ) ]   
    [ WITH NO_INFOMSGS ]   
[;]  
```  
  
## <a name="arguments"></a>Arguments  
SIZE = { *target_size* [ MB | **GB** | TB ]  } | **DEFAULT**.  
*target_size* représente la taille voulue pour le journal des transactions, sur tous les nœuds de calcul, après l’exécution de DBCC SHRINKLOG. Il s’agit d’un entier supérieur à 0.  
La taille du journal est mesurée en mégaoctets (Mo), gigaoctets (Go) ou téraoctets (To). Il s’agit de la taille combinée du journal des transactions sur tous les nœuds de calcul.  
Par défaut, DBCC SHRINKLOG réduit le journal des transactions à la taille de journal stockée dans les métadonnées de la base de données. La taille de journal dans les métadonnées est déterminée par le paramètre LOG_SIZE dans [CREATE DATABASE &#40;Azure SQL Data Warehouse&#41;](../../t-sql/statements/create-database-azure-sql-data-warehouse.md) ou [ALTER DATABASE &#40;Azure SQL Data Warehouse&#41;](../../t-sql/statements/alter-database-azure-sql-data-warehouse.md). DBCC SHRINKLOG réduit la taille du journal des transactions à la taille par défaut quand `SIZE=DEFAULT` est spécifié ou quand la clause `SIZE` est omise.
  
WITH NO_INFOMSGS  
Les messages d’information ne sont pas affichés dans les résultats DBCC SHRINKLOG.  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’autorisation ALTER SERVER STATE.
  
## <a name="general-remarks"></a>Remarques d'ordre général  
DBCC SHRINKLOG ne modifie pas la taille de journal stockée dans les métadonnées de la base de données. Les métadonnées continuent de contenir le paramètre LOG_SIZE qui a été spécifié dans l’instruction CREATE DATABASE ou ALTER DATABASE.
  
## <a name="examples"></a>Exemples 
### <a name="a-shrink-the-transaction-log-to-the-original-size-specified-by-create-database"></a>A. Réduire le journal des transactions à la taille d’origine spécifiée par CREATE DATABASE.  
Supposons que le journal des transactions pour la base de données Addresses a été défini sur 100 Mo lors de la création de la base de données Addresses. Autrement dit, l’instruction CREATE DATABASE pour Addresses indiquait LOG_SIZE = 100 Mo. Supposons maintenant que le journal a atteint une taille de 150 Mo que vous voulez ramener à 100 Mo.
  
Chacune des instructions suivantes va tenter de réduire le journal des transactions pour la base de données Addresses à la taille par défaut de 100 Mo. Si la réduction du journal à 100 Mo entraîne une perte de données, DBCC SHRINKLOG ramène le journal à la plus petite taille possible, supérieure à 100 Mo, sans perte de données.
  
```sql
USE Addresses;  
DBCC SHRINKLOG ( SIZE = 100 MB );  
DBCC SHRINKLOG ( SIZE = DEFAULT );  
DBCC SHRINKLOG;  
```  
  
  
