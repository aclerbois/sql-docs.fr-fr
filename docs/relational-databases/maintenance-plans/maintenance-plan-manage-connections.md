---
title: Plan de maintenance (Gérer les connexions) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: maintenance-plans
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.maint.connections.f1
ms.assetid: 95ad9375-6584-423e-b9de-0e86782f8017
caps.latest.revision: 14
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 99250afb057830b8a6bc06eb5c2727e8d38bab88
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="maintenance-plan-manage-connections"></a>Plan de maintenance (Gérer les connexions)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez la boîte de dialogue **Gérer les connexions** pour spécifier les propriétés des connexions utilisées par les plans de maintenance. Lorsque vous créez un plan de maintenance, une connexion locale est créée avec le serveur où vous avez créé le plan. Utilisez cette connexion pour créer des tâches qui effectuent des travaux sur cette connexion locale. Si nécessaire, utilisez la boîte de dialogue **Gérer les connexions** pour les ajouter. Lorsque vous configurez des connexions supplémentaires, elles apparaissent dans la zone Connexions de chaque tâche.  
  
## <a name="options"></a>Options  
 **Server**  
 Instance du serveur.  
  
 **Authentification**  
 Indique si la connexion utilise l'authentification Windows ou l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

> [!IMPORTANT]  
> Le package est stocké dans la base de données **msdb** avec le niveau **ProtectionLevel** défini sur **ServerStorage** ; par conséquent, quand *l’authentification SQL Server* est utilisée, le mot de passe n’est pas chiffré dans **msdb**. Vous pouvez utiliser *l’authentification SQL Server* tant que **msdb** est sécurisée, mais il est recommandé d’utiliser *l’authentification Windows*

## <a name="see-also"></a> Voir aussi  
 [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md)  
  
  
