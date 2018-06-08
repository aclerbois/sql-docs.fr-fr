---
title: Afficher des données spatiales dans l’Explorateur d’objets | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 59cca562-e3f5-4257-b868-adcbcc0142cc
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 03d9283ffdca172629fc7d5b84f775c3a936e1c6
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="view-spatial-data-in-object-explorer"></a>Afficher des données spatiales dans l'Explorateur d'objets
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  La fenêtre **Résultats spatiaux** de l'Éditeur de requête propose des outils de mappage visuel permettant d'afficher des résultats de données spatiales en plus des données affichées au format grille dans la fenêtre **Résultats** . Pour afficher des données spatiales dans la fenêtre **Résultats spatiaux** , vos résultats de requête doivent contenir au moins une colonne de données spatiales avec des données géométriques ou géographiques.  
  
### <a name="to-view-spatial-data-in-the-spatial-results-window"></a>Pour afficher des données spatiales dans la fenêtre Résultats spatiaux  
  
1.  Dans l'Éditeur de requête, cliquez sur l'onglet **Résultats spatiaux** .  
  
    > [!NOTE]  
    >  Cette fenêtre n'est pas disponible si vos résultats de requête ne contiennent pas de données spatiales ou si vous spécifiez que les résultats doivent être retournés sous forme de texte.  
  
2.  Sélectionnez la colonne à afficher dans la liste **Sélectionner la colonne spatiale** . Si votre table ne contient qu'une seule colonne spatiale, celle-ci constitue l'option par défaut dans la liste.  
  
3.  Sélectionnez la colonne non spatiale à utiliser comme étiquettes de données dans la liste **Sélectionner la colonne d’étiquette** .  
  
4.  Sélectionnez la projection voulue pour les données géographiques dans la liste **Sélectionner la projection** . La projection par défaut est équirectangulaire ; les autres projections possibles sont les projections de Mercator, de Robinson ou de Bonne.  
  
    > [!NOTE]  
    >  **Sélectionner la projection** n’est pas disponible si votre colonne spatiale contient des données géométriques.  
  
5.  Ajustez le curseur **Zoom** pour augmenter la taille visuelle des éléments mappés. Pour les formes polygonales, l'étiquette est visible uniquement lorsque la forme est suffisamment grande pour contenir le texte d'étiquette.  
  
## <a name="see-also"></a> Voir aussi  
 [Fenêtre Résultats spatiaux](../../relational-databases/scripting/spatial-results-window.md)   
 [Éditeur de requête du moteur de base de données &#40;SQL Server Management Studio&#41;](../../relational-databases/scripting/database-engine-query-editor-sql-server-management-studio.md)   
 [Éditeurs de texte et de requête &#40;SQL Server Management Studio&#41;](../../relational-databases/scripting/query-and-text-editors-sql-server-management-studio.md)  
  
  
