---
title: Déplacer des éléments dans une solution | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-solutions
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- moving items
- solutions [SQL Server Management Studio], item relocation
- relocating items
ms.assetid: b40a24eb-f528-4e70-b26e-5eaf6e0ed1ed
caps.latest.revision: 3
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2c803d45b83e7fdacd51ecf43019b0877d68bea5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="move-items-in-a-solution"></a>Déplacer des éléments dans une solution
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Les éléments de projet des projets [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] sont les requêtes, les connexions et les fichiers divers. Vous pouvez déplacer les requêtes et les fichiers divers d'un projet vers un autre dans l'Explorateur de solutions, mais vous ne pouvez pas déplacer les connexions.  
  
### <a name="to-move-items-in-solution-explorer"></a>Pour déplacer des éléments dans l'Explorateur de solutions  
  
1.  Dans l'Explorateur de solutions, sélectionnez l'élément que vous souhaitez déplacer.  
  
2.  Dans le menu **Edition** , cliquez sur **Couper**.  
  
3.  Dans l'Explorateur de solutions, sélectionnez la destination.  
  
4.  Dans le menu **Edition** , cliquez sur **Coller**.  
  
Vous pouvez déplacer des éléments en faisant glisser les requêtes et les fichiers divers dans l'Explorateur de solutions. Faire glisser ces éléments vous permet de voir le résultat de l'opération de glissement. Les requêtes déplacées d'un type de projet vers un autre peuvent être considérées comme des fichiers divers dans le projet cible.  
  
> [!NOTE]  
> Le déplacement d'une requête connectée n'entraîne pas le déplacement de la connexion vers le projet cible. Une fois déplacée vers le projet cible, la requête perd alors sa connexion.  
  
## <a name="see-also"></a> Voir aussi  
[Explorateur de solutions](../../ssms/solution/solution-explorer.md)  
[Copier des éléments d'une solution](../../ssms/solution/copy-items-in-a-solution.md)  
  
