---
title: Journal des erreurs SQL Server (groupes de disponibilité Always On) (SQL Server) | Microsoft Docs
ms.custom: ag-guide
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: ''
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 39d0c98d-75af-4dd1-b908-30d31af56f2a
caps.latest.revision: 4
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 0c4c4e04ffa776d5799584800e65fb3f9d9b6d9e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-error-log-always-on-availability-groups"></a>Journal des erreurs SQL Server (groupes de disponibilité Always On)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le journal des erreurs SQL Server signale les événements qui affectent les groupes de disponibilité Always On, notamment :  
  
-   Communication avec le cluster WSFC (clustering de basculement Windows Server)  
  
-   Transitions d’état des réplicas de disponibilité  
  
-   Transitions d’état des bases de données de disponibilité  
  
-   État de connectivité des bases de données de disponibilité entre les réplicas principal et secondaire  
  
-   États des points de terminaison de groupe de disponibilité  
  
-   États des écouteurs de groupe de disponibilité  
  
-   État du bail entre la DLL de ressource SQL Server (en cours d’exécution dans le cluster WSFC) et l’instance SQL Server (pour plus d’informations, consultez [Fonctionnement : délai d’expiration de bail Always On SQL Server](http://blogs.msdn.com/b/psssql/archive/2012/09/07/how-it-works-sql-server-alwayson-lease-timeout.aspx))  
  
-   Événements d’erreur dans le groupe de disponibilité  


Les symptômes suivants doivent vous inciter à passer en revue le journal des erreurs SQL Server :  

-   Impossibilité d’accéder aux bases de données de disponibilité  
  
-   Basculement inattendu du groupe de disponibilité  
  
-   État Résolution affecté de manière inattendue au groupe de disponibilité  
  
-   Groupe de disponibilité dans un état indéterminé  
  
Pour plus d’informations, consultez [Afficher le journal des erreurs SQL Server &#40;SQL Server Management Studio&#41;](~/relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md).  
  
  