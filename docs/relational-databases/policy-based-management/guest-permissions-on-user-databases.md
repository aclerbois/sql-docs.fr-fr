---
title: Autorisations Invité sur les bases de données utilisateur | Microsoft Docs
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
ms.assetid: 540f1c6d-df51-497e-958a-3a0f429d2920
caps.latest.revision: 8
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 86f0403d3ca4d5c279663664dcf5291815921411
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="guest-permissions-on-user-databases"></a>Autorisations Invité sur les bases de données utilisateur
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette règle détermine si l'utilisateur invité a l'autorisation d'accéder à la base de données. Cette règle s'applique uniquement aux bases de données utilisateur.  
  
## <a name="best-practices-recommendations"></a>Meilleures pratiques recommandées  
 Révoquez l'autorisation d'accès à la base de données octroyée à l'utilisateur invité si elle n'est pas nécessaire.  
  
 Vous ne pouvez pas supprimer l’utilisateur guest, mais vous pouvez le désactiver en révoquant son autorisation CONNECT. Pour ce faire, vous devez exécuter REVOKE CONNECT FROM GUEST dans toute base de données autre que la base master, tempdb ou msdb.  
  
## <a name="for-more-information"></a>Pour plus d'informations  
 [Sécurisation de SQL Server](../../relational-databases/security/securing-sql-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Contrôler et appliquer les meilleures pratiques à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)  
  
  
