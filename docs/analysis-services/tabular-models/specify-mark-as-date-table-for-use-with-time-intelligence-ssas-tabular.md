---
title: Spécifier la marque comme Table de dates | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f6a4ad929c866658ce241f33ddd2a5326dc78e19
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="specify-mark-as-date-table-for-use-with-time-intelligence"></a>Spécifier la marque comme Table de dates à utiliser avec time intelligence
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Pour pouvoir utiliser les fonctions time intelligence dans les formules DAX, vous devez spécifier une table de dates et une colonne d’identificateur unique (datetime) du type de données Date. Une fois qu'une colonne dans la table de dates est spécifiée comme identificateur unique, vous pouvez créer des relations entre les colonnes de cette table et de toutes les tables de faits.  
  
 Lorsque vous utilisez les fonctions time intelligence, les règles suivantes s’appliquent :  
  
-   Lorsque vous utilisez les fonctions time intelligence DAX, ne spécifiez jamais une colonne datetime à partir d’une table de faits. Créez toujours une table de dates distincte dans votre modèle, avec au moins une colonne datetime de type de données Date et avec des valeurs uniques.  
  
-   Vérifiez que votre table de dates a une plage de dates continue.  
  
-   La colonne datetime dans la table de dates doit avoir une granularité de jour (sans fractions d'un jour).  
  
-   Vous devez spécifier une table de dates et une colonne d'identificateur unique à l'aide de la boîte de dialogue **Marquez la table de dates** .  
  
-   Créez des relations entre les tables de faits et les colonnes de type de données Date dans la table de dates.  
  
#### <a name="to-specify-a-date-table-and-unique-identifier"></a>Pour spécifier une table de dates et un identificateur unique  
  
1.  Dans le concepteur de modèles, cliquez sur la table de dates.  
  
2.  Cliquez sur le menu **Table** , cliquez sur **Date**, puis cliquez sur **Marquer en tant que table de dates**  
  
3.  Dans la boîte de dialogue **Marquer en tant que table de dates** , dans la zone de liste **Date** , sélectionnez une colonne à utiliser comme identificateur unique. Cette colonne doit contenir des valeurs uniques et doit avoir le type de données Date. Par exemple :  
  
    |Date|  
    |----------|  
    |7/1/2010 12:00:00 AM|  
    |7/2/2010 12:00:00 AM|  
    |7/3/2010 12:00:00 AM|  
    |7/4/2010 12:00:00 AM|  
    |7/5/2010 12:00:00 AM|  
  
4.  Si nécessaire, créez les relations entre les tables de faits et la table de dates.  
  
## <a name="see-also"></a>Voir aussi  
 [Calculs](../../analysis-services/tabular-models/calculations-ssas-tabular.md)   
 [Fonctions Time intelligence (DAX)](http://msdn.microsoft.com/en-us/91df278d-4b28-40c1-a572-cdb91f081517)  
  
  
