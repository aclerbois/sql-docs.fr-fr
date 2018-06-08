---
title: Ajouter un élément de collecte à un jeu d’éléments de collecte (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- collection items [SQL Server]
- collection sets [SQL Server], adding items
ms.assetid: 9fe6454e-8c0e-4b50-937b-d9871b20fd13
caps.latest.revision: 21
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: aa263e4b3a21f283adf1da24b817cd2a0f31fba4
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="add-a-collection-item-to-a-collection-set-transact-sql"></a>Ajouter un élément de collecte à un jeu d'éléments de collecte (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez ajouter un élément de collecte à un jeu d'éléments de collecte existant à l'aide des procédures stockées fournies avec le collecteur de données.  
  
 Effectuez les étapes suivantes à l'aide de l'éditeur de requête dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="add-a-collection-item-to-a-collection-set"></a>Ajouter un élément de collecte à un jeu d'éléments de collecte  
  
1.  Arrêtez le jeu d’éléments de collecte auquel vous souhaitez ajouter l’élément en exécutant la procédure stockée **sp_syscollector_stop_collection_set** . Par exemple, pour arrêter un jeu d'éléments de collecte nommé « Test Collection Set », exécutez les instructions suivantes :  
  
    ```sql  
    USE msdb  
    DECLARE @csid int  
    SELECT @csid = collection_set_id  
    FROM syscollector_collection_sets  
    WHERE name = 'Test Collection Set'  
    SELECT @csid  
    EXEC dbo.sp_syscollector_stop_collection_set @collection_set_id = @csid  
    ```  
  
    > [!NOTE]  
    >  Vous pouvez également arrêter le jeu d'éléments de collecte en utilisant l'Explorateur d'objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [Démarrer ou arrêter un jeu d’éléments de collecte](../../relational-databases/data-collection/start-or-stop-a-collection-set.md).  
  
2.  Déclarez le jeu d'éléments de collecte auquel vous souhaitez ajouter l'élément de collecte. Le code suivant fournit un exemple de la façon dont déclarer l'ID du jeu d'éléments de collecte.  
  
    ```sql  
    DECLARE @collection_set_id_1 int  
    SELECT @collection_set_id_1 = collection_set_id FROM [msdb].[dbo].[syscollector_collection_sets]  
    WHERE name = N'Test Collection Set'; -- name of collection set  
    ```  
  
3.  Déclarez le type de collecteur. Le code suivant fournit un exemple de la façon dont déclarer le type de collecteur Requête T-SQL générique.  
  
    ```sql  
    DECLARE @collector_type_uid_1 uniqueidentifier  
    SELECT @collector_type_uid_1 = collector_type_uid FROM [msdb].[dbo].[syscollector_collector_types]   
       WHERE name = N'Generic T-SQL Query Collector Type';  
    ```  
  
     Vous pouvez exécuter le code suivant pour obtenir la liste des types de collecteurs installés :  
  
    ```sql  
    USE msdb  
    SELECT * from syscollector_collector_types  
    GO  
    ```  
  
4.  Exécutez la procédure stockée **sp_syscollector_create_collection_item** pour créer l’élément de collecte. Vous devez déclarer le schéma de l'élément de collecte de manière à ce qu'il soit mappé au schéma requis pour le type de collecteur voulu. L'exemple suivant utilise le schéma d'entrée Requête T-SQL générique.  
  
    ```sql  
    DECLARE @collection_item_id int;  
    EXEC [msdb].[dbo].[sp_syscollector_create_collection_item]   
    @name=N'OS Wait Stats', --name of collection item  
    @parameters=N'  
    <ns:TSQLQueryCollector xmlns:ns="DataCollectorType">  
     <Query>  
      <Value>select * from sys.dm_os_wait_stats</Value>  
      <OutputTable>os_wait_stats</OutputTable>  
    </Query>  
    </ns:TSQLQueryCollector>',  
    @collection_item_id = @collection_item_id OUTPUT,  
    @frequency = 60,  
    @collection_set_id = @collection_set_id_1, --- Provides the collection set ID number  
    @collector_type_uid = @collector_type_uid_1 -- Provides the collector type UID  
    SELECT @collection_item_id     
    ```  
  
5.  Avant de démarrer le jeu d'éléments de collecte mis à jour, exécutez la requête suivante pour vérifier que le nouvel élément de collecte a été créé :  
  
    ```xaml  
    USE msdb  
    SELECT * from syscollector_collection_sets  
    SELECT * from syscollector_collection_items  
    GO  
    ```  
  
     Les jeux d’éléments de collecte et leurs éléments de collecte figurent sous l’onglet **Résultats** .  
  
## <a name="see-also"></a> Voir aussi  
 [Créer un jeu d’éléments de collecte personnalisé qui utilise le type de collecteur Requête T-SQL générique &#40;Transact-SQL&#41;](../../relational-databases/data-collection/create-custom-collection-set-generic-t-sql-query-collector-type.md)   
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)  
  
  
