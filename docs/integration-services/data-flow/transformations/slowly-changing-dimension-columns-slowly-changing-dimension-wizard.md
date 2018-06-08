---
title: Colonnes de dimensions à variation lente (Assistant Dimension à variation lente) | Microsoft Docs
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
- sql13.dts.loaddimwizard.scdsupport.f1
ms.assetid: 36de99d5-5368-48e0-b876-17e9c6862c6c
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0c5cd64b84fa7b01cb8d7da7cdd3d880c89821c8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="slowly-changing-dimension-columns-slowly-changing-dimension-wizard"></a>Colonnes de dimensions à variation lente (Assistant Dimension à variation lente)
  Utilisez la boîte de dialogue **Colonnes de dimensions à variation lente** pour sélectionner un type de modification pour chaque colonne de dimension à variation lente.  
  
 Pour en savoir plus sur cet Assistant, consultez [Slowly Changing Dimension Transformation](../../../integration-services/data-flow/transformations/slowly-changing-dimension-transformation.md).  
  
## <a name="options"></a>Options  
 **Colonnes de dimension**  
 Sélectionnez une colonne de dimension dans la liste.  
  
 **Modifier de type**  
 Sélectionnez un **Attribut fixe**ou sélectionnez l’un des deux types d’attributs variables. Utilisez **Attribut fixe** lorsque la valeur d'une colonne ne doit pas changer ; [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] traite alors les modifications comme des erreurs. Utilisez **Modification d'attribut** pour remplacer les valeurs existantes par des valeurs modifiées. Utilisez **Attribut d'historique** pour enregistrer les valeurs modifiées dans de nouveaux enregistrements tout en marquant comme obsolètes les anciens enregistrements.  
  
 **Supprimer**  
 Sélectionnez une colonne de dimension et supprimez-la de la liste des colonnes mappées en cliquant sur **Supprimer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer les sorties à l'aide de l'Assistant Dimension à variation lente](../../../integration-services/data-flow/transformations/configure-outputs-using-the-slowly-changing-dimension-wizard.md)  
  
  
