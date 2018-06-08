---
title: Utiliser les alertes pour les événements des agents de réplication | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- viewing alerts
- Queue Reader Agent, alerts
- alerts [SQL Server replication]
- predefined replication alerts [SQL Server replication]
- Log Reader Agent, alerts
- Distribution Agent, alerts
- Merge Agent, alerts
- agents [SQL Server replication], alerts
- displaying alerts
- Snapshot Agent, alerts
ms.assetid: 8c42e523-7020-471d-8977-a0bd044b9471
caps.latest.revision: 39
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e194a220520c65a637048a10b8f552338a39d05c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-alerts-for-replication-agent-events"></a>Utiliser les alertes pour les événements des agents de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] et l'Agent [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permettent de surveiller les événements, par exemple, les événements de l'Agent de réplication, à l'aide d'alertes. L'Agent[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] surveille, dans le journal des applications, des événements associés à des alertes. Si un tel événement se produit, l'Agent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] répond automatiquement, en exécutant une tâche que vous avez définie et/ou en envoyant un e-mail ou un message par radio-messagerie à un opérateur spécifié. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] inclut un ensemble d'alertes prédéfinies d'Agents de réplication que vous pouvez configurer pour exécuter une tâche et/ou avertir un opérateur. Pour plus d'informations sur la définition d'une tâche à exécuter, consultez la section « Automatisation d'une réponse à une alerte » de la présente rubrique.  
  
 Les alertes suivantes sont installées lorsqu'un ordinateur est configuré en tant que serveur de distribution :  
  
|ID du message|Alerte prédéfinie|Condition provoquant le déclenchement de l'alerte|Entrée d'informations supplémentaires dans msbd..sysreplicationalerts|  
|----------------|----------------------|-----------------------------------------|-----------------------------------------------------------------|  
|14150|**Réplication : succès de l'agent**|Arrêt normal de l'Agent.|Oui|  
|14151|**Réplication : échec de l'agent**|Arrêt de l'Agent en raison d'une erreur.|Oui|  
|14152|**Réplication : nouvelle tentative de l'agent**|L'agent s'arrête après l'échec du renouvellement d'une opération (l'Agent a rencontré une erreur de type serveur non disponible, interblocage, échec de la connexion, ou dépassement du délai d'attente).|Oui|  
|14157|**Réplication : suppression de l'abonnement expiré**|Suppression de l'abonnement expiré|non|  
|20572|**Réplication : abonnement réinitialisé après l'échec de validation**|Le travail de réponse « Réinitialiser les abonnements après échec de la validation des données » a réussi à réinitialiser un abonnement.|non|  
|20574|**Réplication : l'Abonné n'a pas réussi la validation des données**|L'Agent de distribution ou de fusion n'a pas réussi la validation des données.|Oui|  
|20575|**Réplication : l'Abonné a passé la validation des données**|L'Agent de distribution ou de fusion a réussi la validation des données.|Oui|  
|20578|**Réplication : arrêt personnalisé de l'Agent**|||  
|22815|**Alerte de détection de conflit d'égal à égal**|L'Agent de distribution a détecté un conflit lorsqu'il essaie d'appliquer une modification à un nœud d'égal à égal.|Oui|  
  
 En plus de ces alertes, le moniteur de réplication comprend un ensemble d'avertissements et d'alertes liées aux états et aux performances. Pour plus d’informations, voir [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md). Vous pouvez aussi définir des alertes pour d'autres événements de réplication à l'aide de l'infrastructure d'alertes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Créer un événement défini par l’utilisateur](http://msdn.microsoft.com/library/03d71a35-97fa-4bba-aa9a-23ac9c9cf879).  
  
 **Pour configurer des alertes de réplication prédéfinies**  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] : [Configurer des alertes de réplication prédéfinies &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/administration/configure-predefined-replication-alerts-sql-server-management-studio.md)  
  
