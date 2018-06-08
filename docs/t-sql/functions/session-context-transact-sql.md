---
title: SESSION_CONTEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/22/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SESSION_CONTEXT
- sys.SESSION_CONTEXT
- SESSION_CONTEXT_TSQL
- sys.SESSION_CONTEXT_TSQL
helpviewer_keywords:
- SESSION_CONTEXT function
ms.assetid: b6bdbc54-331a-43cc-ab3d-3872d6a12100
caps.latest.revision: 11
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 8d8992335c2166d4cfad8ff2e489f58510574656
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sessioncontext-transact-sql"></a>SESSION_CONTEXT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Renvoie la valeur de la clé spécifiée dans le contexte de la session en cours. La valeur est définie à l’aide de la procédure [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SESSION_CONTEXT(N'key')  
```  
  
## <a name="arguments"></a>Arguments  
 'key'  
 Clé (type sysname) de la valeur en cours de récupération.  
  
## <a name="return-type"></a>Type de retour  
 **sql_variant**  
  
## <a name="return-value"></a>Valeur retournée  
 Valeur associée à la clé spécifiée dans le contexte de session, ou NULL si aucune valeur n’a été définie pour cette clé.  
  
## <a name="permissions"></a>Autorisations  
 Tout utilisateur peut lire le contexte de session pour sa session.  
  
## <a name="remarks"></a>Notes   
 Le comportement MARS de SESSION_CONTEXT est similaire à celui de CONTEXT_INFO. Si un lot MARS définit une paire clé-valeur, la nouvelle valeur n’est pas renvoyée dans d’autres lots MARS sur la même connexion, sauf s’ils ont démarré après le lot qui a défini la nouvelle valeur. Si plusieurs lots MARS sont actifs sur une connexion, les valeurs ne peuvent pas être définies comme « read_only ». Cela empêche les conditions de concurrence et le non-déterminisme quant à la valeur qui « gagne ».  
  
## <a name="examples"></a>Exemples  
 L’exemple simple suivant attribue la valeur 4 au contexte de session pour la clé `user_id`, puis utilise la fonction **SESSION_CONTEXT** pour extraire la valeur.  
  
```  
EXEC sp_set_session_context 'user_id', 4;  
SELECT SESSION_CONTEXT(N'user_id');  
```  
  
## <a name="see-also"></a> Voir aussi  
 [sp_set_session_context &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)   
 [CURRENT_TRANSACTION_ID &#40;Transact-SQL&#41;](../../t-sql/functions/current-transaction-id-transact-sql.md)   
 [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md)   
 [CONTEXT_INFO  &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)   
 [SET CONTEXT_INFO &#40;Transact-SQL&#41;](../../t-sql/statements/set-context-info-transact-sql.md)  
  
  
