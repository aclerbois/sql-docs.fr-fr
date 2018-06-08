---
title: Propriétés personnalisées de la destination DataReader | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f151c3e8-3811-457d-a3d3-6158ca65a646
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 17976fb408d2bc96cc905cce1a45bc4313ed72f4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="datareader-destination-custom-properties"></a>Propriétés personnalisées de la destination DataReader
  La destination DataReader comporte à la fois des propriétés personnalisées et les propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la destination DataReader. Toutes les propriétés sont en lecture/écriture, à l’exception de **DataReader** .  
  
|Nom de la propriété|Type de données|Description|  
|-------------------|---------------|-----------------|  
|DataReader|String|Nom de classe de la destination DataReader.|  
|FailOnTimeout|Booléen|Indique s’il faut faire échouer l’opération ou non quand un **ReadTimeout** se produit. La valeur par défaut de cette propriété est **False**.|  
|ReadTimeout|Entier|Nombre de millisecondes devant s'écouler avant l'expiration du délai d'attente. La valeur par défaut de cette propriété est 30000 (30 secondes).|  
  
 L'entrée et les colonnes d'entrée de la destination DataReader ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, consultez [Destination DataReader](../../integration-services/data-flow/datareader-destination.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
  
