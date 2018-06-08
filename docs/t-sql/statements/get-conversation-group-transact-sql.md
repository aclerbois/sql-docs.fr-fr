---
title: GET CONVERSATION GROUP (Transact-SQL) | Microsoft Docs
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
- GET
- CONVERSATION_GROUP_TSQL
- GET_TSQL
- GET_CONVERSATION_GROUP_TSQL
- GET CONVERSATION GROUP
- CONVERSATION GROUP
- GET CONVERSATION
- GET_CONVERSATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GET CONVERSATION GROUP statement
- conversations [Service Broker], groups
ms.assetid: 4da8a855-33c0-43b2-a49d-527487cb3b5c
caps.latest.revision: 39
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 4f85628c2725c4a71c54349e94468607a0178895
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="get-conversation-group-transact-sql"></a>GET CONVERSATION GROUP (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne l'identificateur du groupe de conversations pour le prochain message à recevoir et verrouille le groupe de conversations pour la conversation contenant ce message. L'identificateur du groupe de conversations peut être utilisé pour extraire des informations sur l'état des conversations avant d'extraire le message lui-même.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
[ WAITFOR ( ]  
   GET CONVERSATION GROUP @conversation_group_id  
      FROM <queue>  
[ ) ] [ , TIMEOUT timeout ]  
[ ; ]  
  
<queue> ::=  
{  
    [ database_name . [ schema_name ] . | schema_name . ] queue_name  
}  
```  
  
## <a name="arguments"></a>Arguments  
 WAITFOR  
 Indique que l'instruction GET CONVERSATION GROUP attend qu'un message arrive dans la file d'attente si celle-ci est actuellement vide.  
  
 *@conversation_group_id*  
 Variable utilisée pour contenir l'ID de groupe de conversations retourné par l'instruction GET CONVERSATION GROUP. La variable doit être de type **uniqueidentifier**. Si aucun groupe de conversations n'est disponible, elle prend la valeur NULL.  
  
 FROM  
 Spécifie la file d'attente à partir de laquelle le groupe de conversations doit être extrait.  
  
 *database_name*  
 Nom de la base de données qui contient la file d'attente à partir de laquelle le groupe de conversations doit être extrait. Quand aucun argument *database_name* n’est fourni, la base de données active est utilisée par défaut.  
  
 *schema_name*  
 Nom du schéma auquel appartient la file d'attente à partir de laquelle le groupe de conversations doit être extrait. Quand aucun argument *schema_name* n’est fourni, le schéma par défaut de l’utilisateur actif est utilisé par défaut.  
  
 *queue_name*  
 Nom de la file d'attente à partir de laquelle le groupe de conversations doit être extrait.  
  
 TIMEOUT *timeout*  
 Spécifie la durée en millisecondes pendant laquelle Service Broker attend l'arrivée d'un message dans la file d'attente. Cette clause ne peut être utilisée qu’avec la clause WAITFOR. Si une instruction utilisant WAITFOR ne comprend pas cette clause ou si la valeur de *timeout* est -1, le temps d’attente est illimité. Si le délai expire, GET CONVERSATION GROUP affecte à la variable *@conversation_group_id* la valeur NULL.  
  
## <a name="remarks"></a>Notes   
  
> [!IMPORTANT]  
>  Si l’instruction GET CONVERSATION GROUP n’est pas la première d’un lot ou d’une procédure stockée, l’instruction qui précède doit se terminer par un point-virgule (**;**), le terminateur d’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Si l'instruction GET CONVERSATION GROUP spécifie une file d'attente indisponible, cette instruction échoue et une erreur [!INCLUDE[tsql](../../includes/tsql-md.md)] se produit.  
  
 Cette instruction retourne le groupe de conversations suivant lorsque toutes les conditions suivantes sont vraies :  
  
-   Le groupe de conversations peut être verrouillé.  
  
-   Le groupe de conversations a des messages disponibles dans la file d'attente.  
  
-   Le groupe de conversations a le niveau de priorité le plus élevé de tous les groupes de conversations qui répondent aux critères répertoriés précédemment. Le niveau de priorité d'un groupe de conversations est le plus élevé assigné à toute conversation qui est membre du groupe et qui a des messages dans la file d'attente.  
  
 Les appels successifs à GET CONVERSATION GROUP au sein d'une même transaction peuvent verrouiller plus d'un groupe de conversations. Si aucun groupe de conversations n'est disponible, l'instruction retourne la valeur NULL comme identificateur de groupe de conversations.  
  
 Lorsque la clause WAITFOR est spécifiée, l'instruction attend pendant le délai spécifié ou jusqu'à ce qu'un groupe de conversations soit disponible. Si la file d'attente est supprimée pendant que l'instruction est en attente, cette dernière retourne immédiatement une erreur.  
  
 GET CONVERSATION GROUP n'est pas valide dans une fonction définie par l'utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Pour obtenir un identificateur de groupe de conversations à partir d'une file d'attente, l'utilisateur doit détenir l'autorisation RECEIVE sur la file d'attente.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-getting-a-conversation-group-waiting-indefinitely"></a>A. Obtention d'un groupe de conversations avec attente illimitée  
 L'exemple suivant attribue à `@conversation_group_id` l'identificateur de groupe de conversations pour le prochain message disponible dans `ExpenseQueue`. La commande attend jusqu'à ce qu'un message soit disponible.  
  
```  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
WAITFOR (  
 GET CONVERSATION GROUP @conversation_group_id  
     FROM ExpenseQueue  
) ;  
```  
  
### <a name="b-getting-a-conversation-group-waiting-one-minute"></a>B. Obtention d'un groupe de conversations avec attente d'une minute  
 L'exemple suivant attribue à `@conversation_group_id` l'identificateur de groupe de conversations pour le prochain message disponible dans `ExpenseQueue`. Si aucun message n'est disponible au bout d'une minute, GET CONVERSATION GROUP retourne le résultat sans changer la valeur de `@conversation_group_id`.  
  
```  
DECLARE @conversation_group_id UNIQUEIDENTIFIER  
  
WAITFOR (  
    GET CONVERSATION GROUP @conversation_group_id   
    FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="c-getting-a-conversation-group-returning-immediately"></a>C. Obtention d'un groupe de conversations avec retour immédiat  
 L'exemple suivant attribue à `@conversation_group_id` l'identificateur de groupe de conversations pour le prochain message disponible dans `ExpenseQueue`. Si aucun message n'est disponible, `GET CONVERSATION GROUP` retourne immédiatement le résultat sans modifier `@conversation_group_id`.  
  
```  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
GET CONVERSATION GROUP @conversation_group_id  
FROM AdventureWorks.dbo.ExpenseQueue ;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [MOVE CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/move-conversation-transact-sql.md)  
  
  
