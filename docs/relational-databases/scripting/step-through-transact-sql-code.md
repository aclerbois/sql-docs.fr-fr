---
title: Exécuter pas à pas du code Transact-SQL | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, debugging code
- Transact-SQL debugger, step over
- Transact-SQL debugger, step out
- Transact-SQL debugger, step into
ms.assetid: e09079b8-c4c9-42b4-821b-4ce81a98a086
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: d8eabcfdb8b27da03b06698f52cdaa7f7cc570e0
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="step-through-transact-sql-code"></a>Exécuter pas à pas du code Transact-SQL
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] vous permet de contrôler les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui sont exécutées dans une fenêtre de l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Vous pouvez suspendre le débogueur au niveau d'instructions individuelles, puis afficher l'état des éléments de code à ce stade.  
  
## <a name="breakpoints"></a>Points d’arrêt  
 Un point d'arrêt indique au débogueur de suspendre l'exécution à une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] spécifique. Pour plus d’informations sur les points d’arrêt, consultez [Utilisation de points d’arrêt Transact-SQL](../../relational-databases/scripting/transact-sql-breakpoints.md).  
  
## <a name="controlling-statement-execution"></a>Contrôle de l'exécution d'instructions  
 Dans le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] , vous pouvez spécifier les options suivantes pour exécuter le code [!INCLUDE[tsql](../../includes/tsql-md.md)] à partir de l'instruction actuelle :  
  
-   Exécuter le code jusqu'au point d'arrêt suivant.  
  
-   Effectuer un pas à pas détaillé dans l'instruction suivante.  
  
     Si l'instruction suivante appelle une procédure stockée, une fonction ou un déclencheur [!INCLUDE[tsql](../../includes/tsql-md.md)] , le débogueur affiche une nouvelle fenêtre de l'éditeur de requête contenant le code du module. La fenêtre est en mode débogage, et l'exécution s'arrête à la première instruction dans le module. Vous pouvez ensuite parcourir le code du module, en définissant par exemple des points d'arrêt ou en exécutant le code pas à pas.  
  
-   Effectuer un pas à pas principal dans l'instruction suivante.  
  
     L'instruction suivante est exécutée. Toutefois, si l'instruction appelle une procédure stockée, une fonction ou un déclencheur, le code de module s'exécute jusqu'à ce qu'il se termine, et les résultats sont retournés au code appelant. Si vous êtes certain qu'une procédure stockée ne comporte aucune erreur, vous pouvez faire un pas à pas principal. L'exécution s'arrête à l'instruction qui suit l'appel à la procédure stockée, à la fonction ou au déclencheur.  
  
-   Effectuer un pas à pas sortant dans une procédure stockée, une fonction ou un déclencheur.  
  
     L'exécution s'arrête à l'instruction qui suit l'appel à la procédure stockée, à la fonction ou au déclencheur.  
  
-   Exécuter le code à partir de l'emplacement actuel jusqu'à l'emplacement actuel du pointeur et ignorer tous les points d'arrêt.  
  
 Le tableau suivant répertorie les différentes façons dont vous pouvez contrôler l'exécution des instructions dans le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
|Action|Action à effectuer :|  
|------------|---------------------|  
|Exécuter toutes les instructions à partir de l'instruction actuelle jusqu'au point d'arrêt suivant|Cliquez sur **Continuer** dans le menu **Déboguer** .<br /><br /> Cliquez sur le bouton **Continuer** dans la barre d’outils **Déboguer** .|  
|Effectuer un pas à pas détaillé dans l'instruction ou le module suivant|Cliquez sur **Pas à pas détaillé** dans le menu **Déboguer** .<br /><br /> Cliquez sur le bouton **Pas à pas détaillé** dans la barre d’outils **Déboguer** .<br /><br /> Appuyez sur F11.|  
|Effectuer un pas à pas principal dans l'instruction ou le module suivant|Cliquez sur **Pas à pas principal** dans le menu **Déboguer** .<br /><br /> Cliquez sur le bouton **Pas à pas principal** dans la barre d’outils **Déboguer** .<br /><br /> Appuyez sur F10.|  
|Effectuer un pas à pas sortant dans un module|Cliquez sur **Pas à pas sortant** dans le menu **Déboguer** .<br /><br /> Cliquez sur le bouton **Pas à pas sortant** dans la barre d’outils **Déboguer** .<br /><br /> Appuyez sur Maj+F11.|  
|Exécuter le code jusqu'à l'emplacement du curseur actuel|Cliquez avec le bouton droit dans la fenêtre de l’éditeur de requête, puis cliquez sur **Exécuter jusqu’au curseur**.<br /><br /> Appuyez sur Ctrl+F10.|  
  
## <a name="see-also"></a> Voir aussi  
 [Informations du débogueur Transact-SQL](../../relational-databases/scripting/transact-sql-debugger-information.md)  
  
  
