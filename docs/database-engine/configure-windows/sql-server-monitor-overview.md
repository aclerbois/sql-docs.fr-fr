---
title: Vue d’ensemble du moniteur SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.sqlservermonitor.main.f1
helpviewer_keywords:
- SQL Server Monitor [SQL Server]
ms.assetid: 048ae16d-31c3-489a-9f1e-1400a3bacd39
caps.latest.revision: 22
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 2f140b823aa8e8d06a096162f291a9080a344eee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-monitor-overview"></a>Vue d'ensemble du moniteur SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le moniteur SQL Server n'effectue pas de fonctions de surveillance, mais il héberge des modules qui ont ce rôle. Le moniteur SQL Server inclut le moniteur de réplication et le moniteur de mise en miroir de bases de données.  
  
 Pour utiliser l'un de ces modules, sélectionnez le module souhaité dans le menu **Atteindre** . Le module sélectionné comprend le contenu des volets de navigation et d'informations, l'interaction utilisateur dans les volets d'informations ainsi que les requêtes portant sur le contenu et l'état.  
  
> [!NOTE]  
>  Pour plus d’informations sur ces moniteurs, consultez [Surveillance de la réplication](../../relational-databases/replication/monitor/monitoring-replication-overview.md) et [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md).  
  
## <a name="permissions"></a>Autorisations  
  
-   Moniteur de réplication  
  
     Pour surveiller la réplication, vous devez être membre du rôle de serveur fixe **sysadmin** sur le serveur de distribution ou membre du rôle de base de données fixe **replmonitor** dans la base de données de distribution. Un administrateur système peut ajouter n'importe quel utilisateur au rôle **replmonitor** , ce qui lui permet de visionner l'activité de réplication dans le Moniteur de réplication ; cependant, l'utilisateur ne peut pas administrer la réplication.  
  
-   Moniteur de mise en miroir de bases de données  
  
     Pour surveiller la mise en miroir de bases de données, vous devez être membre du rôle de serveur fixe **sysadmin** ou membre du rôle de base de données fixe **dbm_monitor** sur l’instance de serveur. Si vous êtes membre de **sysadmin** ou **dbm_monitor** sur une seule des instances de serveur partenaire, le moniteur peut se connecter uniquement à ce partenaire ; il ne peut pas extraire d’informations de l’autre partenaire. Pour plus d'informations, consultez [Database Mirroring Monitor Overview](../../database-engine/database-mirroring/database-mirroring-monitor-overview.md).  
  
## <a name="menu-options"></a>Options de menu  
 Le moniteur SQL Server comporte un menu de commandes spécifiques. Ce menu peut également comprendre des commandes du module sélectionné.  
  
 Les options de menu suivantes appartiennent au moniteur SQL Server.  
  
 **Fichier**  
 Ce menu comprend la commande **Quitter** .  
  
 **Action**  
 Comprend le menu contextuel du nœud sélectionné dans l'arborescence de navigation.  
  
 **Atteindre**  
 Comprend une liste des composants de surveillance :  
  
-   Mise en miroir de bases de données  
  
-   REPLICATION  
  
 **Pour utiliser SQL Server Management Studio pour contrôler la mise en miroir de base de données**  
  
-   [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
