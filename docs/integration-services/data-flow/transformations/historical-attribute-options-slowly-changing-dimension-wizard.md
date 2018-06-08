---
title: Options des attributs d’historique (Assistant Dimension à variation lente) | Microsoft Docs
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
f1_keywords:
- sql13.dts.loaddimwizard.histattriboption.f1
ms.assetid: a176ec66-ec39-4c99-99d1-c1afa8450e1e
caps.latest.revision: 22
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: dcf6e290bf12600dee4b6483ca305507a5888973
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="historical-attribute-options-slowly-changing-dimension-wizard"></a>Options des attributs d'historique (Assistant Dimension à variation lente)
  Utilisez la boîte de dialogue **Options des attributs d'historique** pour afficher les attributs d'historique par dates de début et de fin ou pour enregistrer les attributs d'historique dans une colonne créée spécialement à cet effet.  
  
 Pour en savoir plus sur cet Assistant, consultez [Slowly Changing Dimension Transformation](../../../integration-services/data-flow/transformations/slowly-changing-dimension-transformation.md).  
  
## <a name="options"></a>Options  
 **Utiliser une seule colonne pour afficher les enregistrements actifs et expirés**  
 Si vous choisissez d'utiliser une seule colonne pour enregistrer l'état des attributs d'historique, les options suivantes sont disponibles :  
  
|Option|Description|  
|------------|-----------------|  
|**Colonne d'indicateur d'enregistrement actif**|Sélectionnez une colonne dans laquelle indiquer l'enregistrement actif.|  
|**Valeur si actif**|Utilisez soit **True** soit **Current** pour indiquer que l'enregistrement est actif.|  
|**Valeur d'expiration**|Utilisez soit **False** soit **Expired** pour indiquer que l'enregistrement est une valeur historique.|  
  
 **Utiliser les dates de début et de fin pour identifier les enregistrements actifs et expirés**  
 La table de dimension pour cette option doit comprendre une colonne de date. Si vous choisissez d'afficher les attributs d'historique par dates de début et de fin, les options suivantes sont disponibles.  
  
|Option|Description|  
|------------|-----------------|  
|**Colonne de date de début**|Sélectionnez la colonne de la table de dimension qui doit contenir la date de début.|  
|**Colonne de date de fin**|Sélectionnez la colonne de la table de dimension qui doit contenir la date de fin.|  
|**Variable de définition des valeurs de date**|Sélectionnez une variable de date dans la liste.|  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer les sorties à l'aide de l'Assistant Dimension à variation lente](../../../integration-services/data-flow/transformations/configure-outputs-using-the-slowly-changing-dimension-wizard.md)  
  
  
