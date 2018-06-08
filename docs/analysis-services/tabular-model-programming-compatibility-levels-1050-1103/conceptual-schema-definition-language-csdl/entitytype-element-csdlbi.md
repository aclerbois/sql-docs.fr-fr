---
title: Élément EntityType (CSDLBI) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 60d1b91ab417343a9dffe7e83520ebcd868c5d6d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="entitytype-element-csdlbi"></a>Élément EntityType (CSDLBI)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  L'élément **EntityType** est un type complexe qui représente la structure d'une entité de niveau supérieur, par exemple un client ou un tri, dans un modèle de données. L'élément **bi:EntityType** étend la définition de l'élément [EntityType](http://msdn.microsoft.com/library/bb399206.aspx) utilisé dans l' [infrastructure de données d'entités](http://msdn.microsoft.com/library/bb399567.aspx).  
  
 Un élément EntityType doit être spécifié pour chacune des entités qui sont incluses dans le modèle de données. Les sous-éléments de l'élément EntityType décrivent les colonnes et les mesures dans la table. Les relations entre les tables sont incluses dans **EntityContainer**.  
  
## <a name="elements-and-attributes"></a>Éléments et attributs  
 Le tableau suivant répertorie les éléments et les attributs qui définissent l'élément **EntityType** . Consultez également les attributs applicables à l'élément [EntityType](http://msdn.microsoft.com/library/bb399206.aspx) .  
  
|Nom|Est obligatoire|Description|  
|----------|-----------------|-----------------|  
|Sommaire|Non|Chaîne qui contient les types de données possibles dans une colonne. La valeur est dérivée de la valeur de DimensionAttributeTypeEnumType dans le modèle de données.<br /><br /> Si la valeur de DimensionAttributeTypeEnumType est « ExtendedType », la valeur de Contents est dérivée de l'élément ExtendedType de DimensionAttribute. Le client n'est pas nécessaire pour répondre à ces valeurs.|  
|DefaultDetails|Non|Liste des références de propriété, qui représentent l'ensemble de colonnes dans la table.<br /><br /> Consultez [élément DefaultDetails & #40 ; CSDLBI & #41 ; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/defaultdetails-element-csdlbi.md).|  
|DefaultImage|Non|Référence à une colonne qui contient l'image utilisée pour illustrer l'entité.<br /><br /> Dans les modèles multidimensionnels, cet élément correspond à un attribut binaire de l'attribut de dimension. Si cet attribut est présent, l'élément doit contenir un seul élément MemberRef.<br /><br /> Consultez [élément MemberRef & #40 ; CSDLBI & #41 ; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/memberref-element-csdlbi.md).|  
|DefaultMeasure|Non|Référence à une mesure dans l'entité qui doit être utilisée comme valeur par défaut durant les calculs sur l'entité. Si cet argument n'est pas spécifié, la valeur par défaut est SUM.<br /><br /> Consultez [élément MemberRef & #40 ; CSDLBI & #41 ; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/memberref-element-csdlbi.md).|  
|DisplayKey|Non|Liste de références à des colonnes ou aux fins de rôle, constituant un identificateur fort qui désigne une instance d'entité.<br /><br /> Consultez [élément DisplayKey & #40 ; CSDLBI & #41 ; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/displaykey-element-csdlbi.md).|  
|Hiérarchie|Non|Liste de hiérarchies dans le modèle.<br /><br /> Consultez [élément Hierarchy & #40 ; CSDLBI & #41 ; ](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/hierarchy-element-csdlbi.md).|  
|ReferenceName|Oui|Identificateur qui peut être utilisé pour référencer cette entité dans une requête DAX (Data Analysis Expressions).<br /><br /> Si cet attribut n'est pas présent, le nom complet du champ de l'entité est utilisé.|  
|SortMembers|Non|Liste de propriétés sur lesquelles opérer un tri. L'attribut SortDirection indique si l'ordre est croissant ou décroissant.|  
  
## <a name="contents-element"></a>Élément Contents  
 L'élément **Contents** est un type simple qui décrit le type de données de l'entité.  
  
 Le contenu de l'entité (colonne) peut prendre l'une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|Regular|Pas définie autrement.|  
|Time|Les attributs représentent des périodes de temps, telles que des années, des semestres, des trimestres, des mois ou des jours.|  
|Géographie (Geography)|Les attributs représentent des informations géographiques, telles que des villes ou des codes postaux.|  
|Organisation (Organization)|Les attributs représentent des informations organisationnelles, telles que des employés ou des filiales.|  
|BillOfMaterials|Les attributs représentent des informations d'inventaire ou de fabrication, telles que des listes de composants pour des produits.|  
|Comptes (Accounts)|Les attributs représentent un graphique de comptes utilisé à des fins de génération de rapports financiers.|  
|Clients (Customers)|Les attributs représentent des informations relatives à des clients ou des contacts.|  
|Produits (Products)|Les attributs représentent des informations relatives à des produits.|  
|Scénario|Les attributs représentent des informations de planification ou d'analyse stratégique.|  
|Quantitative|Les attributs représentent des informations quantitatives.|  
|Utilitaire|Les attributs représentent des informations diverses.|  
|Monétaire (Currency)|Contient des données et des métadonnées monétaires.|  
|Taux (Rates)|Les attributs représentent des informations relatives à des taux de devises.|  
|Canal (Channel)|Les attributs représentent des informations relatives à des canaux.|  
|Promotion|Les attributs représentent des informations de promotion commerciale.|  
  
## <a name="example"></a>Exemple  
 **Sous forme de tableau**  
  
 L'exemple suivant illustre une partie de la représentation de CSDLBI version 1.1 de la table Geography utilisée dans le modèle tabulaire AdventureWorks. La colonne RowNumber est une colonne masquée générée automatiquement comme identificateur de ligne dans les modèles tabulaires, et par conséquent elle possède l'attribut Contents, **RowNumber**.  
  
```  
  
<EntityType   
     Name="DimGeography">  
     <Key>  
        <PropertyRef Name="RowNumber" />  
     </Key>  
     <Property   
        Name="RowNumber"   
        Type="Int64" Nullable="false">  
     <bi:Property   
        Hidden="true"   
        Contents="RowNumber"   
        Stability="RowNumber" />  
     </Property>  
....  
  
```  
  
## <a name="example"></a>Exemple  
 **Multidimensionnel**  
  
 L'exemple suivant affiche les éléments EntityType dans CSDLBI version 1.1 qui représentent une partie d'une dimension de temps du cube Contoso Operations.  
  
```  
<EntityType   
       Name="CalendarQuarter">  
    <Key>  
       <PropertyRef Name="RowNumber" />  
    </Key>  
  
    <Property Name="RowNumber"   
       Type="Int64"   
       Nullable="false">  
    <bi:Property   
       Hidden="true"   
       Contents="RowNumber"   
       Stability="RowNumber"   
    />  
    </Property>  
  
    <Property Name="CalendarQuarter2"   
       Type="String"   
       MaxLength="Max"   
       Unicode="true"   
       FixedLength="false"   
       Nullable="false">  
    <bi:Property   
       Caption="CalendarQuarter"   
       ReferenceName="CalendarQuarter"   
    />  
    </Property>  
   <bi:EntityType />  
</EntityType>  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Informations techniques de référence pour les Annotations BI au langage CSDL](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/technical-reference-for-bi-annotations-to-csdl.md)  
  
  
