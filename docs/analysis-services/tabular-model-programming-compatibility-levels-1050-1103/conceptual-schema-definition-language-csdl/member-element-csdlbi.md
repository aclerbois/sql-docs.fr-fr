---
title: Élément Member (CSDLBI) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 39c8271c188d22cf6246e6c6c969b4a3d224b3e8
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="member-element-csdlbi"></a>Élément Member (CSDLBI)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  L'élément Member est un type complexe qui sert de base aux autres éléments.  
  
 Ses attributs s'affichent dans les colonnes, les mesures, les propriétés de navigation, les hiérarchies et les niveaux.  
  
## <a name="elements-and-attributes"></a>Éléments et attributs  
 Le tableau suivant répertorie les éléments et les attributs qui définissent l'élément Member.  
  
|Nom|Est obligatoire|Description|  
|----------|-----------------|-----------------|  
|Nom||Nom donné au membre (une colonne, une mesure, une propriété de navigation, une hiérarchie ou un niveau) défini par l'implémentation du type TMember|  
|Légende|Oui|Nom complet du membre.|  
|ContextualNameRule|Oui|Format d'affectation des noms qui est utilisé pour lever toute ambiguïté des membres. Le contenu de cet attribut est défini par le type simple ContextualNameRule.|  
|Hidden||Valeur booléenne qui indique si le membre est caché dans le client.<br /><br /> La valeur par défaut est False, ce qui signifie que les colonnes sont visibles dans le client.|  
|ReferenceName||Identificateur utilisé pour référencer le membre dans une requête DAX. Si cet attribut est omis, le nom du champ est utilisé.|  
  
## <a name="contextualnamerule-element"></a>Élément ContextualNameRule  
 Ce type simple définit le format d'affectation des noms qui est utilisé pour lever toute ambiguïté des membres.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Aucune|Utilise le nom de l'attribut.|  
|Contexte|Utilise le nom de la relation entrante.|  
|Fusion|Concatène le nom de la relation entrante et le nom de la propriété.|  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation du modèle d’objet tabulaire au niveau de compatibilité 1050 1103 via des niveaux](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/representation/understanding-tabular-object-model-at-levels-1050-through-1103.md)  
  
  
