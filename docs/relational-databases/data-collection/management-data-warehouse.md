---
title: Entrepôt de données de gestion | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data collector [SQL Server], management data warehouse
- data warehouse
- management data warehouse
ms.assetid: 9874a8b2-7ccd-494a-944c-ad33b30b5499
caps.latest.revision: 43
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 943d896b2afd0e0fe30a211899f0fe805d89cbf8
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="management-data-warehouse"></a>entrepôt de données de gestion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'entrepôt de données de gestion est une base de données relationnelle qui contient les données collectées à partir d'un serveur faisant office de cible de collecte de données. Ces données permettent de générer les rapports pour les jeux d'éléments de collecte de données système ainsi que de créer des rapports personnalisés.  
  
 L'infrastructure du collecteur de données définit les travaux et les plans de maintenance nécessaires pour implémenter les stratégies de rétention définies par l'administrateur de base de données.  
  
> [!IMPORTANT]  
>  Pour cette version du collecteur de données, l'entrepôt de données de gestion est créé à l'aide du mode de récupération simple, pour réduire la journalisation. Vous devez implémenter le mode de récupération adapté à votre organisation.  
  
## <a name="deploying-and-using-the-data-warehouse"></a>Déploiement et utilisation de l'entrepôt de données  
 Vous pouvez installer l'entrepôt de données de gestion sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui exécute le collecteur de données. Toutefois, si les ressources ou les performances du serveur constituent un problème sur le serveur analysé, vous pouvez installer l'entrepôt de données de gestion sur un autre ordinateur.  
  
 Les schémas requis et leurs objets pour les jeux d'éléments de collecte de données système prédéfinis sont créés lorsque vous créez l'entrepôt de données de gestion. Les schémas créés sont les schémas Core et Snapshots. Un troisième schéma, custom_snapshots, est créé lors de la création de jeux d’éléments de collecte définis par l’utilisateur qui incluent des éléments de collecte qui utilisent le type de collecteur Requête T-SQL générique.  
  
###### <a name="core-schema"></a>Schéma principal (Core)  
 Le schéma principal (Core) décrit les tables, les procédures stockées et les vues utilisées pour organiser et identifier les données collectées. Ces tables sont réparties entre toutes les tables de données créées pour les types de collecteurs individuels. Ce schéma est verrouillé et seul le propriétaire de la base de données de l'entrepôt de données de gestion peut le modifier. Les noms des tables de ce schéma incluent le préfixe « core ».  
  
 Le tableau ci-dessous décrit les tables de base de données incluses dans le schéma principal. Ces tables de base de données permettent au collecteur de données d'effectuer le suivi des données indiquant d'où les données proviennent, qui a inséré les données et quand elles ont été téléchargées vers l'entrepôt de données.  
  
|Nom de la table|Description|  
|----------------|-----------------|  
|core.performance_counter_report_group_items|Stocke des informations sur la manière dont les rapports de l'entrepôt de données de gestion doivent regrouper et agréger les compteurs de performance.|  
|core.snapshots_internal|Identifie chaque nouvel instantané. Une nouvelle ligne est insérée dans cette table lorsqu'un package de téléchargement commence à télécharger un nouveau lot de données.|  
|core.snapshot_timetable_internal|Stocke des informations sur les heures de l'instantané. L'heure de l'instantané est stockée dans une table séparée car de nombreux instantanés peuvent s'exécuter pratiquement en même temps.|  
|core.source.info_internal|Stocke des informations sur la source de données. Cette table est mise à jour lorsqu'un nouveau jeu d'éléments de collecte commence à télécharger des données dans l'entrepôt de données.|  
|core.supported_collector_types_internal|Contient les ID des types de collecteurs enregistrés qui peuvent télécharger des données vers l'entrepôt de données de gestion. Cette table est mise à jour uniquement lorsque le schéma de l'entrepôt est mis à jour pour prendre en charge un nouveau type de collecteur. Lorsque l'entrepôt de données de gestion est créé, cette table est remplie avec les ID des types de collecteurs fournis par le collecteur de données.|  
|core.wait_categories|Contient les catégories utilisées pour grouper des types d'attente en fonction de la caractéristique wait_type.|  
|core.wait_types|Contient les types d'attente reconnus par le collecteur de données.|  
|core.purge_info_internal|Indique qu'une demande a été faite pour arrêter la suppression de données de l'entrepôt de données de gestion.|  
  
 Les tables précédentes sont utilisées avec des tables de type de collecteur pour stocker des informations. Par exemple, le type de collecteur Trace SQL générique utilise les tables suivantes pour stocker les données de trace :  
  
