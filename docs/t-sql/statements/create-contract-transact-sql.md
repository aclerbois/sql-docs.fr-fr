---
title: CREATE CONTRACT (Transact-SQL) | Microsoft Docs
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
- CONTRACT_TSQL
- CREATE_CONTRACT_TSQL
- CREATE CONTRACT
- CONTRACT
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE CONTRACT statement
- contracts [Service Broker], creating
- message types [Service Broker], contracts
ms.assetid: 494cbfa6-8e93-4161-a64d-90d681915211
caps.latest.revision: 48
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 70191a58152cb2754a5713f452065324b8c2fa85
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="create-contract-transact-sql"></a>CREATE CONTRACT (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un nouveau contrat. Un contrat définit les types de messages utilisés dans une conversation [!INCLUDE[ssSB](../../includes/sssb-md.md)] et détermine quel côté de la conversation peut envoyer des messages de ce type. Chaque conversation observe un contrat. Le service initiateur spécifie le contrat associé à la conversation au démarrage de celle-ci. Le service cible spécifie les contrats pour lesquels il accepte des conversations.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CREATE CONTRACT contract_name  
   [ AUTHORIZATION owner_name ]  
      (  {   { message_type_name | [ DEFAULT ] }  
          SENT BY { INITIATOR | TARGET | ANY }   
       } [ ,...n] )   
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 *contract_name*  
 Nom du contrat à créer. Un nouveau contrat est créé dans la base de données active et il appartient au principal spécifié dans la clause AUTHORIZATION. Les noms du serveur, de la base de données et du schéma ne peuvent pas être spécifiés. *contract_name* peut comporter jusqu’à 128 caractères.  
  
> [!NOTE]  
>  Ne créez pas de contrat qui utilise le mot clé ANY pour *contract_name*. Lorsque vous spécifiez ANY pour un nom de contrat dans CREATE BROKER PRIORITY, la priorité est considérée pour tous les contrats. Elle n'est pas limitée à un contrat dont le nom est ANY.  
  
 AUTHORIZATION *owner_name*  
 Définit comme propriétaire du contrat le rôle ou l'utilisateur de base de données spécifié. Quand l’utilisateur actuel est **dbo** ou **sa**, l’argument *owner_name* peut être le nom de n’importe quel utilisateur ou rôle valide. Sinon, *owner_name* doit être le nom de l’utilisateur actuel, le nom d’un utilisateur pour lequel l’utilisateur actuel possède des autorisations d’emprunt d’identité ou le nom d’un rôle auquel appartient l’utilisateur actuel. Lorsque cette clause est omise, le contrat appartient à l'utilisateur actif.  
  
 *message_type_name*  
 Nom d'un type de message à inclure dans le contrat.  
  
 SENT BY  
 Spécifie quel point de terminaison peut envoyer un message du type indiqué. Les contrats mentionnent les messages que les services peuvent utiliser pour avoir des conversations spécifiques. Chaque conversation comporte deux points de terminaison : l’*initiateur* (service qui démarre la conversation) et la *cible* (service contacté par l’initiateur).  
  
 INITIATOR  
 Indique que seul l'initiateur de la conversation peut envoyer des messages du type spécifié. Un service qui démarre une conversation est appelé l’*initiateur* de cette conversation.  
  
 TARGET  
 Indique que seule la cible de la conversation peut envoyer des messages du type spécifié. Un service qui accepte une conversation démarrée par un autre service est appelé la *cible* de cette conversation.  
  
 ANY  
 Indique que les messages de ce type peuvent être envoyés par l'initiateur comme par la cible de la conversation.  
  
 [ DEFAULT ]  
 Indique que le contrat prend en charge les messages du type de message par défaut. Par défaut, toutes les bases de données contiennent un type de message nommé DEFAULT. Ce type de message utilise l'option de validation NONE. Dans le contexte de cette clause, DEFAULT n'est pas un mot clé et il doit être délimité comme un identificateur. Microsoft SQL Server fournit également un contrat DEFAULT qui spécifie le type de message DEFAULT.  
  
## <a name="remarks"></a>Notes   
 L'ordre des types de messages dans le contrat n'a pas d'importance. Une fois que la cible a reçu le premier message, [!INCLUDE[ssSB](../../includes/sssb-md.md)] permet à chaque côté de la conversation d'envoyer à tout moment les messages autorisés pour ce côté. Par exemple, si l’initiateur de la conversation peut envoyer le type de message **//Adventure-Works.com/Expenses/SubmitExpense**, [!INCLUDE[ssSB](../../includes/sssb-md.md)] l’autorise à envoyer un nombre illimité de messages **SubmitExpense** au cours de la conversation.  
  
 Il est impossible de modifier les types de messages et leurs directions dans un contrat. Pour modifier le paramètre AUTHORIZATION au niveau d'un contrat, utilisez l'instruction ALTER AUTHORIZATION.  
  
 Un contrat doit permettre à l'initiateur d'envoyer un message. L'instruction CREATE CONTRACT échoue lorsque le contrat ne contient pas au moins un type de message SENT BY ANY ou SENT BY INITIATOR.  
  
 Indépendamment du contrat, un service peut toujours recevoir les types de messages `http://schemas.microsoft.com/SQL/ServiceBroker/DialogTimer`, `http://schemas.microsoft.com/SQL/ServiceBroker/Error` et `http://schemas.microsoft.com/SQL/ServiceBroker/EndDialog`. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise ces types de messages pour la communication entre le système et l'application.  
  
 Un contrat ne peut pas être un objet temporaire. Les noms de contrats commençant par # sont autorisés mais ce sont des objets permanents.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres des rôles de base de données fixes **db_ddladmin** ou **db_owner** et les membres du rôle serveur fixe **sysadmin** peuvent créer des contrats.  
  
 L’autorisation REFERENCES sur un contrat est accordée par défaut au propriétaire du contrat, aux membres du rôle de base de données fixe **db_ddladmin** ou **db_owner**, ainsi qu’aux membres du rôle serveur fixe **sysadmin**.  
  
 L'utilisateur exécutant l'instruction CREATE SERVICE doit disposer de l'autorisation REFERENCES sur tous les types de messages spécifiés.  
  
## <a name="examples"></a>Exemples  
 **A. Création d’un contrat**  
  
 L'exemple suivant crée un contrat de remboursement de frais basé sur trois types de messages.  
  
```  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]           
    VALIDATION = WELL_FORMED_XML ;           
  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/ExpenseApprovedOrDenied]           
    VALIDATION = WELL_FORMED_XML ;           
  
CREATE MESSAGE TYPE           
    [//Adventure-Works.com/Expenses/ExpenseReimbursed]           
    VALIDATION= WELL_FORMED_XML ;           
  
CREATE CONTRACT            
    [//Adventure-Works.com/Expenses/ExpenseSubmission]           
    ( [//Adventure-Works.com/Expenses/SubmitExpense]           
          SENT BY INITIATOR,           
      [//Adventure-Works.com/Expenses/ExpenseApprovedOrDenied]           
          SENT BY TARGET,           
      [//Adventure-Works.com/Expenses/ExpenseReimbursed]           
          SENT BY TARGET           
    ) ;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [DROP CONTRACT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-contract-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
