---
title: Environnements multi-utilisateurs (Visual Database Tools) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-visual-db
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- users [SQL Server], multiuser environments
- conflict resolution [Visual Database Tools]
- multiple users making database changes
- multiuser environments [Visual Database Tools]
- database modifications [SQL Server]
- version control [Visual Database Tools]
- Visual Database Tools [SQL Server], multiuser environments
ms.assetid: 330bd48c-9427-4967-b58e-b7c492d5ee36
caps.latest.revision: 3
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 41d58d45e9411caa3734f95986f3d22a7cb77529
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="multiuser-environments-visual-database-tools"></a>Environnements multi-utilisateurs (Visual Database Tools)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Dans un environnement multi-utilisateur, d'autres personnes peuvent se connecter et apporter des modifications à la base de données sur laquelle vous travaillez. Il en résulte que plusieurs personnes peuvent utiliser les mêmes objets d'une même base de données en même temps. Par conséquent, dans un environnement multi-utilisateur, vous ne devez pas oublier que d'autres utilisateurs peuvent modifier des bases de données en même temps que vous.  
  
Les autorisations d'accès sont un problème clé des bases de données dans un environnement multi-utilisateur. Les autorisations qui vous sont accordées sur la base de données déterminent l'étendue de l'utilisation que vous pouvez en faire. Par exemple, pour apporter des modifications à des objets d'une base de données, vous devez avoir sur cette base des autorisations d'écriture appropriées. Pour plus d'informations sur les autorisations d'accès à votre base de données, consultez sa documentation. Pour plus d’informations, consultez [Autorisations et Visual Database Tools &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/permissions-and-visual-database-tools-visual-database-tools.md).  
  
Lorsque vous enregistrez les modifications apportées à des tables, le Concepteur de tables vérifie que la base de données n'a pas été modifiée depuis le précédent enregistrement des modifications. Si un autre utilisateur y a apporté des modifications, vous êtes averti que la base de données a changé. Vous serez peut-être obligé d'harmoniser ces modifications. Pour plus d’informations, consultez [Réconcilier les modifications effectuées par plusieurs utilisateurs &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/reconcile-changes-made-by-multiple-users-visual-database-tools.md).  
  
Dans un environnement multi-utilisateur, vous devez tenir compte de certaines considérations afin d'éviter les conflits entre les modifications. Pour plus d’informations, consultez [Problèmes liés à l’évolution d’une base de données &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/issues-of-database-evolution-visual-database-tools.md).  
  
Pour éviter les problèmes, vous pouvez modifier une copie de la base de données, à l'instar d'une base de données test, puis créer un script de modification que vous exécutez ensuite pour appliquer ces modifications à la base de données d'origine après avoir résolu les conflits hors connexion. Pour plus d’informations, consultez [Bases de données de développement, de test et de production &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/development-test-and-production-databases-visual-database-tools.md).  
  
