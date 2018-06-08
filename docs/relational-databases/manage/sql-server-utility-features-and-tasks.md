---
title: Fonctionnalités et tâches de l’utilitaire SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: maintenance-plans
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server utility [SQL Server]
- utility control point
- Utility growth rate
- Multiserver management
- UCP
- Multi-server management [SQL Server]
ms.assetid: 6e6cbd25-6b1c-4e21-9ade-4584e243fd8f
caps.latest.revision: 10
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 64b7b244132621ea88696ccbc795f465b8bcf56f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-utility-features-and-tasks"></a>Fonctionnalités et tâches de l'utilitaire SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ont de plus en plus besoin de gérer leur environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans son ensemble, ce à quoi répond cette version via le concept d'application et de gestion multiserveur de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="benefits-of-the-sql-server-utility"></a>Avantages de l'utilitaire SQL Server  
 L'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] modélise les entités d'une organisation associées à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]dans une vue unifiée. L'Explorateur d'utilitaire et les points de vue de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) fournissent aux administrateurs une vue holistique de l'intégrité des ressources [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] via une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sert de point de contrôle de l'utilitaire (UCP). La combinaison du résumé et des données détaillées présentée dans l'UCP, à la fois pour les stratégies de sous-exploitation et de surexploitation et pour divers paramètres clés, permet d'identifier facilement les possibilités de consolidation de ressources et la surexploitation des ressources. Les stratégies de contrôle d'intégrité sont configurables et peuvent être ajustées pour modifier des seuils d'exploitation des ressources supérieurs ou inférieurs. Vous pouvez modifier des stratégies de surveillance globales ou configurer des stratégies de surveillance individuelles pour chaque entité gérée dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
##  <a name="typical_scenarios"></a> Mise en route avec l'utilitaire SQL Server  
 Le scénario d'utilisateur classique commence par la création d'un point de contrôle d'utilitaire (UCP) qui établit le point de raisonnement central de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'UCP fournit une vue consolidée de l'intégrité des ressources recueillie à partir des instances managées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Après avoir créé l'UCP, vous inscrivez des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin qu'elles puissent être gérées par l'UCP.  
  
 Chaque instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et d'application de la couche Données gérée par l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être surveillée en fonction des définitions de stratégie globales ou des définitions de stratégie individuelles.  
  
## <a name="related-tasks"></a>Related Tasks  
 Utilisez les rubriques ci-dessous pour démarrer avec l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|||  
|-|-|  
|**Description**|**Rubrique**|  
|Décrit des éléments à prendre en considération lors de la configuration d'un serveur pour exécuter des jeux d'éléments de collecte d'utilitaire et de non-utilitaire sur la même instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|[Considérations sur l'exécution de jeux d'éléments de collecte de l'utilitaire et non-utilitaire sur la même instance de SQL Server](../../relational-databases/manage/run-utility-and-non-utility-collection-sets-on-same-sql-instance.md)|  
|Décrit comme créer un point de contrôle d'utilitaire SQL Server.|[Créer un point de contrôle de l’utilitaire SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/create-a-sql-server-utility-control-point-sql-server-utility.md)|  
|Décrit comment se connecter à un Utilitaire SQL Server.|[Se connecter à un utilitaire SQL Server](../../relational-databases/manage/connect-to-a-sql-server-utility.md)|  
|Décrit comment inscrire une instance de SQL Server auprès d'un point de contrôle d'utilitaire.|[Inscrire une instance de SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/enroll-an-instance-of-sql-server-sql-server-utility.md)|  
|Décrit comment utiliser l'Explorateur d'utilitaire pour gérer l'Utilitaire SQL Server.|[Utiliser l'Explorateur de l'utilitaire pour gérer l'Utilitaire SQL Server](../../relational-databases/manage/use-utility-explorer-to-manage-the-sql-server-utility.md)|  
|Décrit comment surveiller les instances de SQL Server dans l'Utilitaire SQL Server.|[Surveiller des instances de SQL Server dans l'utilitaire SQL Server](../../relational-databases/manage/monitor-instances-of-sql-server-in-the-sql-server-utility.md)|  
|Décrit comment afficher les résultats d'une stratégie de contrôle d'intégrité des ressources.|[Consulter les résultats d’une stratégie de contrôle d’intégrité des ressources &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/view-resource-health-policy-results-sql-server-utility.md)|  
|Décrit comment modifier une définition de stratégie de contrôle d'intégrité des ressources.|[Modifier une définition de la stratégie de contrôle d’intégrité des ressources &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/modify-a-resource-health-policy-definition-sql-server-utility.md)|  
|Décrit comment configurer votre entrepôt de données UCP.|[Configurer votre entrepôt de données de point de contrôle de l’utilitaire &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/configure-your-utility-control-point-data-warehouse-sql-server-utility.md)|  
|Décrit comment configurer les stratégies de contrôle d'intégrité de l'utilitaire.|[Configurer des stratégies de contrôle d’intégrité &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/configure-health-policies-sql-server-utility.md)|  
|Décrit comment ajuster l'atténuation dans les stratégies d'utilisation du processeur.|[Réduire le bruit dans les stratégies d’utilisation du processeur &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/reduce-noise-in-cpu-utilization-policies-sql-server-utility.md)|  
|Décrit comment supprimer une instance de SQL Server d'un UCP.|[Supprimer une instance de SQL Server de l'utilitaire SQL Server](../../relational-databases/manage/remove-an-instance-of-sql-server-from-the-sql-server-utility.md)|  
|Décrit comment modifier le compte proxy pour le collecteur de données de l'utilitaire sur une instance gérée de SQL Server.|[Modifier le compte proxy pour le jeu d’éléments de collecte de l’utilitaire sur une instance gérée de SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/change-proxy-account-for-utility-collection-on-managed-sql-server.md)|  
|Décrit comment déplacer un UCP d'une instance de SQL Server vers une autre.|[Déplacer un UCP d’une instance de SQL Server vers une autre &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/move-a-ucp-from-one-instance-of-sql-server-to-another-sql-server-utility.md)|  
|Décrit comment supprimer un UCP.|[Supprimer un point de contrôle de l’utilitaire &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/remove-a-utility-control-point-sql-server-utility.md)|  
|Décrit comment résoudre les problèmes de l'utilitaire SQL Server.|[Résolution des problèmes liés à l'utilitaire SQL Server](http://msdn.microsoft.com/library/f5f47c2a-38ea-40f8-9767-9bc138d14453)|  
|Décrit comment résoudre les problèmes d'intégrité des ressources de SQL Server.|[Résoudre les problèmes de contrôle d’intégrité de SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/troubleshoot-sql-server-resource-health-sql-server-utility.md)|  
|Liens vers les rubriques d'aide (F1) de l'Explorateur d'utilitaire.|[Aide sur l'Explorateur d'objets accessible via la touche F1](../../relational-databases/manage/utility-explorer-f1-help.md)|  
  
  
