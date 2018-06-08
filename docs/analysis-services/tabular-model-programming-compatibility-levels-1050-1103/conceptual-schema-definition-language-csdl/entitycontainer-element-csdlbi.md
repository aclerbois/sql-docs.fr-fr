---
title: Élément EntityContainer (CSDLBI) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 551464b3b4f057a63f13ffbaf53af91b03104af5
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="entitycontainer-element-csdlbi"></a>Élément EntityContainer (CSDLBI)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  L'élément EntityContainer est un type complexe, basé sur le type CSDL, EntityContainer, qui définit une collection d'entités dans un modèle de données simple. Dans une application Business Intelligence, le modèle de données représenté par un EntityContainer peut contenir plusieurs tables avec une colonne liée par des relations, ainsi que des calculs, des mesures et des indicateurs de performance clés. Il est similaire sur le plan conceptuel à une base de données ou à une source de données.  
  
 L'EntityContainer doit spécifier chaque type d'entité inclus dans le modèle de données, notamment les tables et les relations. Des informations sur ces entités de modèle sont spécifiées en répertoriant les entités enfants du type, élément Entity. Pour plus d’informations, consultez [Élément EntityType &#40;CSDLBI&#41;](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/entitytype-element-csdlbi.md).  
  
 Des propriétés telles que le classement et le langage sont définies au niveau de l'EntityContainer, et non sur des objets individuels. Toutefois, les colonnes et les attributs du texte du modèle peuvent avoir des légendes ou des traductions dans d'autres langues.  
  
## <a name="elements-and-attributes"></a>Éléments et attributs  
 Le tableau ci-dessous décrit les éléments et les attributs qui définissent l'EntityContainer.  
  
|Nom|Est obligatoire|Description|  
|----------|-----------------|-----------------|  
|Nom|Oui|Nom du modèle de données.|  
|Légende|Non|Description de la base de données ou du modèle de données.|  
|Culture|Oui|Chaîne qui contient le LCID de la demande.|  
|CompareOptions|Oui|Options de tri et de comparaison de chaîne spécifiques à la langue pour le modèle.|  
|DirectQueryMode|Non|Énumération qui indique le mode de requête lorsque le modèle utilise le mode DirectQuery.|  
|Élément EntitySet|Oui|[Élément EntitySet & #40 ; CSDLBI & #41 ;](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/entityset-element-csdlbi.md)|  
|Élément AssociationSet|Non|[Élément AssociationSet & #40 ; CSDLBI & #41 ;](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/associationset-element-csdlbi.md)|  
  
## <a name="compareoptions-element"></a>CompareOptions, élément  
 L'attribut CompareOptions définit les propriétés de classement appliquées au modèle de données. Les propriétés définies par l'option CompareOptions sont dérivées des paramètres d'ordre de tri, de respect du jeu de caractères Kana et de respect de la casse définis dans la base de données Analysis Services au moment de la conception de modèle. Le tableau suivant décrit les valeurs qui sont incluses dans l'attribut CompareOptions.  
  
|Valeur|Description|  
|-----------|-----------------|  
|IgnoreCase|Valeur booléenne qui indique si les comparaisons de chaîne doivent ignorer la casse.|  
|IgnoreNonSpace|Valeur booléenne qui indique si les comparaisons de chaîne doivent ignorer les caractères de combinaison de non-espacement, tels que les signes diacritiques.|  
|IgnoreKanaType|Valeur booléenne qui indique si les comparaisons de chaîne doivent ignorer le type kana.|  
|IgnoreWidth|Valeur booléenne qui indique si les comparaisons de chaîne doivent ignorer la largeur des caractères.|  
  
## <a name="directquerymode"></a>DirectQueryMode  
 **DirectQueryMode**  
  
 Le type simple, DirectQueryMode, définit le type de requête utilisé par défaut lorsque le modèle est activé pour extraire directement les données d'une source de données relationnelle. Cette propriété s'applique uniquement aux modèles tabulaires exécutés en mode DirectQuery. Le tableau suivant liste les valeurs possibles d'énumération en mode DirectQuery.  
  
|Valeur|Description|  
|-----------|-----------------|  
|InMemory|Indique que les requêtes sur le modèle utilisent les données du cache.|  
|InMemoryWithDirectQuery|Indique que, par défaut, les requêtes sur le modèle utilisent les données de la source de données relationnelle.|  
|DirectQueryWithInMemory|Indique, par défaut, que les requêtes sur le modèle utilisent les données du cache.|  
|DirectQuery|Indique que les requêtes sur le modèle utilisent uniquement les données de la source de données relationnelle.|  
  
## <a name="example"></a>Exemple  
 **Sous forme de tableau**  
  
 L'exemple suivant, en CSDLBI version 1.1, représente une partie du modèle de données tabulaire AdventureWorks.  
  
```  
  
<EntityContainer   
     Name="Sandbox">  
   <EntitySet   
       Name="DimEmployee"   
       EntityType="Sandbox.DimEmployee">  
   <bi:EntitySet />  
   </EntitySet>  
  
   <EntitySet   
     Name="DimProduct"   
       EntityType="Sandbox.DimProduct">  
    <bi:EntitySet />  
    </EntitySet>  
  
<bi:EntityContainer Caption="AWSimple" Culture="en-US">  
  
```  
  
## <a name="example"></a>Exemple  
 **Multidimensionnel**  
  
 L'exemple suivant, en CSDLBI version 1.1, est un extrait du cube Contoso Operations.  
  
```  
  
<EntityContainer   
     Name="Sandbox">  
   <EntitySet   
      Name="Bike"   
      EntityType="Sandbox.Bike">  
    <bi:EntitySet Hidden="true" />  
   </EntitySet>  
…  
<bi:EntityContainer   
   Caption="CSDLTest"   
   Culture="en-US">  
<bi:CompareOptions   
   IgnoreCase="true" />  
</bi:EntityContainer>  
</EntityContainer>  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Élément EntitySet & #40 ; CSDLBI & #41 ;](../../../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/conceptual-schema-definition-language-csdl/entityset-element-csdlbi.md)  
  
  
