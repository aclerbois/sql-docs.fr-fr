---
title: SEND (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SEND_ON_CONVERSATION_TSQL
- ON_CONVERSATION_TSQL
- SEND
- SEND_TSQL
- SEND ON CONVERSATION
- ON CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker], message sending
- SEND statement
- messages [Service Broker], sending
- sending messages
ms.assetid: b6e66aeb-1714-4c2b-b7c2-d386d77b0d46
caps.latest.revision: 32
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 70bfd0c1f507a9ee27cd0f1286dfa29f9b57071f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="send-transact-sql"></a>SEND (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Envoie un message à l'aide d'une ou de plusieurs conversations existantes.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SEND  
   ON CONVERSATION [(]conversation_handle [,.. @conversation_handle_n][)]  
   [ MESSAGE TYPE message_type_name ]  
   [ ( message_body_expression ) ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 ON CONVERSATION *conversation_handle [.. @conversation_handle_n]*  
 Spécifie les conversations auxquelles appartient le message. *conversation_handle* doit contenir un identificateur de conversation valide. Le même descripteur de conversation ne peut pas être utilisé plus d'une fois.  
  
 MESSAGE TYPE *message_type_name*  
 Spécifie le type du message envoyé. Ce type de message doit être inclus dans les contrats de service qu'utilisent ces conversations. Ces contrats doivent autoriser l'envoi de ce type de message depuis cette partie de la conversation. Par exemple, les services cibles des conversations ne peuvent envoyer que les messages spécifiés dans le contrat comme SENT BY TARGET ou SENT BY ANY. Si cette clause est omise, le message est du type DEFAULT.  
  
 *message_body_expression*  
 Fournit une expression représentant le corps du message. *message_body_expression* est facultatif. Toutefois, si *message_body_expression* est présent, le type d’expression doit pouvoir être converti vers le type **varbinary(max)**. Cette expression ne peut pas avoir la valeur NULL. Si cette clause est omise, le corps du message est vide.  
  
## <a name="remarks"></a>Notes   
  
> [!IMPORTANT]  
>  Si l'instruction SEND n'est pas la première instruction d'un lot ou d'une procédure stockée, l'instruction qui précède doit se terminer par un point-virgule (;).  
  
 L'instruction SEND transmet un message à partir des services situés à une extrémité d'une ou de plusieurs conversations [!INCLUDE[ssSB](../../includes/sssb-md.md)] aux services situés à l'autre extrémité de ces conversations. L'instruction RECEIVE permet ensuite de récupérer le message envoyé depuis les files d'attente associées aux services cibles.  
  
 Les descripteurs de conversation fournis à la clause ON CONVERSATION proviennent de l'une des trois sources suivantes :  
  
-   Lorsque vous envoyez un message qui n'est pas une réponse à un message reçu d'un autre service, utilisez le descripteur de conversation retourné par l'instruction BEGIN DIALOG qui a créé la conversation.  
  
-   Lorsque vous envoyez un message qui est une réponse à un message reçu précédemment d'un autre service, utilisez le descripteur de conversation retourné par l'instruction RECEIVE qui a retourné le message d'origine.  
  
 Dans de nombreux cas, le code qui contient l’instruction SEND est distinct du code qui contient l’instruction BEGIN DIALOG ou RECEIVE qui fournit le descripteur de conversation. Dans ces cas, le descripteur de conversation doit être l'un des éléments de données inclus dans les informations d'état passées au code qui contient l'instruction SEND.  
  
 Les messages qui sont envoyés aux services dans d’autres instances du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] sont stockés dans une file d’attente de transmission dans la base de données actuelle jusqu’à ce qu’ils puissent être transmis aux files d’attente de service dans les instances distantes. Les messages envoyés aux services dans la même instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sont placés directement dans les files d’attente associées à ces services. Si une condition empêche un message local d'être placé directement dans la file d'attente du service cible, il peut être stocké dans la file d'attente de transmission jusqu'à ce que la condition soit résolue. Cela peut se produire, par exemple, pour certains types d'erreurs ou lorsque la file d'attente du service cible est inactive. Vous pouvez utiliser la vue système **sys.transmission_queue** pour consulter les messages dans la file d’attente de transmission.  
  
 SEND est une instruction atomique, autrement dit, si une instruction SEND qui envoie un message sur plusieurs conversations échoue, par exemple si une conversation se trouve dans un état d'erreur, aucun message ne sera stocké dans la file d'attente de transmission ni mis dans une file d'attente de service cible.  
  
 Service Broker optimise le stockage et la transmission des messages envoyés sur plusieurs conversations dans la même instruction SEND.  
  
 Les messages dans les files d'attente de transmission d'une instance sont transmis dans un ordre défini selon les critères suivants :  
  
-   Le niveau de priorité du point de terminaison de leur conversation associée.  
  
-   dans un même niveau de priorité, leur ordre d'envoi dans la conversation.  
  
 Les niveaux de priorité spécifiés dans les priorités de conversation ne sont appliqués aux messages dans la file d'attente de transmission que si l'option de base de données HONOR_BROKER_PRIORITY a la valeur ON. Si HONOR_BROKER_PRIORITY a la valeur OFF, le niveau de priorité par défaut 5 est attribué à tous les messages placés dans la file d'attente de transmission pour cette base de données. Les niveaux de priorité ne sont pas appliqués à une instruction SEND dans laquelle les messages sont placés directement dans une file d’attente de service dans la même instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 L'instruction SEND verrouille séparément chaque conversation sur laquelle un message est envoyé pour garantir la remise ordonnée par conversation.  
  
 L'instruction SEND n'est pas valide dans une fonction définie par l'utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Pour envoyer un message, l'utilisateur actuel doit avoir l'autorisation RECEIVE sur la file d'attente de chaque service qui envoie le message.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous illustre le début d'un dialogue et l'envoi d'un message XML dans le cadre du dialogue. Pour envoyer le message, l’exemple convertit l’objet xml au format **varbinary(max)**.  
  
```  
DECLARE @dialog_handle UNIQUEIDENTIFIER,  
        @ExpenseReport XML ;  
  
SET @ExpenseReport = < construct message as appropriate for the application > ;  
  
BEGIN DIALOG @dialog_handle  
FROM SERVICE [//Adventure-Works.com/Expenses/ExpenseClient]  
TO SERVICE '//Adventure-Works.com/Expenses'  
ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseProcessing] ;  
  
SEND ON CONVERSATION @dialog_handle  
    MESSAGE TYPE [//Adventure-Works.com/Expenses/SubmitExpense]  
    (@ExpenseReport) ;  
```  
  
 L'exemple ci-dessous illustre le début de trois dialogues et l'envoi d'un message XML dans le cadre de ces dialogues.  
  
```  
DECLARE @dialog_handle1 UNIQUEIDENTIFIER,  
        @dialog_handle2 UNIQUEIDENTIFIER,  
        @dialog_handle3 UNIQUEIDENTIFIER,  
        @OrderMsg XML ;  
  
SET @OrderMsg = < construct message as appropriate for the application > ;  
  
BEGIN DIALOG @dialog_handle1  
FROM SERVICE [//InitiatorDB/InitiatorService]  
TO SERVICE '//TargetDB1/TargetService’  
ON CONTRACT [//AllDBs/OrderProcessing] ;  
  
BEGIN DIALOG @dialog_handle2  
FROM SERVICE [//InitiatorDB/InitiatorService]  
TO SERVICE '//TargetDB2/TargetService’  
ON CONTRACT [//AllDBs/OrderProcessing] ;  
  
BEGIN DIALOG @dialog_handle3  
FROM SERVICE [//InitiatorDB/InitiatorService]  
TO SERVICE '//TargetDB3/TargetService’  
ON CONTRACT [//AllDBs/OrderProcessing] ;  
  
SEND ON CONVERSATION (@dialog_handle1, @dialog_handle2, @dialog_handle3)  
    MESSAGE TYPE [//AllDBs/OrderMsg]  
    (@OrderMsg) ;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)   
 [sys.transmission_queue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)  
  
  
