---
title: SQL Server Service Broker | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.SSBMSGTYPEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBCONTRACTPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBQUEUEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBREMSVCBINDPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBROUTEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBPRIORITYPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBSERVICEPROPERTIES.GENERAL.F1
helpviewer_keywords:
- Broker See Service Broker
- SQL Server Service Broker
- Service Broker
ms.assetid: 8b8b3b57-fd46-44de-9a4e-e3a8e3999c1e
caps.latest.revision: 22
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 84b41966c15fab6e89d5a307666d4005a1f7400b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-service-broker"></a>SQL Server Service Broker
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSB](../../includes/sssb-md.md)] fournit la prise en charge native des applications de messagerie et de mise en file d’attente dans le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Cette opération permet aux développeurs de créer plus facilement des applications perfectionnées qui utilisent des composants de [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour la communication entre des bases de données disparates. Les développeurs peuvent utiliser [!INCLUDE[ssSB](../../includes/sssb-md.md)] pour créer facilement des applications fiables et distribuées.  
  
 Les développeurs d'applications qui utilisent [!INCLUDE[ssSB](../../includes/sssb-md.md)] peuvent distribuer les charges de données sur plusieurs bases de données sans développer des mécanismes de messagerie et de communication complexes. Il est ainsi possible de réduire le travail de développement et de test puisque [!INCLUDE[ssSB](../../includes/sssb-md.md)] gère les chemins de communication dans le contexte d'une conversation. Les performances sont aussi meilleures. Par exemple, les bases de données frontales prenant en charge les sites Web peuvent enregistrer des informations et mettre des tâches intensives en file d'attente dans des bases de données principales. [!INCLUDE[ssSB](../../includes/sssb-md.md)] garantit que toutes les tâches sont gérées dans le contexte des transactions afin d’assurer une fiabilité et une cohérence techniques.  

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]
  
## <a name="where-is-the-documentation-for-service-broker"></a>Emplacement de la documentation de Service Broker  
 La documentation de référence pour [!INCLUDE[ssSB](../../includes/sssb-md.md)] est incluse dans la documentation de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . Cette documentation de référence comprend les sections suivantes :  
  
-   [Instructions DDL &#40;Data Definition Language &#41; &#40;Transact-SQL&#41;](~/mdx/mdx-data-definition-statements-mdx.md) pour les instructions CREATE, ALTER et DROP  
  
-   [Instructions de Service Broker](../../t-sql/statements/service-broker-statements.md)  
  
-   [Affichages catalogue relatifs à Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/service-broker-catalog-views-transact-sql.md)  
  
-   [Vues de gestion dynamique liées à Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
-   [Utilitaire ssbdiagnose &#40;Service Broker&#41;](../../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)  
  
 Consultez la [documentation précédemment publiée](http://go.microsoft.com/fwlink/?LinkId=231312) pour les concepts [!INCLUDE[ssSB](../../includes/sssb-md.md)] et pour les tâches de gestion et de développement. Cette documentation n'est pas reproduite dans la documentation de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] en raison de quelques modifications apportées dans [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="whats-new-in-service-broker"></a>Nouveautés dans Service Broker  
 Aucune modification importante n'a été introduite dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  Les modifications suivantes ont été introduites dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
### <a name="messages-can-be-sent-to-multiple-target-services-multicast"></a>Les messages peuvent être envoyés à des services cibles (multidiffusion).  
 La syntaxe de l’instruction [SEND &#40;Transact-SQL&#41;](../../t-sql/statements/send-transact-sql.md) a été étendue pour permettre la multidiffusion au moyen de la prise en charge de plusieurs descripteurs de conversation.  
  
### <a name="queues-expose-the-message-enqueued-time"></a>Les files d'attente exposent le temps d'empilement des messages.  
 Les files d’attente ont une nouvelle colonne, **message_enqueue_time**, qui indique depuis combien de temps un message est dans la file d’attente.  
  
### <a name="poison-message-handling-can-be-disabled"></a>La gestion des messages incohérents peut être désactivée  
 Les instructions [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md) et [ALTER QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-queue-transact-sql.md) ont désormais la possibilité d’activer ou désactiver la gestion des messages incohérents en ajoutant la clause `POISON_MESSAGE_HANDLING (STATUS = ON | OFF)`. L’affichage catalogue **sys.service_queues** comprend maintenant la colonne **is_poison_message_handling_enabled** pour indiquer si le message incohérent est activé ou désactivé.  
  
### <a name="always-on-support-in-service-broker"></a>Prise en charge d’Always On dans Service Broker  
 Pour plus d’informations, consultez [Service Broker avec les groupes de disponibilité Always On (SQL Server)](../../database-engine/availability-groups/windows/service-broker-with-always-on-availability-groups-sql-server.md).  
  
  

