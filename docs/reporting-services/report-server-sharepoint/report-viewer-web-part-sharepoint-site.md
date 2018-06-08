---
title: Composant WebPart Visionneuse de rapports sur un site SharePoint - SSRS | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server-sharepoint
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: c29118ab8807d8bd0d51ee593488c66c89bf2942
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-viewer-web-part-on-a-sharepoint-site---reporting-services"></a>Composant WebPart Visionneuse de rapports sur un site SharePoint - Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)])

Le composant WebPart Visionneuse de rapports à une composant WebPart personnalisé. Vous pouvez l’utiliser pour afficher, parcourir, imprimer et exporter des rapports sur un serveur de rapports dans un site SharePoint. Le composant WebPart Visionneuse de rapports est associé aux fichiers de définition de rapport (.rdl) qui sont traités par un serveur de rapports Microsoft SQL Server Reporting Services. 

Le composant WebPart Visionneuse de rapports le plus récent peut également fournir des rapports paginés déployés sur Power BI Report Server. Le composant WebPart ne fonctionne pas avec les rapports Power BI.

## <a name="why-the-report-viewer-web-part-is-re-introduced"></a>Pourquoi le composant WebPart Visionneuse de rapports est-il réintroduit ?

Le composant WebPart Visionneuse de rapports était disponible dans le complément Reporting Services pour les produits SharePoint. Il était spécifique aux serveurs de rapports en mode intégré SharePoint. Le mode intégré SharePoint a été déprécié après SQL Server 2016.

Depuis SQL Server 2017, il n’existe qu’un seul mode d’installation de Reporting Services : le **mode natif**. Vous pouvez incorporer tous les types de rapports à l’aide d’un composant WebPart Visionneuse de pages en indiquant le paramètre d’URL *rs:Embed=true*. L’incorporation de rapports dans les pages SharePoint est un scénario d’intégration demandé par les clients et le composant WebPart Visionneuse de rapports mis à jour permet ce scénario pour les rapports paginés.

Même si le composant WebPart Visionneuse de pages suffit à incorporer un rapport paginé dans une page SharePoint, le composant WebPart Visionneuse de rapports mis à jour offre des fonctionnalités supplémentaires.

* Afficher/masquer des boutons de barre d’outils spécifiques
* Remplacer des valeurs de paramètre de rapport
* Connecter des composants WebPart Filtre aux paramètres de rapport

## <a name="download-the-report-viewer-web-part-solution-package"></a>Télécharger le package de solution du composant WebPart Visionneuse de rapports

Le composant WebPart Visionneuse de rapports est disponible sur le Centre de téléchargement Microsoft.

[Télécharger le package de solution du composant WebPart Visionneuse de rapports](https://www.microsoft.com/download/details.aspx?id=55949)

## <a name="considerations-and-limitations"></a>Observations et limitations

Les éléments répertoriés sont spécifiques au composant WebPart Visionneuse de rapports mis à jour.

* Le composant WebPart ne peut être utilisé que sur les pages SharePoint *classiques*.
* Seuls les rapports paginés (RDL) sont pris en charge pour l’incorporation dans le composant WebPart Visionneuse de rapports. Si vous cherchez à incorporer des rapports Power BI ou mobiles, vous pouvez utiliser le paramètre d’URL *rs:Embed=true*.

## <a name="next-steps"></a>Étapes suivantes

Pour commencer à utiliser le composant WebPart Visionneuse de rapports mis à jour, consultez [Déployer le composant WebPart Visionneuse de rapports de SQL Server Reporting Services sur un site SharePoint](deploy-report-viewer-web-part.md).
