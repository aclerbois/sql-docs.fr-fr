---
title: Un rapport sur votre évaluation consolidée à l’aide de Power BI (données Assistant Migration SQL Server) | Documents Microsoft
ms.custom: ''
ms.date: 09/07/2017
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.suite: sql
ms.technology: dma
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
caps.latest.revision: ''
author: HJToland3
ms.author: jtoland
manager: craigg
ms.openlocfilehash: 0c7479a1e55d90d59fbcc289978b943a8cd3c195
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-on-your-consolidated-assessments-by-using-power-bi-data-migration-assistant"></a>Un rapport sur votre évaluation consolidée à l’aide de Power BI (Assistant Migration de données)

Cet article décrit comment créer un rapport Power BI pour les évaluations de migration consolidée.

Pour plus d’informations sur le regroupement des évaluations de la migration à l’aide de l’Assistant de Migration de données, consultez [consolider les rapports d’évaluation](../dma/dma-consolidatereports.md).

## <a name="sample-power-bi-reports"></a>Exemples de rapports Power BI

Vous pouvez télécharger des exemples de rapports Power BI pour les évaluations de migration consolidées à partir de ce [référentiel Github](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/data-migration-assistant).

Les rapports suivants sont inclus : 

- [Tableau de bord](#dashboard--details)

  Inclut des statistiques d’instantané et un rapport détaillé.

- [Local mise à niveau de disponibilité](#on-premises-upgrade-readiness--details)

  La source de données est la vue UpgradeSuccessRanking dans la base de données DMAReporting.  Ce rapport indique la réussite de mise à niveau de pourcentage pour vos bases de données évalués.

- [Parité des fonctionnalités sur site](#on-premise-feature-parity--details)

  Présente les recommandations de la fonctionnalité de la version de SQL Server cible.

- [Préparation de mise à niveau base de données SQL Azure](#azure-sql-db-upgrade-readiness--details)

  La source de données est la vue UpgradeSuccessRanking dans la base de données DMAReporting.  Ce rapport indique la réussite de mise à niveau de pourcentage pour les bases de données évaluée pour les migrations de la base de données SQL Azure.

- [Fonctionnalités de base de données SQL non pris en charge Azure](#azure-sql-db-unsupported-features--details)

  Affiche les fonctionnalités de vos bases de données existants qui ne sont pas pris en charge dans la base de données SQL Azure (V12).

Vous pouvez modifier ces rapports pour fonctionner avec votre environnement en modifiant la source de données dans Power BI. 

1. Sélectionnez la flèche déroulante à côté **modifier les requêtes**, puis sélectionnez **paramètres de source de données**.

   ![Modifier le menu de requêtes, les paramètres de source de données](../dma/media/DataSourceSettings.png)

1. Sélectionnez **modifier Source...** et entrez les valeurs de serveur et de base de données.

   ![Base de données, de serveur et de modifier la source](../dma/media/ChangeSource.png)

1. Sélectionnez **OK**, puis sélectionnez **fermer**.

1. Actualiser vos rapports.

   ![Actualiser le rapport Power BI](../dma/media/RefreshReport.png)

### <a name="dashboard-report"></a>Rapport de tableau de bord

![Rapport de tableau de bord](../dma/media/DashboardReport.png)

Le tableau de bord affiche les détails de toutes vos évaluations. Vous pouvez utiliser les segments sur le côté gauche de filtrer par instance ou base de données. Vous pouvez utiliser le graphique à barres pour explorer des catégories spécifiques pour voir où se situent les problèmes.

Pour Explorer, sélectionnez le cercle avec la flèche vers le bas dans l’angle supérieur droit du graphique à barres.

![Extraction de la catégorie](../dma/media/CategoryDrillDown.png)

La séquence d’extraction est définie comme indiqué dans l’image suivante (sous **axe**). Pour modifier la séquence, faites glisser les colonnes à l’ordre souhaité.

![Axe de graphique à barres, des visualisations](../dma/media/VisualizationsAxis.png)

Cette vue est encore plus efficace lorsque vous tout d’abord filtrez par une base de données spécifique, puis Explorer les problèmes de catégorie spécifique. Dans l’exemple suivant, la base de données des ressources humaines est sélectionné pour l’instance **SQL01** pour afficher tous les objets qui empêchent les migrations (modifications avec rupture).

![Dernières modifications de base de données des ressources humaines](../dma/media/BreakingChanges.png)

### <a name="on-premises-upgrade-readiness-report"></a>Local mise à niveau de disponibilité du rapport

![Local mise à niveau de disponibilité du rapport](../dma/media/OnPremisesUpgradeReadinessReport.png)

Ce rapport affiche un instantané de l’état de préparation de vos bases de données sont à migrer vers une version ultérieure de SQL Server. Les données de ce rapport proviennent de dbo. UpgradeSuccessFactor\_vue locale dans la base de données DMAReporting.

Le filtrage par instance et le nom de la base de données et à l’aide de cartes de score en haut, vous pouvez afficher les version de la base de données pourrait être migré en trop. Par exemple, si vous filtrez par la base de données AdventureWorks 2012, vous pouvez voir que la base de données est prêt à passer à toutes les versions de SQL Server répertoriées dans le rapport. Ceci est déterminé en garantissant qu'aucune modification avec rupture pour ce niveau de compatibilité et de la base de données.

![Facteur de réussite de mise à niveau pour la base de données AdventureWorks](../dma/media/UpgradeSuccessFactor.png)

### <a name="on-premises-feature-parity-report"></a>Rapport de la parité des fonctionnalités sur site

![Rapport de la parité des fonctionnalités sur site](../dma/media/OnPremisesFeatureParityReport.png)

Utilisez ce rapport pour mettre en évidence les nouvelles fonctionnalités qui peuvent être utilisées pour la base de données dans la version de SQL Server cible.

Lorsque vous sélectionnez une fonctionnalité dans le graphique en entonnoir, les données en bas met en évidence les objets qui sont affectés par la fonctionnalité. Dans l’exemple suivant, la **Stretch database pour des économies de stockage** fonctionnalité est sélectionnée, et une table est répertoriée qui peut tirer parti de cette fonctionnalité.

![Recommandation de fonctionnalité de base de données Stretch](../dma/media/FeatureRecommend_StretchDatabase.png)

### <a name="azure-sql-db-upgrade-readiness-report"></a>Rapport de vérification de la disponibilité de base de données SQL Azure

![Rapport de vérification de la disponibilité de base de données SQL Azure](../dma/media/AzureSQLDBUpgradeReadinessReport.png)

Ce rapport présente la disponibilité de base de données à migrer vers Azure SQL Database V12. Les données à partir de ce rapport proviennent de dbo. Vue UpgradeSuccessRanking dans la base de données DMAReporting.

### <a name="azure-features-parity-report"></a>Rapport de parité de fonctionnalités Azure

![Rapport de parité de fonctionnalités Azure](../dma/media/AzureFeaturesParityReport.png)

Utilisez ce rapport pour mettre en surbrillance le *des fonctionnalités au niveau de l’instance* qui ne sont pas pris en charge par Azure SQL Database V12.

Lorsque vous sélectionnez une fonctionnalité dans le graphique en entonnoir, les données dans la partie inférieure répertorient les instances et les fonctionnalités de base de données qui ne sont pas pris en charge. Dans l’exemple suivant, cette fonctionnalité est activée : **toujours sur la configuration du groupe de disponibilité n’est pas pris en charge dans la base de données SQL Azure**.  

![Toujours sur la fonctionnalité groupe de disponibilité](../dma/media/Feature_AlwaysOnAvailability.png)

 
### <a name="azure-sql-db-unsupported-features-report"></a>Rapport de fonctionnalités de base de données SQL non pris en charge Azure

![Rapport de fonctionnalités de base de données SQL non pris en charge Azure](../dma/media/AzureSQLDBUnsupportedFeaturesReport.png)

Ce rapport met en évidence les fonctionnalités qui ne sont pas pris en charge pour une donnée **base de données** lorsque la cible est la base de données SQL Azure (V12).

En filtrant par la valeur de nom et la fonctionnalité de base de données dans le graphique en entonnoir, vous pouvez afficher des détails sur la fonctionnalité non prise en charge. Les détails incluent des recommandations pour résoudre le problème et l’objet concerné.

Par exemple, le filtrage par la base de données DTC et **bases de données en lecture seule ne peut pas être mis à niveau**, vous pouvez voir une liste des objets qui sont affectés.

![Bases de données en lecture seule ne peut pas être mis à niveau du problème](../dma/media/ReadOnlyDatabases.png)

## <a name="see-also"></a>Voir aussi

[Vue d’ensemble de l’Assistant de Migration de données](../dma/dma-overview.md)

[Téléchargement de l’Assistant Migration de données](https://www.microsoft.com/download/details.aspx?id=53595)

[Téléchargement de BI Power](https://powerbi.microsoft.com/)
