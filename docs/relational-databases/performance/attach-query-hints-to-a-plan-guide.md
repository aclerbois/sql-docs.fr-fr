---
title: Attacher des indicateurs de requête à un repère de plan | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 2131f796-6359-4f9e-9047-da0b3d4dedaf
caps.latest.revision: 6
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: ad96c6c7a9b0904c48f49c5459ce46588bb559b3
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="attach-query-hints-to-a-plan-guide"></a>Attacher des indicateurs de requête à un repère de plan
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Toute combinaison d'indicateurs de requête valides peut être utilisée dans un repère de plan. Lorsqu'un repère de plan correspond à une requête, la clause OPTION spécifiée dans la clause des indicateurs d'un repère de plan est ajoutée à la requête avant qu'elle ne soit compilée et optimisée. Si une requête mise en correspondance avec un repère de plan possède déjà une clause OPTION, les indicateurs de requête spécifiés dans le repère de plan remplacent ceux existant dans la requête. Toutefois, pour qu'un repère de plan corresponde à une requête possédant déjà une clause OPTION, vous devez inclure la clause OPTION de la requête lorsque vous spécifiez le texte de la requête avec laquelle établir la correspondance dans l'instruction sp_create_plan_guide. Si vous souhaitez que les indicateurs spécifiés dans le repère de plan soient ajoutés aux indicateurs qui existent déjà sur la requête, au lieu de les remplacer, vous devez spécifier les indicateurs d'origine et les indicateurs supplémentaires dans la clause OPTION du repère de plan.  
  
> [!CAUTION]  
>  Les repères de plan qui utilisent les indicateurs de requête à mauvais escient peuvent entraîner des problèmes de compilation, d'exécution ou de performances. Les repères de plan doivent être utilisés uniquement par des administrateurs de base de données et des développeurs expérimentés.  
  
## <a name="common-query-hints-used-in-plan-guides"></a>Indicateurs de requêtes courants utilisés dans les repères de plan  
 Les requêtes qui peuvent tirer parti des repères de plan sont généralement les requêtes paramétrables et celles qui peuvent présenter des performances médiocres parce qu'elles utilisent des plans de requête en cache dont les valeurs de paramètre ne représentent pas un scénario pessimiste ou le plus représentatif. Les indicateurs de requête OPTIMIZE FOR et RECOMPILE peuvent être employés pour résoudre ce problème. OPTIMIZE FOR prescrit à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'utiliser une valeur particulière pour un paramètre quand la requête est optimisée. RECOMPILE indique au serveur d'ignorer le plan de requête après exécution, forçant l'optimiseur de requête à recompiler un nouveau plan de requête lors de l'exécution suivante de la même requête. Pour obtenir un exemple, consultez [Repères de plan](../../relational-databases/performance/plan-guides.md).  
  
 Vous pouvez aussi spécifier les indicateurs de table INDEX, FORCESCAN et FORCESEEK en tant qu'indicateurs de requête. Lorsqu'ils sont spécifiés en tant qu'indicateurs de requête, ces indicateurs se comportent comme une table inline ou une astuce de vue. L'indicateur INDEX force l'optimiseur de requête à utiliser uniquement les index spécifiés pour accéder aux données dans la table ou la vue référencée. L'indicateur FORCESEEK force l'optimiseur à utiliser uniquement une opération de recherche d'index pour accéder aux données dans la table ou la vue référencée. Ces indicateurs fournissent des fonctionnalités de repère de plan supplémentaires et vous permettent de mieux contrôler l'optimisation des requêtes qui utilisent le repère de plan.  
  
  
