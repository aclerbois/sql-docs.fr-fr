---
title: Classés de colonnes (exploration de données) | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b014f82376b15d9d29834103d8827bf31231ef90
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="classified-columns-data-mining"></a>Colonnes classifiées (exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous définissez une colonne classée, vous créez une relation entre la colonne actuelle et une autre colonne de la structure d'exploration de données. Les données de la colonne de structure d'exploration de données que vous désignez comme colonne classée contiennent des informations catégorielles qui décrivent les valeurs dans une autre colonne de la structure d'exploration de données.  
  
 Par exemple, vous avez deux colonnes avec des données numériques : une colonne, [achats annuels], contient tous les achats annuels par client pour une année civile spécifique, et l’autre colonne, [écarts types], contient les écarts types de ces valeurs. Dans ce cas, vous pouvez indiquer la colonne [achats annuels] comme colonne classifiée, et le modèle peut utiliser cette relation dans l’analyse.  
  
> [!NOTE]  
>  Les algorithmes fournis dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne prennent pas en charge l’utilisation des colonnes classifiées ; cette fonctionnalité est fournie pour une utilisation dans la création d’algorithmes personnalisés.  
  
## <a name="defining-a-classified-column"></a>Définition d'une colonne classée  
 Le type de données d’une colonne classifiée doit être **Long** ou **Double**.  
  
 La liste suivante décrit les types de contenu pris en charge par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour les colonnes classifiées.  
  
 **PROBABILITY**  
 La valeur de la colonne représente la probabilité de la valeur associée et est comprise entre 0 et 1.  
  
 **VARIANCE**  
 La valeur de la colonne représente la variance de la valeur associée.  
  
 **STDEV**  
 La valeur de la colonne représente l'écart type de la valeur associée.  
  
 **PROBABILITY_VARIANCE**  
 La valeur de la colonne représente la variance de la probabilité de la valeur associée.  
  
 **PROBABILITY_STDEV**  
 La valeur de la colonne représente l'écart type de la probabilité de la valeur associée.  
  
 **SUPPORT**  
 La valeur de la colonne représente le poids, ou facteur de réplication de cas, de la valeur associée.  
  
## <a name="see-also"></a>Voir aussi  
 [Contenu des Types de & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/content-types-data-mining.md)   
 [Les Structures d’exploration de données & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-structures-analysis-services-data-mining.md)   
 [Types de données & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/data-types-data-mining.md)  
  
  
