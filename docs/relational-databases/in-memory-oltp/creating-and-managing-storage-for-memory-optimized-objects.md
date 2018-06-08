---
title: Création et gestion du stockage des objets mémoire optimisés | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 622aabe6-95c7-42cc-8768-ac2e679c5089
caps.latest.revision: 64
author: CarlRabeler
ms.author: carlrab
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 5254d3d2c40200be5e0773d642ebed485b9cd0f0
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="creating-and-managing-storage-for-memory-optimized-objects"></a>Création et gestion du stockage des objets mémoire optimisés
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Le moteur [!INCLUDE[hek_2](../../includes/hek-2-md.md)] est intégré dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ce qui vous permet d’avoir des tables optimisées en mémoire et des tables sur disque (traditionnelles) dans la même base de données. Toutefois, la structure de stockage des tables optimisées en mémoire est différente de celle des tables sur disque.  
  
 Les principales caractéristiques du stockage des tables sur disque sont les suivantes :  
  
-   Elles sont mappées à groupe de fichiers contenant un ou plusieurs fichiers.  
  
-   Chaque fichier est divisé en étendues de 8 pages de 8 Ko chacune.  
  
-   Une étendue peut être partagée entre plusieurs tables, mais il existe une correspondance univoque entre une page allouée et la table ou l’index. En d’autres termes, une page ne peut pas contenir de lignes appartenant à plusieurs tables ou index.  
  
-   Les données sont placées dans la mémoire (le pool de mémoires tampons) en fonction des besoins et les pages modifiées ou créées sont écrites de façon asynchrone sur le disque, générant essentiellement des E/S aléatoires.  
  
 Les principales caractéristiques du stockage des tables optimisées en mémoire sont les suivantes :  
  
-   Toutes les tables optimisées en mémoire sont mappées à un groupe de fichiers/données optimisées en mémoire. Ce groupe de fichiers utilise une syntaxe et une sémantique similaires au flux de fichiers.  
  
-   Il n’y a aucune page et les données sont conservées sur une ligne.  
  
-   Toutes les modifications apportées aux tables optimisées en mémoire sont stockées à la fin des fichiers actifs. La lecture et l’écriture des fichiers sont séquentielles.  
  
-   Une mise à jour correspond à une suppression suivie d’une insertion. Les lignes supprimées ne sont pas immédiatement supprimées du stockage. Les lignes supprimées le sont par un processus d’arrière-plan, appelé MERGE, basé sur une stratégie et décrit dans [Durabilité pour les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/durability-for-memory-optimized-tables.md).  
  
-   Contrairement aux tables sur disque, le stockage des tables optimisées en mémoire n’est pas compressé. Lors de la migration d’une table sur disque (ligne ou page) compressée vers une table optimisée en mémoire, vous devez prendre en compte la modification de taille.  
  
-   Une table optimisée en mémoire peut être durable ou non durable. Seul le stockage des tables durables optimisées en mémoire est à configurer.  
  
 Cette section décrit les paires de fichiers de point de contrôle d'autres aspects du stockage des données dans des tables mémoire optimisées.  
  
 Rubriques de cette section :  
  
-   [Configuration du stockage des tables mémoire optimisées](../../relational-databases/in-memory-oltp/configuring-storage-for-memory-optimized-tables.md)  
  
-   [Groupe de fichiers mémoire optimisé](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md)  
  
-   [Durabilité pour les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/durability-for-memory-optimized-tables.md)  
  
-   [Opération de point de contrôle pour les tables mémoire optimisées](../../relational-databases/in-memory-oltp/checkpoint-operation-for-memory-optimized-tables.md)  
  
-   [Définition de la durabilité des objets mémoire optimisés](../../relational-databases/in-memory-oltp/defining-durability-for-memory-optimized-objects.md)  
  
-   [Comparaison du stockage des tables sur disque et du stockage des tables mémoire optimisées](../../relational-databases/in-memory-oltp/comparing-disk-based-table-storage-to-memory-optimized-table-storage.md)  
  
## <a name="see-also"></a> Voir aussi  
 [OLTP en mémoire &#40;Optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