## <a name="viewing-the-application-log-directly"></a>Affichage direct du journal des applications  
 Pour consulter le journal des applications Windows, utilisez l'Observateur d'événements [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows. Le journal des applications comporte les messages d'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ainsi que des messages se rapportant à toutes les activités de l'ordinateur. À la différence du journal des erreurs [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , chaque démarrage de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne crée pas un nouveau journal des applications (chaque session [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] écrit des nouveaux événements dans un journal des applications existant) ; en revanche, vous pouvez spécifier la durée de rétention des événements enregistrés. Lorsque vous affichez le journal des applications Windows, vous pouvez filtrer le journal en fonction d'événements spécifiques. Pour plus d'informations, consultez la documentation Windows.  
  
## <a name="automating-a-response-to-an-alert"></a>Automatisation d'une réponse à une alerte  
 La réplication fournit un travail de réponse aux abonnements dont la validation des données échoue ainsi qu'une infrastructure permettant de créer des réponses automatiques supplémentaires aux alertes. Le travail de réponse est intitulé **Réinitialiser les abonnements après échec de la validation des données** et stocké dans le dossier [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Travaux **de l'Agent** de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations sur l’activation de ce travail de réponse, consultez [Configurer des alertes de réplication prédéfinies &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/administration/configure-predefined-replication-alerts-sql-server-management-studio.md). Si certains articles d'une publication transactionnelle ne peuvent pas être validés, le travail de réponse ne réinitialise que ces articles. Si certains articles d'une publication de fusion ne peuvent pas être validés, le travail de réponse réinitialise tous les articles de la publication.  
  
### <a name="framework-for-automating-responses"></a>Infrastructure d'automatisation des réponses  
 Généralement, lorsqu'une alerte survient, la seule information dont vous disposez pour vous aider à comprendre la raison de l'alerte et l'action appropriée à entreprendre, se trouve dans le message d'alerte lui-même. L'analyse de ces informations peut être fastidieuse et sujette à erreurs. La réplication facilite l'automatisation des réponses en fournissant des informations supplémentaires sur l'alerte dans la table système **sysreplicationalerts** ; les données fournies sont déjà analysées dans une forme facilement utilisable pour les programmes personnalisés.  
  
 Si, par exemple, les données de la table **Sales.SalesOrderHeader** de l'Abonné A ne peuvent pas être validées, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peut déclencher le message 20574, qui vous avertit de l'échec. Le message que vous recevez peut se présenter comme suit : « L'Abonné 'A' avec un abonnement à l'article 'SalesOrderHeader' de la publication 'MyPublication' n'a pas réussi la validation de données ».  
  
 Si vous créez une réponse basée sur le message, vous devez extraire manuellement du message, le nom de l'Abonné, le nom de l'article, le nom de la publication et l'erreur. Cependant, puisque l'Agent de distribution et l'Agent de fusion écrivent ces mêmes informations dans la table **sysreplicationalerts** , ainsi que les détails comme le type d'Agent, l'heure de l'alerte, la base de données de publication, la base de données Abonné et le type de publication, le travail de réponse peut obtenir directement ces informations à partir de la table. Il n'est pas possible d'associer la ligne exacte correspondant à une instance précise de l'alerte, mais la table possède une colonne **status** qui peut être utilisée pour assurer le suivi des entrées. Les entrées de cette table sont conservées pendant la période de rétention de l'historique.  
  
 Par exemple, si vous voulez créer un travail dans [!INCLUDE[tsql](../../../includes/tsql-md.md)] , en réponse au message d'alerte 20574, vous pouvez utiliser la logique suivante :  
  
```  
declare @publisher sysname, @publisher_db sysname, @publication sysname, @publication_type int, @article sysname, @subscriber sysname, @subscriber_db sysname, @alert_id int  
declare hc cursor local for select publisher, publisher_db, publication, publication_type, article, subscriber,   
  subscriber_db, alert_id from   
  msdb..sysreplicationalerts where  
  alert_error_code = 20574 and status = 0  
  for read only  
open hc  
fetch hc into  @publisher, @publisher_db, @publication, @publication_type, @article, @subscriber, @subscriber_db, @alert_id  
while (@@fetch_status <> -1)  
begin  
/* Do custom work  */  
/* Update status to 1, which means the alert has been serviced. This prevents subsequent runs of this job from doing this again */  
update msdb..sysreplicationalerts set status = 1 where alert_id = @alert_id  
 fetch hc into  @publisher, @publisher_db, @publication, @publication_type, @article, @subscriber, @subscriber_db, @alert_id  
end  
close hc  
deallocate hc  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Administration de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-administration.md)   
 [Best Practices for Replication Administration](../../../relational-databases/replication/administration/best-practices-for-replication-administration.md)   
 [Surveillance &#40;réplication&#41;](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
