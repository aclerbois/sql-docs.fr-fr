---
title: sp_estimated_rowsize_reduction_for_vardecimal (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_estimated_rowsize_reduction_for_vardecimal
- sp_estimated_rowsize_reduction_for_vardecimal_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_estimated_rowsize_reduction_for_vardecimal
- decimal data type, compressing
- numeric data type, compressing
- estimate decimal compression
- table compression [SQL Server]
ms.assetid: 0fe45983-f9f2-4c7f-938a-0fd96e1cbe8d
caps.latest.revision: 23
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 1d699e62251f8f2c750795d2389f338e9bd1bf11
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spestimatedrowsizereductionforvardecimal-transact-sql"></a>sp_estimated_rowsize_reduction_for_vardecimal (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Estime la réduction de la taille moyenne des lignes si vous activez le format de stockage vardecimal sur une table. Utilisez ce nombre pour estimer la réduction globale de la taille de la table. Dans la mesure où l'échantillonnage statistique permet de calculer la réduction moyenne de la taille de ligne, considérez-le simplement comme une estimation. La taille de ligne peut augmenter après l'activation du format de stockage vardecimal, mais cela reste rare.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt la compression ROW et PAGE. Pour plus d'informations, consultez [Data Compression](../../relational-databases/data-compression/data-compression.md). Pour les effets de la compression de la taille des tables et des index, consultez [sp_estimate_data_compression_savings &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_estimated_rowsize_reduction_for_vardecimal [ [ @table_name = ] 'table'] [;]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@table=** ] **'***table***'**  
 Nom en trois parties de la table dont le format de stockage doit être modifié. *table* est **nvarchar(776)**.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le jeu de résultats suivant est retourné pour fournir des informations sur la taille actuelle et estimée de la table.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**avg_rowlen_fixed_format**|**Decimal (12, 2)**|Représente la longueur de la ligne au format de stockage décimal fixe.|  
|**avg_rowlen_vardecimal_format**|**Decimal (12, 2)**|Représente la taille de ligne moyenne lorsque le format de stockage vardecimal est utilisé.|  
|**row_count**|**int**|Nombre de lignes dans la table.|  
  
## <a name="remarks"></a>Notes  
 Utilisez **sp_estimated_rowsize_reduction_for_vardecimal** pour estimer les gains obtenus si vous activez une table pour le format de stockage vardecimal. Par exemple, si la taille moyenne de la ligne peut être réduite de 40 %, vous pouvez réduire la taille de la table de 40 %. Vous n'économiserez peut-être pas d'espace en fonction du facteur de remplissage et de la taille de la ligne. Par exemple, si vous disposez d'une ligne d'une longueur de 8 000 octets et que vous réduisez sa taille de 40 %, vous ne pourrez toujours pas intégrer plus d'une ligne sur une page de données, ce qui ne génère pas de gains.  
  
 Si les résultats de **sp_estimated_rowsize_reduction_for_vardecimal** indiquent que la table augmentera, cela signifie que plusieurs lignes de la table utilisent quasiment la précision complète des types de données décimal, et l’ajout de la faible surcharge requise pour le format de stockage vardecimal dépasse les gains dérivés du format de stockage vardecimal. Dans ce cas très peu fréquent, n'activez pas le format de stockage vardecimal.  
  
 Si une table est activée pour le format de stockage vardecimal, utilisez **sp_estimated_rowsize_reduction_for_vardecimal** pour estimer la taille moyenne de la ligne si le format de stockage vardecimal est désactivé.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CONTROL sur la table.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant estime la réduction de la taille de ligne si la table `Production.WorkOrderRouting` de la base de données `AdventureWorks2012` est compressée.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_estimated_rowsize_reduction_for_vardecimal 'Production.WorkOrderRouting' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_db_vardecimal_storage_format &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md)   
 [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)  
  
  
