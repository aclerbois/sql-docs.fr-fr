---
title: Définir des requêtes nommées dans une vue de Source de données (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: bee48d9927e9caaea28fd201480e507e5cfa7d9d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="define-named-queries-in-a-data-source-view-analysis-services"></a>Définir des requêtes nommées dans une vue de source de données (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une requête nommée est une expression SQL représentée sous forme de table. Dans une requête nommée, vous pouvez spécifier une expression SQL pour sélectionner les lignes et les colonnes retournées d'une ou de plusieurs tables dans une ou plusieurs sources de données. Une requête nommée est similaire à toute autre table dans une vue de source de données (DSV) avec des lignes et des relations, si ce n'est que la requête nommée se base sur une expression.  
  
 Une requête nommée vous permet d'étendre le schéma relationnel des tables existantes dans une vue de source de données (DSV) sans modifier la source de données sous-jacente. Par exemple, une série de requêtes nommées peuvent être utilisées pour diviser une table de dimension complexe en tables plus petites et plus simples à utiliser dans les dimensions de base de données. Vous pouvez également utiliser une requête nommée pour joindre plusieurs tables de base de données d'une ou plusieurs sources de données dans une seule table de vue de source de données.  
  
## <a name="creating-a-named-query"></a>Création d'une requête nommée  
  
> [!NOTE]  
>  Vous ne pouvez pas ajouter un calcul nommé dans une requête nommée, ni baser une requête nommée sur une table contenant un calcul nommé.  
  
 Lorsque vous créez une requête nommée, vous spécifiez un nom, la requête SQL retournant les colonnes et les données pour la table, et en option une description de la requête nommée. L'expression SQL peut faire référence à d'autres tables dans la vue de source de données. Une fois la requête nommée définie, la requête SQL dans une requête nommée est envoyée au fournisseur de la source de données et validée dans sa globalité. Si le fournisseur ne trouve pas d'erreur dans la requête SQL, la colonne est ajoutée dans la table.  
  
 Les tables et les colonnes référencées dans la requête SQL ne doivent pas être qualifiées ou doivent être qualifiées par le nom de table uniquement. Par exemple, pour faire référence à la colonne SaleAmount dans une table, `SaleAmount` ou `Sales.SaleAmount` est valide, mais `dbo.Sales.SaleAmount` génère une erreur.  
  
 **Remarque** : quand vous définissez une requête nommée qui interroge une source de données [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0, cela a pour effet de faire échouer une requête nommée qui contient une sous-requête corrélée et une clause GROUP BY. Pour plus d’informations, consultez [Erreur interne avec l’instruction SELECT contenant une sous-requête corrélée et GROUP BY](http://support.microsoft.com/kb/274729) dans la Base de connaissances [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
## <a name="add-or-edit-a-named-query"></a>Ajouter ou modifier une requête nommée  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet ou connectez-vous à la base de données qui contient la vue de source de données dans laquelle vous voulez ajouter une requête nommée.  
  
2.  Dans l’Explorateur de solutions, développez le dossier **Vues des sources de données** , puis double-cliquez sur la vue de source de données.  
  
3.  Dans le volet **Tables** ou **Diagramme** , cliquez avec le bouton droit dans une zone ouverte, puis cliquez **Nouvelle requête nommée**.  
  
4.  Dans la boîte de dialogue **Créer une requête nommée** , procédez comme suit :  
  
    1.  Dans la zone de texte **Nom** , tapez un nom de requête.  
  
    2.  Si vous le souhaitez, dans la zone **Description** , tapez la description de la requête.  
  
    3.  Dans la zone de liste **Source de données** , sélectionnez la source de données dans laquelle la requête nommée sera exécutée.  
  
    4.  Tapez la requête dans le volet inférieur ou utilisez les outils graphiques de création de requêtes pour créer une requête.  
  
    > [!NOTE]  
    >  L'interface utilisateur de création de requêtes dépend de la source de données. Au lieu d'obtenir une interface utilisateur graphique, vous pouvez obtenir une interface utilisateur générique, basée sur du texte. Vous pouvez accomplir les mêmes tâches avec ces différentes interfaces utilisateur, mais vous devez procéder différemment. Pour plus d’informations, consultez [Boîte de dialogue Créer/Modifier la requête nommée &#40;Analysis Services - Données multidimensionnelles&#41;](http://msdn.microsoft.com/library/8e192ad6-a0b1-4e21-bb3f-087c93e62941).  
  
5.  Cliquez sur **OK**. Une icône représentant deux tables superposées apparaît dans l'en-tête de la table pour indiquer que la table a été remplacée par une requête nommée.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de sources de données dans les modèles multidimensionnels](../../analysis-services/multidimensional-models/data-source-views-in-multidimensional-models.md)   
 [Définir des calculs nommés dans une vue de Source de données & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/define-named-calculations-in-a-data-source-view-analysis-services.md)  
  
  
