---
title: Déplacer un UCP d’une instance de SQL Server vers une autre (utilitaire SQL Server) | Microsoft Docs
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
ms.assetid: b402fd9e-0bea-4c38-a371-6ed7fea12e96
caps.latest.revision: 9
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: b2dbe11fd8837bf84fa7f1e3195f6831e33c2ebd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="move-a-ucp-from-one-instance-of-sql-server-to-another-sql-server-utility"></a>Déplacer un UCP d'une instance de SQL Server vers une autre (utilitaire SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment déplacer un point de contrôle de l'utilitaire (UCP) d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers une autre dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="move-a-ucp-from-one-instance-of-sql-server-to-another"></a>Déplacer un UCP d'une instance de SQL Server vers une autre.  
  
1.  Créez un UCP sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui sera la nouvelle instance hôte de l'UCP. Pour plus d’informations, consultez [Créer un point de contrôle de l’utilitaire SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/create-a-sql-server-utility-control-point-sql-server-utility.md)  
  
2.  Si des paramètres de stratégie non définis par défaut ont été définis pour toutes instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans votre utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], notez les seuils de stratégie afin que vous puissiez les rétablir sur le nouvel UCP. Les stratégies par défaut sont appliquées lorsque des instances sont ajoutées au nouvel UCP. Si les stratégies par défaut sont appliquées, le mode Liste de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche **Global** dans la colonne **Type de stratégie** .  
  
3.  Supprimez toutes les instances gérées de l'ancien UCP. Pour plus d’informations, consultez [Supprimer une instance de SQL Server de l’utilitaire SQL Server](../../relational-databases/manage/remove-an-instance-of-sql-server-from-the-sql-server-utility.md).  
  
4.  Sauvegardez l'entrepôt de données de gestion de l'utilitaire (UMDW) à partir de l'ancien UCP. Le nom de fichier est Sysutility_mdw_\<GUID>_DATA et l’emplacement par défaut de la base de données est \<Lecteur système>:\Program Files\Microsoft SQL Server\MSSQL10_50.<Nom_UCP>\MSSQL\Data\\, où \<Lecteur système> est normalement le lecteur C:\. Pour plus d’informations, consultez [Copier des bases de données avec la sauvegarde et la restauration](../../relational-databases/databases/copy-databases-with-backup-and-restore.md).  
  
5.  Restaurez la sauvegarde de l'UMDW sur le nouvel UCP. Pour plus d’informations, consultez [Copier des bases de données avec la sauvegarde et la restauration](../../relational-databases/databases/copy-databases-with-backup-and-restore.md).  
  
6.  Inscrivez des instances dans le nouvel UCP pour qu'elles soient managées par l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Inscrire une instance de SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/enroll-an-instance-of-sql-server-sql-server-utility.md).  
  
7.  Implémentez des définitions de stratégies personnalisées pour les instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le cas échéant.  
  
8.  Attendez environ 1 heure que se terminent la collecte de données et les opérations d'agrégation.  
  
9. Pour actualiser les données, cliquez avec le bouton droit sur le nœud **Instances gérées** dans l’ **Explorateur de l’utilitaire**, puis sélectionnez **Actualiser**. Les données en mode Liste sont affichées dans le volet Contenu de l’**Explorateur de l’utilitaire**. Pour plus d’informations, consultez [Consulter les résultats d’une stratégie de contrôle d’intégrité des ressources &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/view-resource-health-policy-results-sql-server-utility.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalités et tâches de l'utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Inscrire une instance de SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/enroll-an-instance-of-sql-server-sql-server-utility.md)  
  
  
