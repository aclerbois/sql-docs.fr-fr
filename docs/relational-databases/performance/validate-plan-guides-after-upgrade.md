---
title: Valider des repères de plan après la mise à niveau | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- plan guides [SQL Server], validating after upgrade
ms.assetid: a55ebd88-6f58-454d-b1c4-991b88add522
caps.latest.revision: 6
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: b34ed34d52136866657779463134a030eeb0eb51
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="validate-plan-guides-after-upgrade"></a>Valider des repères de plan après la mise à niveau
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Il est recommandé de réévaluer et de tester les définitions des repères de plan lorsque vous mettez à niveau votre application vers une nouvelle version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les contraintes liées au paramétrage des performances et le comportement de la mise en correspondance des repères de plan peuvent changer. Même si un repère de plan non valide n'entraîne pas l'échec d'une requête, le plan est compilé sans utiliser le repère de plan et peut ne pas être le meilleur choix. Après avoir mis à niveau une base de données vers [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], nous recommandons d'effectuer les tâches suivantes :  
  
-   Validez les repères de plan existants à l’aide de la fonction [sys.fn_validate_plan_guide](../../relational-databases/system-functions/sys-fn-validate-plan-guide-transact-sql.md) .  
  
-   Faites appel à des événements étendus pour rechercher les plans erronés sur période déterminée à l’aide de l’événement [Plan Guide Unsuccessful](../../relational-databases/event-classes/plan-guide-unsuccessful-event-class.md) .  
  
  
