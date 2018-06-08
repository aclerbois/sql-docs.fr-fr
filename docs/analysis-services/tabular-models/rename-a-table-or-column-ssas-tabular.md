---
title: Renommer une Table ou colonne | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0c8564b7a61a73937bc5f9a207c98fd77c7b1bb7
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="rename-a-table-or-column"></a>Renommer une table ou une colonne 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Vous pouvez modifier le nom d'une table pendant le processus d'importation en tapant un **Nom convivial** dans la page **Sélectionner des tables et des vues** de l' **Assistant Importation de table**. Vous pouvez également changer le nom des tables et des colonnes si vous importez des données en spécifiant une requête dans la page **Spécifier une requête SQL** de l' **Assistant Importation de table**.  
  
 Une fois que vous avez ajouté les données au modèle, le nom (ou le titre) d'une table s'affiche sous l'onglet Table, au bas du générateur de modèles. Vous pouvez changer le nom de votre table afin de lui donner un nom plus approprié. Vous pouvez également renommer une colonne après l’ajout de données au classeur. Cette option est notamment importante lorsque vous avez importé des données à partir de plusieurs sources et souhaitez vous assurer que les colonnes dans les différentes tables ont des noms qui sont faciles à distinguer.  
  
### <a name="to-rename-a-table"></a>Pour renommer une table  
  
1.  Dans le générateur de modèles, cliquez avec le bouton droit sur l’onglet contenant la table que vous souhaitez renommer, puis sélectionnez **Renommer**.  
  
2.  Tapez le nouveau nom.  
  
    > [!NOTE]  
    >  Vous pouvez modifier d'autres propriétés d'une table, notamment les mappages de colonnes et les informations de connexion, en utilisant la boîte de dialogue **Modifier les propriétés de la table** . Toutefois, vous ne pouvez pas changer son nom dans cette boîte de dialogue.  
  
### <a name="to-rename-a-column"></a>Pour renommer une colonne  
  
1.  Dans le générateur de modèles, double-cliquez sur l’en-tête de la colonne que vous souhaitez renommer ou cliquez dessus avec le bouton droit et sélectionnez **Renommer la colonne** dans le menu contextuel.  
  
2.  Tapez le nouveau nom.  
  
## <a name="naming-requirements-for-columns-and-tables"></a>Exigences concernant l'affectation de noms pour les colonnes et les tables  
 Les mots et caractères suivants ne peuvent pas être utilisés dans le nom d'une table ou d'une colonne :  
  
-   Espaces à gauche ou à droite  
  
-   Caractères de contrôle  
  
-   Les caractères suivants (qui ne sont pas valides dans les noms d’objets Analysis Services) :., ' : / \\*|? & % $! %$!+=()[]5D;{}{}<>  
  
-   Mots clés réservés Analysis Services, notamment les opérateurs et les noms de fonction MDX et DMX.  
  
## <a name="effect-of-renaming-on-existing-tables-columns-and-calculations"></a>Effet de l'attribution d'un nouveau nom sur les tables existantes, les colonnes et les calculs  
 Chaque fois que vous modifiez le nom d'une table, vous modifiez le nom de l'objet de table sous-jacente, qui peut contenir plusieurs colonnes ou mesures. Toutes les colonnes qui figurent dans la table, ainsi que toutes les relations qui utilisent la table, doivent être mises à jour pour utiliser le nouveau nom dans leurs définitions. Cette mise à jour se produit automatiquement dans la plupart des cas.
  
 Tous les calculs qui utilisent la table renommée, ou qui utilisent des colonnes à partir de la table renommée, doivent également être mises à jour, et les données dérivées de ces calculs doivent être actualisées et recalculées. Selon le nombre de tables et de calculs affectés, cette opération peut prendre quelque temps. Par conséquent, le meilleur moment pour renommer des tables est soit pendant le processus d'importation, soit avant de commencer à générer des relations et des calculs complexes.  
  
## <a name="see-also"></a>Voir aussi  
 [Tables et colonnes](../../analysis-services/tabular-models/tables-and-columns-ssas-tabular.md)   
 [Importation à partir de PowerPivot](../../analysis-services/tabular-models/import-from-power-pivot-ssas-tabular.md)   
 [Importer depuis Analysis Services](../../analysis-services/tabular-models/import-from-analysis-services-ssas-tabular.md)  
  
  
