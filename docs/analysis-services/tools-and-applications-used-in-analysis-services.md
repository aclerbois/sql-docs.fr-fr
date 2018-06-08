---
title: Outils et applications utilisés dans Analysis Services | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e2ff1808739eff4aed9ad34bef5d512f7539b7a6
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="tools-and-applications-used-in-analysis-services"></a>Outils et applications utilisés dans Analysis Services
[!INCLUDE[ssas-appliesto-sqlas-aas](../includes/ssas-appliesto-sqlas-aas.md)]

  Trouvez les outils et les applications que vous avez besoin pour créer des modèles Analysis Services et la gestion des bases de données déploiement.  
  
## <a name="analysis-services-model-designers"></a>Concepteurs de modèles Analysis Services  
 Les modèles sont créés à l’aide de modèles de projet dans SQL Server Data Tools (SSDT), un interpréteur de commandes de Visual Studio. Modèles de projet fournissent des concepteurs de modèles pour créer les objets de modèle de données qui constituent une solution Analysis Services. SSDT est un téléchargement web gratuit mis à jour tous les mois.

 **[Télécharger SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)** 
  
 Les modèles présentent un paramètre de niveau de compatibilité qui détermine les fonctionnalités disponibles et la version d’Analysis Services exécutant le modèle.  Si vous pouvez spécifier qu'un niveau de compatibilité donné est déterminée en partie par le Générateur de modèles.  
  
 Modèles tabulaires qui utilisent les fonctionnalités les plus récentes, telles que les fichiers BIM en format JSON tabulaire et bidirectionnelles du filtrage croisé, doivent être créés ou mis à niveau au niveau de compatibilité 1200 ou supérieur.  
  
 Si vous avez besoin d’un niveau de compatibilité inférieur, peut-être parce que vous voulez déployer un modèle sur une version antérieure d’Analysis Services, vous pouvez toujours utiliser le Concepteur de modèle dans SSDT. Les versions plus récentes de l’outil prend en charge la création d’un type de modèle (tabulaire ou multidimensionnel), au niveau de compatibilité.   

## <a name="administrative-tools"></a>Outils d'administration  
  
 SQL Server Management Studio (SSMS) est l’outil d’administration principal pour toutes les fonctionnalités de SQL Server, y compris Analysis Services. SSMS est un téléchargement web gratuit mis à jour tous les mois. 
  
**[Télécharger SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)** 
  
 SSMS comprend les événements étendus (xEvents), qui fournit une alternative légère aux traces du Générateur de profils SQL Server utilisé pour l’activité d’analyse et de diagnostic des problèmes sur SQL Server 2016 et serveurs Azure Analysis Services. Pour en savoir plus, consultez [Monitor Analysis Services with SQL Server Extended Events](../analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events.md) .  
  
### <a name="sql-server-profiler"></a>SQL Server Profiler  
 Bien qu’il soit officiellement conseillé d’utiliser les événements xEvents à la place, SQL Server Profiler offre une méthode familière pour surveiller les connexions, l’exécution des requêtes MDX et les autres opérations de serveur. SQL Server Profiler est installé par défaut. Il peut s’avérer avec les applications SQL Server sur les applications dans Windows.  
  
### <a name="powershell"></a>PowerShell  
 Vous pouvez utiliser des commandes PowerShell pour effectuer de nombreuses tâches d'administration. Consultez [référence PowerShell](../analysis-services/powershell/analysis-services-powershell-reference.md) pour plus d’informations.  
  
### <a name="community-and-third-party-tools"></a>Communauté et outils tiers  
 Consultez la [page CodePlex consacrée à Analysis Services](http://sqlsrvanalysissrvcs.codeplex.com/) pour accéder à des exemples de code fournis par la Communauté. Les[forums](http://social.msdn.microsoft.com/Forums/sqlserver/home?forum=sqlanalysisservices) peuvent être utiles pour trouver des recommandations sur les outils tiers qui prennent en charge Analysis Services.  
  
## <a name="see-also"></a>Voir aussi  
 [Niveau de compatibilité d’une base de données multidimensionnelle](../analysis-services/multidimensional-models/compatibility-level-of-a-multidimensional-database-analysis-services.md)   
 [Niveau de compatibilité pour les modèles tabulaires](../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md)  
  
  
