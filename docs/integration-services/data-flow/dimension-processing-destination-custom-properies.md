---
title: Propriétés personnalisées de la destination de traitement de dimension | Microsoft Docs
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
ms.assetid: 9700f663-53f2-49b6-b1ef-92c7b752d6a1
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5d862b49ef67e0dd163a90894d166be0bc623e60
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dimension-processing-destination-custom-properies"></a>Propriétés personnalisées de la destination de traitement de dimension
  La destination de traitement de dimension comporte à la fois des propriétés personnalisées et les propriétés communes à l'ensemble des composants de flux de données.  
  
 Le tableau suivant décrit les propriétés personnalisées de la destination de traitement de dimension. Toutes les propriétés sont en lecture/écriture.  
  
|Propriété|Type de données|Description|  
|--------------|---------------|-----------------|  
|ASConnectionString|String|Chaîne de connexion d'une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
|KeyDuplicate|Integer (énumération)|Quand UseDefaultConfiguration a la valeur **False**, valeur qui indique comment gérer les erreurs de clé en double. Les valeurs possibles sont **IgnoreError** (0), **ReportAndContinue** (1) et **ReportAndStop** (2). La valeur par défaut de cette propriété est **IgnoreError** (0).|  
|KeyErrorAction|Integer (énumération)|Quand UseDefaultConfiguration a la valeur **False**, valeur qui indique comment gérer les erreurs de clé. Les valeurs possibles sont **ConvertToUnknown** (0) et **DiscardRecord** (1). La valeur par défaut de cette propriété est **ConvertToUnknown** (0).|  
|KeyErrorLimit|Entier|Quand UseDefaultConfiguration a la valeur **False**, limite supérieure des erreurs de clé activées.|  
|KeyErrorLimitAction|Integer (énumération)|Quand UseDefaultConfiguration a la valeur **False**, valeur qui indique l’action à entreprendre quand la limite **KeyErrorLimit** est atteinte. Les valeurs possibles sont **StopLogging** (1) et **StopProcessing** (0). La valeur par défaut de cette propriété est **StopProcessing** (0).|  
|KeyErrorLogFile|String|Quand UseDefaultConfiguration a la valeur **False**, chemin et nom du fichier journal des erreurs.|  
|KeyNotFound|Integer (énumération)|Quand UseDefaultConfiguration a la valeur **False**, valeur qui indique comment gérer les erreurs de clé manquante. Les valeurs possibles sont **IgnoreError** (0), **ReportAndContinue** (1) et **ReportAndStop** (2). La valeur par défaut de cette propriété est **IgnoreError** (0).|  
|NullKeyConvertedToUnknown|Integer (énumération)|Quand UseDefaultConfiguration a la valeur **False**, valeur qui indique comment gérer les clés null converties en valeur inconnue. Les valeurs possibles sont **IgnoreError** (0), **ReportAndContinue** (1) et **ReportAndStop** (2). La valeur par défaut de cette propriété est **IgnoreError** (0).|  
|NullKeyNotAllowed|Integer (énumération)|Quand UseDefaultConfiguration a la valeur **False**, valeur qui indique comment gérer les clés null non autorisées. Les valeurs possibles sont **IgnoreError** (0), **ReportAndContinue** (1) et **ReportAndStop** (2). La valeur par défaut de cette propriété est **IgnoreError** (0).|  
|ProcessType|Integer (énumération)|Type de traitement de dimension que la transformation utilise. Les valeurs sont **ProcessAdd** (1) (incrémentiel), **ProcessFull** (0) et **ProcessUpdate** (2).|  
|UseDefaultConfiguration|Booléen|Valeur qui spécifie si la transformation utilise la configuration d'erreur par défaut. Si cette propriété a la valeur **False**, la transformation inclut des informations sur le traitement des erreurs.|  
  
 L'entrée et les colonnes d'entrée de la destination de traitement de dimension ne disposent pas de propriétés personnalisées.  
  
 Pour plus d’informations, consultez [Destination de traitement de dimension](../../integration-services/data-flow/dimension-processing-destination.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
  
