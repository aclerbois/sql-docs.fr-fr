---
title: Règles relatives à la mise à jour des résultats (Visual Database Tools) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-visual-db
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- View Designer, Results pane
- updating query results
- Query Designer [SQL Server], Results pane
- Results pane
ms.assetid: de131ef0-ccbd-446f-9400-b93c7b8fa537
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ba9929b2588bb3d168e54d49c0eb95278011dbec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rules-for-updating-results-visual-database-tools"></a>Règles relatives à la mise à jour des résultats (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Dans de nombreux cas, vous pouvez mettre à jour le jeu de résultats affiché dans le [volet Résultats](../../ssms/visual-db-tools/results-pane-visual-database-tools.md), avec quelques exceptions.  
  
En général, pour mettre à jour des résultats, le [Concepteur de requêtes et de vues](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) doit disposer de suffisamment d’informations pour identifier de façon unique la ligne de la table. C'est le cas par exemple si la requête inclut une clé primaire dans la liste de sortie. En outre, vous devez être autorisé à mettre à jour la base de données.  
  
Si votre requête est basée sur une vue, vous devriez pouvoir la mettre à jour. Les mêmes règles s'appliquent, sauf qu'il s'agit des tables sous-jacentes de la vue et non simplement de la vue elle-même.  
  
> [!NOTE]  
> Le Concepteur de requêtes et de vues ne peut pas déterminer à l'avance si vous pouvez mettre à jour un ensemble de résultats basé sur une vue. En conséquence, il affiche toutes les vues, même celles que vous ne pouvez pas mettre à jour.  
  
Le tableau suivant récapitule les cas spécifiques dans lesquels vous pourriez mettre à jour ou non des résultats de requête dans le volet Résultats. Dans de nombreux cas, c'est la base de données que vous utilisez qui décide si vous pouvez mettre à jour des résultats de requête.  
  
|Requête|Les résultats peuvent-ils être mis à jour ?|  
|---------|---------------------------|  
|Requête basée sur une table dont la clé primaire figure dans la liste de sortie|Oui (sauf dans les cas répertoriés ci-dessous).|  
|Requête basée sur une table n'ayant ni index unique ni clé primaire|Dépend de la requête et de la base de données. Certaines bases de données autorisent la mise à jour s'il y a suffisamment d'informations pour identifier des enregistrements de façon unique.|  
|Requête basée sur plusieurs tables non jointes|Non.|  
|Requête basée sur des données marquées en lecture seule dans la base de données|Non.|  
|Requête basée sur une vue qui implique une table sans contrainte|Oui (sauf dans les cas répertoriés ci-dessous).|  
|Requête basée sur des tables jointes avec une relation de type un-à-un|Oui (sauf dans les cas répertoriés ci-dessous).|  
|Requête basée sur des tables jointes avec une relation de type un-à-plusieurs|Généralement, oui.|  
|Requête basée sur trois tables ou plus entre lesquelles existe une relation plusieurs-à-plusieurs|Non.|  
|Requête basée sur une table dans laquelle la mise à jour n'est pas autorisée|Suppression autorisée, mais mise à jour interdite.|  
|Requête basée sur une table dans laquelle la suppression n'est pas autorisée|Mise à jour autorisée, mais suppression interdite.|  
|Requête d'agrégation|Non.|  
|Requête basée sur une sous-requête contenant des totaux ou des fonctions d'agrégation|Non.|  
|Requête utilisant le mot clé DISTINCT pour exclure des lignes en double|Non.|  
|Requête dont la clause FROM inclut une fonction définie par l'utilisateur qui renvoie un tableau et qui contient plusieurs instructions de sélection|Non.|  
|Requête dont la clause FROM inclut une fonction définie par l'utilisateur en ligne|Oui.|  
  
En outre, il est quelquefois impossible de mettre à jour certaines colonnes spécifiques des résultats. La liste suivante indique des types de colonnes spécifiques impossibles à mettre à jour dans le volet Résultats.  
  
-   Colonnes basées sur des expressions  
  
-   Colonnes basées sur des fonctions scalaires définies par l'utilisateur  
  
-   Lignes ou colonnes supprimées par un autre utilisateur  
  
-   Lignes ou colonnes verrouillées par un autre utilisateur (en général, les lignes verrouillées peuvent être mises à jour dès qu'elles sont déverrouillées)  
  
-   Colonnes de type Timestamp ou BLOB  
  
## <a name="see-also"></a> Voir aussi  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
