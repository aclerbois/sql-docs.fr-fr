---
title: Sécurité au niveau de l’objet de modèle tabulaire | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 98caa08ef6c3dcba37043124d0263507097a4374
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="object-level-security"></a>Sécurité au niveau de l’objet
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
Sécurité de modèle de données commence par la mise en oeuvre efficace [rôles](../../analysis-services/tabular-models/roles-ssas-tabular.md) et des filtres au niveau des lignes pour définir les autorisations utilisateur sur les objets de modèle de données et des données. À partir de modèles tabulaires de 1400, vous pouvez également définir au niveau de l’objet de sécurité, qui inclut la sécurité au niveau de la table et la sécurité au niveau de la colonne dans la [objet rôles](../../analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl.md).

## <a name="table-level-security"></a>Sécurité au niveau des tables

Avec la sécurité au niveau de la table, vous pouvez restreindre pas uniquement l’accès aux données de table, mais également les noms de table sensibles, contribue à empêcher les utilisateurs malveillants de découvrir si une table existe. 

 Sécurité au niveau de la table est définie dans les métadonnées en fonction de JSON dans Model.bim, [TMSL Tabular Model Scripting Language ()](../../analysis-services/tabular-model-scripting-language-tmsl-reference.md), ou [le modèle d’objet tabulaire (TOM)](../../analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo.md). Définir le **metadataPermission** propriété de la **autorisations de table** classe dans le [objet rôles](../../analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl.md) à **aucun**.

Dans cet exemple, la propriété metadataPermission de la classe d’autorisations de table pour la table Product est définie sur none :

```
"roles": [
  {
    "name": "Users",
    "description": "All allowed users to query the model",
    "modelPermission": "read",
    "tablePermissions": [
      {
        "name": "Product",
        "metadataPermission": "none"
      }
    ]
  }
```

## <a name="column-level-security"></a>Sécurité au niveau de la colonne

Similaire à la sécurité au niveau de la table, avec une sécurité au niveau des colonnes vous pas uniquement restreignent l’accès aux données de la colonne, mais également les noms de colonne sensible, contribue à empêcher les utilisateurs malveillants à partir de la détection d’une colonne.

 Sécurité au niveau de la colonne est définie dans les métadonnées en fonction de JSON dans Model.bim, [TMSL Tabular Model Scripting Language ()](../../analysis-services/tabular-model-scripting-language-tmsl-reference.md), ou [le modèle d’objet tabulaire (TOM)](../../analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo.md). Définir le **metadataPermission** propriété de la **columnPermissions** classe dans le [objet rôles](../../analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl.md) à **aucun**.

Dans cet exemple, la propriété metadataPermission de la classe columnPermissions pour la colonne de taux de Base dans la table Employees est définie sur none :

```
"roles": [
  {
    "name": "Users",
    "description": "All allowed users to query the model",
    "modelPermission": "read",
    "tablePermissions": [
      {
        "name": "Employee",
        "columnPermissions": [
          {
            "name": "Base Rate",
            "metadataPermission": "none"
          }
        ]
      }
    ]
  }
```

## <a name="restrictions"></a>Restrictions

*  Impossible de définir la sécurité au niveau de la table pour un modèle si elle s’arrête une chaîne de relation. Une erreur est générée au moment du design.
 Par exemple, s’il existe des relations entre des tables A et B et B et C, vous ne pouvez pas sécuriser b. Si la table B est sécurisée, une requête sur la table A ne peut pas transit les relations entre la table A et B et B et C. Dans ce cas, une relation distincte peut être configurée entre les tables A et B.

    ![Sécurité au niveau des tables](../../analysis-services/tabular-models/media/ssas-ols.png)  


*  Sécurité de niveau ligne et la sécurité au niveau de l’objet ne peut pas être combinés à partir de différents rôles, car il peut introduire des accès involontaire à des données sécurisées. Une erreur est générée au moment de la requête pour les utilisateurs qui sont membres d’une telle combinaison des rôles.

*  Des calculs dynamiques (mesures, indicateurs de performance clés, DetailRows) sont automatiquement restreints si elles font référence à une table sécurisée ou une colonne. Il n’existe aucun mécanisme pour explicitement sécuriser une mesure, il est possible pour implicitement sécuriser une mesure en mettant à jour de l’expression pour faire référence à une table sécurisée ou une colonne.

*  Des relations qui font référence à une colonne sécurisée de travail fournie de la table dans la colonne n’est pas sécurisée.




## <a name="see-also"></a>Voir aussi  
[Roles](../../analysis-services/tabular-models/roles-ssas-tabular.md)  
[Roles, objet (TMSL)](../../analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl.md)  
[Langage TMSL (Tabular Model Scripting Language)](../../analysis-services/tabular-model-scripting-language-tmsl-reference.md)  
[Modèle d’objet tabulaire (TOM)](../../analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo.md).

  
