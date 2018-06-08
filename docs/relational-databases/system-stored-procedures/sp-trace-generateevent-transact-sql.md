---
title: sp_trace_generateevent (Transact-SQL) | Documents Microsoft
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
- sp_trace_generateevent_TSQL
- sp_trace_generateevent
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_generateevent
ms.assetid: 3ef05bfb-b467-4403-89cc-6e77ef9247dd
caps.latest.revision: 35
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 398fb058ae7be57cf0c26b26e77d6e82aafd0df3
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sptracegenerateevent-transact-sql"></a>sp_trace_generateevent (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un événement défini par l'utilisateur dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
>**Remarque :** cette procédure stockée est **pas** déconseillée. Toutes les autres procédures stockées liées à la trace sont déconseillées.  
  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_trace_generateevent [ @eventid = ] event_id   
     [ , [ @userinfo = ] 'user_info' ]  
     [ , [ @userdata = ] user_data ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@eventid=**] *event_id*  
 ID de l'événement à activer. *event_id* est **int**, sans valeur par défaut. L’ID doit être un des numéros d’événements de 82 à 91, qui représentent les événements définis par l’utilisateur en tant qu’ensemble avec [sp_trace_setevent](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md).  
  
 [ **@userinfo**=] **'***user_info***'**  
 Chaîne facultative, définie par l'utilisateur, qui identifie le motif de l'événement. *user_info* est **nvarchar (128)**, avec NULL comme valeur par défaut.  
  
 [ **@userdata**=] *user_data*  
 Données facultatives spécifiées par l'utilisateur et se rapportant à l'événement. *user_data* est **varbinary (8000)**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Le tableau suivant décrit les valeurs de code que les utilisateurs peuvent recevoir à la fin de l'exécution de la procédure stockée.  
  
|Code de retour| Description|  
|-----------------|-----------------|  
|**0**|Aucune erreur.|  
|**1**|Erreur inconnue.|  
|**3**|L'événement spécifié n'est pas valide. L'événement peut ne pas exister ou être inapproprié pour la procédure stockée.|  
|**13**|Mémoire insuffisante. Renvoyé lorsqu'il n'y a pas assez de mémoire pour exécuter l'action spécifiée.|  
  
## <a name="remarks"></a>Notes  
 **sp_trace_generateevent** effectue la plupart des actions précédemment exécutées par le **xp_trace_\***  les procédures stockées étendues. Utilisez **sp_trace_generateevent** au lieu de **xp_trace_generate_event**.  
  
 Seuls les numéros d’ID des événements définis par l’utilisateur peuvent être utilisés avec **sp_trace_generateevent**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur si d'autres numéros d'identification des événements sont utilisés.  
  
 Les paramètres de Trace de SQL toutes les procédures stockées (**sp_trace_xx**) sont de type strict. Si ces paramètres ne sont pas appelés avec des types de données appropriés pour les paramètres d'entrée tels qu'ils sont spécifiés dans la description de l'argument, la procédure stockée renvoie une erreur.  
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit disposer de l'autorisation ALTER TRACE.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant crée un événement pouvant être configuré par l'utilisateur dans un exemple de table.  
  
```  
--Create a sample table.  
CREATE TABLE user_config_test(col1 int, col2 char(10));  
  
--DROP the trigger if it already exists.  
IF EXISTS  
   (SELECT * FROM sysobjects WHERE name = 'userconfig_trg')  
   DROP TRIGGER userconfig_trg;  
  
--Create an ON INSERT trigger on the sample table.  
CREATE TRIGGER userconfig_trg  
   ON user_config_test FOR INSERT;  
AS  
EXEC master..sp_trace_generateevent  
   @event_class = 82, @userinfo = N'Inserted row into user_config_test';  
  
--When an insert action happens, the user-configurable event fires. If   
you were capturing the event id=82, you will see it in the Profiler output.  
INSERT INTO user_config_test VALUES(1, 'abc');  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fn_trace_geteventinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Trace SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  
