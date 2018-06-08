---
title: Trier les données dans une Table | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ef0a5de0958fbea806063c17dd5f9f1912449be4
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="sort-data-in-a-table"></a>Trier des données dans une table 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Vous pouvez trier des données en fonction du texte (de A à Z ou de Z à A) ou des nombres (du plus petit au plus grand ou du plus grand au plus petit) dans une ou plusieurs colonnes.  
  
### <a name="to-sort-the-data-in-a-table-based-on-a-text-column"></a>Pour trier les données dans une table selon une colonne de texte  
  
1.  Dans le générateur de modèles, sélectionnez une colonne de données alphanumériques, une plage de cellules dans une colonne ou assurez-vous que la cellule active se trouve dans une colonne de table qui contient des données alphanumériques, puis cliquez sur la flèche dans l'en-tête de la colonne d'après lequel effectuer le filtrage.  
  
2.  Dans le menu Filtre automatique, effectuez l'une des opérations suivantes :  
  
    -   Pour trier par ordre alphanumérique croissant, cliquez sur **Trier de A à Z**.  
  
    -   Pour trier par ordre alphanumérique décroissant, cliquez sur **Trier de Z à A**.  
  
    > [!NOTE]  
    >  Dans certains cas, les données importées à partir d'une autre application peuvent comporter des espaces de début. Vous devez supprimer ces espaces de début afin de pouvoir trier correctement les données.  
  
### <a name="to-sort-the-data-in-a-table-based-on-a-numeric-column"></a>Pour trier les données dans une table selon une colonne numérique  
  
1.  Dans le générateur de modèles, sélectionnez une colonne de données alphanumériques, une plage de cellules dans une colonne ou assurez-vous que la cellule active se trouve dans une colonne de table qui contient des données alphanumériques, puis cliquez sur la flèche dans l'en-tête de la colonne d'après lequel effectuer le filtrage.  
  
2.  Dans le menu Filtre automatique, effectuez l'une des opérations suivantes :  
  
    -   Pour trier par ordre croissant, cliquez sur **Trier du plus petit au plus grand**.  
  
    -   Pour trier par ordre décroissant, cliquez sur **Trier du plus grand au plus petit**.  
  
    > [!NOTE]  
    >  Si les résultats ne sont pas ce attendus, il se peut que la colonne contienne des nombres stockés sous forme de texte plutôt que sous forme de nombres. Par exemple, les nombres négatifs importés à partir de certains logiciels de comptabilité ou un nombre entré avec un signe ' (apostrophe) de début sont stockés sous forme de texte.  
  
## <a name="see-also"></a>Voir aussi  
 [Filtrer et trier des données](http://msdn.microsoft.com/library/55ebd7a6-2458-4398-911f-fcfeb2413f1b)   
 [Perspectives](../../analysis-services/tabular-models/perspectives-ssas-tabular.md)   
 [Roles](../../analysis-services/tabular-models/roles-ssas-tabular.md)  
  
  
