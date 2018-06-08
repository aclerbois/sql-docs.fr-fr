---
title: Tables et colonnes | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 10133b2843c01f16134c028140394247c2669236
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="tables-and-columns"></a>Tables et colonnes 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Après avoir ajouté des tables et des données dans un modèle à l'aide de l'Assistant Importation de table, vous pouvez commencer à utiliser les tables en ajoutant de nouvelles colonnes de données, en créant des relations entre les tables, en définissant des calculs qui étendent les données, et en filtrant et en triant des données dans les tables pour un affichage plus aisé.  
  
 Sections de cette rubrique :  
  
-   [Avantages](#bkmk_benefits)  
  
-   [Utilisation de tables et de colonnes](#bkmk_working)  
  
-   [Tâches associées](#bkmk_related_tasks)  
  
##  <a name="bkmk_benefits"></a> Avantages  
 Les tables, dans les modèles tabulaires, fournissent l'infrastructure dans laquelle les colonnes et d'autres métadonnées sont définies. Les tables incluent les éléments suivants :  
  
 **Définition de table**  
 La définition de table se compose de l'ensemble de colonnes. Les colonnes peuvent être importées à partir d'une source de données ou être ajoutées manuellement, comme pour les colonnes calculées.  
  
 **Métadonnées de table**  
 Les relations, les mesures, les rôles, les perspectives et les données collées sont des métadonnées qui définissent des objets dans le contexte d'une table.  
  
 **Données**  
 Les données sont remplies dans les colonnes de table lorsque vous importez d'abord les tables à l'aide de l'Assistant Importation de table ou en créant de nouvelles données dans les colonnes calculées. Lorsque des données changent au niveau de la source, ou lorsqu'un modèle est supprimé de la mémoire, vous devez exécuter une opération de traitement pour remplir à nouveau les données dans les tables.  
  
##  <a name="bkmk_working"></a> Utilisation de tables et de colonnes  
 Dans le générateur de modèles, vous ne créez pas de nouvelles tables de modèle directement. Un nouvel onglet est créé automatiquement chaque fois que des données sont collées ou importées depuis une autre source de données. Chaque onglet (dans le générateur de modèles) contient une table de données, qui peut inclure les éléments suivants :  
  
-   une table ou une vue unique d'une base de données relationnelle, ou d'une autre source non relationnelle, telle qu'un cube Analysis Services ;  
  
-   un jeu tabulaire de données importé à partir d'un fichier de flux de données ou d'un fichier texte.  
  
-   Une combinaison de données relationnelles et de données tabulaires (HTML) copiées et collées dans la table.  
  
 Lorsque vous importez des données, chaque table, vue, feuille ou fichier de données est ajouté(e) en tant que table dans le générateur de modèles. Vous ajoutez en général des données de différentes sources dans des onglets distincts, mais vous pouvez combiner des données dans une table individuelle en utilisant **Coller** et **Coller par ajout**. Pour plus d’informations, consultez [copier et coller des données](../../analysis-services/tabular-models/ssas-import-data-copy-and-paste-data.md).  
  
 Après avoir ajouté les données dont vous avez besoin, vous pouvez créer des relations supplémentaires entre les tables, rechercher ou référencer des valeurs associées dans d'autres tables ou créer des valeurs dérivées en ajoutant de nouvelles colonnes calculées.  
  
 Si vous utilisez des jeux de données très volumineux, vous pouvez filtrer certaines données afin qu'elles ne soient pas visibles. Vous pouvez également trier les données dans un ordre différent. À l'aide du générateur de modèles, vous pouvez utiliser les fonctionnalités de filtre, de tri et de masquage pour afficher, ou non, des colonnes entières ou certaines données.  
  
##  <a name="bkmk_related_tasks"></a> Tâches associées  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Ajouter des colonnes à une table](../../analysis-services/tabular-models/add-columns-to-a-table-ssas-tabular.md)|Décrit comment ajouter une colonne source à une définition de table.|  
|[Supprimer une colonne](../../analysis-services/tabular-models/delete-a-column-ssas-tabular.md)|Explique comment supprimer une colonne de table de modèle à l'aide du concepteur de modèles ou à l'aide de la boîte de dialogue Propriétés de la table.|  
|[Changer des mappages de filtres de lignes, de tables ou de colonnes](../../analysis-services/tabular-models/change-table-column-or-row-filter-mappings-ssas-tabular.md)|Décrit comment modifier des mappages de filtres de lignes, de tables ou de colonnes à l'aide de l'aperçu de table ou de l'éditeur de requête SQL dans la boîte de dialogue Modifier les propriétés de la table.|  
|[Spécifier la marque comme table de dates pour l'utiliser avec Time Intelligence](../../analysis-services/tabular-models/specify-mark-as-date-table-for-use-with-time-intelligence-ssas-tabular.md)|Explique comment utiliser la boîte de dialogue Marquer en tant que table de dates pour spécifier une table de dates et une colonne d'identificateur unique. La spécification d'une table de dates et d'un identificateur unique est nécessaire lorsqu'on utilise des fonctions Time Intelligence dans des formules DAX.|  
|[Ajouter une table](../../analysis-services/tabular-models/add-a-table-ssas-tabular.md)|Explique comment ajouter une table à partir d'une source de données à l'aide d'une connexion existante à la source de données.|  
|[Supprimer une table](../../analysis-services/tabular-models/delete-a-table-ssas-tabular.md)|Décrit comment supprimer des tables dans votre base de données model de l'espace de travail dont vous n'avez plus besoin.|  
|[Renommer une table ou une colonne](../../analysis-services/tabular-models/rename-a-table-or-column-ssas-tabular.md)|Décrit comment renommer une table ou une colonne pour la rendre plus identifiable dans votre modèle.|  
|[Définir le type de données d’une colonne](../../analysis-services/tabular-models/set-the-data-type-of-a-column-ssas-tabular.md)|Décrit la manière de modifier le type de données d'une colonne. Le type de données définit comment les données de la colonne sont stockées et présentées.|  
|[Masquer ou figer des colonnes](../../analysis-services/tabular-models/hide-or-freeze-columns-ssas-tabular.md)|Décrit comment masquer les colonnes que vous ne souhaitez pas afficher et comment conserver une zone d'un visible modèle pendant que vous faites défiler vers une autre zone du modèle en figeant (verrouillant) des colonnes spécifiques dans une région.|  
|[Colonnes calculées](../../analysis-services/tabular-models/ssas-calculated-columns.md)|Les rubriques de cette section décrivent comment utiliser les colonnes calculées pour ajouter des données agrégées à votre modèle.|  
|[Filtrer et trier des données](http://msdn.microsoft.com/library/55ebd7a6-2458-4398-911f-fcfeb2413f1b)|Les rubriques de cette section décrivent comment filtrer ou trier des données à l'aide de contrôles dans le générateur de modèles.|  
  
  
