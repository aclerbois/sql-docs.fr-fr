---
title: Constantes énumérées ADOX | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- enumerated constants [ADOX]
ms.assetid: 9d91f511-d46f-44ef-97ef-77bf93836186
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0ba95b08ddaa4a75a8243f6830b0f9ce9637d819
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="adox-enumerated-constants"></a>Constantes énumérées ADOX
Pour faciliter le débogage, les constantes énumérées ADOX répertorient une valeur pour chaque constante. Toutefois, cette valeur est purement indicative et peut changer d’une version d’ADOX à l’autre. Votre code doit uniquement dépendre du nom, pas la valeur réelle, de constantes énumérées.  
  
 Les constantes énumérées suivantes sont définies.  
  
|Énumération| Description|  
|-----------------|-----------------|  
|[ActionEnum](../../../ado/reference/adox-api/actionenum.md)|Spécifie le type d’action à effectuer lorsque **SetPermissions** est appelée.|  
|[AllowNullsEnum](../../../ado/reference/adox-api/allownullsenum.md)|Spécifie si les enregistrements avec des valeurs null sont indexés.|  
|[ColumnAttributesEnum](../../../ado/reference/adox-api/columnattributesenum.md)|Spécifie les caractéristiques d’un **colonne**.|  
|[DataTypeEnum](../../../ado/reference/ado-api/datatypeenum.md)|Spécifie le type de données d’une **champ**, **paramètre**, ou **propriété**.|  
|[InheritTypeEnum](../../../ado/reference/adox-api/inherittypeenum.md)|Indique comment les objets hériteront des autorisations définies dans **SetPermissions**.|  
|[KeyTypeEnum](../../../ado/reference/adox-api/keytypeenum.md)|Spécifie le type de **clé**: unique, primaire ou étrangère.|  
|[ObjectTypeEnum](../../../ado/reference/adox-api/objecttypeenum.md)|Spécifie le type d’objet de base de données pour lequel définir des autorisations ou la propriété.|  
|[RightsEnum](../../../ado/reference/adox-api/rightsenum.md)|Spécifie les droits ou autorisations pour un groupe ou utilisateur sur un objet.|  
|[RuleEnum](../../../ado/reference/adox-api/ruleenum.md)|Spécifie la règle à appliquer lorsqu’un **clé** est supprimé.|  
|[SortOrderEnum](../../../ado/reference/adox-api/sortorderenum.md)|Spécifie l’ordre de tri pour une colonne indexée.|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des API ADOX](../../../ado/reference/adox-api/adox-api-reference.md)   
 [Extensions ADO pour le langage de définition de données et la sécurité (ADOX)](../../../ado/guide/extensions/ado-extensions-for-data-definition-language-and-security-adox.md)
