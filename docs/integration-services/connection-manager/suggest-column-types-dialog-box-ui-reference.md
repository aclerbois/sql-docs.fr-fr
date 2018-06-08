---
title: Référence de l’interface utilisateur de la boîte de dialogue Suggérer les types de colonnes | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.suggestdatatypes.f1
ms.assetid: 8d5652e0-cf57-483f-828b-10f00c08418b
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 701c59d20963141cd4a8abba7f2e4c7895069483
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="suggest-column-types-dialog-box-ui-reference"></a>Référence de l'interface utilisateur de la boîte de dialogue Suggérer les types de colonnes
  Utilisez la boîte de dialogue **Suggérer les types de colonnes** pour identifier le type de données et la longueur des colonnes dans un gestionnaire de connexions de fichiers plats en se basant sur un échantillonnage du contenu du fichier.  
  
 Pour en savoir plus sur les types de données utilisés par [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], consultez [Types de données d’Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="options"></a>Options  
 **Nombre de lignes**  
 Tapez ou sélectionnez le nombre de lignes de l'échantillon utilisées par l'algorithme.  
  
 **Suggérer le plus petit type de données integer**  
 Désactivez cette case à cocher pour ignorer l'évaluation. Si elle est activée, détermine le plus petit type de données integer possible pour les colonnes contenant des données numériques entières.  
  
 **Suggérer le plus petit type de données real**  
 Désactivez cette case à cocher pour ignorer l'évaluation. Si elle est activée, détermine si les colonnes contenant des données numériques réelles peuvent utiliser le plus petit type de données real, DT_R4.  
  
 **Identifier les colonnes booléennes en utilisant les valeurs suivantes**  
 Tapez les deux valeurs à utiliser en tant que valeurs booléennes True et False. Ces valeurs doivent être séparées par une virgule, la première correspondant à la valeur True.  
  
 **Remplir les colonnes de la chaîne**  
 Activez cette case à cocher pour autoriser le remplissage de la chaîne.  
  
 **Pourcentage de remplissage**  
 Tapez ou sélectionnez le pourcentage des longueurs de colonnes à ajouter à la longueur des colonnes pour les données de type character. Ce pourcentage doit être un nombre entier.  
  
## <a name="see-also"></a> Voir aussi  
 [Guide de référence des erreurs et des messages propres à Integration Services](../../integration-services/integration-services-error-and-message-reference.md)   
 [Gestionnaire de connexions de fichiers plats](../../integration-services/connection-manager/flat-file-connection-manager.md)  
  
  
