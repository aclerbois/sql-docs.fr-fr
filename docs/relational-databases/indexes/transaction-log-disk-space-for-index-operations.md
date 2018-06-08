---
title: Espace disque du journal des transactions pour les opérations d’index | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- index disk space [SQL Server]
- space [SQL Server], indexes
- transaction logs [SQL Server], disk space
- disk space [SQL Server], transaction logs
- space [SQL Server], transaction logs
ms.assetid: 4f8a4922-4507-4072-be67-c690528d5c3b
caps.latest.revision: 17
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 4e3c0e02d5c25c62775af001068ee3e92b63f224
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transaction-log-disk-space-for-index-operations"></a>Espace disque du journal des transactions pour les opérations d'index
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les opérations d'index à grande échelle peuvent générer des chargements de données volumineux susceptibles de saturer le journal des transactions rapidement. Pour que l'opération d'index puisse être restaurée, le journal des transactions ne peut pas être tronqué avant la fin de celle-ci ; toutefois, il peut être sauvegardé pendant l'opération d'index. Par conséquent, le journal des transactions doit posséder suffisamment de place pour stocker les transactions de l'opération d'index et toutes les transactions utilisateur concurrentes qui interviennent pendant celle-ci. Cela concerne à la fois les opérations d'index hors ligne et les opérations d'index en ligne. Étant donné que les tables sous-jacentes sont inaccessibles pendant une opération d'index hors ligne, le nombre de transactions utilisateur peut être faible et le journal ne peut pas croître rapidement. Les opérations d'index en ligne n'empêchent pas l'activité utilisateur concurrente ; par conséquent, les opérations d'index en ligne à grande échelle combinées à des transactions utilisateur concurrentes significatives peuvent provoquer une croissance continue du journal des transactions en l'absence d'option permettant de le tronquer.  
  
## <a name="recommendations"></a>Recommandations  
 Lorsque vous exécutez des opérations d'index à grande échelle, tenez compte des recommandations suivantes :  
  
1.  Assurez-vous que le journal des transactions a été sauvegardé et tronqué avant d'exécuter des opérations d'index à grande échelle en ligne et qu'il possède suffisamment d'espace pour stocker l'index et les transactions utilisateur projetés.  
  
2.  Pensez à attribuer la valeur ON à l'option SORT_IN_TEMPDB pour l'opération d'index. Cela permet de séparer les transactions d'index des transactions utilisateur concurrentes. Les transactions d’index seront stockées dans le journal des transactions **tempdb** tandis que les transactions utilisateur concurrentes seront stockées dans le journal des transactions de la base de données utilisateur. Le cas échéant, cela permet de tronquer le journal des transactions de la base de données utilisateur pendant l'opération d'index. En outre, si le journal **tempdb** ne se trouve pas sur le même disque que le journal de la base de données utilisateur, les deux journaux ne sont pas en concurrence pour le même espace disque.  
  
    > [!NOTE]  
    >  Vérifiez que la base de données **tempdb** et le journal des transactions possèdent suffisamment d’espace disque pour gérer l’opération d’index. Le journal des transactions **tempdb** ne peut pas être tronqué avant la fin de l’opération d’index.  
  
3.  Utilisez un mode de récupération de base de données qui permet une journalisation minimale de l'opération d'index. Cela peut réduire la taille du journal et empêcher celui-ci de saturer l'espace journal.  
  
4.  N'exécutez pas l'opération d'index en ligne dans une transaction explicite. Le journal n'est pas tronqué avant la fin de la transaction explicite.  
  
## <a name="related-content"></a>Contenu associé  
 [Espace disque nécessaire pour les opérations DDL d'index](../../relational-databases/indexes/disk-space-requirements-for-index-ddl-operations.md)  
  
 [Exemple d'espace disque d'un index](../../relational-databases/indexes/index-disk-space-example.md)  
  
  
