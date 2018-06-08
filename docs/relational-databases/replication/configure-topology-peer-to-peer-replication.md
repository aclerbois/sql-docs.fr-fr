---
title: Configurer la topologie (réplication d’égal à égal) | Microsoft Docs
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
f1_keywords:
- sql13.rep.p2pwizard.peers.f1
ms.assetid: 5377c59f-2e25-4852-a306-c87ae3dca9fd
caps.latest.revision: 29
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 2c534b0114c3e54d9a3ecd17bdde03d779c7edc5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-topology-peer-to-peer-replication"></a>Configurer la topologie (réplication d'égal à égal)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez la page **Configurer la topologie** pour effectuer des tâches de configuration communes, telles que l'ajout de nouveaux nœuds, la suppression de nœuds et l'ajout de nouvelles connexions entre des nœuds existants. Le nœud que vous avez sélectionné dans la page **Publication** de cet Assistant s'affiche dans l'aire de conception. Pour spécifier des options de configuration, cliquez avec le bouton droit sur un nœud, une connexion ou l'aire de conception.  
  
> [!NOTE]  
>  L'Assistant Configurer la topologie d'égal à égal demande des informations de topologie lorsque l'Assistant est fermé. Si l'Assistant est fermé et ouvert de nouveau avant que tous les nœuds répondent à la demande d'informations, l'Assistant peut afficher un réseau partiel.  
  
## <a name="options"></a>Options  
 La page **Configurer la topologie** contient des éléments d'interface et des options qui sont disponibles lorsque vous cliquez avec le bouton droit sur un élément. Le tableau suivant décrit chaque élément d'interface.  
  
|Élément d'interface|Description|  
|-----------------------|-----------------|  
|Aire de conception|Affiche d'autres éléments d'interface. Pour ajouter des éléments, cliquez avec le bouton droit sur l'aire de conception.|  
|![Le premier nœud dans une topologie](../../relational-databases/replication/media/p2pwizard-firstnode.gif "Le premier nœud dans une topologie")|Nœud d'origine dans la topologie. Les nouveaux nœuds sont initialisés grâce à une copie de la base de données de publication du nœud d'origine.|  
|![Un nœud pour lequel nous avons des informations complètes](../../relational-databases/replication/media/p2pwizard-complete.gif "Un nœud pour lequel nous avons des informations complètes")|Nœud exécutant une instance de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou d'une version ultérieure, sur lequel la réplication possède des informations complètes. Pour spécifier des options de configuration, cliquez avec le bouton droit sur le nœud.|  
|![Un nœud pour lequel nous avons des informations incomplètes](../../relational-databases/replication/media/p2pwizard-incomplete.gif "Un nœud pour lequel nous avons des informations incomplètes")|Nœud sur lequel la réplication possède des informations incomplètes. Pour spécifier des options de configuration, cliquez avec le bouton droit sur le nœud.<br /><br /> La réplication possède des informations incomplètes pour l'une des raisons suivantes :<br /><br /> -Le nœud exécute une instance de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]qui ne stocke pas toutes les métadonnées requises par l’assistant.<br /><br /> -Le nœud exécute une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais la réplication ne peut pas récupérer les informations d’abonnement du nœud. Pour résoudre ce problème :<br /><br /> Assurez-vous que la base de données au niveau du nœud est en ligne et que vous pouvez vous connecter à celle-ci en utilisant les mêmes informations d'identification que les Agents de distribution qui se connectent au nœud.<br /><br /> Assurez-vous que l'Agent de lecture du journal et les Agents de distribution qui se connectent au nœud sont en cours d'exécution.<br /><br /> Assurez-vous que le délai d'actualisation est suffisant pour collecter toutes les informations sur la topologie. Pour définir le délai, cliquez avec le bouton droit sur l'aire de conception, puis cliquez sur **Définir le délai d'actualisation**.|  
|Ligne grise avec des flèches|Connexion entre deux nœuds. Pour ajouter une connexion, cliquez avec le bouton droit sur l'un des nœuds que vous souhaitez connecter. Pour supprimer une connexion, cliquez avec le bouton droit sur la connexion.<br /><br /> Si la ligne a une flèche unique, la réplication possède des informations incomplètes sur l'un des nœuds.|  
  
