---
title: Transactions XTP de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 443d67e4-1c7f-41d7-b18d-2d657f58c22a
caps.latest.revision: 7
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: a9da78f30f38fb5cbb72e573497f93a138fec37e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-xtp-transactions"></a>Transactions XTP de SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  L’objet de performance des transactions XTP de SQL Server comprend des compteurs liés aux transactions impliquant l’OLTP en mémoire dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Ce tableau décrit les compteurs **Transactions XTP de SQL Server** .  
  
|Compteur|Description|  
|-------------|-----------------|  
|**Abandons en cascade par seconde**|Nombre de transactions restaurées en raison d'une restauration de dépendance de validation (en moyenne), par seconde.|  
|**Dépendances de validation prises par seconde**|Nombre de dépendances de validation prises par des transactions (en moyenne), par seconde.|  
|**Transactions en lecture seule préparées par seconde**|Nombre de transactions en lecture seule qui ont été préparées pour le traitement de validation, par seconde.|  
|**Actualisations de point de sauvegarde par seconde**|Nombre de fois qu'un point de sauvegarde a été « actualisé » (en moyenne), par seconde. Une actualisation de point de sauvegarde se produit lorsqu'un point de sauvegarde existant est réinitialisé au point actuel dans la durée de vie de la transaction.|  
|**Restaurations de point de sauvegarde par seconde**|Nombre de fois qu'une transaction a été restaurée à un point de sauvegarde (en moyenne), par seconde.|  
|**Points de sauvegarde créés par seconde**|Nombre de points de sauvegarde créés (en moyenne), par seconde.|  
|**Échec de validation des transactions par seconde**|Nombre de transactions ayant échoué lors du traitement de validation (en moyenne), par seconde.|  
|**Transactions abandonnées par l'utilisateur par seconde**|Nombre de transactions abandonnées par l'utilisateur (en moyenne), par seconde.|  
|**Transactions abandonnées par seconde**|Nombre de transactions abandonnées par l'utilisateur et par le système (en moyenne), par seconde.|  
|**Transactions créées par seconde**|Nombre de transactions créées dans le système (en moyenne), par seconde.<br /><br /> Les transactions XTP sont comptées différemment que les transactions sur disque (comme obtenu par réflexion dans Databases:Transactions/sec). Par exemple, les transactions created/sec comptent les transactions read/only, contrairement à Databases:Transactions/sec.|  
  
## <a name="see-also"></a> Voir aussi  
 [SQL Server XTP &#40;OLTP en mémoire&#41;, compteurs de performances](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)  
  
  
