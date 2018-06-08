---
title: Définir l’option de base de données AUTO_SHRINK sur OFF | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
ms.assetid: 16403850-d745-4754-b84f-5f01aaecd24e
caps.latest.revision: 11
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d06bbc185765524c451f7681c8be8f5020f7d527
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-the-autoshrink-database-option-to-off"></a>Définir l'option de base de données AUTO_SHRINK sur OFF
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette règle vérifie si l'option de base de données AUTO_SHRINK est définie avec la valeur OFF. La réduction et le développement fréquents d'une base de données peuvent entraîner une fragmentation physique.  
  
## <a name="best-practices-recommendations"></a>Meilleures pratiques recommandées  
 Définissez l'option de base de données AUTO_SHRINK avec la valeur OFF. Si vous savez que vous n'aurez pas besoin de l'espace que vous récupérez, vous pouvez le récupérer en réduisant manuellement la base de données.  
  
## <a name="for-more-information"></a>Pour plus d'informations  
 Article [315512](http://go.microsoft.com/fwlink/?linkid=117750)de la Base de connaissances Microsoft  
  
## <a name="see-also"></a> Voir aussi  
 [Contrôler et appliquer les meilleures pratiques à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)  
  
  
