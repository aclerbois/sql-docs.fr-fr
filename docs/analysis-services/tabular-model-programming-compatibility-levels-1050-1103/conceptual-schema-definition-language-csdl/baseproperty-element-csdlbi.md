---
title: Élément BaseProperty (CSDLBI) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f1e14b3bcfe862083aa78d634ca20ecaa0f4dd5c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="baseproperty-element-csdlbi"></a>Élément BaseProperty (CSDLBI)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  L'élément BaseProperty est un type complexe qui sert de base aux autres éléments.  
  
 Ses attributs peuvent apparaître dans les colonnes et dans les mesures.  
  
## <a name="elements-and-attributes"></a>Éléments et attributs  
 Le tableau suivant répertorie les éléments et les attributs qui définissent l'élément BaseProperty.  
  
|Nom|Est obligatoire|Description|  
|----------|-----------------|-----------------|  
|Alignment|Non|Nom donné au membre (une colonne, une mesure, une propriété de navigation, une hiérarchie ou un niveau) défini par l'implémentation du type Member.|  
|FormatString|Non|Nom complet du membre.|  
|IsRightToLeft|Non|Valeur booléenne qui indique si le champ contient du texte qui doit être lu de droite à gauche.<br /><br /> Si cet attribut est omis, la valeur par défaut (du modèle) est utilisée.|  
|SortDirection|Non|Valeur qui indique comment les valeurs des champs sont généralement triées. Le contenu de cet attribut est défini par le type simple SortDirection.<br /><br /> En cas d'omission, un ordre de tri est affecté par défaut en fonction du type de données du champ.|  
|Unités|Non|Symbole qui est appliqué aux valeurs de champ pour exprimer des unités.<br /><br /> En cas d'omission, les unités sont inconnues.|  
  
## <a name="alignment-element"></a>Élément Alignment  
 Ce type simple définit le format d'affectation des noms qui est utilisé pour lever toute ambiguïté des membres.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Aucune|Utilise le nom de l'attribut.|  
|Contexte|Utilise le nom de la relation entrante.|  
|Fusion|Selon les règles de grammaire, concatène le nom de la relation entrante et le nom de l'attribut.|  
  
## <a name="sortdirection-element"></a>Élément SortDirection  
 Ce type simple définit le format d'affectation des noms qui est utilisé pour lever toute ambiguïté des membres.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Aucune|Utilise le nom de l'attribut.|  
|Contexte|Utilise le nom de la relation entrante.|  
|Fusion|Concatène le nom de la relation entrante et le nom de la propriété.|  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation du modèle d’objet tabulaire au niveau de compatibilité 1050 1103 via des niveaux](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/understanding-tabular-object-model-at-levels-1050-through-1103.md)  
  
  
