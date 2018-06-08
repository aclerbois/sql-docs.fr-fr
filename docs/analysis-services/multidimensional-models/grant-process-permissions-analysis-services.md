---
title: Accorder des autorisations de traitement (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c855bf2ecc14490b2298cf1fa240509a07369cf1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="grant-process-permissions-analysis-services"></a>Octroyer des autorisations de traitement (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  En tant qu'administrateur, vous pouvez créer un rôle dédié aux opérations de traitement Analysis Services, ce qui vous permet de déléguer cette tâche spécifique à d'autres utilisateurs ou à des applications utilisées pour le traitement planifié sans assistance. Les autorisations de traitement peuvent être accordées au niveau de la base de données, du cube, de la dimension et de la structure d'exploration de données. À moins de travailler sur une base de données de cube ou tabulaire très grande, nous vous recommandons d'accorder des droits de traitement au niveau de la base de données, y compris à tous les objets (même à ceux ayant des dépendances les uns envers les autres).  
  
 Les autorisations sont accordées par le biais de rôles qui associent des objets à des autorisations et des comptes d'utilisateurs ou de groupes Windows. N'oubliez pas que les autorisations s'ajoutent les unes aux autres. Si un rôle accorde l'autorisation de traiter un cube tandis qu'un second rôle accorde au même utilisateur l'autorisation de traiter une dimension, les autorisations des deux rôles différents se combinent pour accorder à l'utilisateur l'autorisation de traiter le cube et de traiter la dimension spécifiée dans cette base de données.  
  
> [!IMPORTANT]  
>  Un utilisateur dont le rôle ne dispose que d’autorisations de traitement ne peut pas utiliser [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour se connecter à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et traiter des objets. Ces outils nécessitent l’autorisation **Lire la définition** pour accéder aux métadonnées des objets. Sans la capacité à utiliser l'un de ces outils, vous devez utiliser le script XMLA pour exécuter une opération de traitement.  
>   
>  Nous vous suggérons également d’accorder des autorisations **Lire la définition** à des fins de test. Un utilisateur disposant des autorisations **Lire la définition** et **Traiter la base de données** peut traiter des objets dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]de manière interactive. Pour plus d’informations, consultez [Octroyer des autorisations Lire la définition sur des métadonnées d’objets &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-read-definition-permissions-on-object-metadata-analysis-services.md) .  
  
## <a name="set-processing-permissions-at-the-database-level"></a>Définir les autorisations de traitement au niveau de la base de données  
 Cette section explique comment les non-administrateurs peuvent activer le traitement de tous les cubes, dimensions, structures d'exploration et modèles d'exploration dans la base de données.  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l’instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], ouvrez le dossier Bases de données et sélectionnez une base de données.  
  
2.  Cliquez avec le bouton droit sur **Rôles** | **Nouveau rôle**. Entrez un nom et une description.  
  
3.  Dans le volet **Général** , cochez la case **Traiter la base de données** . Sélectionnez **Lire la définition** pour activer également le traitement interactif par l’intermédiaire de l’un des outils SQL Server, tels que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
4.  Dans le volet **Appartenance** , ajoutez les comptes d’utilisateurs et de groupes Windows ayant l’autorisation de traiter tout objet dans cette base de données.  
  
5.  Cliquez sur **OK** pour terminer la définition du rôle.  
  
## <a name="set-processing-permissions-on-individual-objects"></a>Définir des autorisations de traitement sur des objets spécifiques  
 Vous pouvez définir des autorisations de traitement sur des cubes, des dimensions, des structures d'exploration de données ou des modèles spécifiques.  
  
 Le traitement peut échouer si vous excluez par inadvertance des objets qui doivent être traités ensemble (par exemple si vous activez le traitement d'un cube mais pas de ses dimensions associées). Comme il est facile d'oublier certaines dépendances d'objets, vous devez impérativement effectuer des tests rigoureux lors de la définition des autorisations sur des objets spécifiques.  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l’instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], ouvrez le dossier Bases de données et sélectionnez une base de données.  
  
2.  Cliquez avec le bouton droit sur **Rôles** | **Nouveau rôle**. Entrez un nom et une description.  
  
3.  Dans le volet **Général** , décochez la case **Traiter la base de données** . Les autorisations de base de données outrepassent la capacité à définir des autorisations sur des objets de niveau inférieur en grisant ou en rendant non sélectionnables des options de rôle.  
  
     Techniquement, aucune autorisation de base de données n'est nécessaire pour les rôles de traitement dédiés. Mais sans **Lire la définition** au niveau de la base de données, vous ne pouvez pas afficher la base de données dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ce qui rend plus difficile l’exécution des tests.  
  
4.  Sélectionnez les objets à traiter :  
  
    -   Dans le volet **Cubes** , cochez la case **Traiter** correspondant à chaque cube.  
  
    -   Dans le volet **Dimensions** , sélectionnez **Toutes les dimensions de la base de données**, puis **Traiter** pour chaque dimension. Ou sélectionnez toutes les lignes, puis utilisez Maj+clic pour basculer entre les sélections de case à cocher.  
  
5.  Dans le volet **Appartenance** , ajoutez les comptes d’utilisateurs et de groupes Windows ayant l’autorisation de traiter ces objets.  
  
6.  Cliquez sur **OK** pour terminer la définition du rôle.  
  
## <a name="test-processing"></a>Tester le traitement  
  
1.  Maintenez enfoncée la touche Maj et cliquez avec le bouton droit sur [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], sélectionnez **Exécuter en tant qu’autre utilisateur** et connectez-vous à l’instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l’aide d’un compte Windows assigné au rôle que vous testez.  
  
2.  Ouvrez le dossier Bases de données et sélectionnez une base de données. Seules les bases de données accessibles aux rôles auxquels votre compte a une appartenance seront visibles.  
  
3.  Cliquez avec le bouton droit sur un cube ou une dimension et sélectionnez **Traiter**. Choisissez une option de traitement. Testez toutes les options pour toutes les combinaisons d'objets. Si des erreurs dues à des objets manquants se produisent, ajoutez les objets au rôle.  
  
## <a name="set-processing-permissions-on-a-data-mining-structure"></a>Définir des autorisations de traitement sur une structure d'exploration de données  
 Vous pouvez créer un rôle accordant l'autorisation de traiter des structures d'exploration de données. Ceci comprend le traitement de tous les modèles d'exploration de données.  
  
 Les autorisations **Extraire** et **Lire la définition** utilisées pour parcourir une structure et un modèle d'exploration de données sont atomiques et peuvent être ajoutées au même rôle ou réparties dans des rôles différents.  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l’instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], ouvrez le dossier Bases de données et sélectionnez une base de données.  
  
2.  Cliquez avec le bouton droit sur **Rôles** | **Nouveau rôle**. Entrez un nom et une description. Dans le volet **Général** , vérifiez que les cases d’autorisations de bases de données sont décochées. Les autorisations de base de données outrepassent la capacité à définir des autorisations sur des objets de niveau inférieur en grisant ou en rendant non sélectionnables des options de rôle.  
  
3.  Dans le volet **Structures d’exploration de données** , cochez la case **Traiter** correspondant à chaque structure d’exploration de données.  
  
4.  Dans le volet **Appartenance** , ajoutez les comptes d’utilisateurs et de groupes Windows ayant l’autorisation de traiter tout objet dans cette base de données.  
  
5.  Cliquez sur **OK** pour terminer la définition du rôle.  
  
## <a name="see-also"></a>Voir aussi  
 [Traiter une base de données, une table ou une partition &#40;Analysis Services&#41;](../../analysis-services/tabular-models/process-database-table-or-partition-analysis-services.md)   
 [Traitement d’un modèle multidimensionnel &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services.md)   
 [Accorder des autorisations de base de données & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/grant-database-permissions-analysis-services.md)   
 [Grant autorisations Lire la définition de métadonnées d’objet & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/grant-read-definition-permissions-on-object-metadata-analysis-services.md)  
  
  
