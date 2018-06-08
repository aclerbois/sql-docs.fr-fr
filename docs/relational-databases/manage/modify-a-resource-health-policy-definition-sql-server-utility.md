---
title: Modifier une définition de la stratégie de contrôle d’intégrité des ressources (utilitaire SQL Server) | Microsoft Docs
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
f1_keywords:
- sql13.SWB.UE.UTILITY.ADMINISTRATION.F1
ms.assetid: 27bec0b6-92e9-448e-8c70-fe36802cf128
caps.latest.revision: 9
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 65106ce68705f630b3955865b6967cfde0aae9a8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modify-a-resource-health-policy-definition-sql-server-utility"></a>Modifier une définition de la stratégie de contrôle d'intégrité des ressources (utilitaire SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment modifier une définition de stratégie de contrôle d'intégrité des ressources dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Avant de modifier une stratégie d'utilisation des ressources dans votre utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez créer un point de contrôle de l'utilitaire (UCP). Pour plus d’informations, consultez [Fonctionnalités et tâches de l’utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Les stratégies d’utilisation des ressources de l’utilitaire peuvent être configurées pour des applications de la couche Données et des instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les stratégies d’utilisation des ressources peuvent être définies globalement pour toutes les applications de la couche Données et toutes les instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Elles peuvent également être définies individuellement pour chaque application de la couche Données et chaque instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez également implémenter des stratégies globales et configurer des applications de la couche Données ou des instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] individuelles avec leurs propres définitions de stratégie.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="modify-global-resource-utilization-policies-in-a-sql-server-utility"></a>Modifier des stratégies globales d'utilisation des ressources dans un utilitaire SQL Server.  
  
1.  Connectez-vous à l'UCP dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Dans le volet de navigation Explorateur de l'utilitaire, cliquez sur **Administration de l'utilitaire** pour afficher ou modifier des stratégies globales de surveillance, puis sur l'onglet **Stratégie** dans le volet de contenu de l'Explorateur de l'utilitaire.  
  
3.  Dans le volet de contenu de l’Explorateur de l’utilitaire, sélectionnez **Définir des stratégies globales de surveillance de la couche Données** ou **Définir des stratégies globales de surveillance d’instance gérée** en cliquant sur la flèche ou la description de la stratégie.  
  
4.  Utilisez les contrôles à droite des descriptions de stratégie pour définir les seuils de stratégie de sous-exploitation ou de surexploitation.  
  
5.  Utilisez les boutons **Appliquer**, **Ignorer**ou **Paramètres par défaut** selon vos besoins. La modification de la stratégie peut prendre jusqu’à 15 minutes pour se répercuter dans les informations du tableau de bord et du mode Liste de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
6.  Pour actualiser les données, cliquez avec le bouton droit sur le nœud **Administration de l’utilitaire** dans le volet de navigation Explorateur de l’utilitaire et sélectionnez **Actualiser**.  
  
#### <a name="modify-resource-health-policy-definitions-for-an-individual-data-tier-application-or-an-individual-managed-instance-of-sql-server-in-a-sql-server-utility"></a>Modifier les définitions de la stratégie de contrôle d'intégrité des ressources pour une application de la couche Données individuelle ou une instance gérée individuelle de SQL Server dans un utilitaire SQL Server  
  
1.  Connectez-vous à l'UCP dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Dans le volet de navigation Explorateur de l’utilitaire, cliquez sur **Applications de la couche Données déployées**ou sur **Instances gérées**pour afficher ou modifier des stratégies de surveillance pour une application de la couche Données ou une instance gérée individuelle.  
  
3.  En mode Liste du volet de contenu de l’Explorateur de l’utilitaire, cliquez sur l’application de la couche Données ou sur le nom de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dont vous souhaitez modifier les stratégies, puis cliquez sur l’onglet **Détails de stratégie** .  
  
4.  Sélectionnez la stratégie à afficher ou à modifier en cliquant sur la flèche ou sur la description de la stratégie. Les stratégies globales sont sélectionnées par défaut.  
  
5.  Sélectionnez la case d’option **Substituer la stratégie globale** pour remplacer les stratégies globales et implémenter une définition de stratégie individuelle pour l’application de la couche Données spécifiée.  
  
6.  Utilisez les contrôles à droite de la description de la stratégie pour définir les seuils de stratégie de sous-exploitation ou de surexploitation.  
  
7.  Utilisez les boutons **Appliquer**, **Ignorer**ou **Paramètres par défaut** selon vos besoins. La modification de la stratégie peut prendre jusqu’à 15 minutes pour se répercuter dans les informations du tableau de bord et du mode Liste de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
8.  Pour actualiser les données, cliquez avec le bouton droit sur le nœud **Applications de la couche Données déployées** dans le volet de navigation de l’Explorateur de l’utilitaire et sélectionnez **Actualiser**.  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalités et tâches de l'utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Consulter les résultats d’une stratégie de contrôle d’intégrité des ressources &#40;Utilitaire SQL Server&#41;](../../relational-databases/manage/view-resource-health-policy-results-sql-server-utility.md)  
  
  
