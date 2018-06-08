---
title: Créer des alias de tables (Visual Database Tools) | Microsoft Docs
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
- table aliases [SQL Server]
- aliases [SQL Server], tables
ms.assetid: 49e61e85-8abf-4ca7-8c70-7e9f8f1078bd
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 72c601874272e871b6d413907945e6432c5eaeaf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-table-aliases-visual-database-tools"></a>Créer des alias de tables (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Les alias servent à faciliter les travaux impliquant des manipulations de noms de tables. L'emploi d'alias s'avère utile dans les circonstances suivantes :  
  
-   Vous souhaitez raccourcir et simplifier la lecture de l’instruction dans le [volet SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md) .  
  
-   Vous vous référez souvent au nom de la table dans votre requête (lors de la qualification des noms de colonnes) et vous souhaitez vous assurer que vous ne dépassez pas une longueur de caractère spécifique dans votre requête (certaines bases de données imposent une longueur maximale aux requêtes).  
  
-   Vous utilisez plusieurs instances de la même table (dans une jointure réflexive, par exemple) et vous avez besoin d'une méthode vous permettant de vous référer à l'une ou l'autre instance.  
  
Vous pouvez, par exemple, créer un alias `"e"` pour le nom de la table `employee_information`, puis vous référer à cette table sous la forme `"e"` dans le reste de la requête.  
  
### <a name="to-create-an-alias-for-a-table-or-table-valued-object"></a>Pour créer un alias pour une table ou un objet table  
  
1.  Ajoutez la table ou l'objet table à votre requête.  
  
2.  Dans le **volet Schéma**, cliquez avec le bouton droit sur l’objet pour lequel vous souhaitez créer un alias, puis sélectionnez **Propriétés** dans le menu contextuel.  
  
3.  Dans la fenêtre **Propriétés** , entrez l’alias dans le champ **Alias** .  
  
## <a name="see-also"></a> Voir aussi  
[Ajouter des tables à des requêtes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md)  
[Trier et regrouper des résultats de requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
[Résumer les résultats de la requête &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md)  
[Effectuer des opérations de base concernant les requêtes &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/perform-basic-operations-with-queries-visual-database-tools.md)  
  
