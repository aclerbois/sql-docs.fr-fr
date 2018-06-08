---
title: SQL Server XTP (OLTP en mémoire), compteurs de performances | Microsoft Docs
ms.custom: ''
ms.date: 04/06/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: fe3cbaf4-65f4-44c5-acc6-7b735cda0c5d
caps.latest.revision: 17
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 7e61613a4da7caa82be3b3ece5c423a003bd0b24
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-xtp-in-memory-oltp-performance-counters"></a>SQL Server XTP (OLTP en mémoire), compteurs de performances
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit des objets et des compteurs qui peuvent être utilisés par l’Analyseur de performances pour analyser l’activité de l’OLTP en mémoire. Les objets et les compteurs sont partagés entre toutes les instances d’une version donnée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l’ordinateur, à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 Dans le passé, les noms d’objets et de compteurs contenaient *XTP*, comme dans **Curseurs XTP**. À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], les noms suivent le modèle :  
  
-   **Curseurs XTP** **SQL Server** *\<version>*  
  
 Pour *\<version>*, la valeur peut être 2016 par exemple.  
  
##  <a name="SQLServerPOs"></a> Objets de performances XTP SQL Server  
 Le tableau ci-dessous décrit les objets de performances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Objet de performance|Description|  
|------------------------|-----------------|  
|[Curseurs XTP SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-cursors.md)|L’objet de performance Curseurs XTP SQL Server contient des compteurs liés aux curseurs internes du moteur OLTP en mémoire. Les curseurs constituent les blocs de construction de bas niveau que le moteur OLTP en mémoire utilise pour traiter les requêtes [!INCLUDE[tsql](../../includes/tsql-md.md)] . Par conséquent, vous ne pouvez pas les contrôler directement.|  
|[Bases de données SQL Server XTP](../../relational-databases/performance-monitor/sql-server-xtp-databases.md)|L’objet de performances Bases de données SQL Server XTP fournit des compteurs spécifiques de base de données de performance fournit des compteurs spécifiques de base de données OLTP en mémoire.|  
|[Garbage collection XTP de SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-garbage-collection.md)|L’objet de performance Garbage collection XTP de SQL Server contient les compteurs liés au garbage collector du moteur OLTP en mémoire.|  
|[Administrateur d’E/S SQL Server 2016 XTP](../../relational-databases/performance-monitor/sql-server-xtp-io-governor.md)|L’objet de performances Administrateur d’E/S SQL Server XTP contient des compteurs liés à l’Administrateur de taux d’E/S OLTP en mémoire.|
|[Processeur fantôme XTP de SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-phantom-processor.md)|L’objet de performance Processeur fantôme XTP de SQL Server contient des compteurs liés au sous-système de traitement fantôme du moteur OLTP en mémoire. Ce composant détecte les lignes fantômes dans les transactions exécutées dans le niveau d'isolation SERIALIZABLE.|  
|[Stockage XTP SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-storage.md)|L’objet de performance Stockage XTP SQL Server contient des compteurs liés au stockage OLTP en mémoire dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Journal des transactions XTP de SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-transaction-log.md)|L’objet de performance Journal des transactions XTP de SQL Server comprend des compteurs liés à la consignation des transactions de l’OLTP en mémoire dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Transactions XTP de SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-transactions.md)|L’objet de performance Transactions XTP de SQL Server comprend des compteurs liés aux transactions du moteur OLTP en mémoire dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
  
