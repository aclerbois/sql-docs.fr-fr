---
title: Vérification de l’exécution d’un rapport | Microsoft Docs
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
- auditing [Reporting Services]
- verifying report execution
- logs [Reporting Services], verifying report run
- report execution data [Reporting Services]
- status information [Reporting Services]
- report processing [Reporting Services], verifying execution
- checking report execution
ms.assetid: 18a98f2f-6b40-454e-9b37-568ed1a96458
caps.latest.revision: 37
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 94611570a2c000b5380a304a79785d24d58b46cc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="verifying-a-report-run"></a>Vérification de l'exécution d'un rapport
  Pour afficher des informations sur l'état du traitement d'un rapport, vous pouvez utiliser les fichiers journaux ou vous référer aux informations d'état qui s'affichent avec le rapport dans le Gestionnaire de rapports.  
  
## <a name="sources-of-report-execution-data"></a>Sources de données d'exécution d'un rapport  
 Les journaux d'exécution de rapports fournissent des informations complètes sur l'exécution d'un rapport, y compris le nom du rapport, le nom de l'utilisateur ayant exécuté le rapport, l'heure d'exécution du rapport ainsi que l'extension de remise utilisée pour distribuer le rapport. Pour afficher et analyser ces données, vous pouvez copier le journal d'exécution du rapport dans des tables de base de données qui sont faciles à interroger et à éditer.  
  
 Les fichiers journaux contiennent de nombreuses entrées concernant l'exécution du rapport et d'autres activités liées au serveur. Si vous souhaitez vérifier uniquement le moment de la dernière exécution d'un rapport, comme les fichiers journaux contiennent beaucoup de données, vous pouvez opter pour l'utilisation du Gestionnaire de rapports. Si vous avez besoin d'informations supplémentaires, vous devez afficher les fichiers journaux.  
  
> [!NOTE]  
>  Le Gestionnaire de rapports n'affiche pas les horaires de traitement des rapports qui s'exécutent à la demande.  
  
 Le tableau suivant indique comment afficher la date et l'heure de traitement des divers types de rapports.  
  
|Pour ce type de rapport|Les informations de date et d'heure se situent ici|Pour afficher ces informations, procédez comme suit|  
|-----------------------------|-----------------------------------------------|-----------------------------------------------|  
|Rapport s'exécutant en tant qu'instantané de rapport.|Dans la page Contenu. Pour plus d’informations, consultez [Page Contenu &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/6b16869b-158a-4934-9c85-bee934b35378).|1) Localisez le dossier contenant le rapport.<br /><br /> 2) Choisissez d’afficher le contenu du dossier en mode Détails.<br /><br /> 3) Notez la date et l’heure figurant dans la colonne **Lors de l’exécution** .|  
|Instantané dans l'historique de rapport.|Dans la page des propriétés de l'historique. Pour plus d’informations, consultez [Page de propriétés Options d’instantanés &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/f6641f59-5267-4f57-8957-63b93d1a9679).|1) Ouvrez le rapport.<br /><br /> 2) Cliquez sur la page **Propriétés** .<br /><br /> 3) Cliquez sur l’onglet **Historique** .<br /><br /> 4) Notez la date et l’heure figurant dans la colonne **Lors de l’exécution** .|  
|Rapport mis en cache.|Dans la planification utilisée pour créer et actualiser le rapport mis en cache.|1) Ouvrez le rapport.<br /><br /> 2) Cliquez sur la page **Propriétés** .<br /><br /> 3) Cliquez sur l’onglet **Exécution** .<br /><br /> 4) Ouvrez la planification.|  
  
## <a name="see-also"></a> Voir aussi  
 [Fichiers journaux et sources de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md)   
 [Définir les propriétés de traitement d'un rapport](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896)  
  
  
