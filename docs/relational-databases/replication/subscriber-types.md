---
title: Types d’abonnés | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.subscribertypes.f1
ms.assetid: a70656cb-21c9-4489-be77-ccd396747e3b
caps.latest.revision: 28
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3bd057c91b8b092df2e5857e06d9509966fc6903
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="subscriber-types"></a>Types d'Abonnés
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  La réplication de fusion vous permet de préciser les types d'Abonnés qu'une publication doit prendre en charge. La sélection des ensembles de types d'Abonnés définit le *niveau de compatibilité de la publication*ce qui détermine les fonctionnalités pouvant être utilisées par la publication.  
  
 Après un instantané de publication, le niveau de compatibilité de la publication peut alors être rehaussé (en d'autres termes, rendue plus restrictif) dans la page **Général** de la boîte de dialogue **Propriétés de la publication** ; le niveau de compatibilité ne peut cependant pas être réduit.  
  
## <a name="options"></a>Options  
 Sélectionnez chaque type d'Abonné que votre publication doit prendre en charge.  
  
 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]  
 La publication peut utiliser toutes les fonctionnalités.  
  
 [!INCLUDE[ssEW](../../includes/ssew-md.md)]  
 La publication requiert que les fichiers d'instantané soient au format caractère (ceci est géré automatiquement par l'Agent d'instantané). [!INCLUDE[ssEW](../../includes/ssew-md.md)] est également limité par un certain nombre de restrictions indépendantes du niveau de compatibilité.  
  
 Si cette option est sélectionnée, l'option de synchronisation Web est alors activée pour la publication en question. Pour plus d'informations sur la synchronisation Web, consultez [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Afficher et modifier les propriétés d’un serveur de distribution et d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [Référence des propriétés &#40;réplication&#41;](../../relational-databases/replication/properties-reference-replication.md)  
  
  
