---
title: L’élément MemberRef (CSDLBI) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 79b6f2684dabcb45cfddac626bc2a6aa140298f3
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="memberref-element-csdlbi"></a>Élément MemberRef (CSDLBI)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  L'élément MemberRef spécifie le nom d'une propriété qui est la cible d'une référence.  
  
## <a name="elements-and-attributes"></a>Éléments et attributs  
 Le tableau suivant répertorie les éléments et les attributs qui définissent l'élément MemberRef.  
  
|Nom|Est obligatoire|Description|  
|----------|-----------------|-----------------|  
|Nom|Oui|Nom de la propriété contenue dans un élément MemberRef.|  
  
## <a name="memberrefs-element"></a>Élément MemberRefs  
 MemberRefs est un type complexe qui définit une collection de membres dans laquelle chaque membre est contenu dans un élément MemberRef.  
  
 Le tableau suivant répertorie les éléments et les attributs de type MemberRefs.  
  
|Nom|Est obligatoire|Description|  
|----------|-----------------|-----------------|  
|MemberRef|Oui|Chaîne contenant la référence de membre.|  
  
## <a name="example"></a>Exemple  
 **Sous forme de tableau**  
  
 L'exemple suivant, en CSDLBI version 1.1, représente une partie du modèle de données exemple AdventureWorks qui définit la table Products. L'élément MemberRef est utilisé pour chaque colonne incluse dans l'ensemble de champs par défaut du modèle.  
  
```  
  
<bi:EntityType>  
   <bi:DefaultDetails>  
     <bi:MemberRef Name="Color" />  
     <bi:MemberRef Name="DealerPrice" />  
     <bi:MemberRef Name="Status" />  
   </bi:DefaultDetails>  
  
```  
  
## <a name="example"></a>Exemple  
 **Multidimensionnel**  
  
 L'exemple suivant, en CSDLBI version 1.1, représente une partie du cube Contoso Operations qui définit la table Bikes. Dans cet exemple, un élément MemberRef est utilisé pour spécifier le nom de la colonne utilisée en tant qu'ensemble de champs par défaut dans les rapports et un autre élément MemberRef permet de référencer la colonne qui fournit l'ordre de tri.  
  
```  
  
<bi:DefaultDetails>  
   <bi:MemberRef Name="Color" />  
</bi:DefaultDetails>  
  
<bi:SortMembers>  
   <bi:MemberRef Name="Color" />  
</bi:SortMembers>  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Informations techniques de référence pour les Annotations BI au langage CSDL](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/technical-reference-for-bi-annotations-to-csdl.md)  
  
  
