---
title: Abonnés de réplication et groupes de disponibilité Always On (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/08/2018
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- failover subscribers with AlwaysOn
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 0995f269-0580-43ed-b8bf-02b9ad2d7ee6
caps.latest.revision: 19
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 0ae868a9446b7cc140b6af61362358bb784488b9
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34769005"
---
# <a name="replication-subscribers-and-always-on-availability-groups-sql-server"></a>Abonnés de réplication et groupes de disponibilité Always On (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Quand un groupe de disponibilité Always On contenant une base de données est un abonné de réplication et bascule, l’abonnement de réplication peut échouer. Pour les abonnés transactionnels, l'agent de distribution continue la réplication automatiquement si l'abonnement utilise le nom de l'écouteur de groupe de disponibilité de l'abonné. Pour les abonnés de fusion, un administrateur de réplication doit reconfigurer l'abonné manuellement, en recréant l'abonnement.  
  
## <a name="what-is-supported"></a>Ce qui est pris en charge  
 La réplication de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge le basculement automatique du serveur de publication et le basculement automatique des abonnés. Le basculement d'un serveur de distribution dans une base de données de disponibilité n'est pas pris en charge. Les abonnés de fusion peuvent faire partie d’un groupe de disponibilité, toutefois des actions manuelles sont requises pour configurer le nouvel abonné après un basculement. Les groupes de disponibilité ne peuvent pas être associés à des scénarios Websync et ssNoVersion Compact.  
  
## <a name="how-to-create-transactional-subscription-in-an-always-on-environment"></a>Procédure de création d’un abonnement transactionnel dans un environnement Always On  
 Pour la réplication transactionnelle, utilisez la procédure suivante pour configurer et basculer un groupe de disponibilité d'abonné :  
  
1.  Avant de créer l’abonnement, ajoutez la base de données de l’abonné au groupe de disponibilité Always On approprié.  
  
2.  Ajoutez l'écouteur de groupe de disponibilité de l'abonné en tant que serveur lié à tous les nœuds du groupe de disponibilité. Cette étape garantit que tous les partenaires de basculement potentiels ont connaissance de l'écouteur et peuvent s'y connecter.  
  
3.  À l'aide du script de la section **Création d'un abonnement de réplication transactionnelle par émission de données** ci-dessous, créez l'abonnement avec le nom de l'écouteur du groupe de disponibilité de l'abonné. Après un basculement, le nom de l'écouteur demeure valide, alors que le nom du serveur réel de l'abonné dépend du nœud réel qui est devenu le nouveau nœud principal.  
  
    > [!NOTE]  
    >  L'abonnement doit être créé à l'aide d'un script [!INCLUDE[tsql](../../../includes/tsql-md.md)] ; il ne peut pas être créé à l'aide de [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
4.  Pour la création d'un abonnement par extraction de données :  
  
    1.  Dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], sur le nœud principal de l'abonné, ouvrez l'arborescence de l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    2.  Identifiez le travail de l' **Agent de distribution par extraction de données** et modifiez le travail.  
  
    3.  À l'étape de travail **Exécution de l'Agent** , vérifiez les paramètres `-Publisher` et `-Distributor` . Assurez-vous que ces paramètres contiennent les noms corrects du serveur direct et de l'instance du serveur de publication et du serveur de distribution.  
  
    4.  Remplacez le paramètre `-Subscriber` par le nom de l'écouteur de groupe de disponibilité de l'abonné.  
  
 Si vous créez votre abonnement en suivant ces étapes, vous n'aurez aucune action à accomplir après un basculement.  
  
## <a name="creating-a-transactional-replication-push-subscription"></a>Création d'un abonnement de réplication transactionnelle par émission de données  
  
```  
-- commands to execute at the publisher, in the publisher database:  
use [<publisher database name>]  
EXEC sp_addsubscription @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @destination_db = N'<subscriber database name>',   
       @subscription_type = N'Push',   
       @sync_type = N'automatic', @article = N'all', @update_mode = N'read only', @subscriber_type = 0;  
GO  
  
EXEC sp_addpushsubscription_agent @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @subscriber_db = N'<subscriber database name>',   
       @job_login = null, @job_password = null, @subscriber_security_mode = 1;  
GO  
```  
  
## <a name="to-resume-the-merge-agents-after-the-availability-group-of-the-subscriber-fails-over"></a>Pour récupérer les agents de fusion après le basculement du groupe de disponibilité de l'abonné  
 Pour une réplication de fusion, un administrateur de réplication doit reconfigurer l'abonné manuellement en procédant comme suit :  
  
1.  Exécutez **sp_subscription_cleanup** pour supprimer l’ancien abonnement pour l’abonné. Effectuez cette action sur le nouveau réplica principal (qui était auparavant le réplica secondaire).  
  
2.  Recréez l'abonnement en en créant un nouveau, en commençant par un nouvel instantané.  
  
> [!NOTE]  
>  Le processus actuel n’est pas pratique pour les abonnés de réplication de fusion ; toutefois, le scénario principal de la réplication de fusion implique des utilisateurs déconnectés (bureaux, ordinateurs portables, appareils combinés téléphoniques) qui n’utilisent pas les groupes de disponibilité Always On sur l’abonné.  
  
  
