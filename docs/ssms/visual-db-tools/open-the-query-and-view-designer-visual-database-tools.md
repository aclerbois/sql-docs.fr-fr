---
title: Ouvrir le Concepteur de requêtes et de vues (Visual Database Tools) | Microsoft Docs
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
- opening View Designer
- View Designer, opening
- Query Designer [SQL Server], opening
- opening Query Designer
ms.assetid: b473f258-d53c-41c0-9ad9-528a2ff141f4
caps.latest.revision: 4
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 7c899f595c0c351982f7d4e0bfbf3ad52c17f0f0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="open-the-query-and-view-designer-visual-database-tools"></a>Ouvrir le Concepteur de requêtes et de vues (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Le Concepteur de requêtes et de vues s'affiche lorsque vous ouvrez la définition d'une vue, lorsque vous affichez les résultats d'une requête ou d'une vue, ou encore lorsque vous créez ou ouvrez une requête. Il se compose de quatre volets distincts :  
  
-   Ce volet donne une représentation graphique des tables ou des objets tables que vous avez sélectionnés dans la connexion de données. Il montre également les jointures entre ces tables et/ou ces objets.  
  
-   Ce volet permet d'utiliser une grille similaire à une feuille de calcul pour spécifier des options de requêtes, telles que les colonnes de données à afficher, la façon de trier les résultats et les lignes à sélectionner.  
  
-   Vous pouvez utiliser le volet SQL pour créer votre propre instruction SQL, ou utiliser le volet Critères et le volet Schéma pour créer l'instruction, auquel cas les instructions SQL sont créées dans le volet SQL. Pendant la génération de la requête, le volet SQL se met automatiquement à jour et se reformate pour que sa lecture soit facile.  
  
-   Le volet Résultats montre les résultats de la dernière requête Select exécutée. (Les résultats des autres types de requêtes sont affichés dans des boîtes de messages.)  
  
-   Ces volets sont utiles pour travailler à la fois avec des requêtes et avec des vues.  
  
-   Lorsque vous ouvrez une vue ou une requête, une partie ou la totalité des volets s'affichent. Les volets qui s'ouvrent sont déterminés par les paramètres de la boîte de dialogue **Options** et du système de gestion de bases de données auquel vous êtes connecté. Par défaut, les quatre volets s'ouvrent.  
  
### <a name="to-open-the-query-and-view-designer-for-a-view"></a>Pour ouvrir le Concepteur de requêtes et de vues pour une vue  
  
1.  Dans l'Explorateur d'objets, cliquez avec le bouton droit sur la vue que vous souhaitez ouvrir et cliquez sur **Conception** ou sur **Ouvrir la vue**.  
  
    Si vous avez choisi **Conception**, les volets du Concepteur de requêtes et de vues s'ouvrent comme défini par les options sélectionnées dans la boîte de dialogue **Options** . Si vous avez choisi **Ouvrir la vue**, seul le volet Résultats s'ouvre par défaut.  
  
### <a name="to-open-the-query-and-view-designer-for-an-existing-query"></a>Pour ouvrir le Concepteur de requêtes et de vues pour une requête existante  
  
1.  Dans l'Explorateur de solutions, développez le dossier **Requêtes** .  
  
2.  Double-cliquez sur la requête à ouvrir.  
  
3.  Mettez en surbrillance les instructions de la requête, cliquez avec le bouton droit sur la zone en surbrillance puis cliquez sur **Concevoir une requête dans l'éditeur**.  
  
## <a name="see-also"></a> Voir aussi  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
[Outils du concepteur de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md)  
  
