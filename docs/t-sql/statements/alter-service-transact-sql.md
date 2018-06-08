---
title: ALTER SERVICE (Transact-SQL) | Microsoft Docs
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
- ALTER SERVICE
- ALTER_SERVICE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- modifying services
- contracts [Service Broker], modifying
- ALTER SERVICE statement
- services [Service Broker], modifying
ms.assetid: 2b4608f7-bb2e-4246-aa29-b52c55995b3a
caps.latest.revision: 31
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2a96de3e7df2c381ce8b1fe30c98769050fc65ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-service-transact-sql"></a>ALTER SERVICE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie un service existant.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ALTER SERVICE service_name   
   [ ON QUEUE [ schema_name . ]queue_name ]   
   [ ( < opt_arg > [ , ...n ] ) ]  
[ ; ]  
  
<opt_arg> ::=  
   ADD CONTRACT contract_name | DROP CONTRACT contract_name  
```  
  
## <a name="arguments"></a>Arguments  
 *service_name*  
 Nom du service à modifier. Les noms du serveur, de la base de données et du schéma ne peuvent pas être spécifiés.  
  
 ON QUEUE [ *schema_name***.** ] *queue_name*  
 Spécifie la nouvelle file d'attente de ce service. [!INCLUDE[ssSB](../../includes/sssb-md.md)] déplace tous les messages destinés à ce service de la file d'attente actuelle vers la nouvelle file d'attente.  
  
 ADD CONTRACT *contract_name*  
 Spécifie un contrat à ajouter à l'ensemble de contrats exposé par ce service.  
  
 DROP CONTRACT *contract_name*  
 Spécifie un contrat à supprimer de l'ensemble de contrats exposé par ce service. [!INCLUDE[ssSB](../../includes/sssb-md.md)] envoie un message d'erreur sur toutes les conversations existantes avec ce service qui utilisent ce contrat.  
  
## <a name="remarks"></a>Notes   
 Lorsque l'instruction ALTER SERVICE supprime un contrat d'un service, ce dernier ne peut plus être la destination des conversations qui utilisent ce contrat. Par conséquent, [!INCLUDE[ssSB](../../includes/sssb-md.md)] n'autorise aucune nouvelle conversation avec le service sur ce contrat. Les conversations existantes qui utilisent le contrat ne sont pas affectées.  
  
 L'instruction ALTER AUTHORIZATION permet de modifier l'AUTORISATION pour un service.  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, l’autorisation de modification d’un service est octroyée au propriétaire de ce service, aux membres des rôles de base de données fixes **db_ddladmin** ou **db_owner** et aux membres du rôle serveur fixe **sysadmin**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-queue-for-a-service"></a>A. Modification de la file d'attente d'un service  
 L'exemple suivant modifie le service `//Adventure-Works.com/Expenses` pour qu'il utilise la file d'attente `NewQueue`.  
  
```  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    ON QUEUE NewQueue ;  
```  
  
### <a name="b-adding-a-new-contract-to-the-service"></a>B. Ajout d'un nouveau contrat au service  
 L'exemple suivant modifie le service `//Adventure-Works.com/Expenses` pour qu'il autorise les dialogues sur le contrat `//Adventure-Works.com/Expenses`.  
  
```  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    (ADD CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
### <a name="c-adding-a-new-contract-to-the-service-dropping-existing-contract"></a>C. Ajout d'un nouveau contrat au service et suppression du contrat existant  
 L'exemple suivant modifie le service `//Adventure-Works.com/Expenses` pour qu'il autorise les dialogues sur le contrat `//Adventure-Works.com/Expenses/ExpenseProcessing` et qu'il les interdise sur le contrat `//Adventure-Works.com/Expenses/ExpenseSubmission`.  
  
```  
ALTER SERVICE [//Adventure-Works.com/Expenses]  
    (ADD CONTRACT [//Adventure-Works.com/Expenses/ExpenseProcessing],   
     DROP CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-service-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
