---
title: Procédures stockées compilées en mode natif | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- natively compiled stored procedures
ms.assetid: d5ed432c-10c5-4e4f-883c-ef4d1fa32366
caps.latest.revision: 54
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: accf11eacbd0feaa074979b96d2bc90a34d92079
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="natively-compiled-stored-procedures"></a>procédures stockées compilées en mode natif
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Les procédures stockées compilées en mode natif sont des procédures stockées [!INCLUDE[tsql](../../includes/tsql-md.md)] compilées en code natif qui accèdent aux tables optimisées en mémoire. Elles permettent aux requêtes et à la logique métier qu'elles contiennent de s'exécuter rapidement. Pour plus d'informations sur le processus de compilation en mode natif, consultez [Native Compilation of Tables and Stored Procedures](../../relational-databases/in-memory-oltp/native-compilation-of-tables-and-stored-procedures.md). Pour plus d’informations sur la migration des procédures stockées sur disque vers des procédures stockées compilées en mode natif, consultez [Problèmes de migration pour les procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/migration-issues-for-natively-compiled-stored-procedures.md).  
  
> [!NOTE]  
>  La différence entre les procédures stockées (sur disque) interprétées et les procédures stockées compilées en mode natif réside dans le fait qu’une procédure stockée interprétée est compilée lors de la première exécution, tandis qu’une procédure stockée compilée en mode natif l’est au moment de sa création. Avec des procédures stockées compilées en mode natif, de nombreuses conditions d’erreur peuvent être détectées lors de la création et entraîner l’échec de la création de la procédure stockée compilée en mode natif (dépassement arithmétique, conversion de type et certaines conditions de division par zéro). Avec des procédures stockées interprétées, ces conditions d’erreur ne provoquent généralement pas d’échec quand la procédure stockée est créée, mais toutes les exécutions échouent.  
  
 Rubriques de cette section :  
  
-   [Création de procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/creating-natively-compiled-stored-procedures.md)  
  
-   [Blocs Atomic](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md)  
  
-   [Fonctionnalités prises en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)  
  
-   [DDL pris en charge pour les modules T-SQL compilés en mode natif](../../relational-databases/in-memory-oltp/supported-ddl-for-natively-compiled-t-sql-modules.md)  
  
-   [Procédures stockées compilées en mode natif et options SET d'exécution](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures-and-execution-set-options.md)  
  
-   [Meilleures pratiques pour appeler des procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/best-practices-for-calling-natively-compiled-stored-procedures.md)  
  
-   [Surveillance des performances des procédures stockées compilées en mode natif](../../relational-databases/in-memory-oltp/monitoring-performance-of-natively-compiled-stored-procedures.md)  
  
-   [Appeler des procédures stockées compilées en mode natif à partir d'applications d'accès aux données](../../relational-databases/in-memory-oltp/calling-natively-compiled-stored-procedures-from-data-access-applications.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Tables optimisées en mémoire](../../relational-databases/in-memory-oltp/memory-optimized-tables.md)  
  
  
