---
title: Ajouter un instantané à un historique de rapport (Gestionnaire de rapports) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- report history [Reporting Services], adding snapshots
- historical data [Reporting Services]
- snapshots [Reporting Services], adding report snapshots
- adding snapshots to report history
- report snapshots [Reporting Services], adding
ms.assetid: 3aafb183-789e-46ac-966c-881dc549b31d
caps.latest.revision: 35
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 3cf4f07b6070cc0cc0dc364292a285c51f9e2787
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-a-snapshot-to-report-history-report-manager"></a>ajout d'un instantané à un historique de rapport (Gestionnaire de rapports)
  L'historique de rapport est un ensemble d'instantanés de rapport que vous créez au fil du temps. Un instantané de rapport est un rapport contenant des informations de mise en page et des résultats de requêtes récupérés à un moment précis. Contrairement aux rapports à la demande, qui récupèrent les résultats des requêtes récentes lorsque vous les sélectionnez, les instantanés de rapport sont traités par planification, puis enregistrés sur un serveur de rapports. Lorsque vous sélectionnez un instantané de rapport pour le visualiser, le serveur de rapports récupère le rapport stocké dans la base de données du serveur de rapports, puis affiche les données et la mise en page telles qu'elles étaient lors de la création de l'instantané.  
  
 Les instantanés de rapport ne sont pas enregistrés dans un format de rendu particulier. Ils sont générés dans un format d'affichage final (par exemple, au format HTML) uniquement à la demande d'un utilisateur ou d'une application. Le rendu différé permet de disposer d'un instantané portable. Le rendu du rapport peut être effectué dans le format approprié pour le périphérique ou le navigateur Web à l'origine de la demande.  
  
### <a name="to-manually-add-snapshots-to-report-history"></a>Pour ajouter manuellement des instantanés à un historique de rapport  
  
1.  Dans le Gestionnaire de rapports, accédez à la page **Contenu** , pointez sur l’élément pour lequel vous souhaitez consulter l’historique et cliquez sur la flèche déroulante.  
  
2.  Dans le menu déroulant, cliquez sur **Afficher l’historique du rapport**.  
  
3.  Cliquez sur **Nouvel instantané**. Un instantané est créé dans la colonne **Lors de l’exécution** .  
  
    > [!NOTE]  
    >  Pour effectuer cette opération, l’historique de rapport doit être configuré par l’administrateur pour **Autoriser la création manuelle de l’historique**. Pour plus d’informations, consultez [Limiter l’historique de rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/reports/limit-report-history-report-manager.md).  
  
4.  Cliquez sur **Appliquer**.  
  
### <a name="to-automatically-add-all-snapshots-to-report-history"></a>Pour ajouter automatiquement tous les instantanés à un historique de rapport  
  
1.  Pour un rapport déjà configuré pour s'exécuter en tant qu'instantané d'exécution de rapport, vous pouvez définir des propriétés supplémentaires afin d'enregistrer une copie de l'instantané dans l'historique de rapport chaque fois qu'il est actualisé.  
  
2.  Dans le Gestionnaire de rapports, accédez à la page **Contenu** , pointez sur l’élément pour lequel vous souhaitez consulter l’historique et cliquez sur la flèche déroulante.  
  
3.  Dans le menu déroulant, cliquez sur **Gérer**.  
  
4.  Cliquez sur **Options d’instantanés**.  
  
5.  Cochez la case **Stocker toutes les captures instantanées d’exécution de rapport dans l’historique**.  
  
6.  Cliquez sur **Appliquer**.  
  
### <a name="to-automatically-add-snapshots-to-report-history-based-on-a-schedule"></a>Pour ajouter automatiquement tous les instantanés à un historique de rapport planifié  
  
1.  Dans le Gestionnaire de rapports, accédez à la page **Contenu** , pointez sur l’élément pour lequel vous souhaitez consulter l’historique et cliquez sur la flèche déroulante.  
  
2.  Dans le menu déroulant, cliquez sur **Gérer**.  
  
3.  Cliquez sur **Options d’instantanés**.  
  
4.  Cochez la case **Utilisez la planification ci-dessous pour ajouter des instantanés à l’historique de rapport**. Effectuez l'une des opérations suivantes :  
  
    -   Sélectionnez **Planification spécifique aux rapports**. Indiquez les détails de la planification, sélectionnez des dates de début et de fin pour cette opération, puis cliquez sur **OK**.  
  
    -   Sélectionnez **Planification partagée**. Dans la liste, désignez la planification de votre choix.  
  
5.  Cliquez sur **Appliquer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer les propriétés d’exécution d’un rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/reports/configure-execution-properties-for-a-report-report-manager.md)   
 [Ouvrir et fermer un rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/reports/open-and-close-a-report-report-manager.md)   
 [Limiter l’historique de rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/reports/limit-report-history-report-manager.md)   
 [Planifications](../../reporting-services/subscriptions/schedules.md)   
 [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896)  
  
  
