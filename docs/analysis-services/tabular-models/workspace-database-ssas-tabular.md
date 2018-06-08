---
title: Base de données de l’espace de travail de SQL Server Data Tools | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 817c3b821fef5fe1c8dcfb539e93b9bf275ee5d9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="workspace-database"></a>Base de données d’espace de travail 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  La base de données de l'espace de travail de modèles tabulaires, utilisée lors de la création d'un modèle, est créée lorsque vous créez un projet de modèle tabulaire dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].
  
## <a name="specifying-a-workspace-instance"></a>Spécification d’une instance d’espace de travail  
  Quand vous créez un projet de modèle tabulaire dans SSDT, vous pouvez spécifier une instance d’Analysis Services à utiliser lors de la création de votre projet. À partir de la version de septembre 2016 (14.0.60918.0) de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], vous disposez de deux modes pour spécifier une instance d’espace de travail quand vous créez un projet de modèle tabulaire. 

**Espace de travail intégré** : utilise la propre instance d’Analysis Services interne de SSDT.

**Serveur d’espace de travail** : une base de données d’espace de travail est créée sur une instance d’Analysis Services explicite, souvent sur le même ordinateur que SSDT ou un autre ordinateur du même réseau.
  
### <a name="integrated-workspace"></a>Espace de travail intégré
Avec l’espace de travail intégré, une base de données de travail est créée en mémoire à l’aide de la propre instance d’Analysis Services implicite de SSDT. Le mode d’espace de travail intégré réduit considérablement la complexité de la création de projets tabulaires dans SSDT, car une installation distincte et explicite de SQL Server Analysis Services n’est pas nécessaire.

En utilisant le mode d’espace de travail intégré, le modèle tabulaire SSDT démarre dynamiquement sa propre instance de SSAS interne en arrière-plan et charge la base de données. Vous pouvez ajouter et afficher des tables, des colonnes et des données dans le Générateur de modèles. Si vous ajoutez d’autres tables, colonnes, relations, etc., vous modifiez la base de données d’espace de travail. Le mode d’espace de travail intégré ne modifie pas le fonctionnement du modèle tabulaire SSDT avec une base de données et un serveur d’espace de travail. Le changement réside dans l’emplacement où le modèle tabulaire SSDT héberge la base de données d’espace de travail.

Vous pouvez sélectionner le mode d’espace de travail intégré lors de la création d’un projet de modèle tabulaire dans SSDT.

![Mode d’espace de travail intégré SSAS](../../analysis-services/tabular-models/media/ssas-integrated-workspace-mode.png)

En utilisant les propriétés de base de données d’espace de travail et de serveur d’espace de travail pour model.bim, vous pouvez découvrir le nom de la base de données temporaire et le port TCP de l’instance de SSAS interne où le modèle tabulaire SSDT héberge la base de données. Vous pouvez vous connecter à la base de données d’espace de travail avec SSMS tant que le modèle tabulaire SSDT a chargé la base de données. Le paramètre Rétention de l’espace de travail spécifie que le modèle tabulaire SSDT conserve la base de données d’espace de travail sur le disque, mais plus en mémoire, une fois un projet de modèle fermé. Cela garantit une utilisation moindre de la mémoire que si le modèle était conservé en mémoire en permanence. Si vous souhaitez contrôler ces paramètres, affectez à la propriété du mode d’espace de travail intégré la valeur False, puis indiquez un serveur d’espace de travail explicite. Un serveur d’espace de travail explicite est également pertinent si les données que vous importez dans un modèle dépassent la capacité de mémoire de votre station de travail SSDT.

> [!NOTE]  
>  Lorsque vous utilisez le mode intégré espace de travail, l’instance Analysis Services locale est 64 bits, tandis que SSDT s’exécute dans l’environnement 32 bits de Visual Studio. Si vous vous connectez à des sources de données spéciales, veillez à installer les versions 32 bits et 64 bits des fournisseurs de données correspondants sur votre station de travail. Le fournisseur 64 bits est requis pour l’instance d’Analysis Services 64 bits et la version 32 bits est requise pour l’Assistant Importation de Table dans SSDT.

