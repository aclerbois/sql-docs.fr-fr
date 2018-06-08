---
title: Séparateur de capture de données modifiées | Documents Microsoft
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
- sql13.ssis.designer.cdcsplitter.f1
ms.assetid: 167bc5c6-fa36-439d-987c-b20acd1a77e2
caps.latest.revision: 8
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1993bf22018399d3cef329a75fb77e15a7a08746
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cdc-splitter"></a>Séparateur de capture de données modifiées
  Le séparateur de capture de données modifiées fractionne un flux de lignes de modification d'un flux de données de source CDC en flux de données distincts pour les opérations d'insertion, de mise à jour et de suppression. Le flux de données est fractionné en fonction de la colonne requise `__$operation` et de ses valeurs standard dans les tables de modification de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] .  
  
|Valeur de l'opération|Sortie|Description|  
|------------------------|------------|-----------------|  
| 1|DELETE|Ligne supprimée|  
|2|Insert|Ligne insérée (non disponible en cas d’utilisation du mode de capture de données modifiées **Net avec fusion** )|  
|3|Update|Ligne avant la mise à jour (disponible uniquement en cas d’utilisation du mode de capture de données modifiées **Tout avec les anciennes valeurs** )|  
|4|Update|Ligne après la mise à jour (suit avant la mise à jour)|  
|5|Update|Ligne de fusion (disponible uniquement en cas d’utilisation du mode de capture de données modifiées **Net avec fusion** )|  
|Autres|Error||  
  
 Vous pouvez utiliser le séparateur pour vous connecter aux sorties INSERT, DELETE et UPDATE afin d'effectuer d'autres opérations de traitement.  
  
 La transformation de séparateur de capture de données modifiées a une sortie standard et une sortie d'erreur.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 La transformation de séparateur de capture de données modifiées a une sortie d'erreur. Les lignes d'entrée avec une valeur de colonne $operation non valide sont considérées comme incorrectes et sont gérées conformément à la propriété **ErrorRowDisposition** de l'entrée.  
  
 La sortie d'erreur du composant contient les colonnes de sortie suivantes :  
  
-   **Code d'erreur**: la valeur est 1.  
  
-   **Colonne d’erreur**: colonne source à l’origine de l’erreur (pour les erreurs de conversion).  
  
-   **Colonnes de ligne d'erreur**: colonnes d'entrée de la ligne à l'origine de l'erreur.  
  
## <a name="configuring-the-cdc-splitter"></a>Configuration du séparateur de capture de données modifiées  
 Le séparateur de capture de données modifiées ne comporte aucune propriété configurable.  
  
 Pour plus d'informations sur l'utilisation du séparateur de capture de données modifiées, consultez Composants de capture de données modifiées pour Microsoft SQL Server Integration Services.  
  
 La boîte de dialogue **Éditeur avancé** contient les propriétés qui peuvent être définies par programme.  
  
 Pour ouvrir la boîte de dialogue **Éditeur avancé** :  
  
-   Sur l'écran **Flux de données** de votre projet [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] , cliquez avec le bouton droit sur le séparateur de capture de données modifiées, puis sélectionnez **Afficher l'éditeur avancé**.  
  
## <a name="see-also"></a> Voir aussi  
 [Diriger le flux de capture de données modifiées en fonction du type de modification](../../integration-services/data-flow/direct-the-cdc-stream-according-to-the-type-of-change.md)  
  
  
