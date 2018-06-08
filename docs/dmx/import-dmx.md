---
title: IMPORTATION (DMX) | Documents Microsoft
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
- IMPORT
dev_langs:
- DMX
helpviewer_keywords:
- IMPORT statement
- mining structures [DMX], importing
ms.assetid: c053bc88-2daf-4ebb-81d7-5a330250536d
caps.latest.revision: 42
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: ae93e70bb0e5672ada244064feb12750a96d4ffd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="import-dmx"></a>IMPORT (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Charge un modèle ou une structure d'exploration de données à partir d'un fichier de sauvegarde Analysis Services (.abf) sur le serveur.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
IMPORT FROM <filename>  
```  
  
## <a name="arguments"></a>Arguments  
 *filename*  
 Chaîne contenant le nom et l'emplacement du fichier à importer.  
  
## <a name="remarks"></a>Notes  
 Si aucun objet n'est spécifié, la totalité du contenu du fichier .dmb est chargée. Si le fichier .dmb comprend une base de données qui n'existe pas sur le serveur, la base de données est créée.  
  
 Pour exporter ou importer des objets, vous devez être administrateur de base de données ou de serveur.  
  
## <a name="import-from-file-example"></a>Exemple de l'importation à partir d'un fichier  
 L'exemple suivant importe la totalité du contenu du fichier contenant la structure et le modèle d'association sur le serveur actuel.  
  
```  
IMPORT FROM 'C:\TEMP\Association_NEW.dmb'  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Data Mining Extensions &#40;DMX&#41; instructions de définition de données](../dmx/dmx-statements-data-definition.md)   
 [Data Mining Extensions &#40;DMX&#41; instructions de Manipulation de données](../dmx/dmx-statements-data-manipulation.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)   
 [EXPORTER &AMP;#40;DMX&AMP;#41;](../dmx/export-dmx.md)   
 [Exporter et importer des objets d’exploration de données](../analysis-services/data-mining/export-and-import-data-mining-objects.md)  
  
  
