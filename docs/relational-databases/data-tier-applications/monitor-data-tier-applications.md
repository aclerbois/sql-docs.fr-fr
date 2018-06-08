---
title: Analyser les applications de la couche Données | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: data-tier-applications
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-data-tier-apps
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- monitoring [SQL Server], data-tier applications
- monitoring server performance [SQL Server], DACs
- data-tier application [SQL Server], monitor
ms.assetid: d2765828-2385-4019-aef2-1de3ab7d1b26
caps.latest.revision: 11
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: f25b948403332fe5edf048e68bc210f786f9898c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="monitor-data-tier-applications"></a>Analyser les applications de la couche Données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Une application de la couche Données (DAC) peut être analysée à partir de l’ **Explorateur de l’utilitaire** et de l’ **Explorateur d’objets** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS), avec les vues et les tables système. De plus, tous les objets dans la base de données contenue dans la DAC peuvent être analysés à l'aide des techniques d'analyse de [!INCLUDE[ssDE](../../includes/ssde-md.md)] et de base de données standard.  
  
## <a name="before-you-begin"></a>Avant de commencer  
 Si vous déployez une DAC dans une instance gérée du [!INCLUDE[ssDE](../../includes/ssde-md.md)], les informations sur la DAC déployée sont incorporées dans l'utilitaire SQL Server lorsque le jeu d'éléments de collecte de l'utilitaire est envoyé de l'instance au point de contrôle de l'utilitaire. Vous pouvez ensuite afficher les informations intégrité de base sur la DAC à l'aide de **l'Explorateur de l'utilitaire** [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 L' **Explorateur d'objets** de SSMS affiche les informations de configuration de base sur chaque DAC déployée sur une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)], que l'instance soit gérée ou non dans l'utilitaire SQL Server. En outre, la base de données associée à une DAC déployée peut être analysée à l'aide des mêmes procédures d'analyse que celles employées pour toute base de données.  
  
## <a name="using-the-sql-server-utility"></a>Utilisation de l'utilitaire SQL Server  
 La page de détails **Applications de la couche de données déployées** dans l’ [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] **Utility Explorer** displays a dashboard that reports the resource utilization of all DACs that have been deployed to managed instances of the [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Le volet supérieur de la page des détails répertorie chaque DAC déployée avec des indicateurs visuels qui signalent si leur utilisation du processeur et des ressources de fichiers se situe hors des stratégies définies pour l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous sélectionnez une DAC en mode Liste, des détails supplémentaires sont affichés sous les onglets dans le volet inférieur de la page. Pour plus d’informations sur les informations présentées dans la page de détails, consultez [Détails des applications de la couche Données déployées &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/79c41dd9-abcb-434e-9326-00a341d5c867).  
  
 Après avoir utilisé la page des détails **Applications de la couche Données déployées** pour identifier rapidement toutes les DAC qui utilisent trop ou pas assez leurs ressources matérielles, vous pouvez élaborer des plans pour traiter tous les problèmes. Plusieurs DAC qui n'utilisent pas pleinement leurs ressources matérielles actuelles pourraient être consolidées en un serveur unique, ce qui libérerait certains des serveurs pour d'autres utilisations. Si une DAC utilise trop les ressources sur son serveur en cours, elle peut être déplacée vers un plus grand serveur ou d'autres ressources peuvent être ajoutées au serveur en cours.  
  
 Les limites minimale et maximale pour l'utilisation des ressources sont définies par des stratégies d'analyse des applications définies dans la page des détails **Administration de l'utilitaire** . Les administrateurs de la base de données peuvent adapter les stratégies pour qu'elles correspondent aux limites établies par leurs organisations. Par exemple, une société peut définir 75 % comme utilisation maximale du processeur pour une DAC alors qu'une autre société peut affecter 80 % comme valeur maximale. Pour plus d’informations sur la définition des stratégies d’analyse des applications, consultez [Administration de l’utilitaire &#40;SQL Server Utility&#41;](http://msdn.microsoft.com/library/3e5a00c3-8905-40f0-9ddc-d924df9c2f0d).  
  
 Pour afficher la page des détails **Applications de la couche Données déployées** :  
  
1.  Sélectionnez le menu **Affichage/Explorateur de l’utilitaire** .  
  
2.  Connectez l’ **Explorateur de l’utilitaire** au point de contrôle d’utilitaire (UCP).  
  
3.  Sélectionnez le menu **Affichage/Détails de l’Explorateur de l’utilitaire** .  
  
4.  Sélectionnez le nœud **Applications de la couche Données déployées** dans l’ **Explorateur de l’utilitaire**.  
  
 Les informations de la page des détails **Applications de la couche Données déployées** proviennent des données dans l’entrepôt de données de gestion de l’utilitaire, qui recueille par défaut les données toutes les 15 minutes. L'intervalle peut également être adapté à l'aide de la page des détails **Administration de l'utilitaire** .  
  
## <a name="using-object-explorer"></a>Utilisation de l'Explorateur d'objets  
 L' **Explorateur d'objets** SSMS affiche les informations de configuration de base sur chaque DAC déployée vers une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Cela inclut à la fois les instances gérées inscrites dans l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les instances autonomes qui ne peuvent pas être affichées dans l’ **Explorateur de l’utilitaire**.  
  
 Pour consulter les détails d'une DAC déployée vers une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]:  
  
1.  Sélectionnez le menu **Affichage/Explorateur d’objets** .  
  
2.  Connectez-vous à l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]à partir du volet de l'Explorateur d'objets.  
  
3.  Sélectionnez le menu **Affichage/Détails de l’Explorateur d’objets** .  
  
4.  Sélectionnez le nœud serveur dans l’ **Explorateur d’objets** qui mappe à l’instance, puis naviguez jusqu’au nœud **Gestion\Applications de la couche Données** .  
  
5.  Le mode Liste dans le volet supérieur de la page des détails répertorie chaque DAC déployée vers l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Sélectionnez une DAC pour afficher les informations dans le volet d'informations au bas de la page.  
  
 Le menu contextuel du nœud **Applications de la couche Données** est également utilisé pour déployer une nouvelle DAC ou supprimer une DAC existante.  
  
## <a name="using-the-dac-system-views-and-tables"></a>Utilisation des vues et tables système de la DAC  
 La table système msdb.dbo.sysdac_history_internal enregistre la réussite ou l'échec de toutes les actions de gestion de la DAC effectuées sur une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. La table enregistre l'heure de chaque action et la connexion à l'origine de l'action. Pour plus d’informations, consultez [sysdac_history_internal &#40;Transact-SQL&#41;](../../relational-databases/system-tables/data-tier-application-tables-sysdac-history-internal.md).  
  
 Les vues système de la DAC signalent les informations de catalogue de base. Pour plus d’informations, consultez [Vues de l’application de la couche Données &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/0de01328-d7a6-4677-b7a0-dcd3098c23d4).  
  
## <a name="monitoring-dac-databases"></a>Analyse des bases de données de la DAC  
 Après le déploiement réussi d'une DAC, la base de données contenue dans la DAC fonctionne de la même façon que toute autre base de données. Utilisez des outils et techniques du [!INCLUDE[ssDE](../../includes/ssde-md.md)] standard pour analyser les performances, le journal, les événements et l'utilisation des ressources de la base de données.  
  
## <a name="see-also"></a> Voir aussi  
 [Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [Déployer une application de la couche Données](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)  
  
  
