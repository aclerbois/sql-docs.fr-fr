---
title: sp_addmessage (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_addmessage
- sp_addmessage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addmessage
ms.assetid: 54746d30-f944-40e5-a707-f2d9be0fb9eb
caps.latest.revision: 25
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 964f0909c136eddc86571ce776b559083c8ce3e1
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddmessage-transact-sql"></a>sp_addmessage (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Stocke un nouveau message d’erreur définis par l’utilisateur dans une instance de la [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Les messages stockés à l’aide de **sp_addmessage** peuvent être affichés à l’aide de la **sys.messages** affichage catalogue.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addmessage [ @msgnum= ] msg_id , [ @severity= ] severity , [ @msgtext= ] 'msg'   
     [ , [ @lang= ] 'language' ]   
     [ , [ @with_log= ] { 'TRUE' | 'FALSE' } ]   
     [ , [ @replace= ] 'replace' ]   
```  
  
## <a name="arguments"></a>Arguments  
 [ **@msgnum****=** ] *msg_id*  
 ID du message. *msg_id* est **int** avec NULL comme valeur par défaut. *msg_id* d’erreur définis par l’utilisateur, messages peuvent être un entier compris entre 50 001 et 2 147 483 647. La combinaison de *msg_id* et *langage* doit être unique ; une erreur est retournée si l’ID existe déjà pour la langue spécifiée.  
  
 [  **@severity =** ]*gravité*  
 Est le niveau de gravité de l’erreur. *gravité* est **smallint** avec NULL comme valeur par défaut. Les niveaux valides vont de 1 à 25. Pour plus d’informations sur les niveaux de gravité, consultez [Niveaux de gravité des erreurs du moteur de base de données](../../relational-databases/errors-events/database-engine-error-severities.md).  
  
 [  **@msgtext =** ] **'***msg***'**  
 Est le texte du message d’erreur. *Msg* est **nvarchar (255)** avec NULL comme valeur par défaut.  
  
 [  **@lang =** ] **'***langage***'**  
 Langue du message. *langage* est **sysname** avec NULL comme valeur par défaut. Étant donné que plusieurs langues peuvent être installés sur le même serveur, *langage* spécifie la langue dans laquelle chaque message est écrit. Lorsque *langage* est omis, la langue est la langue par défaut pour la session.  
  
 [  **@with_log =** ] { **'** TRUE **'** | **'FALSE'** }  
 Argument utilisé lorsque le message doit être consigné dans le journal des applications Windows. **@with_log** est **varchar (5)** avec la valeur FALSE par défaut. Si sa valeur est TRUE, l'erreur est automatiquement écrite dans le journal des applications Windows. Si sa valeur est FALSE, l'erreur n'est pas automatiquement écrite dans le journal des applications Windows ; c'est la façon dont elle a été déclenchée qui détermine si l'erreur sera ou non écrite dans le journal. Seuls les membres de la **sysadmin** rôle de serveur peut utiliser cette option.  
  
> [!NOTE]  
>  Si un message est écrit dans le journal des applications Windows, il est également écrit dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] fichier journal des erreurs.  
  
 [ **@replace** *=* ] **'***remplacer***'**  
 S’il est spécifié comme chaîne *remplacer*, un message d’erreur existant est remplacé par le nouveau niveau de texte et la gravité de message. *Remplacez* est **varchar(7)** avec NULL comme valeur par défaut. Cette option doit être spécifiée si *msg_id* existe déjà. Si vous remplacez un message en anglais, Message en anglais, le niveau de gravité est remplacé pour tous les messages dans tous les autres langages qui ont le même *msg_id*.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun  
  
## <a name="remarks"></a>Notes  
 Pour les versions non anglaises de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la version anglaise d'un message doit exister préalablement à l'ajout d'un message dans une autre langue. Le degré de gravité des deux versions du message doit correspondre.  
  
 Lors de la localisation des messages qui contiennent des paramètres, utilisez les numéros de paramètres correspondant aux paramètres des messages d'origine. Insérez un point d'exclamation (!) après chaque numéro de paramètre.  
  
|Message d'origine|Message localisé|  
|----------------------|-----------------------|  
|''Message d'origine param 1 : %s,<br /><br /> param 2 : %d'|''Message localisé param 1 : %1!,<br /><br /> param 2 : %2!'|  
  
 La syntaxe pouvant différer d'une langue à l'autre, il se peut que les numéros de paramètres apparaissent dans un ordre différent par rapport au message d'origine.  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’appartenance dans le **sysadmin** ou **serveradmin** rôles serveur fixes.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-defining-a-custom-message"></a>A. Définition d'un message personnalisé  
 L’exemple suivant ajoute un message personnalisé à **sys.messages**.  
  
```  
USE master;  
GO  
EXEC sp_addmessage 50001, 16,   
   N'Percentage expects a value between 20 and 100.   
   Please reexecute with a more appropriate value.';  
GO  
```  
  
### <a name="b-adding-a-message-in-two-languages"></a>B. Ajout d'un message en deux langues  
 L'exemple suivant ajoute d'abord un message en anglais, puis ajoute ce même message en français`.`  
  
```  
USE master;  
GO  
EXEC sp_addmessage @msgnum = 60000, @severity = 16,   
   @msgtext = N'The item named %s already exists in %s.',   
   @lang = 'us_english';  
  
EXEC sp_addmessage @msgnum = 60000, @severity = 16,   
   @msgtext = N'L''élément nommé %1! existe déjà dans %2!',   
   @lang = 'French';  
GO  
```  
  
### <a name="c-changing-the-order-of-parameters"></a>C. Modification de l'ordre des paramètres  
 L'exemple suivant ajoute d'abord un message en anglais, puis un message localisé dans lequel l'ordre des paramètres est modifié.  
  
```  
USE master;  
GO  
  
EXEC sp_addmessage   
    @msgnum = 60000,   
    @severity = 16,  
    @msgtext =   
        N'This is a test message with one numeric  
        parameter (%d), one string parameter (%s),   
        and another string parameter (%s).',  
    @lang = 'us_english';  
  
EXEC sp_addmessage   
    @msgnum = 60000,   
    @severity = 16,  
    @msgtext =   
        -- In the localized version of the message,  
        -- the parameter order has changed. The   
        -- string parameters are first and second  
        -- place in the message, and the numeric   
        -- parameter is third place.  
        N'Dies ist eine Testmeldung mit einem   
        Zeichenfolgenparameter (%3!),  
        einem weiteren Zeichenfolgenparameter (%2!),   
        und einem numerischen Parameter (%1!).',  
    @lang = 'German';  
GO    
  
-- Changing the session language to use the U.S. English  
-- version of the error message.  
SET LANGUAGE us_english;  
GO  
  
RAISERROR(60000,1,1,15,'param1','param2') -- error, severity, state,  
GO                                       -- parameters.  
  
-- Changing the session language to use the German  
-- version of the error message.  
SET LANGUAGE German;  
GO  
  
RAISERROR(60000,1,1,15,'param1','param2'); -- error, severity, state,   
GO                                       -- parameters.  
```  
  
## <a name="see-also"></a>Voir aussi  
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [sp_altermessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)   
 [sp_dropmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