### <a name="options-for-the-design-surface"></a>Options relatives à l'aire de conception  
 **Redessiner le graphique**  
 Cette option vous permet de redessiner les objets dans l'aire de conception sans actualiser la topologie. Le fait de redessiner peut fournir une meilleure vue de la topologie.  
  
 **Actualiser la topologie**  
 Cette option vous permet d'interroger chaque nœud dans la topologie et d'afficher des informations mises à jour sur chaque nœud. Si vous avez plusieurs nœuds, ce processus peut prendre plusieurs minutes.  
  
 Si l'Assistant demande des informations de topologie et que vous fermez puis rouvrez l'Assistant avant que tous les nœuds répondent à la demande, cette page peut ne pas afficher tous les nœuds dans la topologie.  
  
 **Ajouter un nouveau nœud homologue**  
 Cette option vous permet d'ajouter une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à la topologie d'égal à égal. L'ajout d'une instance en tant que nœud crée une publication au niveau de cette instance une fois l'Assistant terminé. Après avoir ajouté le nœud, cliquez dessus avec le bouton droit pour ajouter une connexion entre le nouveau nœud et un nœud existant.  
  
 Pour participer à une topologie d'égal à égal, l'instance doit remplir les conditions suivantes :  
  
-   elle doit déjà être configurée en tant que serveur de distribution ou être associée à un serveur de distribution distant ;  
  
-   elle doit contenir une copie de la base de données impliquée dans la réplication (cette copie est généralement une sauvegarde restaurée de la base de données de publication d'origine).  
  
 **Sélectionner les nœuds à afficher**  
 Cette option vous permet de sélectionner les nœuds à afficher dans l'aire de conception. Cette option est utile si la topologie possède un grand nombre de nœuds. N'oubliez pas que vous pouvez ajouter des connexions uniquement entre des nœuds qui sont visibles dans l'aire de conception.  
  
 **Définir le délai d'actualisation**  
 Cette option vous permet de spécifier la durée d'exécution du processus d'actualisation avant qu'il arrive à expiration.  
  
### <a name="options-for-each-node"></a>Options pour chaque nœud  
 **Ajouter une nouvelle connexion homologue**  
 Cette option vous permet d'ajouter une connexion entre deux nœuds. Par exemple, si vous ajoutez une connexion entre un nœud A et un nœud B, la réplication ajoute deux abonnements : le premier autorise le nœud A à recevoir des modifications de la publication au niveau du nœud B, et le second autorise le nœud B à recevoir des modifications de la publication au niveau du nœud A.  
  
 **Supprimer le nœud homologue**  
 Cette option vous permet de supprimer un nœud de la topologie. Par exemple, si vous supprimez le nœud C, la publication au niveau de ce nœud est supprimée. Les abonnements entre le nœud A et le nœud C sont aussi supprimés, de même qu'entre le nœud B et le nœud C. La base de données au niveau du nœud C n'est pas supprimée, et la publication et la distribution ne sont pas désactivées.  
  
> [!NOTE]  
>  Lorsque vous configurez la réplication d'égal à égal, vous spécifiez un ID pour chaque nœud. Cet ID, qui doit être unique sur tous les nœuds de la topologie, est stocké dans la colonne originator_id de la table système [MSpeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md). Si un nœud est supprimé de la topologie, l'ID est toujours conservé dans la table d'historique. L'ID est conservé pour empêcher de faux conflits d'avoir lieu si des modifications du nœud supprimé sont encore répliquées à travers la topologie. Si vous souhaitez réutiliser l'ID pour un nouveau nœud, vous devez tout d'abord supprimer manuellement l'ID de la table MSpeer_originatorid_history à tous les nœuds. Avant de supprimer un ID pour un nœud, exécutez [sp_requestpeerresponse](../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md) pour vérifier que toutes les modifications apportées à partir de ce nœud ont été répliquées.  
  
 **Se connecter à TOUS les nœuds affichés**  
 Cette option vous permet d'jouter des connexions entre le nœud sélectionné et tous les autres nœuds. Par exemple, si vous avez sélectionné cette option pour le nœud C dans une topologie à trois nœuds, la réplication ajoute quatre abonnements : deux qui autorisent les nœuds A et B à recevoir des modifications de la publication au niveau du nœud C, et deux qui autorisent le nœud C à recevoir des modifications des publications au niveau des nœuds A et B.  
  
 **Sélectionner les nœuds à afficher**  
 Cette option vous permet de sélectionner les nœuds à afficher dans l'aire de conception. Cette option est utile si la topologie possède un grand nombre de nœuds. N'oubliez pas que vous pouvez ajouter des connexions uniquement entre des nœuds qui sont visibles dans l'aire de conception.  
  
### <a name="options-for-the-connection-arrows"></a>Options pour les flèches de connexion  
 **Supprimer la connexion homologue**  
 Cette option vous permet de supprimer une connexion entre deux nœuds. Par exemple, si vous supprimez une connexion entre un nœud A et un nœud B, la réplication supprime deux abonnements : l'un qui autorise le nœud A à recevoir des modifications de la publication au niveau du nœud B, et l'autre qui autorise le nœud B à recevoir des modifications de la publication au niveau du nœud A.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [Administrer une topologie d’égal à égal &#40;programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/administration/administer-a-peer-to-peer-topology-replication-transact-sql-programming.md)   
 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)  
  
  
