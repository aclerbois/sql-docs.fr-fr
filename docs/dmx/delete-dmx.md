---
title: DELETE (DMX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: data-mining
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DELETE
dev_langs:
- DMX
helpviewer_keywords:
- DELETE statement [DMX]
- mining structures [DMX], clearing
- clearing mining models
- deleting mining models
- mining models [Analysis Services], clearing
- deleting mining structures
ms.assetid: 5a8204c3-a3df-4d97-9c1d-d997d24c70e3
caps.latest.revision: 35
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 4537efcd717b43465e12eadff4d567a179841ed2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="delete-dmx"></a>DELETE (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Supprime un modèle d'exploration de données, une structure d'exploration de données ou une structure d'exploration de données et tous ses modèles d'exploration de données associés selon les clauses DMX (Data Mining Extensions) que vous utilisez.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DELETE FROM [MINING MODEL] <model>[.CONTENT]  
DELETE FROM [MINING STRUCTURE] <structure>[.CONTENT]|[.CASES]  
```  
  
## <a name="arguments"></a>Arguments  
 *model*  
 Identificateur du modèle  
  
 *structure*  
 Identificateur de la structure  
  
## <a name="remarks"></a>Notes  
 Si vous ne spécifiez pas **modèle d’exploration de données** ou **STRUCTURE d’exploration de données**, [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] recherche le type d’objet en fonction du nom et traite l’objet correct. Si le serveur contient une structure d'exploration de données et un modèle d'exploration de données portant le même nom, une erreur est retournée.  
  
 Le tableau ci-dessous décrit le résultat obtenu en utilisant différentes formes de la syntaxe.  
  
|.|Résultat|  
|---------------|------------|  
|SUPPRIMER à partir de la STRUCTURE d’exploration de données*\<structure >*<br /><br /> ou<br /><br /> SUPPRIMER à partir de la STRUCTURE d’exploration de données*\<structure >*. CONTENU|Effectue ProcessClear sur la structure d’exploration de données. Tout le contenu est supprimé de la structure d'exploration de données et de ses modèles d'exploration de données associés.|  
|SUPPRIMER à partir de la STRUCTURE d’exploration de données*\<structure >*. CAS|Effectue ProcessClearStructureOnly sur la structure d’exploration de données. Tout le contenu est supprimé de la structure d'exploration de données, en laissant ses modèles d'exploration de données associés intacts. Une fois la structure d'exploration de données supprimée, l'extraction dans les modèles d'exploration de données associés échoue.|  
|Supprimer dans le modèle d’exploration de données*\<modèle >*<br /><br /> ou<br /><br /> Supprimer dans le modèle d’exploration de données*\<modèle >*. CONTENU|Exécute ProcessClear sur le modèle d’exploration de données, mais laisse les valeurs d’état intactes. Les valeurs d'état correspondent aux états possibles d'une colonne. Par exemple, les valeurs d'état d'une colonne Gender seraient Male ou Female.|  
  
 Pour plus d’informations sur les types de traitement, consultez [élément Type &#40;XMLA&#41;](../analysis-services/xmla/xml-elements-properties/type-element-xmla.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime tout le contenu du modèle NB_Sample.  
  
```  
DELETE FROM NB_Sample.CONTENT  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Data Mining Extensions &#40;DMX&#41; instructions de définition de données](../dmx/dmx-statements-data-definition.md)   
 [Data Mining Extensions &#40;DMX&#41; instructions de Manipulation de données](../dmx/dmx-statements-data-manipulation.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)  
  
  
