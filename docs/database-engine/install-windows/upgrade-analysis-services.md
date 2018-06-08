---
title: Mettre à niveau Analysis Services | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- upgrading databases
- databases [Analysis Services], upgrading
- installing Analysis Services, side-by-side installations
- Analysis Services upgrades
- Analysis Services upgrades, about upgrading Analysis Services
- SSAS, database migration
- upgrading Analysis Services
- installing Analysis Services, upgrading
- SSAS, upgrading
ms.assetid: a131d329-386e-4470-aaa9-ffcde4e5ec0c
caps.latest.revision: 79
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 235ed4172275df440f86df47d72f9eed0a1f0a5d
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34772585"
---
# <a name="upgrade-analysis-services"></a>Mettre à niveau Analysis Services

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
  
  Les instances d’Analysis Services peuvent être mises à niveau vers une version SQL Server du même mode serveur pour tirer parti des fonctionnalités introduites dans la version actuelle, comme cela est décrit dans [Nouveautés d’Analysis Services](../../analysis-services/what-s-new-in-analysis-services.md).  
  
 Vous pouvez mettre à niveau chaque instance sur place, indépendamment des autres instances exécutées sur le même matériel. Toutefois, la plupart des administrateurs choisissent d’installer une nouvelle instance de la nouvelle version pour pouvoir tester l’application avant de transférer les charges de travail de production vers le nouveau serveur. Une mise à niveau sur place peut cependant être plus adaptée pour les serveurs de développement ou de test.  
  
## <a name="server-upgrade"></a>Mise à niveau d’un serveur  
 Il existe deux approches de base pour mettre à niveau les serveurs et les bases de données :  
  
> [!NOTE]
> Les niveaux de compatibilité des bases de données attachées à un serveur donné restent inchangés, sauf si vous les modifiez manuellement.
   
  
### <a name="in-place-upgrade"></a>Mise à niveau sur place  
 Le processus de mise à niveau migre automatiquement les bases de données existantes de l’instance antérieure vers la nouvelle instance. Comme les métadonnées et les données binaires sont compatibles entre les deux versions, vous conserverez les données après la mise à niveau et vous n'avez pas à migrer les données manuellement.  
  
 Pour mettre à niveau une instance existante, exécutez le programme d'installation et spécifiez le nom de l'instance existante comme nom de la nouvelle instance.  
  
### <a name="side-by-side-upgrade"></a>Mise à niveau côte à côte  
  
-   Sauvegardez toutes les bases de données et vérifiez que chacune d’elles peut être restaurée. Pour en savoir plus, consultez [Sauvegarde et restauration de bases de données Analysis Services](../../analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases.md).  
  
-   Identifiez un sous-ensemble de rapports, de feuilles de calcul ou d’instantanés de tableaux de bord à utiliser plus tard comme base pour confirmer les opérations de serveur après la mise à niveau. Si possible, collectez les mesures de performances pour pouvoir effectuer des comparaisons avec les mêmes charges de travail sur un serveur mis à niveau.  
  
-   Installez une nouvelle instance d’Analysis Services en choisissant le même mode serveur (tabulaire ou multidimensionnel) que le serveur à remplacer. 
  
     Suivez les tâches consécutives à l’installation pour configurer les ports et ajouter des administrateurs de serveur. Pour en savoir plus, consultez [Configuration consécutive à l’installation &#40;Analysis Services&#41;](../../analysis-services/instances/post-install-configuration-analysis-services.md).  
  
-   Attachez ou restaurez chaque base de données.  
  
-   Exécutez DBCC pour vérifier l’intégrité des bases de données. Les modèles tabulaires sont soumis à une vérification plus approfondie, avec des tests pour les objets orphelins dans l’ensemble de la hiérarchie du modèle. Pour les modèles multidimensionnels, seuls les index de partition sont vérifiés. Pour en savoir plus, consultez [DBCC &#40;Database Consistency Checker&#41; pour les bases de données multidimensionnelles et tabulaires Analysis Services](../../analysis-services/instances/database-consistency-checker-dbcc-for-analysis-services.md).  
  
-   Testez les rapports, feuilles de calcul et tableaux de bord pour vérifier que le comportement ou les calculs n’ont subi aucune modification indésirable. Vous devriez constater une amélioration des performances pour les charges de travail multidimensionnelles et tabulaires.  
  
-   Testez les opérations de traitement et corrigez les éventuels problèmes de connexion ou d’autorisation. Si vous utilisez un compte de service par défaut pour les connexions, le nouveau service s’exécute sous un compte différent. Pour en savoir plus, consultez [Configurer les comptes de service &#40;Analysis Services&#41;](../../analysis-services/instances/configure-service-accounts-analysis-services.md).  
  
-   Testez les opérations de sauvegarde et de restauration sur le serveur mis à niveau, en modifiant les scripts pour qu’ils utilisent le nom du nouveau serveur.  
  
## <a name="database-upgrade"></a>Mise à niveau des bases de données  
 Les bases de données ayant été créées dans des versions antérieures s’exécutent sur le serveur mis à niveau avec le niveau de compatibilité défini initialement. En général, vous pouvez mettre à niveau une base de données ou un modèle pour améliorer son niveau de compatibilité afin d’accéder à de nouvelles fonctionnalités, mais sachez que cela vous oblige à utiliser une version spécifique du serveur.  
  
 En général, pour mettre à niveau une base de données, vous mettez à niveau le modèle dans SQL Server Data Tools (SSDT), puis déployez la solution vers une instance de serveur mise à niveau.
  
 Les bases de données tabulaires et multidimensionnelles suivent des chemins d’accès de version différents. Des modèles multidimensionnels et tabulaires peuvent avoir par coïncidence un niveau de compatibilité similaire.  Les modes avancent à des vitesses différentes si les modifications de fonctionnalités n’affectent que l’un d’eux.  
  
 Le tableau suivant présente simplement les niveaux de compatibilité. Nous vous recommandons toutefois de consulter les articles détaillés pour savoir ce que fournit chaque niveau.  
  
||||  
|-|-|-|  
|Tabulaire|1400|SQL Server 2017|
|Tabulaire|1200|SQL Server 2016|  
|Tabulaire|1103|SQL Server 2014|  
|Tabulaire|1100|SQL Server 2012|  
|(Multidimensionnel)|1100|SQL Server 2012 et versions ultérieures|  
|(Multidimensionnel)|1050|SQL Server 2005, 2008, 2008 R2|  
  
 Pour plus d’informations, consultez [Niveau de compatibilité d’une base de données multidimensionnelle &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/compatibility-level-of-a-multidimensional-database-analysis-services.md) et [Niveau de compatibilité pour les modèles tabulaires Analysis Services](../../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Planification d’une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md)   
 [Mettre à niveau PowerPivot pour SharePoint](../../database-engine/install-windows/upgrade-power-pivot-for-sharepoint.md)   
  
  