###  <a name="bkmk_overview"></a> Serveur d’espace de travail  
 Une base de données d'espace de travail est créée sur l'instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , spécifiée dans la propriété Serveur d'espace de travail, lorsque vous créez un projet Business Intelligence à l'aide de l'un des modèles de projet de modèle tabulaire de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Chaque projet de modèle tabulaire possède sa propre base de données d'espace de travail. Vous pouvez utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour afficher la base de données de l'espace de travail sur le serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Le nom de la base de données de l'espace de travail inclut le nom du projet, suivi d'un trait de soulignement, du nom d'utilisateur, d'un trait de soulignement et d'un identifiant GUID.  
  
 La base de données de l’espace de travail réside en mémoire tandis que le projet de modèle tabulaire est ouvert dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Lorsque vous fermez le projet, la base de données de l'espace de travail est maintenue en mémoire, stockée sur le disque et supprimée de la mémoire (valeur par défaut), ou supprimée de la mémoire, mais pas stockée sur un disque, comme déterminé par la propriété Rétention de l'espace de travail. Pour plus d'informations sur la propriété Rétention de l'espace de travail, consultez [Propriétés de la base de données de l'espace de travail](#bkmk_ws_prop) plus loin dans cette rubrique.  
  
 Après avoir ajouté des données à votre projet de modèle à l’aide de l’Assistant Importation de table ou des commandes Copier/Coller, quand vous affichez les tables, les colonnes et les données dans le Générateur de modèles, vous affichez la base de données d’espace de travail. Si vous ajoutez d’autres tables, colonnes, relations, etc., vous modifiez également la base de données de l’espace de travail.  
  
 Lorsque vous déployez un projet de modèle tabulaire, la base de données model déployée, qui est en fait une copie de la base de données de l'espace de travail, est créée sur l'instance de serveur Analysis Services spécifiée dans la propriété Serveur de déploiement. Pour plus d’informations sur la propriété de serveur de déploiement, consultez [propriétés de projet](../../analysis-services/tabular-models/project-properties-ssas-tabular.md).  
  
 La base de données model de l'espace de travail réside généralement sur localhost ou sur une instance nommée local d'un serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Vous pouvez utiliser une instance distante de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour héberger la base de données de l'espace de travail ; toutefois, cette configuration n'est pas recommandée en raison de la latence au cours des requêtes de données et d'autres restrictions. De façon optimale, l'instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui hébergera les bases de données de l'espace de travail se trouve sur le même ordinateur que [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. La création de projets de modèle sur le même ordinateur que l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui héberge la base de données de l'espace de travail peut améliorer les performances.  
  
 Les bases de données d'espace de travail distantes présentent les restrictions suivantes :  
  
-   Latence potentielle pendant les requêtes.  
  
-   La propriété Sauvegarde de données ne peut pas être définie sur **Sauvegarder sur disque**.  
  
-   Vous ne pouvez pas importer de données d’un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] en créant un projet de modèle tabulaire à l’aide du modèle de projet Importer à partir de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
  > [!IMPORTANT]  
>  Le niveau de compatibilité du modèle doit correspondre au serveur d’espace de travail.
  
> [!NOTE]  
>  Si l'une des tables de votre modèle contient un grand nombre de lignes, envisagez d'importer uniquement un sous-ensemble des données lors de la création du modèle. En important un sous-ensemble des données, vous pouvez réduire le temps de traitement et la consommation des ressources du serveur de base de données de l'espace de travail.  
  
> [!NOTE]  
>  La fenêtre d'aperçu dans la page Sélectionner des tables et des vues de l'Assistant Importation de table, la boîte de dialogue Modifier les propriétés de la table et la boîte de dialogue Gestionnaire de partitions affiche les tables, colonnes et lignes au niveau de la source de données, et peut ne pas afficher les mêmes tables, colonnes et lignes que la base de données de l'espace de travail.  
  
  
##  <a name="bkmk_ws_prop"></a> Propriétés de la base de données de l'espace de travail  
 Les propriétés de base de données d'espace de travail sont incluses dans les propriétés du modèle. Pour afficher les propriétés du modèle, dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], dans l' **Explorateur de solutions**, cliquez sur le fichier **Model.bim** . Les propriétés de modèle peuvent être configurées à l'aide de la fenêtre **Propriétés** . Les propriétés spécifiques de la base de données de l'espace de travail sont les suivantes :  
  
> [!NOTE]  
>  Les propriétés **Integrated Workspace Mode** (Mode d’espace de travail intégré),**Serveur d’espace de travail**, **Rétention de l’espace de travail** et **Sauvegarde des données** ont des paramètres par défaut qui sont appliqués quand vous créez un projet de modèle. Vous pouvez modifier les paramètres par défaut pour les nouveaux projets de modèle dans la page **Modélisation des données** dans les paramètres **Serveur d’analyse** de la boîte de dialogue Outils\Options. Ces propriétés, ainsi que d'autres, peuvent également être définies pour chaque projet de modèle dans la fenêtre **Propriétés** . La modification des paramètres par défaut ne s'applique pas aux projets de modèles déjà créés. Pour plus d’informations, consultez [configurer les propriétés de déploiement et de modélisation de données par défaut](../../analysis-services/tabular-models/configure-default-data-modeling-and-deployment-properties-ssas-tabular.md).  
  
