---
title: À propos de SQL Server Analysis Services | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: overview
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 704c2f1638676bd838c7aac367a1b610143fd85d
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707037"
---
# <a name="about-sql-server-analysis-services"></a>À propos de SQL Server Analysis Services

Analysis Services est un moteur de données analytiques utilisé dans analytique et de décision. Il fournit des modèles de données sémantique de niveau entreprise pour les rapports d’entreprise et les applications clientes, telles que Power BI, Excel, rapports Reporting Services et autres outils de visualisation de données.  

Un flux de travail typique comprend la création d’un projet de modèle de données tabulaire ou multidimensionnel dans Visual Studio, le déploiement du modèle comme base de données à une instance de serveur, configurer le traitement des données périodique et affectation d’autorisations d’accès aux données par les utilisateurs finaux. Lorsqu’il est prêt, votre modèle de données sémantique est accessible par les applications clientes prenant en charge d’Analysis Services comme source de données.  

Analysis Services est disponible dans deux plates-formes différentes : 

**Azure Analysis Services** -prend en charge les modèles tabulaires aux niveaux de compatibilité 1200 et supérieurs. DirectQuery, les partitions, la sécurité de niveau ligne, les relations bidirectionnelles et les traductions sont toutes prises en charge. Pour plus d’informations, consultez [Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/).

**SQL Server Analysis Services** -prend en charge les modèles tabulaires à tous les niveaux de compatibilité, les modèles multidimensionnels, exploration de données et PowerPivot pour SharePoint.
 
 ## <a name="documentation-by-area"></a>Documentation par domaine  
En général, [documentation Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/) est inclus dans la documentation sur Azure. Si vous êtes intéressé par la présence de vos modèles tabulaires dans le cloud, il est préférable de démarrer. Cet article et la documentation de cette section est principalement pour SQL Server Analysis Services. Toutefois, au moins pour les modèles tabulaires, comment créer et déployer vos projets de modèle tabulaire est similaire, quelle que soit la plateforme que vous utilisez. Consultez les sections suivantes pour en savoir plus :

   
*  [Comparaison des Solutions multidimensionnelles et tabulaires](../analysis-services/comparing-tabular-and-multidimensional-solutions-ssas.md)   
*  [Installer SQL Server Analysis Services](../analysis-services/instances/install-windows/install-analysis-services.md)
*  [Modèles tabulaires](../analysis-services/tabular-models/tabular-models-ssas.md)  
*  [Modèles multidimensionnels](../analysis-services/multidimensional-models/multidimensional-models-ssas.md)  
*  [Exploration de données](../analysis-services/data-mining/data-mining-ssas.md)  
*  [PowerPivot pour SharePoint](../analysis-services/power-pivot-sharepoint/power-pivot-for-sharepoint-ssas.md)  
*  [Didacticiels](../analysis-services/analysis-services-tutorials-ssas.md)   
*  [Gestion de serveur](../analysis-services/instances/analysis-services-instance-management.md)    
*  [Documentation pour développeurs](https://msdn.microsoft.com/library/bb500153(SQL.130).aspx)  
*  [Informations techniques de référence](../analysis-services/powershell/technical-reference-ssas.md)

Voir aussi

[Documentation d’Analysis Services Azure](https://docs.microsoft.com/azure/analysis-services/)   
[Documentation SQL Server](../sql-server/sql-server-technical-documentation.md)
