---
title: Définition et identification d’objets (XMLA) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7b3db6fdcd8d9e6bd604d310f8d7858a4a91180e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="defining-and-identifying-objects-xmla"></a>Définition et identification d'objets (XMLA)
  Les objets sont identifiés dans les commandes XMLA (XML for Analysis) à l'aide d'identificateurs et de références d'objet, et ils sont définis à l'aide de commandes XMLA contenant des éléments ASSL (Analysis Services Scripting Language).  
  
## <a name="object-identifiers"></a>Identificateurs d'objet  
 Un objet est identifié à l’aide de l’identificateur unique de l’objet tel que défini sur une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Les identificateurs d'objet peuvent être spécifiés explicitement ou déterminés par l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] au moment où [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] crée l'objet. Vous pouvez utiliser la [Discover](../../analysis-services/xmla/xml-elements-methods-discover.md) méthode pour récupérer les identificateurs d’objet pour ultérieures **Discover** ou [Execute](../../analysis-services/xmla/xml-elements-methods-execute.md) les appels de méthode.  
  
## <a name="object-references"></a>Références d'objet  
 Plusieurs commandes XMLA, tel que [supprimer](../../analysis-services/xmla/xml-elements-commands/delete-element-xmla.md) ou [processus](../../analysis-services/xmla/xml-elements-commands/process-element-xmla.md), utilisez une référence d’objet pour faire référence à un objet d’une façon non ambiguë. Une référence d'objet contient l'identificateur de l'objet sur lequel une commande est exécutée, ainsi que les identificateurs des ancêtres de cet objet. Par exemple, dans le cas d'une partition, la référence d'objet contient l'identificateur d'objet de la partition, ainsi que les identificateurs d'objet du groupe de mesures parent, du cube et de la base de données de cette partition.  
  
## <a name="object-definitions"></a>Définition d'objets  
 Le [créer](../../analysis-services/xmla/xml-elements-commands/create-element-xmla.md) et [Alter](../../analysis-services/xmla/xml-elements-commands/alter-element-xmla.md) commandes XMLA créer ou modifier, respectivement, objets sur un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance. Les définitions de ces objets sont représentées par une [ObjectDefinition](../../analysis-services/xmla/xml-elements-properties/objectdefinition-element-xmla.md) élément qui contient les éléments ASSL. Identificateurs d’objet peuvent être spécifiés explicitement pour les principaux et de nombreux objets secondaires à l’aide de la [ID](../../analysis-services/xmla/xml-elements-properties/id-element-xmla.md) élément. Si le **ID** élément n’est pas utilisé, le [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance fournit un identificateur unique, avec une convention d’affectation de noms qui dépend de l’objet à être identifié. Pour plus d’informations sur l’utilisation de la **créer** et **Alter** commandes pour définir des objets, consultez [création et modification des objets &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/creating-and-altering-objects-xmla.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Objet, élément & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-properties/object-element-xmla.md)   
 [Élément ParentObject &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-properties/parentobject-element-xmla.md)   
 [Élément source &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-properties/source-element-xmla.md)   
 [Élément cible & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-properties/target-element-xmla.md)   
 [Développement avec XMLA dans Analysis Services](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)  
  
  
