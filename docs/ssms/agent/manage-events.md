---
title: Gérer les événements | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- event forwarding servers [SQL Server]
- events [SQL Server], forwarding
- event-triggered jobs [SQL Server]
- forwarding events [SQL Server]
- alerts [SQL Server], forwarded events
- alerts [SQL Server], management servers
- SQL Server Agent alerts, management servers
ms.assetid: 8f4ee7f5-80df-49fd-b2b8-d020e04b6e1b
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a0262ff33df1f98283c7eb5ebdc63256c69f0f88
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-events"></a>Gérer les événements
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Vous pouvez transférer à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] tous les messages d'événements qui correspondent à un niveau de gravité d'erreur ou le dépassent. Cette fonction est qualifiée de *transfert d'événements*. Le serveur de transfert est un serveur dédié qui peut également être un serveur maître. Le transfert d'événements permet de centraliser la gestion des alertes pour un groupe de serveurs, réduisant ainsi la charge de travail sur les serveurs à utilisation intense.  
  
Lorsqu'un serveur reçoit des événements pour un groupe d'autres serveurs, le serveur qui reçoit les événements est qualifié de *serveur de gestion des alertes*. Dans un environnement multiserveur, il est recommandé de désigner le serveur maître comme serveur de gestion des alertes.  
  
## <a name="advantages-of-using-an-alerts-management-server"></a>Avantages de l'utilisation du serveur de gestion des alertes  
Les avantages liés à la désignation d'un serveur de gestion des alertes sont les suivants :  
  
-   **Centralisation**. Il est possible d'effectuer un contrôle centralisé et une vue consolidée des événements de plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] à partir d'un seul serveur.  
  
-   **Évolutivité**. De nombreux serveurs physiques peuvent être administrés comme un serveur logique unique. En fonction des besoins, vous pouvez ajouter et supprimer des serveurs dans ce groupe de serveurs physiques.  
  
-   **Efficacité**. Le temps de configuration est réduit car vous ne devez définir les alertes et les opérateurs qu'une seule fois.  
  
## <a name="disadvantages-of-using-an-alerts-management-server"></a>Inconvénients de l'utilisation du serveur de gestion des alertes  
Les inconvénients liés à la désignation d'un serveur de gestion des alertes sont les suivants :  
  
-   **Augmentation du trafic**. Le transfert d'événements à un serveur de gestion des alertes peut augmenter le trafic réseau. Cette augmentation peut être modérée en limitant le transfert d'événements aux événements qui se situent au-dessus d'un niveau de gravité désigné.  
  
-   **Point de défaillance unique**. Si le serveur de gestion des alertes se déconnecte, aucune alerte n'est émise pour aucun événement sur le groupe de serveurs géré.  
  
-   **Charge du serveur**. La gestion des alertes des événements transférés provoque une augmentation de la charge de traitement sur le serveur de gestion des alertes.  
  
## <a name="guidelines-for-using-an-alerts-management-server"></a>Directives concernant l'utilisation d'un serveur de gestion des alertes  
Lors de la configuration d'un serveur de gestion des alertes, procédez comme suit :  
  
-   Afin de recevoir les événements transférés, le serveur de gestion des alertes doit être une instance par défaut de SQL Server.  
  
-   Évitez d'exécuter des applications critiques ou qui nécessitent une utilisation intensive sur le serveur de gestion des alertes.  
  
-   Planifiez avec soin le trafic réseau impliqué dans la configuration de plusieurs serveurs pour partager le même serveur de gestion des alertes. En cas d'encombrement, réduisez le nombre de serveurs utilisant un serveur particulier de gestion des alertes.  
  
    Les serveurs inscrits dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] constituent la liste des serveurs disponibles devant être choisis par ce serveur comme serveur de transfert des alertes.  
  
-   Définissez sur l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] les alertes qui nécessitent une réponse serveur spécifique, au lieu de transférer les alertes aux serveurs de gestion des alertes.  
  
    Le serveur de gestion des alertes considère tous les serveurs qui lui transfèrent des événements comme un ensemble logique. Par exemple, un serveur de gestion des alertes répond de la même façon à un événement 605 émanant du serveur A ou du serveur B.  
  
-   Après avoir configuré votre système d'alerte, vérifiez régulièrement la présence d'événements de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] dans le journal des applications Microsoft Windows.  
  
    Les conditions d'échec rencontrées par le moteur d'alertes sont consignées dans le journal des applications Windows local avec le nom source « Agent SQL Server ». Par exemple, si l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ne peut pas envoyer par courrier électronique une notification telle qu'elle a été définie, un événement est consigné dans le journal des applications.  
  
Si une alerte définie localement est désactivée et si un événement qui aurait dû déclencher l'alerte se produit, l'événement est transféré au serveur de gestion des alertes (s'il remplit la condition de transfert d'alerte). Ce transfert permet la mise en fonction ou non de priorités locales (alertes définies localement qui sont également définies sur le serveur de gestion des alertes) sur le site local, selon les besoins de l'utilisateur. Vous pouvez également demander que les événements soient transférés, même s'ils sont gérés par des alertes locales.  
  
Les tâches suivantes sont courantes pour la gestion des événements dans un environnement multiserveur :  
  
**Pour désigner un serveur de gestion des alertes**  
  
-   [SQL Server Management Studio](../../ssms/agent/designate-an-events-forwarding-server-sql-server-management-studio.md)  
  
**Pour définir la réponse à une alerte**  
  
-   [SQL Server Management Studio](../../ssms/agent/define-the-response-to-an-alert-sql-server-management-studio.md)  
  
-   [Transact-SQL](http://msdn.microsoft.com/en-us/0525e0a2-ed0b-4e69-8a4c-a9e3e3622fbd)  
  
## <a name="running-event-triggered-jobs"></a>Exécution des travaux déclenchés par un événement  
Vous pouvez définir un travail à exécuter en réponse à une alerte. Par exemple, vous pouvez exécuter un travail qui corrige un problème détecté par l'alerte ou approfondit son diagnostic.  
  
> [!NOTE]  
> Un travail pouvant déclencher un événement, veillez à ne pas créer de boucle récursive de travail d'alerte.  
  
## <a name="see-also"></a> Voir aussi  
[sp_add_notification (Transact-SQL)](http://msdn.microsoft.com/en-us/44bee7d9-7517-4071-99be-8b36f979c7cc)  
  
