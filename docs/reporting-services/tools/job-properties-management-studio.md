---
title: Propriétés du travail (Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.reportserver.jobproperties.f1
ms.assetid: 807ffd0e-9363-4f8f-9c36-c5d746ad19fd
caps.latest.revision: 13
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: e5b9083049d09da625fbd4040e028aac77ac6d12
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="job-properties-management-studio"></a>Propriétés du travail (Management Studio)
  Utilisez la page **Propriétés du travail** pour afficher des informations sur un rapport ou abonnement en cours avant de l’annuler.  
  
 Pour ouvrir cette page, démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à un serveur de rapports, puis ouvrez le dossier **Travaux** . Cliquez avec le bouton droit sur un travail en cours d’exécution, puis cliquez sur **Propriétés**.  
  
> [!NOTE]  
>  Cette fonctionnalité n'est pas prise en charge dans [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] with Advanced Services. La page n'apparaît pas lorsque vous exécutez [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].  
  
## <a name="tasks"></a>Tâches  
 Avant de pouvoir consulter des informations sur un travail, actualisez la page pour récupérer des informations sur les travaux qui sont actuellement en cours d'exécution sur le serveur de rapports :  
  
1.  Ouvrez le dossier du serveur de rapports.  
  
2.  Cliquez avec le bouton droit sur **Travaux**, puis cliquez sur **Actualiser**.  
  
3.  Si un travail est répertorié, cliquez dessus avec le bouton droit, puis cliquez sur **Propriétés**.  
  
## <a name="options"></a>Options  
 **ID de travail**  
 GUID attribué à un travail pendant son traitement. La valeur est générée de façon aléatoire chaque fois qu'un rapport ou abonnement est exécuté.  
  
 **État du travail**  
 Les valeurs valides sont **Nouveau** et **En cours d'exécution**. La valeur de l'état est toujours **Nouveau** lorsque le travail commence. Après 60 secondes, la valeur de l'état devient **En cours d'exécution**. Vous devez actualiser la page pour voir la modification.  
  
 **Type de travail**  
 Les valeurs valides sont **Utilisateur** et **Système**. Un travail utilisateur est un travail qui est initié par un utilisateur individuel. Il peut s'agir de l'exécution d'un rapport sur demande, de la création manuelle d'un instantané d'historique de rapport ou de la création manuelle d'un instantané d'exécution de rapport. Un abonnement standard en cours est également un travail utilisateur. Un travail système est un travail lancé par le serveur de rapports. Les travaux système incluent le traitement des rapports déclenché par une planification.  
  
 **Action du travail**  
 Pour les rapports, cette colonne affiche les processus d'exécution de rapport en cours. Cette valeur est toujours **Rendu**.  
  
 **Description du travail**  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne fournit pas de descriptions de travail par défaut.  
  
 **Nom de serveur**  
 Affiche le nom du serveur de rapports qui traite le travail. Si vous avez configuré un déploiement avec montée en puissance parallèle, cette valeur affichera le serveur qui traite le travail.  
  
 **Nom du rapport**  
 Affiche le nom du rapport. Les abonnements sont identifiés par leur description.  
  
 **Chemin d'accès au rapport**  
 Affiche le chemin d'accès au rapport dans l'arborescence des dossiers du serveur de rapports.  
  
 **Start Time**  
 Indique à quel moment le processus a démarré.  
  
 **Nom d'utilisateur**  
 Pour les processus lancés par un utilisateur, cette colonne affiche le nom de l'utilisateur. Pour les travaux système, il s'agit du nom du serveur de rapports.  
  
## <a name="see-also"></a> Voir aussi  
 [Aide du serveur de rapports dans Management Studio accessible par la touche F1](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)   
 [Se connecter à un serveur de rapports dans Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [Gérer un processus en cours d'exécution](../../reporting-services/subscriptions/manage-a-running-process.md)  
  
  
