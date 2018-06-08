---
title: Vérifier l’état des messages électroniques envoyés avec la messagerie de base de données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: database-mail
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- e-mail [SQL Server], status information
- mail [SQL Server], status information
- Database Mail [SQL Server], message status
- status information [Database Mail]
ms.assetid: eb290f24-b52f-46bc-84eb-595afee6a5f3
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 7a941829ee05c7153b7b6b8a9e5450fe87cbe885
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="check-the-status-of-e-mail-messages-sent-with-database-mail"></a>Vérifier l'état des messages électroniques envoyés avec la messagerie de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment vérifier l'état du message électronique envoyé à l'aide de la messagerie de base de données dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   **Avant de commencer :**  
  
-   **Pour afficher l’état du message électronique envoyé à l’aide de la messagerie de base de données, utilisez :**  [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
 La messagerie de base de données conserve un exemplaire des messages électroniques sortants qu’elle affiche dans les vues **sysmail_allitems**, **sysmail_sentitems**, **sysmail_unsentitems**et **sysmail_faileditems** de la base de données **msdb** . Le programme externe de la messagerie de base de données consigne les activités de l’application dans un journal qu’elle affiche par le biais du journal d’événements des applications Windows et la vue **sysmail_event_log** dans la base de données **msdb** . Pour vérifier l'état d'un message électronique, exécutez une requête sur cette vue. Les messages peuvent présenter quatre états distincts : **sent**(envoyé), **unsent**(non envoyé), **retrying**(nouvel essai) et **failed**(échec).  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour afficher l'état du message électronique envoyé à l'aide de la messagerie de base de données**  
  
1.  Sélectionnez des éléments dans la table **sysmail_allitems** , en spécifiant le ou les messages qui vous intéressent par le biais de l’option **mailitem_id** ou **sent_status**.  
  
2.  Pour vérifier l’état retourné par le programme externe pour les messages électroniques, définissez une jointure allant de **sysmail_allitems** à la vue **sysmail_event_log** dans la colonne **mailitem_id** , comme illustré dans la section suivante.  
  
     Par défaut, le programme externe n'enregistre pas d'informations sur les messages correctement envoyés. Si vous voulez activer le journal pour tous les messages, définissez le niveau de journalisation sur le mode commenté par le biais de la page **Configurer les paramètres du système** de l' **Assistant Configuration de la messagerie de base de données**.  
  
###  <a name="TsqlExample"></a> Exemple (Transact-SQL)  
 L'exemple suivant regroupe des informations sur tous les messages électroniques envoyés à `danw` que le programme externe n'a pas pu envoyer correctement. L'instruction renseigne sur l'objet ainsi que sur la date et l'heure auxquelles la tentative d'envoi du message par le programme externe a échoué, elle indique également le message d'erreur provenant du journal de la messagerie de base de données.  
  
```  
USE msdb ;  
GO  
  
-- Show the subject, the time that the mail item row was last  
-- modified, and the log information.  
-- Join sysmail_faileditems to sysmail_event_log   
-- on the mailitem_id column.  
-- In the WHERE clause list items where danw was in the recipients,  
-- copy_recipients, or blind_copy_recipients.  
-- These are the items that would have been sent  
-- to danw.  
  
SELECT items.subject,  
    items.last_mod_date  
    ,l.description FROM dbo.sysmail_faileditems as items  
INNER JOIN dbo.sysmail_event_log AS l  
    ON items.mailitem_id = l.mailitem_id  
WHERE items.recipients LIKE '%danw%'    
    OR items.copy_recipients LIKE '%danw%'   
    OR items.blind_copy_recipients LIKE '%danw%'  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Journal et audits de la messagerie de base de données](../../relational-databases/database-mail/database-mail-log-and-audits.md)  
  
  
