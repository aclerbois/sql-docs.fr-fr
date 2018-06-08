---
title: Objet de base de données (TMSL) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tmsl
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9da6d97236811aaa0ef8ee757c7b2773bdda496a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="database-object-tmsl"></a>Objet de base de données (TMSL)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Définit une base de données tabulaire au niveau de compatibilité 1200 ou supérieur, basé sur un modèle ayant le même niveau. Cette rubrique documente la définition d’objet de base de données, en fournissant la charge utile pour les demandes de créent, modifier, suppriment et effectuent les tâches de gestion de base de données.  
  
> [!NOTE]  
>  Dans n’importe quel script, qu’une seule base de données en temps peut être référencée. Pour les objets autres que de base de données elle-même, la propriété de base de données est facultative si vous spécifiez le modèle. Il existe un mappage entre un modèle et une base de données qui peut être utilisé pour déduire le nom de la base de données si elle n’est pas explicitement fourni.   
> De même, vous pouvez laisser le modèle, définissant ses propriétés dans la base de données.  
  
## <a name="object-definition"></a>Définition de l'objet  
 Tous les objets ont un ensemble commun de propriétés, y compris le nom, type, la description, une collection de propriétés et annotations. **Base de données** objets ont également les propriétés suivantes.  
  
 CompatibilityLevel  
 Actuellement, les valeurs valides sont 1200, 1400. Niveaux de compatibilité inférieurs utilisent des métadonnées différentes.  
  
 readwritemode  
 Énumère le mode de la base de données. Il est courant de faire une base de données en lecture seule dans les configurations haute disponibilité ou l’évolutivité. Les valeurs valides sont readWrite,  
                en lecture seule,  
                ou readOnlyExclusive. Consultez [haute disponibilité et extensibilité dans Analysis Services](../../analysis-services/instances/high-availability-and-scalability-in-analysis-services.md) et [basculer une base de données Analysis Services entre les modes ReadOnly et ReadWrite](../../analysis-services/multidimensional-models/switch-an-analysis-services-database-between-readonly-and-readwrite-modes.md) pour plus d’informations sur lorsque cette propriété est utilisée.  
  
## <a name="usage"></a>Utilisation  
 **Base de données** objets sont utilisés dans presque toutes les commandes. Consultez [commandes dans le langage de script de modèle tabulaire &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-commands/tmsl-reference-commands.md) pour obtenir la liste. A **base de données** est un enfant d’un objet serveur.  
  
 Lors de la création, du remplacement ou de modification d’un objet de base de données, spécifiez toutes les propriétés en lecture-écriture de la définition d’objet. L’omission d’une propriété en lecture-écriture est considérée comme une suppression.  
  
## <a name="partial-syntax"></a>Syntaxe partielle  
 Étant donné que cette définition d’objet est la taille trop importante, seules les propriétés directes sont répertoriées. Le **modèle** objet fournit la majeure partie de la définition de la base de données. Consultez [objet de modèle &#40;TMSL&#41; ](../../analysis-services/tabular-models-scripting-language-objects/model-object-tmsl.md) à la façon dont l’objet est défini.  
  
```  
    "database": {  
      "type": "object",  
      "properties": {  
        "name": {  
          "type": "string"  
        },  
        "id": {  
          "type": "string"  
        },  
        "description": {  
          "type": "string"  
        },  
        "compatibilityLevel": {  
          "type": "integer"  
        },  
        "readWriteMode": {  
          "enum": [  
            "readWrite",  
            "readOnly",  
            "readOnlyExclusive"  
          ]  
        },  
        "model": {  
          "type": "object",  
          ...  
        }  
    }  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Tabular Model Scripting Language &#40;TMSL&#41; Reference [Informations de référence sur TMSL &#40;Tabular Model Scripting Language&#41;]](../../analysis-services/tabular-model-scripting-language-tmsl-reference.md)   
 [Haute disponibilité et extensibilité dans Analysis Services](../../analysis-services/instances/high-availability-and-scalability-in-analysis-services.md)  
  
  
