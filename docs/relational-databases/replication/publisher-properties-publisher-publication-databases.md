---
title: Propriétés du serveur de publication - Serveur de publication, Bases de données de publication | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rep.configdistwizard.pubproperties.pubdb.f1
helpviewer_keywords:
- Publisher Properties dialog box
ms.assetid: 574ea2e7-4e7b-4733-ab52-429ca93c7b0a
caps.latest.revision: 21
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 666e3998873016dc1a99cfa2214fe120dee12bfb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="publisher-properties---publisher-publication-databases"></a>Propriétés du serveur de publication - Serveur de publication, Bases de données de publication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La page **Bases de données de publication** de la boîte de dialogue **Propriétés du serveur de publication** permet à un utilisateur dans le rôle serveur fixe **sysadmin** d'autoriser la réplication des bases de données. L'activation d'une base de données ne publie pas la base de données ; elle permet à un utilisateur du rôle fixe de base de données **db_owner** de créer des publications dans la base de données.  
  
## <a name="options"></a>Options  
 **Transactionnelle**  
 Activez cette case à cocher pour permettre aux utilisateurs du rôle de base de données fixe **db_owner** de créer des publications d'instantanés ou des publications transactionnelles dans la base de données.  
  
 **Fusion**  
 Activez cette case à cocher pour permettre aux utilisateurs du rôle de base de données fixe **db_owner** de créer des publications de fusion dans la base de données.  
  
## <a name="see-also"></a> Voir aussi  
 [Afficher et modifier les propriétés d’un serveur de distribution et d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [Référence des propriétés &#40;réplication&#41;](../../relational-databases/replication/properties-reference-replication.md)  
  
  
