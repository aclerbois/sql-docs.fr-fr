---
title: BEGIN CONVERSATION TIMER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CONVERSATION
- BEGIN_CONVERSATION_TSQL
- TIMER_TSQL
- TIMER
- CONVERSATION TIMER
- CONVERSATION_TIMER_TSQL
- BEGIN CONVERSATION TIMER
- BEGIN_CONVERSATION_TIMER_TSQL
- CONVERSATION_TSQL
- BEGIN CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN CONVERSATION TIMER statement
- DialogTimer message
- dialogs [Service Broker], beginning
- timeouts [Service Broker]
- timer messages [Service Broker]
- conversations [Service Broker], timers
- starting timers [Service Broker]
- http://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer message
ms.assetid: 98e49b3f-a38f-4180-8171-fa9cb30db4cb
caps.latest.revision: 28
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: bff71c5286d0bf48e43bda62cdc4b7c26a10b038
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="begin-conversation-timer-transact-sql"></a>BEGIN CONVERSATION TIMER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Démarre le minuteur. Quand le délai d’attente expire, [!INCLUDE[ssSB](../../includes/sssb-md.md)] place un message de type `http://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer` sur la file d’attente locale de la conversation.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BEGIN CONVERSATION TIMER ( conversation_handle )  
   TIMEOUT = timeout   
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 BEGIN CONVERSATION TIMER **(***conversation_handle***)**  
 Indique la conversation à minuter. *conversation_handle* doit être de type **uniqueidentifier**.  
  
 TIMEOUT  
 Indique, en secondes, le délai qui doit s'écouler avant le placement du message dans la file d'attente.  
  
## <a name="remarks"></a>Notes   
 Un minuteur de conversation permet à une application de recevoir un message sur une conversation après un délai spécifié. Appeler BEGIN CONVERSATION TIMER sur une conversation avant l'expiration du délai permet de définir une nouvelle valeur d'expiration. Contrairement à la durée de vie d'une conversation, chaque partie d'une conversation dispose d'un minuteur indépendant. Le message **DialogTimer** arrive dans la file d’attente locale sans affecter le côté distant de la conversation. Par conséquent, une application peut utiliser un message de minuteur à quelque fin que ce soit.  
  
 Par exemple, vous pouvez utiliser le minuteur pour éviter à une application d'attendre trop longtemps une réponse. Si l'application est censée terminer un dialogue en 30 secondes, vous pouvez définir le minuteur pour ce dialogue sur 60 secondes (30 secondes plus une période de grâce de 30 secondes). Si le dialogue est toujours ouvert une fois les 60 secondes écoulées, l'application reçoit un message d'expiration du délai dans la file d'attente de ce dialogue.  
  
 Une application peut également utiliser un minuteur pour demander une activation à un moment donné. Par exemple, vous pouvez créer un service qui génère un rapport sur le nombre de connexions actives régulièrement (toutes les x minutes) ou un service qui génère un rapport sur le nombre de bons de commande ouverts tous les soirs. Le service définit un minuteur de conversation de sorte qu’il expire au moment voulu ; quand le minuteur expire, [!INCLUDE[ssSB](../../includes/sssb-md.md)] envoie un message **DialogTimer**. Suite à ce message **DialogTimer**, [!INCLUDE[ssSB](../../includes/sssb-md.md)] démarre la procédure stockée d’activation pour la file d’attente. La procédure stockée envoie un message au service distant et redémarre le minuteur.  
  
 BEGIN CONVERSATION TIMER n'est pas valide dans une fonction définie par l'utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 L’autorisation de définition d’un minuteur de conversation est octroyée par défaut aux utilisateurs qui disposent de l’autorisation SEND sur le service pour la conversation, aux membres du rôle serveur fixe **sysadmin** et aux membres du rôle de base de données fixe **db_owner**.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre comment définir un délai d'expiration de deux minutes pour le dialogue identifié par `@dialog_handle`.  
  
```  
-- @dialog_handle is of type uniqueidentifier and  
-- contains a valid conversation handle.  
  
BEGIN CONVERSATION TIMER (@dialog_handle)  
TIMEOUT = 120 ;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)  
  
  
