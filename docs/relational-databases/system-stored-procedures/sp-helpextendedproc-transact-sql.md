---
title: sp_helpextendedproc (Transact-SQL) | Documents Microsoft
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
- sp_helpextendedproc
- sp_helpextendedproc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpextendedproc
ms.assetid: 7e1f017e-c898-4225-b375-6a73ef9aac7b
caps.latest.revision: 28
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a7e9cc50f543232dea6b5ce39153eee2292284ac
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sphelpextendedproc-transact-sql"></a>sp_helpextendedproc (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Indique les procédures stockées étendues actuellement définies, ainsi que le nom de la bibliothèque de liaison dynamique (DLL) à laquelle la procédure (fonction) appartient.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez l’ [intégration CLR](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpextendedproc [ [@funcname = ] 'procedure' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@funcname =**] **'***procédure***'**  
 Nom de la procédure stockée étendue dont les informations sont signalées. *procédure* est **sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**sysname**|Nom de la procédure stockée étendue.|  
|**DLL**|**nvarchar(255)**|Nom de la DLL.|  
  
## <a name="remarks"></a>Notes  
 Lorsque *procédure* est spécifié, **sp_helpextendedproc** rapports spécifiées de procédure stockée étendue. Lorsque ce paramètre n’est pas fourni, **sp_helpextendedproc** retourne toutes les étendues des noms de procédure stockée et les noms de DLL pour laquelle chaque procédure stockée étendue appartient.  
  
## <a name="permissions"></a>Autorisations  
 L’autorisation d’exécuter **sp_helpextendedproc** est accordée aux **public**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-reporting-help-on-all-extended-stored-procedures"></a>A. Indication d'aide sur toutes les procédures stockées étendues  
 L'exemple suivant vous renseigne sur toutes les procédures stockées étendues.  
  
```  
USE master;  
GO  
EXEC sp_helpextendedproc;  
GO  
```  
  
### <a name="b-reporting-help-on-a-single-extended-stored-procedure"></a>B. Indication d'aide sur une procédure stockée étendue  
 L’exemple suivant indique les `xp_cmdshell` la procédure stockée étendue.  
  
```  
USE master;  
GO  
EXEC sp_helpextendedproc xp_cmdshell;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addextendedproc &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproc-transact-sql.md)   
 [sp_dropextendedproc &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproc-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
