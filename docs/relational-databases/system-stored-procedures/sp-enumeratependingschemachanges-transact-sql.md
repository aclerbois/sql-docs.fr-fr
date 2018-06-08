---
title: sp_enumeratependingschemachanges (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sp_enumeratependingschemachanges
- sp_enumeratependingschemachanges_TSQL
helpviewer_keywords:
- sp_enumeratependingschemachanges
ms.assetid: df169b21-d10a-41df-b3a1-654cfb58bc21
caps.latest.revision: 23
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 74841a258582acce3d9d5a30aa19cd9f000bdb8e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spenumeratependingschemachanges-transact-sql"></a>sp_enumeratependingschemachanges (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Renvoie une liste de toutes les modifications de schéma en attente. Cette procédure stockée peut être utilisée avec [sp_markpendingschemachange](../../relational-databases/system-stored-procedures/sp-markpendingschemachange-transact-sql.md), ce qui permet à un administrateur d’ignorer certaines modifications de schéma pour qu’elles ne sont pas répliquées. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_enumeratependingschemachanges [ @publication = ] 'publication'   
    [ , [ @starting_schemaversion = ] starting_schemaversion ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication=** ] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, sans valeur par défaut.  
  
 [  **@starting_schemaversion=** ] *starting_schemaversion*  
 Plus petit numéro de modification de schéma à inclure dans l'ensemble de résultats.  
  
## <a name="result-set"></a>Jeu de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom_article**|**sysname**|Nom de l’article auquel s’applique la modification de schéma ou **à l’échelle de la Publication** les modifications de schéma qui s’appliquent à la publication entière.|  
|**version de schéma**|**int**|Numéro de la modification de schéma en attente.|  
|**SchemaType**|**sysname**|Valeur de texte représentant le type de modification de schéma.|  
|**schematext**|**nvarchar(max)**|[!INCLUDE[tsql](../../includes/tsql-md.md)] qui décrit la modification de schéma.|  
|**schemastatus**|**nvarchar(10)**|Indique si une modification de schéma est en attente pour l'article. Peut avoir l'une des valeurs suivantes :<br /><br /> **Active** = modification de schéma est en attente<br /><br /> **inactif** = modification de schéma est inactive<br /><br /> **Ignorer** = modification de schéma n’est pas répliquée.|  
|**SchemaGuid**|**uniqueidentifier**|Identifie la modification de schéma.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_enumeratependingschemachanges** est utilisé dans la réplication de fusion.  
  
 **sp_enumeratependingschemachanges**, utilisé avec [sp_markpendingschemachange](../../relational-databases/system-stored-procedures/sp-markpendingschemachange-transact-sql.md), est conçu pour la prise en charge de la réplication de fusion et doit être utilisé uniquement lorsqu’autres actions correctives, telles que la réinitialisation, pas de résoudre la situation.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_enumeratependingschemachanges**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sysmergeschemachange &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysmergeschemachange-transact-sql.md)  
  
  
