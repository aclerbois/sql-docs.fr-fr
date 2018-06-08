---
title: Ajouter le basculement de base de données amélioré à un groupe de disponibilité (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
ms.prod: sql
ms.reviewer: mikeray
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], enhanced database failover
- Availability Groups [SQL Server], failover
ms.assetid: ''
caps.latest.revision: ''
author: allanhirt
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 8afda7e0b837994f528d7aaac97b89c259faf392
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34771295"
---
# <a name="add-enhanced-database-failover-to-an-availability-group-sql-server"></a>Ajouter le basculement de base de données amélioré à un groupe de disponibilité (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Dans SQL Server 2012 et 2014, si une base de données membre d’un groupe de disponibilité sur le réplica principal perd la possibilité d’écrire des transactions, elle ne déclenche pas de basculement même si les réplicas sont synchronisés et configurés pour le basculement automatique.

SQL Server 2016 introduit un nouveau comportement facultatif nommé *basculement de base de données amélioré*, lequel peut être défini par l’Assistant ou à l’aide de Transact-SQL. Si cette option est activée et que le basculement automatique est configuré, quand une base de données membre d’un groupe de disponibilité n’est plus en mesure d’écrire des transactions, un basculement vers un réplica secondaire synchronisé est déclenché.

**Scénario 1**

Un groupe de disponibilité est configuré entre l’instance A et l’instance B, contenant une seule base de données nommée DB1. Le fichier de données de DB1 est sur le lecteur E et son fichier journal des transactions est sur le lecteur F. Le mode de disponibilité est défini sur la validation synchrone avec basculement automatique. La nouvelle option de basculement de base de données amélioré est configurée sur le groupe de disponibilité. Les deux réplicas sont actuellement synchronisés. Un problème entraîne une défaillance du lecteur E. Ce scénario ne déclenche pas de basculement de base de données amélioré, car le lecteur E ne contient pas le journal des transactions.  

**Scénario 2**

La configuration du groupe de disponibilité est la même que celle du scénario 1. Cette fois, ce n’est pas le lecteur E qui est défaillant, mais le lecteur sur lequel se trouve le journal des transactions, à savoir le lecteur F. Un basculement est alors déclenché, car ce scénario remplit la condition requise par le basculement de base de données amélioré : le journal des transactions n’est plus accessible, ce qui signifie que la base de données ne peut pas écrire de transactions.

**Scénario 3**

Un groupe de disponibilité est configuré entre l’instance A et l’instance B, contenant deux bases de données : DB1 et DB2. Le mode de disponibilité défini est la validation synchrone avec un mode de basculement automatique. Le basculement de base de données amélioré est activé. L’accès au disque contenant les données de DB2 et les fichiers journaux des transactions est perdu. Quand le problème est détecté, le groupe de disponibilité bascule automatiquement vers l’instance B.

## <a name="configure-and-view-the-enhanced-database-failover-option"></a>Configurer et afficher l’option de basculement de base de données amélioré

Le basculement de base de données amélioré peut être configuré à l’aide de SQL Server Management Studio ou Transact-SQL. Les applets de commande PowerShell n’ont pas cette possibilité. Par défaut, le basculement de base de données amélioré est désactivé.

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

L’activation du basculement de base de données amélioré est possible pendant la création d’un groupe d’accessibilité à l’aide de SQL Server Management Studio. La seule manière de le désactiver ou de l’activer après avoir créé le groupe de disponibilité consiste à utiliser Transact-SQL.

*Création manuelle d’un groupe de disponibilité*

Utilisez les instructions figurant dans l’article [Utiliser la boîte de dialogue Nouveau groupe de disponibilité (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md) pour créer le groupe de disponibilité. Pour permettre le basculement de base de données amélioré, cochez sa case située en regard de *Détection de l’état d’intégrité au niveau base de données*.

*Utilisation de l’Assistant Groupe de disponibilité*

Utilisez les instructions figurant dans l’article [Utiliser l’Assistant Groupe de disponibilité (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md). L’option permettant d’activer le basculement de base de données amélioré se trouve dans la boîte de dialogue Spécifier le nom du groupe de disponibilité. Pour l’activer, cochez la case située en regard de *Détection de l’état d’intégrité au niveau base de données*.

### <a name="transact-sql"></a>Transact-SQL

Pour configurer le comportement de basculement de base de données amélioré pendant la création d’un groupe de disponibilité, DB_FAILOVER doit être défini sur ON comme suit :

```SQL
CREATE AVAILABILITY GROUP [AGNAME]
WITH ( DB_FAILOVER = ON)
...
```
Pour ajouter ce comportement après avoir configuré un groupe de disponibilité, utilisez la commande ALTER AVAILABILITY GROUP :
```SQL
ALTER AVAILABILITY GROUP [AGNAME] SET (DB_FAILOVER = ON)
```
Pour désactiver ce comportement, exécutez la commande ALTER AVAILABILITY GROUP suivante :
```SQL
ALTER AVAILABILITY GROUP [AGNAME] SET (DB_FAILOVER = OFF)
```
### <a name="dynamic-management-view"></a>Vue de gestion dynamique
Pour déterminer si le basculement de base de données amélioré est activé pour un groupe de disponibilité, interrogez la vue de gestion dynamique `sys.availability_groups`. La colonne `db_failover` indique zéro s’il est désactivé ou 1 s’il est activé. 

## <a name="next-steps"></a>Étapes suivantes 

- [Configurer la détection de l’intégrité de la base de données](sql-server-always-on-database-health-detection-failover-option.md)

- [Utiliser l’Assistant Groupe de disponibilité (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md)

- [Utiliser la boîte de dialogue Nouveau groupe de disponibilité (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
- [Créer un groupe de disponibilité avec Transact-SQL](create-an-availability-group-transact-sql.md)

