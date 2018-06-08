---
title: Déploiements multilingues et globaux (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: install
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: c3d485f8-867c-4aa2-a90d-f38fda192534
caps.latest.revision: 8
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 12e796619f7ab5674f5f94b34ea15d4e2aefe595
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="multi-lingual-and-global-deployments-master-data-services"></a>Déploiements multilingues et globaux (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] prend en charge le déploiement de composants et d'outils dans toutes les langues prises en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Local Language Versions in SQL Server](../../sql-server/install/local-language-versions-in-sql-server.md).  
  
## <a name="how-languages-are-used"></a>Utilisation des langues  
 Le tableau suivant décrit la prise en charge des langues pour les composants et outils des [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .  
  
|Composant ou outil|Description|  
|-----------------------|-----------------|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] Programme d'installation|Sélectionnez le programme d'installation en anglais si vous souhaitez que l'application Web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] soit disponible et prise en charge dans d'autres langues que celle du programme d'installation. Pour plus d'informations, consultez la description du [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] ci-dessous.|  
|[!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)]|La langue d'installation détermine celle du [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] . Par exemple, si vous choisissez l'allemand comme langue d'installation, le [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)] est disponible en allemand sur cet ordinateur.|  
|[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]|Lorsque vous exécutez le programme d'installation en anglais, l'application Web [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] est disponible et prise en charge dans toutes les langues d'application. [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] peut être affiché dans n’importe quelle de ces langues d’application et accepter des données spécifiques aux paramètres régionaux basés sur les préférences linguistiques du navigateur web client. Si les préférences linguistiques sont configurées pour une langue d'application non prise en charge, la langue par défaut du [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] est l'anglais.<br /><br /> Lorsque vous exécutez le programme d'installation dans une langue autre que l'anglais, les ressources sont incluses pour toutes les autres langues de l'application, mais le fait que les clients utilisent [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] dans une langue autre que la langue d'installation sélectionnée n'est pas un scénario pris en charge. Si vous essayez d'accéder au [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] dans une langue différente de la langue d'installation, vous pouvez rencontrer des problèmes avec l'entrée et l'affichage des données dans l'application.|  
|[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] base de données|Les informations dans la base de données des [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] ne sont pas spécifiques à des paramètres régionaux. Cela permet au [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] de décider du mode d'affichage des informations, telles que les dates et nombres, au format déterminé par les préférences linguistiques du navigateur Web client.|  
  
## <a name="see-also"></a> Voir aussi  
 [Installer Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
  
