---
title: Type de données DimensionAttribute (ASSL) | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: assl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ecc12dfb1b3ed5c3f8d7ee10bb00e570b779b935
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dimensionattribute-data-type-assl"></a>Type de données DimensionAttribute (ASSL)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Définit un type de données primitif représentant un attribut dans une dimension.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<DimensionAttribute>  
   <Name>...</Name>  
   <ID>...</ID>  
   <Description>...</Description>  
   <Type>...</Type>  
   <Usage>...</Usage>  
   <Source>...</Source>  
   <EstimatedCount>...</EstimatedCount>  
   <KeyColumns>...</KeyColumns>  
   <NameColumn>...</NameColumn>  
   <ValueColumn>...</ValueColumn>  
   <Translations>...</Translations>  
   <AttributeRelationships>...</AttributeRelationships>  
   <DiscretizationMethod>...</DiscretizationMethod>  
   <DiscretizationBucketCount>...</DiscretizationBucketCount>  
   <RootMemberIf>...</RootMemberIf>  
   <OrderBy>...</OrderBy>  
   <DefaultMember>...</DefaultMember>  
   <OrderByAttributeID>...</OrderByAttributeID>  
   <SkippedLevelsColumn>...</SkippedLevelsColumn>  
   <NamingTemplate>...</NamingTemplate>  
   <MembersWithData>...</MembersWithData>  
   <MembersWithDataCaption>...</MembersWithDataCaption>  
   <NamingTemplateTranslations>...</NamingTemplateTranslations>  
   <CustomRollupColumn>...</CustomRollupColumn>  
   <CustomRollupPropertiesColumn>...</CustomRollupPropertiesColumn>  
   <UnaryOperatorColumn>...</UnaryOperatorColumn>  
   <AttributeHierarchyOrdered>...</AttributeHierarchyOrdered>  
   <MemberNamesUnique>...</MemberNamesUnique>  
   <IsAggregatable>...</IsAggregatable>  
   <AttributeHierarchyEnabled>...</AttributeHierarchyEnabled>  
   <AttributeHierarchyOptimizedState>...</AttributeHierarchyOptimizedState>  
   <AttributeHierarchyVisible>...</AttributeHierarchyVisible>  
   <AttributeHierarchyDisplayFolder>...</AttributeHierarchyDisplayFolder>  
   <KeyUniquenessGuarantee>...<KeyUniquenessGuarantee>  
   <InstanceSelection>...</InstanceSelection>  
   <Annotations>...</Annotations>  
