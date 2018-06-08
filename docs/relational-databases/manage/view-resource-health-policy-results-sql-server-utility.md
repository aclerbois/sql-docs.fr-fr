---
title: Consulter les résultats d’une stratégie de contrôle d’intégrité des ressources (utilitaire SQL Server) | Microsoft Docs
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
ms.assetid: 80cb14fb-f4c6-4be2-ba17-eb4e4cddd35f
caps.latest.revision: 10
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 22f07e28b8b047074424d3480d026b7c5b73cf5e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-resource-health-policy-results-sql-server-utility"></a>Consulter les résultats d'une stratégie de contrôle d'intégrité des ressources (Utilitaire SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez le tableau de bord de l’utilitaire dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour consulter les paramètres des ressources de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les instances managées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les applications de la couche Données. Pour plus d’informations, consultez [Fonctionnalités et tâches de l’utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md).  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="view-sql-server-utility-resource-health-policy-results"></a>Consulter les résultats d'une stratégie de contrôle d'intégrité des ressources de l'utilitaire SQL Server.  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS), cliquez sur **Affichage**, puis sur **Explorateur de l’utilitaire** pour afficher le volet Navigation de l’Explorateur de l’utilitaire. Pour afficher le volet de contenu, cliquez sur **Affichage**, puis sur **Contenu de l'Explorateur de l'utilitaire**.  
  
2.  Dans le volet Navigation, cliquez sur ![](../../relational-databases/manage/media/connect-to-utility.gif "Se connecter à l’utilitaire")**Se connecter à l’utilitaire**. Si vous n’avez pas créé de point de contrôle d’utilitaire (UCP) ou si vous n’avez pas inscrit d’instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d’applications de la couche Données dans l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Fonctionnalités et tâches de l’utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md).  
  
3.  Cliquez sur le nœud UCP pour afficher les données de synthèse des instances managées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et des applications de la couche Données (cliquez avec le bouton droit pour actualiser). Les données du tableau de bord sont affichées dans le volet Contenu.  
  
4.  Cliquez sur le nœud **Instances managées** pour afficher les données en mode Liste des instances managées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (cliquez avec le bouton droit pour actualiser). Les données en mode Liste sont affichées dans le volet Contenu.  
  
5.  Cliquez sur le nœud **Applications de la couche Données déployées** pour afficher les données en mode Liste des applications de la couche Données (cliquez avec le bouton droit pour actualiser). Les données en mode Liste sont affichées dans le volet Contenu.  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalités et tâches de l'utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Réduire le bruit dans les stratégies d’utilisation du processeur &#40;Utilitaire SQL Server&#41;](../../relational-databases/manage/reduce-noise-in-cpu-utilization-policies-sql-server-utility.md)   
 [Détails des applications de la couche Données déployées &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/79c41dd9-abcb-434e-9326-00a341d5c867)   
 [Détails de l’instance gérée &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/6e51b7bb-a733-4852-8c33-7f4dbdf931c2)   
 [Administration de l’utilitaire &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/3e5a00c3-8905-40f0-9ddc-d924df9c2f0d)  
  
  
