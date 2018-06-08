---
title: Combinaisons de serveurs SharePoint et Reporting Services prises en charge | Microsoft Docs
ms.custom: ''
ms.date: 07/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SharePoint mode
- add-in for sharepoint
- rsSharePoint
ms.assetid: dc6a3372-db26-43f0-b7aa-f725acc635c2
caps.latest.revision: 39
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 144b7676a9f1f66427fbc1d5666f5a39c012823f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="supported-combinations-of-sharepoint-and-reporting-services-server"></a>Combinaisons de serveurs SharePoint et Reporting Services prises en charge

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE [ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Un serveur de rapports SQL Server Reporting Services installé en mode SharePoint nécessite une version de SharePoint et le complément SQL Server Reporting Services (rsSharePoint.msi) pour les produits SharePoint que vous installez sur les serveurs SharePoint. Cette rubrique résume les combinaisons prises en charge.

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

## <a name="supported-combinations-of-sharepoint-and-reporting-services-components"></a>Combinaisons de composants SharePoint et Reporting Services prises en charge

 Le tableau suivant résume les combinaisons prises en charge de serveur de rapports, complément Reporting Services pour les produits SharePoint et produits SharePoint. Les associations qui ne sont pas répertoriées dans le tableau suivant ne sont pas prises en charge

### <a name="supported-combinations"></a>Combinaisons prises en charge

||Serveur de rapports|Complément|Version SharePoint|
|-|-------------------|-------------|------------------------|
| 1|SQL Server 2016|SQL Server 2016|SharePoint 2016|
|2|SQL Server 2016|SQL Server 2016|SharePoint 2013|
|3|SQL Server 2014|SQL Server 2014|SharePoint 2013|
|4|SQL Server 2014|SQL Server 2014|SharePoint 2010|
|5|SQL Server 2012 SP3|SQL Server 2014 et SQL Server 2012 SP3|SharePoint 2013|
|6|SQL Server 2012 SP2|SQL Server 2014 et SQL Server 2012 SP2|SharePoint 2013|
|7|SQL Server 2012 SP1|SQL Server 2014 et SQL Server 2012 SP1|SharePoint 2013|
|8|SQL Server 2012 et SQL Server 2012 SP1*|SQL Server 2014|SharePoint 2010|
|9|SQL Server 2012|SQL Server 2012|SharePoint 2010|
|10|SQL Server 2008 R2|SQL Server 2014|SharePoint 2010|
|11|SQL Server 2008 R2|SQL Server 2012 et SQL Server 2012 SP1* ou version ultérieure|SharePoint 2010|
|12|SQL Server 2008 R2|SQL Server 2008 R2|SharePoint 2010|
|13|SQL Server 2008 R2|SQL Server 2008 SP2|SharePoint 2007|
|14|SQL Server 2008 SP2|SQL Server 2008 R2|SharePoint 2010|
|15|SQL Server 2008 SP2|SQL Server 2008 et SQL Server 2008 SP2|SharePoint 2007|

 *Exception : l’intégration de Power View n’est pas prise en charge.

 Pour obtenir des liens vers les pages de téléchargement des compléments, consultez [Où trouver le complément Reporting Services pour les produits SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md).  

 **Considérations supplémentaires :**

- N’oubliez pas de mettre à niveau tous les serveurs SharePoint de la batterie de serveurs. Cela comprend les serveurs d’applications et les serveurs web frontaux.

- La prise en charge de SharePoint 2016, comprenant l’intégration de Power View, nécessite le serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et la version du complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de SQL Server 2016, ou une version ultérieure.

- La prise en charge de SharePoint 2013, comprenant l'intégration de Power View, nécessite le serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] et la version complémentaire [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de SQL Server 2012 SP1, ou une version ultérieure.

- Power View a été introduit dans SQL Server 2012. Par conséquent, l’intégration de Power View dans SharePoint 2010 requiert la version SQL Server 2012 ou une version ultérieure du complément.

- Le complément SQL Server 2008 R2 n'est pas pris en charge par les serveurs de rapports SQL Server 2012 (ou versions ultérieures). Le programme d'installation de SharePoint 2010 installe automatiquement le complément SQL Server 2008 R2. Il doit être désinstallé avant d'installer les nouvelles versions du complément. La mise à niveau sur place du complément n'est pas prise en charge.

- **Mise à niveau :** SharePoint 2010 avec le complément [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] installé ne peut pas être mis à niveau sur place vers SharePoint 2013. SharePoint 2013 requiert SQL Server 2012 SP1 ou une version ultérieure du complément et du serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour plus d'informations sur la mise à niveau, consultez [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).

## <a name="next-steps"></a>Étapes suivantes

 [Où trouver le complément Reporting Services pour les produits SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)   
 [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)   
 [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
