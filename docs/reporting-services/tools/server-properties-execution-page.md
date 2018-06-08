---
title: Propriétés du serveur (page Exécution) | Microsoft Docs
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
- sql13.swb.reportserver.serverproperties.execution.f1
ms.assetid: 53b77db1-b013-4dac-82dd-30c0de276639
caps.latest.revision: 32
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: c37ea4f28191e0b78977e4e7f2fb3bad102547cc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="server-properties-execution-page"></a>Propriétés du serveur (page Exécution)
  Utilisez cette page pour définir un délai d'attente pour l'exécution des rapports. Cette valeur s'applique à tous les rapports traités par l'instance actuelle du serveur de rapports. Vous pouvez remplacer cette valeur pour des rapports individuels. La valeur que vous spécifiez doit convenir à tout le traitement des rapports qui se produit sur le serveur de rapports, outre le traitement des requêtes effectué sur le serveur de base de données lorsque le serveur de rapports récupère les données utilisées dans le rapport.  
  
 Pour ouvrir cette page, démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à une instance de serveur de rapports, cliquez avec le bouton droit sur le nom du serveur de rapports, puis sélectionnez **Propriétés**. Cliquez sur **Exécution** pour ouvrir cette page.  
  
## <a name="options"></a>Options  
 **Ne pas appliquer de délai d'attente pour l'exécution des rapports**  
 Permettez à un serveur de rapports de terminer le traitement d'un rapport sans limite de temps.  
  
 **Limiter l'exécution du rapport au nombre de secondes suivant**  
 Définissez une contrainte de temps sur l'exécution de rapport. Le compte à rebours débute lorsque le rapport est demandé. Si la durée est dépassée avant la fin du traitement du rapport, le serveur de rapports annule le processus et toutes les requêtes intra-processus vers les sources de données externes.  
  
## <a name="see-also"></a> Voir aussi  
 [Définir les propriétés du serveur de rapports &#40;Management Studio&#41;](../../reporting-services/tools/set-report-server-properties-management-studio.md)   
 [Se connecter à un serveur de rapports dans Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md)   
 [Définir les propriétés de traitement d'un rapport](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Définition des valeurs de délai d’attente pour le traitement d’un rapport et d’un dataset partagé &#40;SSRS&#41;](../../reporting-services/report-server/setting-time-out-values-for-report-and-shared-dataset-processing-ssrs.md)   
 [Aide du serveur de rapports dans Management Studio via la touche F1](../../reporting-services/tools/report-server-in-management-studio-f1-help.md)  
  
  
