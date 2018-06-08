---
title: Masquer ou figer des colonnes | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e588e16549a609fe15f0f7d7eaf89a010a5bde2c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="hide-or-freeze-columns"></a>Masquer ou figer des colonnes 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Dans le générateur de modèles, s'il existe des colonnes que vous ne souhaitez pas afficher dans une table, vous pouvez les masquer temporairement. Masquer une colonne vous permet de disposer de plus d'espace à l'écran pour ajouter de nouvelles colonnes ou de travailler uniquement avec les colonnes contenant des données pertinentes. Vous pouvez masquer et afficher des colonnes à partir du menu **Colonne** dans le générateur de modèles ou à partir du menu contextuel disponible dans chaque en-tête de colonne. Pour garder une zone d'un modèle visible pendant que vous faites défiler le modèle jusqu'à une autre zone, vous pouvez verrouiller des colonnes spécifiques dans une zone en les figeant.  
  
> [!IMPORTANT]  
>  La possibilité de masquer des colonnes n'est pas liée à la sécurité des données, mais sert uniquement à simplifier et raccourcir la liste de colonnes visibles dans le générateur de modèles ou les rapports. Pour sécuriser des données, vous pouvez définir des rôles de sécurité. Les rôles peuvent limiter les métadonnées visibles et les données aux seuls objets définis dans le rôle. Pour plus d’informations, consultez [rôles](../../analysis-services/tabular-models/roles-ssas-tabular.md).  
  
 Quand vous masquez une colonne, vous pouvez masquer la colonne pendant que vous travaillez dans le générateur de modèles ou dans les rapports. Si vous masquez toutes les colonnes, la table entière apparaît vide dans le générateur de modèles.  
  
> [!NOTE]  
>  Si vous avez de nombreuses colonnes à masquer, vous pouvez créer une perspective au lieu de masquer et d'afficher des colonnes. Une perspective est une vue personnalisée des données qui simplifie l'utilisation d'un sous-ensemble de données associées. Pour plus d’informations, consultez [créer et gérer des Perspectives](../../analysis-services/tabular-models/create-and-manage-perspectives-ssas-tabular.md)  
  
### <a name="to-hide-an-individual-column"></a>Pour masquer une colonne individuelle  
  
1.  Dans le générateur de modèles, sélectionnez la table contenant la colonne à masquer.  
  
2.  Cliquez avec le bouton droit sur la colonne, puis sélectionnez **Masquer les colonnes**, puis cliquez sur **Depuis le concepteur et les rapports**, **Depuis les rapports**ou **Depuis le concepteur**.  
  
### <a name="to-hide-multiple-columns"></a>Pour masquer plusieurs colonnes  
  
1.  Dans le générateur de modèles, sélectionnez la table contenant les colonnes à masquer.  
  
2.  Cliquez sur le menu **Colonnes** , puis sur **Masquer et afficher.**  
  
3.  Dans la boîte de dialogue **Masquer et afficher les colonnes** , recherchez le nom de chaque colonne que vous souhaitez masquer, puis désélectionnez **Dans le concepteur** et/ou **Dans les rapports**.  
  
4.  Cliquez sur **OK**.  
  
### <a name="to-freeze-columns"></a>Pour figer des colonnes  
  
1.  Dans le générateur de modèles, sélectionnez la table contenant les colonnes à figer.  
  
2.  Sélectionnez une ou plusieurs colonnes à figer.  
  
3.  Cliquez sur le menu **Colonnes** , puis sur **Figer**.  
  
    > [!NOTE]  
    >  Lorsqu'une colonne est figée, elle est déplacée vers la gauche (ou à l'avant) de la table dans le concepteur. Libérer la colonne ne la replace pas à son emplacement d'origine.  
  
## <a name="see-also"></a>Voir aussi  
 [Tables et colonnes](../../analysis-services/tabular-models/tables-and-columns-ssas-tabular.md)   
 [Perspectives](../../analysis-services/tabular-models/perspectives-ssas-tabular.md)   
 [Roles](../../analysis-services/tabular-models/roles-ssas-tabular.md)  
  
  
