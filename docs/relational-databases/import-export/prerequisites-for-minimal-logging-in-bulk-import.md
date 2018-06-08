---
title: Prérequis pour une journalisation minimale dans l’importation en bloc | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: import-export
ms.reviewer: ''
ms.suite: sql
ms.technology: data-movement
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- minimal logging [SQL Server]
- logged bulk copy [SQL Server]
- logs [SQL Server], minimal logging
- minimally logged operations [SQL Server]
- bulk importing [SQL Server], minimal logging
ms.assetid: bd1dac6b-6ef8-4735-ad4e-67bb42dc4f66
caps.latest.revision: 48
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 577f7170bce26241803f757e0eb4b1215b094d54
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="prerequisites-for-minimal-logging-in-bulk-import"></a>Prérequis pour une journalisation minimale dans l’importation en bloc
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Pour une base de données en mode de récupération complète, toutes les opérations d'insertion de lignes effectuées par importation en bloc sont entièrement enregistrées dans le journal des transactions. Les importations de données de grande taille peuvent entraîner un remplissage rapide du journal des transactions si le mode de récupération complète est utilisé. Par opposition, en mode de récupération simple ou en mode de récupération utilisant les journaux de transactions, la journalisation minimale des opérations d'importation en bloc réduit la possibilité qu'une importation en bloc remplira l'espace de journal. La journalisation minimale est également plus efficace qu'une journalisation complète.  
  
> [!NOTE]  
>  Le mode de récupération utilisant les journaux de transactions est conçu pour remplacer temporairement le mode de récupération complète durant les grandes opérations en bloc.  
  
## <a name="table-requirements-for-minimally-logging-bulk-import-operations"></a>Conditions à remplir par les tables pour la journalisation minimale des opérations d'importation en bloc  
 La journalisation minimale nécessite que la table cible remplisse les conditions suivantes :  
  
-   La table n'est pas en cours de réplication.  
  
-   Le verrouillage de la table est activé (à l'aide de TABLOCK). Concernant les tables avec index columnstore cluster, vous n’avez pas besoin de TABLOCK pour la journalisation minimale.  En outre, seul le chargement des données dans des rowgroups compressés fait l’objet d’une journalisation minimale nécessitant une valeur BatchSize de 102 400 ou plus.  
  
    > [!NOTE]  
    >  Bien que les insertions de données ne soient pas consignées dans le journal des transactions lors des opérations d'importation en bloc à journalisation minimale, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] consigne tout de même les allocations d'extensions chaque fois qu'une nouvelle extension est allouée à la table.  
  
-   La table n'est pas une table mémoire optimisée.  
  
 La possibilité de journalisation minimale pour une table dépend également du fait que celle-ci soit indexée et, si elle l'est, du fait qu'elle soit vide :  
  
-   Si la table ne possède aucun index, les pages de données bénéficient de la journalisation minimale.  
  
-   Si la table ne possède pas d'index cluster, mais possède un ou plusieurs index non cluster, les pages de données bénéficient toujours de la journalisation minimale. La manière dont les pages d'index sont journalisées dépend toutefois du remplissage de la table :  
  
    -   Si la table est vide, les pages d'index bénéficient de la journalisation minimale.  
  
    -   Si la table n'est pas vide, les pages d'index bénéficient de la journalisation complète.  
  
        > [!NOTE]  
        >  Si vous démarrez avec une table vide et importez les données en bloc en plusieurs traitements, les pages de données et d'index bénéficient de la journalisation minimale pour le premier traitement, mais à partir du deuxième traitement, seules les pages de données bénéficient de cette journalisation minimale.  
  
-   Si la table possède un index cluster et est vide, les pages de données et d'index bénéficient de la journalisation minimale. En revanche, si une table possède un index cluster BTree et n’est pas vide, les pages de données et d’index bénéficient de la journalisation complète, quel que soit le mode de récupération choisi. Pour les tables avec index columnstore cluster, le chargement des données dans un rowgroup compressé fait toujours l’objet d’une journalisation minimale, que la table soit vide ou non lorsque la valeur BatchSize > = 102 400.  
  
    > [!NOTE]  
    >  Si vous démarrez avec une table rowstore vide et importez les données en bloc en plusieurs lots, les pages de données et d’index bénéficient de la journalisation minimale pour le premier lot, mais à partir du deuxième lot, seules les pages de données bénéficient de la journalisation en bloc.  
  
> [!NOTE]  
>  Lorsque la réplication transactionnelle est activée, les opérations BULK INSERT sont entièrement journalisées, même dans le mode de récupération utilisant les journaux de transactions.  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Afficher ou modifier le mode de récupération d’une base de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)  
  
  
## <a name="see-also"></a> Voir aussi  
 [Modes de récupération &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Indicateurs de table &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)  
  
  
