---
title: Stratégies Always On pour les problèmes opérationnels - Disponibilité Always On | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], troubleshooting
- Availability Groups [SQL Server], policies
ms.assetid: afa5289c-641a-4c03-8749-44862384ec5f
caps.latest.revision: 19
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 8fe881c82173877dba98e99c1af5b197ce5fba20
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34768985"
---
# <a name="always-on-policies-for-operational-issues---always-on-availability"></a>Stratégies Always On pour les problèmes opérationnels - Disponibilité Always On
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Le modèle d'intégrité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] évalue un ensemble de stratégies de gestion basées sur des stratégies prédéfinies. Vous pouvez les utiliser pour afficher l'intégrité d'un groupe de disponibilité AlwaysOn et de ses réplicas de disponibilité et bases de données dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
 **Dans cette rubrique :**  
  
-   [Termes et définitions](#TermsAndDefinitions)  
  
-   [Stratégies prédéfinies et problèmes rencontrés](#Always OnPBM)  
  
-   [Tableau de bord Always On](#Dashboard)  
  
-   [Extension du modèle d’intégrité Always On](#ExtendHealthModel)  
  
-   [Tâches associées](#RelatedTasks)  
  
-   [Contenu connexe](#RelatedContent)  
  
##  <a name="TermsAndDefinitions"></a> Termes et définitions  
 Stratégies prédéfinies Always On  
 Ensemble de stratégies intégrées qui permettent à un administrateur de base de données de vérifier qu’un groupe de disponibilité et ses réplicas de disponibilité et bases de données sont conformes aux états définis par les stratégies Always On.  
  
 [Groupes de disponibilité Always On](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
 Solution de haute disponibilité et de récupération d'urgence qui fournit une alternative au niveau de l'entreprise à la mise en miroir de bases de données.  
  
 Groupe de disponibilité  
 Conteneur d’un jeu discret de bases de données utilisateur, appelées *bases de données de disponibilité*, qui basculent ensemble.  
  
 réplica de disponibilité  
 Instanciation d'un groupe de disponibilité hébergé par une instance spécifique de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et qui conserve une copie locale de chaque base de données de disponibilité appartenant au groupe de disponibilité. Il existe deux types de réplicas de disponibilité : un seul *réplica principal* et un à quatre *réplicas secondaires*. Les instances de serveur qui hébergent les réplicas de disponibilité pour un groupe de disponibilité donné doivent résider sur différents nœuds d'un même cluster WSFC (Clustering de basculement Windows Server).  
  
 Base de données de disponibilité  
 Base de données qui appartient à un groupe de disponibilité. Pour chaque base de données de disponibilité, le groupe de disponibilité contient une seule copie en lecture-écriture ( *base de données primaire*) et une à quatre copies en lecture seule (*bases de données secondaires*).  
  
 Tableau de bord Always On  
 Tableau de bord [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] qui fournit d'un coup d'œil une vue de l'intégrité d'un groupe de disponibilité. Pour plus d’informations, consultez [Tableau de bord Always On](#Dashboard)plus loin dans cette rubrique.  
  
##  <a name="Always OnPBM"></a> Stratégies prédéfinies et problèmes rencontrés  
 Le tableau suivant récapitule les stratégies définies.  
  
|Nom de stratégie|Problème|Catégorie**\***|Facette|  
|-----------------|-----------|--------------------|-----------|  
|État du cluster WSFC|[WSFC cluster service is offline](../../../database-engine/availability-groups/windows/wsfc-cluster-service-is-offline.md).|Critique|Instance de SQL Server|  
|État en ligne du groupe de disponibilité|[Availability group is offline](../../../database-engine/availability-groups/windows/availability-group-is-offline.md).|Critique|Groupe de disponibilité|  
|Disponibilité du groupe de disponibilité pour le basculement automatique|[Availability group is not ready for automatic failover](../../../database-engine/availability-groups/windows/availability-group-is-not-ready-for-automatic-failover.md).|Critique|Groupe de disponibilité|  
|État de synchronisation des données des réplicas de disponibilité|[Some availability replicas are not synchronizing data](../../../database-engine/availability-groups/windows/some-availability-replicas-are-not-synchronizing-data.md).|Avertissement|Groupe de disponibilité|  
|État de synchronisation des données de réplicas synchrones|[Some synchronous replicas are not synchronized](../../../database-engine/availability-groups/windows/some-synchronous-replicas-are-not-synchronized.md).|Avertissement|Groupe de disponibilité|  
|État du rôle des réplicas de disponibilité|[Some availability replicas do not have a healthy role](../../../database-engine/availability-groups/windows/some-availability-replicas-do-not-have-a-healthy-role.md).|Avertissement|Groupe de disponibilité|  
|État de la connexion des réplicas de disponibilité|[Some availability replicas are disconnected](../../../database-engine/availability-groups/windows/some-availability-replicas-are-disconnected.md).|Avertissement|Groupe de disponibilité|  
|État du rôle du réplica de disponibilité|[Availability replica does not have a healthy role](../../../database-engine/availability-groups/windows/availability-replica-does-not-have-a-healthy-role.md).|Critique|réplica de disponibilité|  
|État de la connexion du réplica de disponibilité|[Availability replica is disconnected](../../../database-engine/availability-groups/windows/availability-replica-is-disconnected.md).|Critique|Réplica de disponibilité|  
|État de jointure du réplica de disponibilité|[Le réplica de disponibilité n’est pas joint](../../../database-engine/availability-groups/windows/availability-replica-is-not-joined.md).|Avertissement|Réplica de disponibilité|  
|État de synchronisation des données du réplica de disponibilité|[Data synchronization state of some availability database is not healthy](../../../database-engine/availability-groups/windows/data-synchronization-state-of-some-availability-database-is-not-healthy.md).|Avertissement|Réplica de disponibilité|  
|État de suspension de la base de données de disponibilité|[Availability database is suspended](../../../database-engine/availability-groups/windows/availability-database-is-suspended.md).|Avertissement|Base de données de disponibilité|  
|État de la jointure de la base de données de disponibilité|[Secondary database is not joined](../../../database-engine/availability-groups/windows/secondary-database-is-not-joined.md).|Avertissement|Base de données de disponibilité|  
|État de synchronisation des données de la base de données de disponibilité|[Data synchronization state of availability database is not healthy](../../../database-engine/availability-groups/windows/data-synchronization-state-of-availability-database-is-not-healthy.md).|Avertissement|Base de données de disponibilité|  
  
> [!IMPORTANT]  
>  **\*** Pour les stratégies Always On, les noms de catégorie sont utilisés comme identificateurs. Modifier le nom d’une catégorie Always On compromettrait sa fonctionnalité d’évaluation de l’intégrité. Par conséquent, ne modifiez pas les noms des catégories Always On.  
  
##  <a name="Dashboard"></a> Tableau de bord Always On  
 Le tableau de bord Always On offre un aperçu rapide de l’intégrité d’un groupe de disponibilité. Il inclut les fonctionnalités suivantes :  
  
-   Vous permet de visualiser facilement les détails d'un groupe de disponibilité donné, ses réplicas de disponibilité et de ses bases de données.  
  
-   Affiche des indications visuelles des états clés pour aider les administrateurs de bases de données à prendre rapidement des décisions opérationnelles.  
  
-   Fournit des points de lancement pour résoudre des scénarios.  
  
-   Pour un problème opérationnel donné, remplit la boîte de dialogue **Résultat d’évaluation de stratégie** avec les informations relatives aux violations spécifiques de la stratégie de contrôle d’intégrité Always On et avec des liens d’aide pour y remédier.  
  
-   Fournit une visionneuse d’événements d’intégrité étendus pour afficher les événements précédents liés à des problèmes spécifiques à Always On.  
  
-   Si le basculement sur le groupe de disponibilité est une solution possible à un problème, fournit un point de lancement pour les liens[Assistant Basculer le groupe de disponibilité](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md). Cet Assistant guide l'administrateur de base de données dans le processus manuel de basculement.  
  
##  <a name="ExtendHealthModel"></a> Extension du modèle d’intégrité Always On  
 L'extension du modèle d'intégrité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] consiste à créer vos propres stratégies définies par l'utilisateur et à les placer dans certaines catégories en fonction du type d'objet surveillé.  Après que vous avez modifié quelques paramètres, le tableau de bord Always On évalue automatiquement vos propres stratégies définies par l’utilisateur, ainsi que les stratégies prédéfinies Always On.  
  
 Une stratégie définie par l’utilisateur peut utiliser les facettes PBM disponibles, notamment celles utilisées par les stratégies prédéfinies Always On (consultez [Stratégies prédéfinies et problèmes rencontrés](#Always OnPBM), plus haut dans cette rubrique). La facette serveur fournit les propriétés suivantes pour l’analyse de l’intégrité de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] : (**IsHadrEnabled** et **HadrManagerStatus**). La facette serveur fournit également les propriétés des stratégies suivantes pour l’analyse de la configuration du cluster WSFC : **ClusterQuorumType**et **ClusterQuorumState**.  
  
 Pour plus d’informations, consultez l’article du blog de l’équipe de SQL Server Always On intitulé [The Always On Health Model Part 2 -- Extending the Health Model](https://blogs.msdn.microsoft.com/sqlalwayson/2012/02/13/the-alwayson-health-model-part-2-extending-the-health-model/) (Modèle d’intégrité Always On Partie 2 - Extension du modèle d’intégrité).  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Utiliser les stratégies Always On pour afficher l’intégrité d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)  
  
-   [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
-   [Récupération d’urgence WSFC par le quorum forcé &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)  
  
-   [Forcer un cluster WSFC à démarrer sans quorum](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
-   [Effectuer un basculement manuel forcé d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Résoudre une opération d’ajout de fichier ayant échoué &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="RelatedContent"></a> Contenu associé  
  
-   [The Always On Health Model Part 1 -- Health Model Architecture (Modèle d’intégrité Always On Partie 1 -- Architecture du modèle d’intégrité)](https://blogs.msdn.microsoft.com/sqlalwayson/2012/02/08/the-alwayson-health-model-part-1-health-model-architecture/)  
  
-   [The Always On Health Model Part 2 -- Extending the Health Model](https://blogs.msdn.microsoft.com/sqlalwayson/2012/02/13/the-alwayson-health-model-part-2-extending-the-health-model/)  
  
-   [Microsoft SQL Server Always On Solutions Guide for High Availability and Disaster Recovery (Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la récupération d’urgence)](http://go.microsoft.com/fwlink/?LinkId=227600)  
  
## <a name="see-also"></a> Voir aussi  
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Administration d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)   
 [Surveillance des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
  
