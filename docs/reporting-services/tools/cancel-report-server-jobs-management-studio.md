---
title: Annuler les travaux du serveur de rapports (Management Studio) | Microsoft Docs
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
- sql13.swb.reportserver.cancelreportserverjobs.f1
ms.assetid: 1c5b4975-49e9-4d0b-b298-2638e81edbfd
caps.latest.revision: 14
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 2ad15367b678ae9a304feb3d5a384d0611d6a9c9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cancel-report-server-jobs-management-studio"></a>Annuler les travaux du serveur de rapports (Management Studio)
  Utilisez la boîte de dialogue **Annuler les travaux du serveur de rapports** pour afficher ou annuler les rapports en cours. Cette boîte de dialogue affiche tous les travaux qui sont en cours d'exécution sur le serveur de rapports. Bien que vous ne puissiez pas suspendre ni redémarrer des travaux en cours de traitement, vous pouvez annuler tous les travaux ou des travaux individuels si leur exécution est trop longue.  
  
 Vous pouvez annuler des travaux utilisateur et système.  
  
-   Un travail utilisateur est un travail qui est initié par un utilisateur individuel. Il peut s'agir de l'exécution d'un rapport à la demande, de la création manuelle d'un instantané d'historique de rapport ou de la création manuelle d'un instantané d'exécution de rapport. Un abonnement standard en cours est également un travail utilisateur.  
  
-   Un travail système est un travail lancé par le serveur de rapports. Les travaux système incluent le traitement planifié des rapports.  
  
 Pour ouvrir cette page, démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à un serveur de rapports, cliquez avec le bouton droit sur **Travaux**, puis cliquez sur **Annuler tous les travaux**. Vous pouvez également ouvrir **Travaux**, cliquer avec le bouton droit sur un travail en cours d’exécution sur le serveur de rapports, puis sélectionner **Annuler les travaux**.  
  
 Avant d'annuler un travail, vous pouvez consulter ses propriétés pour déterminer à quel moment il a démarré. Pour plus d’informations, consultez [Propriétés du travail &#40;Management Studio&#41;](../../reporting-services/tools/job-properties-management-studio.md).  
  
> [!NOTE]  
>  Cette fonctionnalité n'est pas prise en charge dans [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] with Advanced Services. La page n'apparaît pas lorsque vous exécutez [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].  
  
## <a name="options"></a>Options  
 **Nom**  
 Affiche le nom du rapport. Les abonnements sont identifiés par leur description.  
  
 **Type**  
 Les valeurs valides sont **Utilisateur** et **Système**.  
  
 **Start Time**  
 Indique à quel moment le travail a démarré.  
  
 **Nom d'utilisateur**  
 Pour les travaux lancés par un utilisateur, cette colonne affiche le nom de l'utilisateur.  
  
 **État**  
 Indique l'état du travail. Les valeurs valides sont **Nouveau** et **En cours d'exécution**. La valeur de l'état est toujours **Nouveau** lorsque le travail commence. Après 60 secondes, la valeur de l'état devient **En cours d'exécution**. Vous devez actualiser la page pour voir la modification.  
  
 **OK**  
 Annulez un seul travail ou plusieurs travaux. Les travaux sont immédiatement annulés et ne peuvent pas être repris. Si vous annulez un travail par erreur, vous devez demander une nouvelle fois le rapport ou l'abonnement pour démarrer un nouveau travail.  
  
## <a name="see-also"></a> Voir aussi  
 [Aide du serveur de rapports dans Management Studio accessible par la touche F1](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)   
 [Se connecter à un serveur de rapports dans Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [Gérer un processus en cours d'exécution](../../reporting-services/subscriptions/manage-a-running-process.md)  
  
  
