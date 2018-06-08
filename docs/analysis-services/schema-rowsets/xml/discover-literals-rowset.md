---
title: Ensemble de lignes DISCOVER_LITERALS | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ed865f22291b3e2e47c4934e455e0e75795d35cc
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discoverliterals-rowset"></a>Ensemble de lignes DISCOVER_LITERALS
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Retourne des informations sur les littéraux pris en charge par le fournisseur XMLA (XML for Analysis) [!INCLUDE[msCoName](../../../includes/msconame-md.md)], y compris les types de données et les valeurs.  
  
 Si vous appelez le [Discover](../../../analysis-services/xmla/xml-elements-methods-discover.md) méthode avec la **DISCOVER_LITERALS** valeur d’énumération dans le [RequestType](../../../analysis-services/xmla/xml-elements-properties/requesttype-element-xmla.md) élément, le **Discover** méthode retourne la **DISCOVER_LITERALS** ensemble de lignes.  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le **DISCOVER_LITERALS** ensemble de lignes contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Longueur| Description|  
|-----------------|--------------------|------------|-----------------|  
|**LiteralName**|**DBTYPE_WSTR**||Nom du littéral décrit dans la ligne.<br /><br /> Par exemple : **DBLITERAL_LIKE_PERCENT**|  
|**LiteralValue**|**DBTYPE_WSTR**||Valeur littérale réelle.<br /><br /> Par exemple, si **LiteralName** est **DBLITERAL_LIKE_PERCENT** et le caractère de pourcentage (**%**) correspond à zéro ou plusieurs caractères dans une clause LIKE, la valeur de la **LiteralValue** colonne est «**%**».|  
|**LiteralInvalidChars**|**DBTYPE_WSTR**||Caractères qui ne sont pas valides dans le littéral.<br /><br /> Par exemple, si les noms de table peuvent contenir la valeur de l’autre chose qu’un caractère numérique, cette chaîne est «**0123456789**».|  
|**LiteralInvalidStartingChars**|**DBTYPE_WSTR**||Caractères qui ne sont pas valides comme premier caractère dans le littéral. Si le littéral peut commencer par n’importe quel caractère valide, il s’agit **null**.|  
|**LiteralMaxLength**|**DBTYPE_I4**||Nombre maximal de caractères du littéral. Si la colonne ne possède pas de longueur maximale ou si elle est inconnue, la valeur est -1.|  
|**LiteralNameEnumValue**|**DBTYPE_I4**|||  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
## <a name="restriction-columns"></a>Colonnes de restriction  
 Le **DISCOVER_LITERALS** ensemble de lignes peut être restreint sur les colonnes dans le tableau suivant.  
  
|Nom de colonne|Indicateur de type|État de la restriction|  
|-----------------|--------------------|-----------------------|  
|**LiteralName**|**DBTYPE_WSTR**|Ce paramètre est facultatif.|  
  
## <a name="see-also"></a>Voir aussi  
 [XML for Analysis ensembles de lignes de schéma](../../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)   
 [Ensemble de lignes DISCOVER_KEYWORDS & #40 ; XMLA & #41 ;](../../../analysis-services/schema-rowsets/xml/discover-keywords-rowset-xmla.md)  
  
  
