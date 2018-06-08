---
title: Réplication SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], about
- replication [SQL Server]
ms.assetid: 3a5f4592-3c61-4b4d-9ceb-39716aeeba41
caps.latest.revision: 58
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e0b9fd3b54f0ccb20f9e5ff632b1c63e11b6f641
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-replication"></a>Réplication SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La réplication repose sur un ensemble de technologies qui permettent de copier et de distribuer des données et des objets de base de données d'une base de données vers une autre, puis de synchroniser ces bases de données afin de préserver leur cohérence. Utilisez la réplication pour distribuer des données en différents emplacements et à des utilisateurs distants ou mobiles sur des réseaux locaux et étendus, des connexions d’accès à distance, des connexions sans fil, et Internet.  
  
 La réplication transactionnelle est généralement utilisée dans des scénarios serveur à serveur qui nécessitent un débit élevé, notamment pour l'amélioration de l'extensibilité et de la disponibilité, l'entrepôt de données et la création de rapports, l'intégration de données depuis plusieurs sites, l'intégration de données hétérogènes et le déchargement du traitement par lots. La réplication de fusion est conçue essentiellement pour les applications mobiles ou les applications de serveur distribuées contenant des conflits de données possibles. Les scénarios courants incluent l'échange de données avec des utilisateurs mobiles, les applications de point de vente aux consommateurs (POS, Consumer Point of Sale) et l'intégration des données à partir de plusieurs sites. La réplication d'instantané est utilisée pour fournir le jeu des données initiales pour la réplication transactionnelle et de fusion ; elle peut s'utiliser également lorsque des actualisations complètes des données sont nécessaires. Avec ces trois types de réplication, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit un système souple et puissant de synchronisation des données dans votre entreprise. La réplication dans SQLCE 3.5 et SQLCE 4.0 est prise en charge sur [!INCLUDE[win8srv](../../includes/win8srv-md.md)] et [!INCLUDE[win8](../../includes/win8-md.md)].  

 Comme alternative à la réplication,les bases de données peuvent être synchronisées à l'aide de Microsoft Sync Framework. Sync Framework inclut les composants et une API intuitive et flexible qui facilitent la synchronisation entre des bases de données SQL Server, SQL Server Express, SQL Server Compact, et SQL Azure. Sync Framework inclut également les classes qui peuvent être adaptées pour synchroniser une base de données SQL Server et toute autre base de données compatible avec ADO.NET. Pour la documentation détaillée des composants de synchronisation de base de données Sync Framework, consultez [Synchronisation des bases de données](http://go.microsoft.com/fwlink/?LinkId=209079). Pour une vue d'ensemble de Sync Framework, consultez le [Centre de développement Microsoft Sync Framework](http://go.microsoft.com/fwlink/?LinkId=209078). Pour une comparaison entre Sync Framework et la réplication de fusion, consultez [Synchronisation de présentation de bases de données](http://msdn.microsoft.com/library/bb902818\(SQL.110\).aspx)  
  
 **Parcourir par zone**  
 - [Nouveautés](../../relational-databases/replication/what-s-new-replication.md)  
 - [Compatibilité descendante](../../relational-databases/replication/replication-backward-compatibility.md)  
 - [Fonctionnalités et tâches de réplication](../../relational-databases/replication/replication-features-and-tasks.md)  
 - [Références techniques](../../relational-databases/replication/technical-reference-replication.md)  
  
  