|Propriété|Paramètre par défaut| Description|  
|--------------|---------------------|-----------------|  
|**Integrated Workspace Mode**|True, False|Si le mode d’espace de travail intégré est activé pour la base de données d’espace de travail quand le projet est créé, cette propriété a la valeur True. Si le mode **Serveur d’espace de travail** est sélectionné quand le projet est créé, cette propriété a la valeur False. | 
|**Base de données d’espace de travail**|Nom|Nom de la base de données d’espace de travail. Cette propriété ne peut pas être modifiée quand **Mode d’espace de travail intégré** a la valeur **True**.|  
|**Rétention de l'espace de travail**|Décharger de la mémoire|Spécifie comment une base de données d'espace de travail est conservée une fois que le projet de modèle a été fermé. Une base de données d'espace de travail inclut les métadonnées du modèle et des données importées. Dans certains cas, la base de données d'espace de travail peut être très volumineuse et consommer une grande quantité de mémoire. Par défaut, lorsque vous fermez un projet de modèle dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], la base de données de l'espace de travail est déchargée de la mémoire. Lors de la modification de ce paramètre, il est important de considérer vos ressources mémoire disponibles ainsi que la fréquence à laquelle vous projetez de travailler sur le projet de modèle. Ce paramètre de propriété a les options suivantes :<br /><br /> **Conserver en mémoire** - Indique de conserver la base de données de l’espace de travail en mémoire une fois un projet de modèle fermé. Cette option consomme davantage de mémoire ; toutefois, lors de l'ouverture d'un projet de modèle dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], moins de ressources sont consommées et la base de données de l'espace de travail se charge plus rapidement.<br /><br /> **Décharger de la mémoire** - Indique de conserver la base de données de l’espace de travail sur le disque, mais plus en mémoire, une fois un projet de modèle fermé. Cette option consomme moins de mémoire ; toutefois, lors de l’ouverture d’un projet de modèle dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], la base de données de l’espace de travail doit être à nouveau attachée ; des ressources supplémentaires sont consommées et le projet de modèle se charge plus lentement que si la base de données de l’espace de travail est conservée en mémoire. Utilisez cette option lorsque les ressources en mémoire sont limitées ou lorsque vous travaillez sur une base de données d'espace de travail distante.<br /><br /> **Supprimer l’espace de travail** - Indique de supprimer la base de données de l’espace de travail de la mémoire et de ne pas la garder sur le disque une fois le projet de modèle fermé. Cette option consomme moins de mémoire et d’espace de stockage ; toutefois, lors de l’ouverture d’un projet de modèle dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], des ressources supplémentaires sont consommées et le projet de modèle se charge plus lentement que si la base de données de l’espace de travail est conservée en mémoire ou sur le disque. Utilisez cette option uniquement si vous travaillez occasionnellement sur les projets de modèle.<br /><br /> Le paramètre par défaut de cette propriété peut être modifié dans la page **Modélisation des données** dans les paramètres **Serveur d’analyse** de la boîte de dialogue Outils\Options. Cette propriété ne peut pas être modifiée quand **Mode d’espace de travail intégré** a la valeur **True**.|  
|**Workspace Server**|localhost|Cette propriété indique le serveur par défaut qui sera utilisé pour héberger la base de données de l'espace de travail tandis que le projet de modèle est créé dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Toutes les instances disponibles d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui fonctionnent sur l'ordinateur local sont incluses dans la zone de liste.<br /><br /> Pour spécifier un autre serveur de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] (s'exécutant en mode tabulaire), tapez le nom de ce serveur. L'utilisateur connecté doit être un administrateur sur le serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .<br /><br /> Notez qu’il est recommandé de spécifier un serveur local [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] comme serveur d’espace de travail. Pour les bases de données d’espace de travail sur serveur distant, l’importation de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] n’est pas prise en charge, les données ne peuvent pas être sauvegardées localement et l’interface utilisateur peut connaître une latence pendant les requêtes.<br /><br /> Le paramètre par défaut de cette propriété peut être modifié dans la page Modélisation des données dans les paramètres [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de la boîte de dialogue Outils\Options. Cette propriété ne peut pas être modifiée quand **Mode d’espace de travail intégré** a la valeur **True**.|  
  
##  <a name="bkmk_use_ssms"></a> Utilisation de SSMS pour gérer la base de données de l'espace de travail  
 Vous pouvez utiliser SQL Server Management Studio (SSMS) pour vous connecter à un serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui héberge une base de données d’espace de travail. En général, aucune gestion de base de données de l'espace de travail n'est nécessaire ; le détachement ou la suppression d'une base de données de l'espace de travail constitue l'exception et cette opération doit être effectuée dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. N'utilisez pas [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour gérer la base de données de l'espace de travail alors que le projet est ouvert dans le générateur de modèles. Vous risqueriez de perdre des données.
   
## <a name="see-also"></a>Voir aussi  
[Propriétés d’un modèle](../../analysis-services/tabular-models/model-properties-ssas-tabular.md) 
  
  
