---
title: Niveau de compatibilité d’une base de données multidimensionnelle (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d8f11bb819073ef054582a55620b553865469466
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="compatibility-level-of-a-multidimensional-database-analysis-services"></a>Niveau de compatibilité d’une base de données multidimensionnelle (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], la propriété du niveau de compatibilité de la base de données détermine le niveau fonctionnel d'une base de données. Les niveaux de compatibilité sont propres à chaque type de modèle. Par exemple, un niveau de compatibilité de **1 100** a une signification différente selon que la base de données est multidimensionnelle ou tabulaire.  
  
 Cette rubrique décrit le niveau de compatibilité des bases de données multidimensionnelles uniquement. Pour plus d’informations sur les solutions tabulaires, consultez [Niveau de compatibilité pour les modèles tabulaires dans Analysis Services](../../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md).  
  
> [!NOTE]  
>  Les modèles tabulaires possèdent des niveaux de compatibilité de base de données qui ne s'appliquent pas aux modèles multidimensionnels. Le niveau de compatibilité **1103** n'existe pas pour les modèles multidimensionnels. Consultez [What is new for the Tabular model in SQL Server 2012 SP1 and compatibility level](http://go.microsoft.com/fwlink/?LinkId=301727) (Nouveautés concernant le modèle tabulaire dans SQL Server 2012 SP1 et le niveau de compatibilité) pour plus d’informations sur **1103** pour les solutions tabulaires.  
  
 **Niveaux de compatibilité des bases de données multidimensionnelles**  
  
 Actuellement, le seul comportement de base de données multidimensionnelle qui varie selon le niveau de compatibilité est l'architecture de stockage de chaînes. En augmentant le niveau de compatibilité d'une base de données, vous pouvez dépasser la limite de 4 Go pour le stockage de chaînes de mesures et de dimensions.  
  
 Pour une base de données multidimensionnelle, les valeurs valides pour la propriété **CompatibilityLevel** sont les suivantes :  
  
|Paramètre| Description|  
|-------------|-----------------|  
|**1050**|Cette valeur n'est pas visible dans un script ou des outils, mais elle correspond aux bases de données créées dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]ou [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Toute base de données pour laquelle **CompatibilityLevel** n'est pas défini explicitement, s'exécute implicitement au niveau **1050** .|  
|**1 100**|C'est la valeur par défaut pour les nouvelles bases de données que vous créez dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Vous pouvez également la spécifier pour les bases de données créées dans les versions antérieures de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour permettre l'utilisation des fonctionnalités prises en charge uniquement à ce niveau de compatibilité (à savoir, le stockage amélioré des chaînes pour les attributs de dimension ou les mesures de comptage des valeurs qui contiennent les données de chaîne).<br /><br /> Les bases de données dont la valeur **CompatibilityLevel** est définie sur **1 100** comportent une propriété supplémentaire, **StringStoresCompatibilityLevel**, qui vous permet de choisir un autre stockage de chaînes pour les partitions et les dimensions.|  
  
> [!WARNING]  
>  La définition de la compatibilité de la base de données sur un niveau supérieur est irrévocable. Après avoir augmenté le niveau de compatibilité à **1 100**, vous devez continuer à exécuter la base de données sur des serveurs plus récents. Vous ne pouvez pas revenir au niveau **1 050**. Vous ne pouvez pas attacher ni restaurer une base de données **1 100** sur une version de serveur antérieure à [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="prerequisites"></a>Configuration requise  
 Les niveaux de compatibilité de la base de données ont été introduits dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. Vous devez disposer de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)][!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou ultérieur pour visualiser ou définir le niveau de compatibilité de la base de données.  
  
 La base de données ne peut pas être un cube local. Les cubes locaux ne prennent pas en charge la propriété **CompatibilityLevel** .  
  
 La base de données doit avoir été créée dans une version précédente (SQL Server 2008 R2 ou antérieure), puis attachée ou restaurée sur un serveur [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)][!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou plus récent. Les bases de données déployées vers SQL Server 2012 sont déjà au niveau **1100** et ne peuvent pas être déclassifiées pour s'exécuter à un niveau inférieur.  
  
## <a name="determine-the-existing-database-compatibility-level-for-a-multidimensional-database"></a>Déterminer le niveau de compatibilité de la base de données existant pour une base de données multidimensionnelle  
 La seule façon d'afficher ou modifier le niveau de compatibilité de la base de données est de passer par XMLA. Vous pouvez afficher ou modifier le script XMLA qui spécifie la base de données dans SQL Server Management Studio.  
  
 Si vous recherchez la définition XMLA d'une base de données pour la propriété **CompatibilityLevel** et qu'elle 'existe pas, vous disposez probablement d'une base de données au niveau de compatibilité **1050** .  
  
 Vous trouverez des instructions pour l'affichage et la modification du script XMLA dans la section suivante.  
  
## <a name="set-the-database-compatibility-level-in-sql-server-management-studio"></a>Définir le niveau de compatibilité de la base de données dans SQL Server Management Studio  
  
1.  Avant d'augmenter le niveau de compatibilité, sauvegardez la base de données au cas où vous souhaiteriez annuler les modifications apportées.  
  
2.  Connectez-vous au serveur [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)][!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui héberge la base de données à l’aide de SQL Server Management Studio.  
  
3.  Cliquez avec le bouton droit sur le nom de la base de données, pointez sur **Générer un script de la base de données en tant que**, sur **ALTER To**, puis sélectionnez **Nouvelle fenêtre d’éditeur de requête**. Une représentation XMLA de la base de données s'ouvre dans une nouvelle fenêtre.  
  
4.  Copiez l'élément XML suivant :  
  
    ```  
    <ddl200:CompatibilityLevel>1100</ddl200:CompatibilityLevel>  
    ```  
  
5.  Collez-le après l'élément de fin `</Annotations>` et avant l'élément `<Language>` . Le XML doit ressembler à l'exemple suivant :  
  
    ```  
    </Annotations>  
    <ddl200:CompatibilityLevel>1100</ddl200:CompatibilityLevel>  
    <Language>1033</Language>  
    ```  
  
6.  Enregistrez le fichier.  
  
7.  Pour exécuter le script, cliquez sur **Exécuter** dans le menu Requête ou appuyez sur F5.  
  
## <a name="supported-operations-that-require-the-same-compatibility-level"></a>Opérations prises en charge qui requièrent le même niveau de compatibilité  
 Les opérations suivantes requièrent que les bases de données sources partagent le même niveau de compatibilité.  
  
1.  La fusion de partitions de bases de données différentes est prise en charge uniquement si les deux bases de données partagent le même niveau de compatibilité.  
  
2.  L'utilisation de dimensions liées d'une autre base de données requiert le même niveau de compatibilité. Par exemple, si vous souhaitez utiliser une dimension liée d’une base de données [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] dans une base de données [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , vous devez déplacer la base de données [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] sur un serveur [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et définir le niveau de compatibilité sur **1 100**.  
  
3.  La synchronisation des serveurs est prise en charge uniquement pour les serveurs qui partagent la même version et le même niveau de compatibilité de base de données.  
  
## <a name="next-steps"></a>Étapes suivantes  
 Après avoir augmenté le niveau de compatibilité de la base de données, vous pouvez définir la propriété **StringStoresCompatibilityLevel** dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Cela augmente le stockage des chaînes de mesures et de dimensions. Pour plus d’informations sur cette fonctionnalité, consultez [Configurer le stockage de chaînes pour des dimensions et des partitions](../../analysis-services/multidimensional-models/configure-string-storage-for-dimensions-and-partitions.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Sauvegarde, restauration et synchronisation de bases de données &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/backing-up-restoring-and-synchronizing-databases-xmla.md)  
  
  
