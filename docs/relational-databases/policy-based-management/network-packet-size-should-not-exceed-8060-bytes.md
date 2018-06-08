---
title: La taille du paquet réseau ne doit pas dépasser 8060 octets | Microsoft Docs
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
helpviewer_keywords:
- Best Practices [Database Engine]
ms.assetid: 86db5da1-afe4-4fbb-8bf8-33cedc7e4361
caps.latest.revision: 8
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 3a79cd7cd9047b93e154bf63bad5b4968a62ff9b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="network-packet-size-should-not-exceed-8060-bytes"></a>La taille du paquet réseau ne doit pas dépasser 8060 octets
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Si la valeur spécifiée pour sp_configure 'network packet size' ou si la taille du paquet réseau de tout utilisateur connecté est supérieure à 8060 octets, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue des opérations d'allocation de mémoire différentes. Cela peut entraîner une augmentation de l'espace d'adressage virtuel de processus qui n'est pas réservé pour le pool de mémoires tampons.  
  
## <a name="best-practices-recommendations"></a>Meilleures pratiques recommandées  
 La taille du paquet réseau ne doit pas dépasser 8060 octets.  
  
## <a name="for-more-information"></a>Pour plus d'informations  
 [Article 903002 de la Base de connaissances Microsoft](http://go.microsoft.com/fwlink/?linkid=117749)  
  
## <a name="see-also"></a> Voir aussi  
 [Contrôler et appliquer les meilleures pratiques à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)  
  
  
