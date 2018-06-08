---
title: Partitioning, élément (DTA) | Documents Microsoft
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: dta
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Partitioning element
ms.assetid: 9bc5d1d5-27a7-4434-966f-c3935794af27
caps.latest.revision: 13
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: b0f97969f67f344d5b2b565938ef2eeef67bd127
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="partitioning-element-dta"></a>Partitioning, élément (Assistant Paramétrage de base de données)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Contient le schéma de partitionnement que vous aimeriez que l'Assistant Paramétrage du moteur de base de données utilise durant l'analyse.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <Partitioning>...</Partitioning>  
```  
  
## <a name="element-characteristics"></a>Caractéristiques de l'élément  
  
|Caractéristique|Description|  
|--------------------|-----------------|  
|**Type de données et longueur**|**string**, aucune longueur maximale.|  
|**Valeurs autorisées**|**NONE**<br /> Aucun partitionnement.<br /><br /> **FULL**<br /> Partitionnement complet. (Améliore les performances.)<br /><br /> **ALIGNED**<br /> Partitionnement aligné uniquement. (Améliore la gestion.)<br /><br /> Utilisez une seule de ces valeurs avec cet élément.<br /><br /> La valeur**ALIGNED** signifie que dans la recommandation générée par l’Assistant Paramétrage du moteur de base de données chaque index proposé est partitionné de la même façon que la table sous-jacente pour laquelle l’index est défini. Les index non-cluster sur une vue indexée sont alignés avec la vue indexée.|  
|**Valeur par défaut**|**NONE**|  
|**Occurrence**|Nécessaire une fois pour l’élément **TuningOptions** , sauf si l’élément **DropOnlyMode** est utilisé. Si **DropOnlyMode** est utilisé, vous ne pouvez pas utiliser **Partitioning**. Ces éléments s'excluent mutuellement.|  
  
## <a name="element-relationships"></a>Relations entre les éléments  
  
|Relation|Éléments|  
|------------------|--------------|  
|**Élément parent**|[Élément TuningOptions &#40;DTA&#41;](../../tools/dta/tuningoptions-element-dta.md)|  
|**Éléments enfants**|Aucun.|  
  
## <a name="example"></a> Exemple  
 Pour obtenir un exemple d’utilisation de cet élément, consultez [Exemple de fichier d’entrée XML simple &#40;Assistant Paramétrage de base de données&#41;](../../tools/dta/simple-xml-input-file-sample-dta.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Référence des fichiers d’entrée XML &#40;Assistant Paramétrage du moteur de base de données&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