</DimensionAttribute>  
```  
  
## <a name="data-type-characteristics"></a>Caractéristiques du type de données  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Types de données de base|Aucune|  
|Types de données dérivés|Aucune|  
  
## <a name="data-type-relationships"></a>Relations du type de données  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|Aucune|  
|Éléments enfants|[Annotations](../../../analysis-services/scripting/collections/annotations-element-assl.md), [AttributeHierarchyDisplayFolder](../../../analysis-services/scripting/properties/attributehierarchydisplayfolder-element-assl.md), [AttributeHierarchyEnabled](../../../analysis-services/scripting/properties/attributehierarchyenabled-element-assl.md), [AttributeHierarchyOptimizedState](../../../analysis-services/scripting/properties/attributehierarchyoptimizedstate-element-assl.md), [AttributeHierarchyOrdered](../../../analysis-services/scripting/properties/attributehierarchyordered-element-assl.md), [AttributeHierarchyVisible](../../../analysis-services/scripting/properties/attributehierarchyvisible-element-assl.md), [AttributeRelationships](../../../analysis-services/scripting/collections/attributerelationships-element-assl.md), [CustomRollupColumn](../../../analysis-services/scripting/objects/customrollupcolumn-element-assl.md), [CustomRollupPropertiesColumn](../../../analysis-services/scripting/objects/customrolluppropertiescolumn-element-assl.md), [DefaultMember](../../../analysis-services/scripting/properties/defaultmember-element-assl.md), [Description](../../../analysis-services/scripting/properties/description-element-assl.md), [DiscretizationBucketCount](../../../analysis-services/scripting/properties/discretizationbucketcount-element-assl.md), [DiscretizationMethod](../../../analysis-services/scripting/properties/discretizationmethod-element-assl.md), [EstimatedCount](../../../analysis-services/scripting/properties/estimatedcount-element-assl.md), [ID](../../../analysis-services/scripting/properties/id-element-assl.md), [InstanceSelection](../../../analysis-services/scripting/properties/instanceselection-element-assl.md), [IsAggregatable](../../../analysis-services/scripting/properties/isaggregatable-element-assl.md), [KeyColumns](../../../analysis-services/scripting/collections/keycolumns-element-assl.md), [KeyUniquenessGuarantee](../../../analysis-services/scripting/properties/keyuniquenessguarantee-element-assl.md), [MemberNamesUnique](../../../analysis-services/scripting/properties/membernamesunique-element-assl.md), [MembersWithData](../../../analysis-services/scripting/properties/memberswithdata-element-assl.md), [MembersWithDataCaption](../../../analysis-services/scripting/properties/memberswithdatacaption-element-assl.md), [Name](../../../analysis-services/scripting/properties/name-element-assl.md), [NameColumn](../../../analysis-services/scripting/objects/namecolumn-element-assl.md), [NamingTemplate](../../../analysis-services/scripting/properties/namingtemplate-element-assl.md), [NamingTemplateTranslations](../../../analysis-services/scripting/collections/namingtemplatetranslations-element-assl.md), [OrderBy](../../../analysis-services/scripting/properties/orderby-element-assl.md), [OrderByAttributeID](../../../analysis-services/scripting/properties/orderbyattributeid-element-assl.md), [RootMemberIf](../../../analysis-services/scripting/properties/rootmemberif-element-assl.md), [SkippedLevelsColumn](../../../analysis-services/scripting/objects/skippedlevelscolumn-element-assl.md), [Source](../../../analysis-services/scripting/properties/source-element-binding-assl.md), [Translations](../../../analysis-services/scripting/collections/translations-element-assl.md), [Type](../../../analysis-services/scripting/properties/type-element-dimensionattribute-assl.md), [UnaryOperatorColumn](../../../analysis-services/scripting/objects/unaryoperatorcolumn-element-assl.md), [Usage](../../../analysis-services/scripting/properties/usage-element-dimensionattribute-assl.md), [ValueColumn](../../../analysis-services/scripting/objects/valuecolumn-element-assl.md)|  
|Éléments dérivés|[Attribute](../../../analysis-services/scripting/objects/attribute-element-assl.md) (collection[Attributes](../../../analysis-services/scripting/collections/attributes-element-assl.md) de [Dimension](../../../analysis-services/scripting/objects/dimension-element-assl.md))|  
  
## <a name="remarks"></a>Notes  
 Les restrictions suivantes s’appliquent lors de l’exécution du service dans les valeurs de propriété de configuration DeploymentMode 1 et 2 (modes SharePoint et tabulaire, utilisés pour exécuter [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] et bases de données model tabulaires) :  
  
-   L'élément utilisé accepte seulement des valeurs KEY ou REGULAR.  
  
-   L'élément IsAggregatable ne peut pas être FALSE.  
  
-   L'élément OrderBy accepte seulement des valeurs KEY ou PROPERTYKEY.  
  
-   Une colonne calculée ne peut pas être une clé primaire dans la table.  
  
-   Une colonne calculée ne peut pas contenir DataSize dans la liaison.  
  
-   Pour chaque colonne calculée, une validation de syntaxe est effectuée avant d'enregistrer la définition d'attribut.  
  
-   Pour AttributeRelationships, RelationshipType doit être défini sur la valeur Flexible.  
  
-   L'attribut « RowNumber », identifié par « RowNumber », doit avoir le type entier.  
  
-   Seul l'attribut « RowNumber » peut avoir le KeyBinding de type RowNumberBinding.  
  
-   Tous les attributs autres que « RowNumber » doivent avoir une cardinalité de 1 par rapport à la clé, à moins que l'attribut lui-même soit la clé.  
  
-   Lorsque la colonne spécifié par OrderBy est également le PropertyKey, OrderByAttributeId ne peut pas pointer sur la colonne de numéro de ligne.  
  
-   Les attributs utilisés comme clés doivent être mis en rapport avec tous les autres attributs ; les autres types de relations ne sont pas prises en charge.  
  
-   L'élément NullProcessing ne peut pas être défini sur « UnknownMember ».  
  
-   Les liaisons ne peuvent pas être définies sur « Value ».  
  
 Les éléments suivants sont pris en charge lors de l’exécution du service de configuration DeploymentMode 1 et 2, les valeurs de propriété (modes SharePoint et tabulaire, utilisés pour exécuter [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] et bases de données model tabulaires) :  
  
-   AttributeHierarchyOptimizedState  
  
-   CustomRollupColumn  
  
-   CustomRollupPropertiesColumn  
  
-   DefaultMember  
  
-   DiscretizationBucketCount  
  
-   DiscretizationMethod  
  
-   SkippedLevelsColumn  
  
-   Source  
  
-   UnaryOperatorColumn  
  
 L’élément correspondant dans le modèle d’objet objets AMO (Analysis Management) est <xref:Microsoft.AnalysisServices.DimensionAttribute>.  
  
## <a name="see-also"></a>Voir aussi  
 [Analysis Services script des Types de données XML Language & #40 ; ASSL & #41 ;](../../../analysis-services/scripting/data-type/analysis-services-scripting-language-xml-data-types-assl.md)  
  
  
