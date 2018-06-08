---
title: sp_altermessage (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_altermessage_TSQL
- sp_altermessage
dev_langs:
- TSQL
helpviewer_keywords:
- sp_altermessage
ms.assetid: 1b28f280-8ef9-48e9-bd99-ec14d79abaca
caps.latest.revision: 32
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: cce6620bbc74d5c83cef907ab87f8252f963748a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaltermessage-transact-sql"></a>sp_altermessage (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie l'état des messages système ou définis par l'utilisateur dans une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Les messages définis par l’utilisateur peuvent être affichés à l’aide de la **sys.messages** affichage catalogue.  

  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_altermessage [ @message_id = ] message_number   ,[ @parameter = ]'write_to_log'  
   ,[ @parameter_value = ]'value'   
```  
  
## <a name="arguments"></a>Arguments  
 [ **@message_id =** ] *numéro_de_message*  
 Numéro d’erreur du message à modifier à partir de **sys.messages**. *numéro_de_message* est **int** sans valeur par défaut.  
  
 [  **@parameter =** ] **' *** écriture_dans_journal*'  
 Est utilisé avec **@parameter_value** pour indiquer que le message est à écrire dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] journal des applications Windows. *écriture_dans_journal* est **sysname** sans valeur par défaut. *écriture_dans_journal* doit être définie sur WITH_LOG ou NULL. Si *écriture_dans_journal* a la valeur WITH_LOG ou NULL et la valeur de **@parameter_value** est **true**, le message est écrit dans le journal des applications Windows. Si *écriture_dans_journal* a la valeur WITH_LOG ou NULL et la valeur de **@parameter_value** est **false**, le message n’est pas toujours écrit dans le journal des applications Windows, mais peut être écrit selon la façon dont l’erreur a été générée. Si *écriture_dans_journal* est spécifié, la valeur de **@parameter_value** doit également être spécifié.  
  
> [!NOTE]  
>  Si un message est écrit dans le journal des applications Windows, il est également écrit dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] fichier journal des erreurs.  
  
 [  **@parameter_value =** ] **' *** valeur*'  
 Est utilisé avec **@parameter** pour indiquer que l’erreur doit être écrite dans le [!INCLUDE[msCoName](../../includes/msconame-md.md)] journal des applications Windows. *valeur* est **varchar (5)**, sans valeur par défaut. Si **true**, l’erreur est automatiquement écrite dans le journal des applications Windows. Si **false**, l’erreur n’est pas toujours écrit dans le journal des applications Windows, mais peut être écrit selon la façon dont l’erreur a été générée. Si *valeur* est spécifié, *écriture_dans_journal* pour **@parameter** doit également être spécifié.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 L’effet de **sp_altermessage** avec le WITH_LOG option est similaire à celui du paramètre RAISERROR WITH LOG, à ceci près que **sp_altermessage** modifie le comportement de journalisation d’un message existant. Si un message a été modifié avec l'option WITH_LOG, il est toujours écrit dans le journal des applications Windows, quelle que soit la manière dont un utilisateur appelle l'erreur. Même si RAISERROR est exécuté sans l'option WITH_LOG, l'erreur est écrite dans le journal des applications Windows.  
  
 Messages système peuvent être modifiés à l’aide de **sp_altermessage**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **serveradmin** rôle serveur fixe.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple suivant, le message `55001` est enregistré dans le journal des applications Windows.  
  
```  
EXECUTE sp_altermessage 55001, 'WITH_LOG', 'true';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_dropmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
