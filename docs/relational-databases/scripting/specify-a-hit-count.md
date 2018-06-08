---
title: Spécifier un nombre d’accès | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- vs.debug.breakpt.hitcount
helpviewer_keywords:
- Transact-SQL debugger, breakpoint hit count
ms.assetid: 24836939-94ed-4e57-aa85-5d6938d859e4
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 169f795ed2671af795802355a2ff0b853a7c6362
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="specify-a-hit-count"></a>Spécifier un nombre d'accès
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Un nombre d'accès à un point d'arrêt est un compteur incrémenté par le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] chaque fois que le point d'arrêt est atteint. Si le nombre d'accès spécifié est atteint et si les conditions de point d'arrêt spécifiées sont satisfaites, le débogueur effectue l'action spécifiée pour le point d'arrêt.  
  
## <a name="hit-count-considerations"></a>Considérations sur le nombre d'accès  
 Par défaut, l'exécution s'arrête pour chaque accès à un point d'arrêt. Vous pouvez choisir entre les options suivantes :  
  
-   Toujours s'arrêter (valeur par défaut).  
  
-   Arrêter lorsque le nombre d'accès est égal à une valeur spécifiée.  
  
-   Arrêter lorsque le nombre d'accès est égal au multiple d'une valeur spécifiée.  
  
-   Arrêter lorsque le nombre d'accès est supérieur ou égal à une valeur spécifiée.  
  
 Les nombres d'accès à un point d'arrêt sont incrémentés dans le cadre de l'étendue d'une session de débogage. Tous les nombres d'accès sont remis à zéro au démarrage de chaque session de débogage.  
  
 Si vous souhaitez connaître le nombre d'accès à un point d'arrêt sans que l'exécution s'arrête, spécifiez un nombre d'accès avec une valeur très élevée afin que le point d'arrêt n'entraîne jamais l'arrêt.  
  
 L'action par défaut pour un point d'arrêt consiste à arrêter l'exécution lorsque le nombre d'accès et la condition de point d'arrêt sont tous les deux satisfaits. Pour plus d’informations sur la spécification d’autres actions, consultez [Spécifier une action de point d’arrêt](../../relational-databases/scripting/specify-a-breakpoint-action.md).  
  
#### <a name="to-specify-a-hit-count"></a>Pour spécifier un nombre d'accès  
  
1.  Dans la fenêtre de l’éditeur, cliquez avec le bouton droit sur le glyphe du point d’arrêt, puis cliquez sur **Nombre d’accès** dans le menu contextuel.  
  
     -ou-  
  
     Dans la fenêtre **Points d’arrêt** , cliquez avec le bouton droit sur le glyphe du point d’arrêt, puis cliquez sur **Nombre d’accès** dans le menu contextuel.  
  
2.  Dans la boîte de dialogue **Nombre d’accès à un point d’arrêt** , sélectionnez le comportement que vous souhaitez dans la zone **Lorsque le point d’arrêt est atteint** .  
  
     Si vous choisissez un paramètre autre que **Toujours s’arrêter**, une zone de texte apparaît à droite de la liste. Entrez un entier dans cette zone de texte pour spécifier le nombre d'accès souhaité.  
  
3.  Cliquez sur **OK** pour implémenter les modifications ou sur **Annuler** pour fermer sans appliquer les modifications.  
  
#### <a name="to-view-or-reset-the-current-hit-count"></a>Afficher ou réinitialiser le nombre d'accès actuel  
  
1.  Dans la fenêtre de l’éditeur, cliquez avec le bouton droit sur le glyphe du point d’arrêt, puis cliquez sur **Nombre d’accès** dans le menu contextuel.  
  
     -ou-  
  
     Dans la fenêtre **Points d’arrêt** , cliquez avec le bouton droit sur le glyphe du point d’arrêt, puis cliquez sur **Nombre d’accès** dans le menu contextuel.  
  
2.  Dans la boîte de dialogue **Nombre d’accès à un point d’arrêt** , le **Nombre d’accès actuel** est affiché juste au-dessus du bouton **Réinitialiser** .  
  
3.  Cliquez sur **Réinitialiser** si vous souhaitez réinitialiser le nombre d’accès actuel à zéro.  
  
4.  Cliquez sur **OK** ou sur **Annuler** pour fermer la boîte de dialogue.  
  
## <a name="see-also"></a> Voir aussi  
 [Spécifier une condition de point d'arrêt](../../relational-databases/scripting/specify-a-breakpoint-condition.md)  
  
  
