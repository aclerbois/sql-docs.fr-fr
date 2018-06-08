---
title: Ajouter une agrégation personnalisée à une Dimension | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cbe5c4a1f043ccc8e7f442213b8b024a3920663e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="bi-wizard---add-a-custom-aggregation-to-a-dimension"></a>Assistant BI - ajouter une agrégation personnalisée à une Dimension
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Ajoutez une agrégation personnalisée à un cube ou à une dimension pour remplacer les agrégations par défaut qui sont associées à un membre de dimension avec un opérateur unaire différent. Cette amélioration spécifie une colonne d'opérateur unaire qui, dans la table de dimension, définit le cumul pour les membres d'une hiérarchie parent-enfant. L'opérateur unaire agit sur l'attribut parent dans une hiérarchie parent-enfant.  
  
> [!NOTE]  
>  Une agrégation personnalisée n'est disponible que pour les dimensions qui sont basées sur des sources de données existantes. Pour les dimensions créées sans utiliser de source de données, vous devez exécuter l'Assistant Génération de schéma pour créer une vue de source de données avant d'ajouter l'agrégation personnalisée.  
  
 Pour ajouter une agrégation personnalisée, vous utilisez l’Assistant Business Intelligence et vous sélectionnez l’option **Spécifier un opérateur unaire** dans la page **Choisir des améliorations** . Cet Assistant vous guide tout au long des étapes de sélection de la dimension à laquelle vous souhaitez appliquer une agrégation personnalisée et d'identification de l'agrégation personnalisée.  
  
> [!NOTE]  
>  Avant d'exécuter l'Assistant Business Intelligence pour ajouter une agrégation personnalisée, vérifiez que la dimension que vous voulez améliorer contient une hiérarchie d'attribut parent-enfant. Pour plus d’informations, consultez [Dimensions parent-enfant](../../analysis-services/multidimensional-models/parent-child-dimension.md).  
  
## <a name="selecting-a-dimension"></a>Sélection d'une dimension  
 Dans la première page **Spécifier un opérateur unaire** de l’Assistant, vous spécifiez la dimension à laquelle vous voulez appliquer une agrégation personnalisée. L'agrégation personnalisée ajoutée à la dimension sélectionnée provoquera des changements dans cette dimension. Ces modifications seront héritées par tous les cubes contenant la dimension sélectionnée.  
  
## <a name="adding-custom-aggregation-unary-operator"></a>Ajout d'une agrégation personnalisée (opérateur unaire)  
 Dans la seconde page **Spécifier un opérateur unaire** , vous spécifiez l’attribut parent souhaité pour l’agrégation personnalisée et la colonne source de la table de dimension pour l’opérateur unaire. La liste**Attribut parent** répertorie les attributs dont la propriété **Utilisation** a la valeur **Parent**. S'il y a plus d'un attribut parent, choisissez l'attribut parent qui correspond à la relation parent-enfant que vous voulez utiliser. Si la liste ne contient aucun attribut parent, la dimension ne possède pas de hiérarchie parent-enfant valide.  
  
 Dans **Colonne source**, vous sélectionnez la colonne de valeurs de chaîne qui contient les opérateurs unaires. (Cette sélection définit la propriété **UnaryOperatorColumn** sur l’attribut parent.) La table de dimension doit aussi avoir une colonne de valeurs de chaîne qui spécifie l'opérateur de cumul unaire. Les valeurs de chaîne de cette colonne doivent contenir des opérateurs d'agrégation valides. Si une ligne est vide, le membre correspondant est calculé normalement. Si la formule figurant dans une colonne n'est pas valide, une erreur se produit au moment de l'exécution lorsqu'une valeur de cellule utilisant le membre est extraite. Pour plus d’informations, consultez [Opérateurs unaires dans les dimensions parent-enfant](../../analysis-services/multidimensional-models/parent-child-dimension-attributes-unary-operators.md).  
  
  