-   core.source_info_internal  
  
-   core.snapshots_internal  
  
-   snapshots.trace_info  
  
-   snapshots.trace_data  
  
###### <a name="snapshots-schema"></a>Schéma des instantanés  
 Le schéma des instantanés décrit les objets nécessaires au stockage et à la gestion des données collectées par les types de collecteurs fournis. Les tables dans ce schéma sont fixes et n'ont pas besoin d'être modifiées pendant la durée de vie du type de collecteur. Si des modifications sont nécessaires, seuls les membres du rôle mdw_admin peuvent modifier le schéma. Ces tables sont créées pour stocker les données collectées par les jeux d'éléments de collecte de données système.  
  
 Les tables ci-dessous illustrent une partie du schéma de l'entrepôt de données de gestion qui est nécessaire pour les jeux d'éléments de collecte Activité du serveur et Statistiques de requête.  
  
-   Tables des ressources au niveau du système  
  
    -   **snapshots.os_wait_stats**  
  
    -   **snapshots.os_latch_stats**  
  
    -   **snapshots.os_schedulers**  
  
    -   **snapshots.os_memory_clerks**  
  
    -   **snapshots.os_memory_nodes**  
  
    -   snapshots.sql_process_and_system_memory  
  
-   Activité du système  
  
    -   snapshots.active_sessions_and_requests  
  
-   Statistiques sur les requêtes  
  
    -   snapshots.query_stats  
  
-   Statistiques d'E/S  
  
    -   **snapshots.io_virtual_file_stats**  
  
-   Texte et plan de requête  
  
    -   snapshots.notable_query_text  
  
    -   snapshots.notable_query_plan  
  
-   Statistiques de requête normalisées  
  
    -   snapshots.distinct_queries  
  
    -   snapshots.distinct_query_to_handle  
  
 **Schéma custom_snapshots**  
  
 Le schéma custom_snapshots décrit les nouvelles tables et vues créées lorsque des types de collecteurs standard ou tiers sont utilisés pour créer des jeux d'éléments de collecte définis par l'utilisateur. Tout type de collecteur qui requiert une nouvelle table de données pour un élément de collecte peut créer cette table dans ce schéma. Les nouvelles tables peuvent être ajoutées dans ce schéma par les membres du rôle mdw_writer. Toute autre modification du schéma ne peut être apportée que par des membres du rôle mdw_admin.  
  
 Vous pouvez obtenir des informations détaillées sur le type et le contenu des données pour les colonnes de table de base de données en lisant la documentation de la procédure stockée du collecteur de données approprié pour chacune des tables.  
  
### <a name="best-practices"></a>Bonnes pratiques  
 Lors de l'utilisation de l'entrepôt de données de gestion, nous vous recommandons d'appliquer les méthodes conseillées suivantes :  
  
-   Ne modifiez pas les métadonnées des tables de l'entrepôt de données de gestion sauf si vous ajoutez un nouveau type de collecteur.  
  
-   Ne modifiez pas directement les données dans l'entrepôt de données de gestion. Le fait de modifier les données recueillies invalide leur légitimité.  
  
-   Au lieu d'utiliser directement les tables, recourez aux procédures stockées et aux fonctions documentées fournies avec le collecteur de données pour accéder aux données d'instance et d'application. Les noms et les définitions de tables peuvent changer, sont modifiés lorsque vous mettez à jour l'application et pourront différer dans des versions futures.  
  
## <a name="change-history"></a>Historique des modifications  
  
|Mise à jour du contenu|  
|---------------------|  
|Ajout de la table core.performance_counter_report_group_items à la section « Schéma principal (Core) ».|  
|Mise à jour de la liste de tables dans la section « Schéma des instantanés ». Ajout de snapshots.os_memory_clerks, snapshots.sql_process_and_system_memory et snapshots.io_virtual_file_stats. Suppression de snapshots.os_process_memory et snapshots.distinct_query_stats.|  
  
## <a name="see-also"></a> Voir aussi  
 [Procédures stockées de l’entrepôt de données de gestion &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/management-data-warehouse-stored-procedures-transact-sql.md)   
 [Procédures stockées du collecteur de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)   
 [Afficher un rapport de jeu d’éléments de collecte &#40;SQL Server Management Studio&#41;](../../relational-databases/data-collection/view-a-collection-set-report-sql-server-management-studio.md)  
  
  
