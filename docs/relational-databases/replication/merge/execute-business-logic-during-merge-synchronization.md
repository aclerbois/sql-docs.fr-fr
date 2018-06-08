---
title: Exécuter la logique métier pendant la synchronisation de fusion | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
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
- custom error resolution [SQL Server replication]
- custom change handling [SQL Server replication]
- errors [SQL Server replication], business logic handlers
- merge replication business logic handlers [SQL Server replication]
- conflict resolution [SQL Server replication], merge replication
- business logic handlers [SQL Server replication]
ms.assetid: 9d4da2ef-c17f-4a31-a1f6-5c3b7ca85f71
caps.latest.revision: 31
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 1cacaad9280c33cb331157386db2f7f919d5bb8c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="execute-business-logic-during-merge-synchronization"></a>Exécuter la logique métier lors de la synchronisation de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'infrastructure de gestion de logique métier permet d'écrire un assembly de code managé qui est appelé pendant le processus de synchronisation de fusion. L'assembly comprend une logique métier qui peut répondre à un certain nombre de conditions au cours de la synchronisation : les modifications de données, les conflits et les erreurs. L'infrastructure de gestionnaires de logique métier propose un modèle de programmation simple et les données fournies par le processus de fusion à votre assembly se présentent sous la forme d'un dataset ADO.NET, de sorte que vous pouvez tirer parti de la connaissance d'ADO.NET au lieu d'apprendre une interface propriétaire. Pour plus d'informations sur la programmation de gestionnaires de logique métier, consultez :  
  
-   Référence de l'interface de programmation d'applications (API) : <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>  
  
-   Instructions sur l’implémentation d’un gestionnaire de logique métier : [Implémenter un gestionnaire de logique métier pour un article de fusion](../../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)  
  
## <a name="uses-for-business-logic-handlers"></a>Utilisations des gestionnaires de logique métier  
 Le processus de synchronisation de fusion peut appeler les gestionnaires de logique métier pour effectuer les opérations suivantes :  
  
-   Gestion personnalisée des modifications  
  
-   Résolution personnalisée des conflits  
  
-   Résolution personnalisée des erreurs  
  
> [!NOTE]  
>  Le gestionnaire de logique métier que vous spécifiez est exécuté pour chaque ligne faisant l'objet d'une synchronisation. Une logique complexe et des appels à d'autres applications ou services réseau peuvent avoir une incidence sur les performances.  
  
### <a name="custom-change-handling"></a>Gestion personnalisée des modifications  
 Le gestionnaire de logique métier peut être appelé au cours du traitement de modifications de données non conflictuelles et peut effectuer l'une des trois actions suivantes :  
  
-   Refuser les données  
  
     Cette option est utile dans le cas d'applications qui ne veulent pas propager des modifications vers un Abonné spécifique ou à partir de celui-ci. Ainsi, un administrateur peut éliminer les insertions qui n'appartiennent pas à la partition de l'Abonné ou éventuellement rejeter des suppressions effectuées au niveau d'un Abonné. Une application qui refuse une commande entrée sur l'Abonné pour indisponibilité de stock en est un autre exemple.  
  
-   Accepter les données  
  
     Cette option est utile pour les applications dans lesquelles il est nécessaire de contrôler les modifications effectuées sur le serveur de publication ou sur l'Abonné avant d'autoriser leur propagation. Par exemple, une application de niveau intermédiaire peut examiner les nouvelles commandes transmises par la force de vente et les intégrer à un processus de flux de travail d'approvisionnement au niveau intermédiaire.  
  
-   Appliquer des données personnalisées  
  
     Cette option est utile pour les applications qui doivent substituer des opérations ou des valeurs de données spécifiques. Par exemple, une application peut transformer une suppression de ligne en mise à jour spéciale qui affecte la valeur « supprimé » à une colonne **status** de la ligne puis effectue un suivi du client à l'origine de la suppression. Cela peut s'avérer utile pour l'audit ou le flux de travail.  
  
### <a name="custom-conflict-resolution"></a>Résolution personnalisée des conflits  
 La réplication de fusion offre des fonctionnalités de résolution ou de détection de conflits. Vous pouvez accepter une stratégie de résolution par défaut ou opter pour une résolution personnalisée des conflits. Pour plus d'informations, voir [Advanced Merge Replication Conflict Detection and Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md). Le gestionnaire de logique métier peut être appelé au cours du traitement de modifications de données conflictuelles et effectuer l'une des deux actions suivantes :  
  
-   Accepter la résolution par défaut  
  
     Cette option est utile pour les applications qui doivent examiner le conflit, effectuer des opérations supplémentaires et éventuellement enregistrer un message de conflit personnalisé dans un journal.  
  
-   Appliquer une résolution de conflits personnalisée  
  
     Cette option est utile pour les applications qui ont besoin de sélectionner des valeurs de données spécifiques à leur logique métier et fournir le processus de synchronisation avec ce dataset personnalisé. Ainsi, une application peut fournir une nouvelle version de la ligne qui prime en cas de conflit en combinant des valeurs des datasets du serveur de publication et de l'Abonné.  
  
### <a name="custom-error-resolution"></a>Résolution personnalisée des erreurs  
 La logique métier peut être appelée au cours de la propagation des modifications qui génèrent des erreurs. La logique peut effectuer l'une des deux actions suivantes :  
  
-   Accepter la résolution d'erreur par défaut  
  
     Cette option est utile pour les applications qui doivent examiner l'erreur, effectuer des opérations supplémentaires et éventuellement enregistrer un message d'erreur personnalisé dans un journal.  
  
-   Accepter la résolution d'erreur personnalisée  
  
     Cette option est utile pour les applications qui ont besoin de sélectionner des valeurs de données spécifiques à leur logique métier et fournir le processus de synchronisation avec ce dataset personnalisé. Si, par exemple, le processus de réplication rencontre une violation de clé dupliquée, le gestionnaire de logique métier peut fournir une nouvelle version de la modification de données dans laquelle le conflit de clé est éliminé. Les modifications apportées au serveur de publication et à l'Abonné peuvent alors être conservées dans la base de données et le processus de réplication n'a plus à compenser l'échec d'insertion par une suppression.  
  
## <a name="deployment-scenarios-for-business-logic-handlers"></a>Scénarios de déploiement des gestionnaires de logique métier  
 Les gestionnaires de logique métier peuvent être déployés sur les serveurs suivants :  
  
-   Le serveur de distribution. Utilisez un abonnement envoyé afin que la logique métier soit exécutée sur le serveur de distribution.  
  
-   Abonné. Utilisez un abonnement extrait afin que la logique métier soit exécutée sur l'Abonné.  
  
-   Serveur IIS (Internet Information Services) en cas d'utilisation de la synchronisation Web. Utilisez un abonnement extrait synchronisé avec la synchronisation Web et le gestionnaire de logique métier s'exécutera sur le serveur IIS.  
  
## <a name="see-also"></a> Voir aussi  
 [Réplication de fusion](../../../relational-databases/replication/merge/merge-replication.md)   
 [Subscribe to Publications](../../../relational-databases/replication/subscribe-to-publications.md)   
 [Synchroniser les données](../../../relational-databases/replication/synchronize-data.md)   
 [Web Synchronization for Merge Replication](../../../relational-databases/replication/web-synchronization-for-merge-replication.md)  
  
  
