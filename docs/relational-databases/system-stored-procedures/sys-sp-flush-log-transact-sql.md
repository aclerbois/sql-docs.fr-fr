---
title: Sys.sp_flush_log (Transact-SQL) | Documents Microsoft
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
- sp_flush_log_TSQL
- sys.sp_flush_log
- sys.sp_flush_log_TSQL
- sp_flush_log
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_flush_log
ms.assetid: 75cc9f52-3b1f-4754-b1e7-ce0dd3323bc9
caps.latest.revision: 6
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 010870b0364cd302928fd9e0cc8491133f2283b4
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysspflushlog-transact-sql"></a>sys.sp_flush_log (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Vide sur le disque le journal des transactions de la base de données active, renforçant ainsi toutes les transactions durables différées déjà validées.  
  
 Si vous choisissez d'utiliser la durabilité différée des transactions en raison des avantages qu'elle offre en matière de performances, mais que vous voulez également disposer d'une limite garantie sur la quantité de données qui sont perdues en cas de défaillance ou de basculement du serveur, exécutez `sys.sp_flush_log` lors d'une planification régulière. Par exemple, si vous voulez avoir la certitude de ne pas perdre plus de x secondes de données, vous devez exécuter `sp_flush_log` toutes les x secondes.  
  
 L'exécution de `sys.sp_flush_log` garantit que toutes les transactions durables différées déjà validées sont rendues durables. Consultez la rubrique conceptuelle [contrôle la durabilité des transactions](../../relational-databases/logs/control-transaction-durability.md) pour plus d’informations.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
  
sys.sp_flush_log  
  
```  
  
#### <a name="parameters"></a>Paramètres  
 Aucun.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Un code de retour de 1 indique un succès.  Toute autre valeur signale un échec.  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="sample-code"></a>Exemple de code  
  
```sql  
.  
EXECUTE sys.sp_flush_log  
  
```  
  
  
