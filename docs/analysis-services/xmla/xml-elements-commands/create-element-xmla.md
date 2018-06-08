---
title: Élément Create (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 78cdf8b38828e8b9f96a89ffc026ec39ae40366b
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34574571"
---
# <a name="create-element-xmla"></a>Élément Create (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../../includes/ssas-appliesto-sqlas-aas.md)]
  Contient des éléments d’Analysis Services Scripting Language (ASSL) utilisés par le **Execute** méthode pour créer des objets sur une instance Analysis Services.  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
  
<Command>  
   <Create Scope="enum" AllowOverwrite="boolean">  
      <ParentObject>...</ParentObject>  
      <ObjectDefinition>...</ObjectDefinition>  
   </Create>  
</Command>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l’élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|Type de données et longueur|None|  
|Valeur par défaut|None|  
|Cardinalité|0-n : élément facultatif pouvant apparaître plusieurs fois.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Élément|  
|------------------|-------------|  
|Éléments parents|[Commandee](../../../analysis-services/xmla/xml-elements-properties/command-element-xmla.md)|  
|Éléments enfants|[ObjectDefinition](../../../analysis-services/xmla/xml-elements-properties/objectdefinition-element-xmla.md), [ParentObject](../../../analysis-services/xmla/xml-elements-properties/parentobject-element-xmla.md)|  
  
## <a name="attributes"></a>Attributs  
  
|Attribute|Description|  
|---------------|-----------------|  
|AllowOverwrite|Attribut **Boolean** facultatif. Si la valeur True, les objets définis dans le **ObjectDefinition** peuvent remplacer des objets existants sur le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance. Si cet attribut est omis ou possède la valeur False, la présence d'un objet existant génère une erreur.|  
|Portée|Attribut **Enum** facultatif. Définit la durée des objets définis dans l'élément **ObjectDefinition** . Si cet attribut est omis, les objets définis dans le **ObjectDefinition** sont conservés dans le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance. La valeur suivante est disponible :<br /><br /> *Session*: les objets définis dans le **ObjectDefinition** l’élément existe uniquement pour la durée du code XML pour la session Analysis (XMLA).<br />                  Notez que lorsque vous utilisez la *Session* définition, le **ObjectDefinition** élément ne peut contenir [Dimension](../../../analysis-services/scripting/objects/dimension-element-assl.md), [Cube](../../../analysis-services/scripting/objects/cube-element-assl.md), ou [MiningModel](../../../analysis-services/scripting/objects/miningmodel-element-assl.md) éléments ASSL.|  
  
## <a name="remarks"></a>Notes  
 Chaque opération **Create** crée un objet principal sous un parent défini par l'élément **ParentObject** . Si l'objet parent est omis, il est considéré comme l'instance [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] de destination. Ceci génère une erreur si le parent d'un objet principal ne correspond pas à l'instance de destination.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant crée une base de données vide nommée **base de données Test** sur une [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] instance.  
  
```  
  
      <Create xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
   <ObjectDefinition>  
      <Database xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
         <Name>Test Database</Name>  
         <Description>A test database.</Description>  
      </Database>  
   </ObjectDefinition>  
</Create>  
```  
  
## <a name="see-also"></a>Voir aussi
 [Commandes &#40;XMLA&#41;](../../../analysis-services/xmla/xml-elements-commands/xml-elements-commands.md)  
  
  
