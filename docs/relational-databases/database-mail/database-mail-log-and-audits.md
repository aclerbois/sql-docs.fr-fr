---
title: Journal et audits de la messagerie de base de données | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
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
- auditing [SQL Server]
- Database Mail [SQL Server], auditing
- logs [Database Mail]
- audits [SQL Server], Database Mail
- Database Mail [SQL Server], logging
ms.assetid: 846589ee-5fe5-4ab3-b335-0c253e569f99
caps.latest.revision: 30
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: d711750d21bef54ace3d99979aaffc3a6930bfbe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="database-mail-log-and-audits"></a>Journal et audits de la messagerie de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La fonctionnalité de journalisation de la messagerie de base de données offre un moyen d'isoler et de résoudre les problèmes. La messagerie de base de données stocke les informations du journal dans la base de données **msdb** . Les informations sur le contenu des messages électroniques de la messagerie de base de données, l'état des messages électroniques et tous les messages reçus (notamment les erreurs) sont enregistrées par la messagerie de base de données et peuvent être utilisées à des fins de dépannage et d'audit.  
  
## <a name="database-mail-logs"></a>Journaux de la messagerie de base de données  
 Les tables de la base de données **msdb** journalisent les informations en provenance du [Programme externe de la messagerie de base de données](../../relational-databases/database-mail/database-mail-external-program.md). Dans [Vues de la messagerie de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/database-mail-views-transact-sql.md), des tables sont exposées à des fins de dépannage. Des erreurs apparaissent dans la vue [sysmail_event_log &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md) si Service Broker ne peut pas activer le programme externe, si celui-ci rencontre des erreurs réseau ou si le serveur SMTP (Simple Mail Transport Protocol) refuse un message électronique. Si le programme externe ne peut pas se connecter aux tables **msdb**, il consigne les erreurs dans le journal des événements des applications Windows.  
  
 Les tables internes de la base de données **msdb** contiennent les messages électroniques et les pièces jointes envoyés depuis la messagerie de base de données, ainsi que l’état actuel de chaque message. La messagerie de base de données met ces tables à jour chaque fois qu'un message est traité.  
  
## <a name="database-mail-auditing-tasks"></a>Tâches d'audit de la messagerie de base de données  
  
|||  
|-|-|  
|**Vérification et gestion des journaux de la messagerie de base de données**|**Lien vers la rubrique**|  
|Vérifier l'état de remise d'un message individuel|[Vérifier l'état des messages électroniques envoyés avec la messagerie de base de données](../../relational-databases/database-mail/check-the-status-of-e-mail-messages-sent-with-database-mail.md)|  
|Nettoyer les messages, les pièces jointes et les entrées de journal de la messagerie de base de données|[sysmail_delete_mailitems_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-delete-mailitems-sp-transact-sql.md)<br /><br /> [sysmail_delete_log_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-delete-log-sp-transact-sql.md)|  
|Archiver les messages et les journaux de la messagerie de base de données|[Créer un travail d'Agent SQL Server pour archiver les messages et les journaux d'événements de la messagerie de base de données](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
