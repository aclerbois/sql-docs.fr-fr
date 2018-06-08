---
title: Espion, fenêtre | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- vs.debug.watch
helpviewer_keywords:
- Watch Window [Transact-SQL]
ms.assetid: 23f3baa4-14c2-4262-92f7-3f43fcfa0436
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d84e3ad2655223dfa0d03fb76491167c5ff136a9
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="transact-sql-debugger---watch-window"></a>Débogueur Transact-SQL - Fenêtre Espion
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  La fenêtre **Espion** affiche des informations sur les expressions que vous avez sélectionnées. Les fenêtres Espion peuvent être au nombre de quatre, au maximum : **Espion 1**, **Espion 2, Espion 3**et **Espion 4**. Les expressions sont évaluées dans l’étendue du frame de pile des appels actuellement sélectionné dans la fenêtre **Pile des appels** . Vous devez être en mode débogage pour surveiller les variables et les expressions.  
  
## <a name="task-list"></a>Liste des tâches  
 **Pour accéder aux fenêtres Espion**  
  
-   Dans le menu **Déboguer** , cliquez sur **Fenêtres**, sur **Espion**puis sur **Espion 1**, **Espion 2, Espion 3**ou **Espion 4**.  
  
 **Pour modifier la valeur d'une expression**  
  
-   Cliquez avec le bouton droit sur l’expression, puis sélectionnez **Modifier la valeur**.  
  
## <a name="columns"></a>Colonnes  
 **Nom**  
 Expressions répertoriées par le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] . Les expressions suivantes sont prises en charge :  
  
-   variables ;  
  
-   Paramètres.  
  
-   Fonctions système dont le nom commence par @@.  
  
-   Expressions générées par l’application d’opérateurs à une ou plusieurs variables, un ou plusieurs paramètres ou une ou plusieurs fonctions système, comme @IntegerCounter + 1 ou FirstName + LastName.  
  
-   Instructions Transact-SQL qui retournent une seule valeur, par exemple : SELECT CharacterCol FROM MyTable WHERE PrimaryKey = 1.  
  
 **Value**  
 Affiche la valeur retournée après que le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] a évalué l’expression spécifiée dans **Nom**.  
  
 Si la longueur d'une expression dépasse la largeur de la colonne **Valeur** , une info-bulle affiche la valeur complète lorsque vous placez le pointeur sur la cellule **Valeur** de cette expression.  
  
 La présence d'une icône de loupe dans une cellule **Valeur** indique que le visualiseur du débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] est disponible. Dans la liste, vous pouvez spécifier **Visualiseur de texte**, **Visualiseur XML**ou **Visualiseur HTML**. Pour démarrer un visualiseur du débogueur, cliquez sur l'icône de loupe. Le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] ouvre une boîte de dialogue qui affiche les données dans un format adapté au type de données.  
  
 **Type**  
 Affiche le type de données de l'expression.  
  
## <a name="see-also"></a> Voir aussi  
 [Débogueur Transact-SQL](../../relational-databases/scripting/transact-sql-debugger.md)   
 [Informations du débogueur Transact-SQL](../../relational-databases/scripting/transact-sql-debugger-information.md)   
 [Variables locales (fenêtre)](../../relational-databases/scripting/transact-sql-debugger-locals-window.md)   
 [Fenêtre Pile des appels](../../relational-databases/scripting/transact-sql-debugger-call-stack-window.md)   
 [Boîte de dialogue Espion express](../../relational-databases/scripting/transact-sql-debugger-quickwatch-dialog-box.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  
