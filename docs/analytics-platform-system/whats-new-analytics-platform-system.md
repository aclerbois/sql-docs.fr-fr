---
title: Nouveautés du système de plateforme Analytique – un entrepôt de données de la montée en puissance parallèle
description: Voir quelles sont les nouveautés dans le système de plateforme Microsoft® Analytique, un dispositif de montée en puissance parallèle sur site qui héberge MPP SQL Server Parallel Data Warehouse.
author: mzaman1
manager: craigg
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: c2408e84e7ff81f54ad00a98f85cd8dce7b04131
ms.sourcegitcommit: fc3cd23685c6b9b6972d6a7bab2cc2fc5ebab5f2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2018
---
# <a name="whats-new-in-analytics-platform-system-a-scale-out-mpp-data-warehouse"></a>Nouveautés du système de plateforme Analytique, un entrepôt de données MPP montée en puissance parallèle
Voir quelles sont les nouveautés dans les dernières mises à jour pour Microsoft® Analytique système de plateforme (APS). Points d’accès est un appareil de montée en puissance parallèle sur site qui héberge MPP SQL Server Parallel Data Warehouse. 


## <a name="aps-au7"></a>POINTS D’ACCÈS AU7
APS2016 est une condition préalable à la mise à niveau vers AU7. Voici les nouvelles fonctionnalités pour les points d’accès AU7 :

### <a name="auto-create-and-auto-update-statistics"></a>Création automatique et la mise à jour automatique des statistiques
APS AU7 crée et met à jour les statistiques automatiquement, par défaut. Pour mettre à jour les paramètres de statistiques, les administrateurs peuvent utiliser un nouvel élément de menu de commutateur de fonctionnalité dans le [Configuration Manager](appliance-configuration.md#CMTasks). Le [commutateur de la fonctionnalité](appliance-feature-switch.md) contrôle les auto-create, la mise à jour automatique et le comportement de mise à jour asynchrone des statistiques. Vous pouvez également mettre à jour les paramètres des statistiques avec le [ALTER DATABASE (Parallel Data Warehouse)](/sql/t-sql/statements/alter-database-parallel-data-warehouse) instruction.

### <a name="t-sql"></a>T-SQL
Sélectionnez @var est désormais prise en charge. Pour plus d’informations, consultez [Sélectionnez variable locale] (/ sql/t-sql/language-elements/select-local-variable-transact-sql) 

Indicateurs de requête hachage et l’ordre de groupe sont désormais prises en charge. Pour plus d’informations, consultez [Hints(Transact-SQL) - requête] (/ sql/t-sql/requêtes/indicateurs-transact-sql-query)

### <a name="feature-switch"></a>Commutateur de fonctionnalité
APS AU7 introduit le commutateur de fonctionnalité dans [Configuration Manager](launch-the-configuration-manager.md). AutoStatsEnabled et DmsProcessStopMessageTimeoutInSeconds sont désormais des options configurables qui peuvent être modifiées par les administrateurs.

### <a name="known-issues"></a>Problèmes connus
APS AU7 logiciels, nous sommes empaquetage et la mise à jour du BIOS Intel qui résout les « attaques côté canal de l’exécution spéculative » (alias. Vulnérabilités spectre et Meltdown). Bien que regroupés, la mise à jour du BIOS est installé manuellement et ne fait pas partie du logiciel APS AU7 installer. Microsoft recommande à tous les clients installent le BIOS mis à jour. Microsoft a mesuré l’effet de l’atténuation de noyau virtuels adresse occultation (KVAS), du noyau Page Table Indirection (KPTI) et prédiction de branche indirecte (IBP) sur les différentes charges de travail SQL dans différents environnements et une dégradation significative sur certaines charges de travail. Nous vous recommandons de tester l’effet de performances de l’activation de la mise à jour avant de les déployer dans un environnement de production. Si l’effet de performances de l’activation de ces fonctionnalités est trop élevée pour une application existante, vous pouvez envisager si isole votre solution de points d’accès de code non fiable en cours d’exécution est une meilleure résistance de votre application. Consultez les conseils de SQL Server [ici](https://support.microsoft.com/en-us/help/4073225/guidance-protect-sql-server-against-spectre-meltdown).

## <a name="aps-2016"></a>APS 2016
Voici les nouvelles fonctionnalités pour les points d’accès 2016 :

### <a name="sql-server-2016"></a>SQL Server 2016

APS 2016 s’exécute sur la dernière version de SQL Server 2016 et utilise le niveau de compatibilité de base de données par défaut 130.  SQL Server 2016 permet de prendre en charge certaines des nouvelles fonctionnalités telles que les index secondaires pour l’index columnstore cluster et Kerberos pour PolyBase. 


### <a name="t-sql"></a>T-SQL
APS 2016 prend en charge ces améliorations de compatibilité de T-SQL.  Ces éléments de langage supplémentaires facilitent la migration à partir de SQL Server et d’autres sources de données. 

- [Classements SQL au niveau des colonnes][] sont désormais pris en charge en plus des classements Windows.
- [Index non cluster sur les index columnstore en cluster][] améliorer les performances des requêtes qui recherchent des valeurs spécifiques dans l’index columnstore cluster. 
- [SÉLECTIONNEZ... DANS][] 
- [sp_spaceused()][] affiche l’espace disque utilisé ou réservé dans une table ou une base de données.
- [Tableaux larges][] prise en charge est le même que SQL Server 2016. La limite précédente de 32 Ko pour la taille de ligne n’existe plus. 

**Types de données**

- [Varchar (max)][], [nvarchar (max)][] et [varbinary (max)][]. Ces types de données LOB ont une taille maximale de 2 Go. Pour charger ces objets utilisent [utilitaire bcp][]. Polybase et dwloader ne prennent pas actuellement en charge ces types de données. 
- [SYSNAME][]
- [UNIQUEIDENTIFIER][]
- [NUMERIC][] et types de données décimal.

**Fonctions de fenêtre**

- [ROWS ou RANGE][] dans la clause OVER de l’instruction SELECT.
- [FIRST_VALUE][]
- [LAST_VALUE][]
- [CUME_DIST][]
- [PERCENT_RANK][]

**Fonctions de sécurité**

- [CHECKSUM()][] et [BINARY_CHECKSUM()][]
- [HAS_PERMS_BY_NAME()][]

**Fonctions supplémentaires**

- [NEWID()][]
- [RAND()][]

### <a name="polybasehadoop-enhancements"></a>Améliorations de PolyBase/Hadoop

- Compatibilité avec Hortonworks HDP 2.4 et HDP 2.5
- Prise en charge Kerberos via les informations d’identification de la portée de la base de données
- Prise en charge des informations d’identification avec des objets BLOB de stockage Azure

### <a name="install-and-upgrade-enhancements"></a>Installer et des améliorations de la mise à niveau

**Les mises à jour Enterprise architecture** la mise à niveau de l’appliance existante vers APS 2016 installe le dernier microprogramme et les mises à jour du pilote, qui incluent les correctifs de sécurité. 

Un nouveau matériel à partir de HPE ou DELL inclut toutes les dernières mises à jour plus :

- Dernière génération la prise en charge (Broadwell)
- Mettre à jour vers DDR4 DIMM
- Débit DIMM améliorés

**Intégration**

- Entièrement prise en charge du nom de domaine complet (FQDN) rend possible de configurer une approbation de domaine à l’appliance. 
- Pour utiliser le nom de domaine complet, vous devez effectuer une mise à niveau complète et de participer au cours de la mise à niveau. 

**Temps mort réduit** l’installation ou la mise à niveau vers les points d’accès 2016 sont plus rapide et nécessite moins de temps mort que les versions précédentes. Pour réduire les temps d’arrêt, l’installation ou la mise à niveau : 

 - Rationalise application des mises à jour WSUS à l’aide d’une image qui contient toutes les mises à jour juin 2016
 - Applique les mises à jour de sécurité avec les mises à jour de pilote et de microprogramme
 - Place les derniers correctifs logiciels et l’utilitaire de vérification de matériel (PAV) sur votre appareil afin qu’ils soient prêtes à être installées sans avoir à les télécharger.


<!--MSDN references-->
[database compatibility level 130]:/sql/t-sql/statements/alter-database-transact-sql-compatibility-level
[Classements SQL au niveau des colonnes]:/sql/relational-databases/collations/collation-and-unicode-support
[Index non cluster sur les index columnstore en cluster]:/sql/t-sql/statements/create-index-transact-sql
[VARCHAR (MAX)]:/sql/t-sql/data-types/char-and-varchar-transact-sql
[NVARCHAR (MAX)]:/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql
[VARBINARY (MAX)]:/sql/t-sql/data-types/binary-and-varbinary-transact-sql
[SYSNAME]:/sql/relational-databases/system-catalog-views/sys-types-transact-sql
[SÉLECTIONNEZ... DANS]:/sql/t-sql/queries/select-into-clause-transact-sql
[sp_spaceused()]:/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql
[Tableaux larges]:/sql/sql-server/maximum-capacity-specifications-for-sql-server
[BULK INSERT]:/sql/t-sql/statements/bulk-insert-transact-sql
[Utilitaire bcp]:/sql/tools/bcp-utility
[UNIQUEIDENTIFIER]:/sql/t-sql/data-types/uniqueidentifier-transact-sql
[NUMERIC]:/sql/t-sql/data-types/decimal-and-numeric-transact-sql
[ROWS ou RANGE]:/sql/t-sql/queries/select-over-clause-transact-sql
[FIRST_VALUE]:/sql/t-sql/functions/first-value-transact-sql
[LAST_VALUE]:/sql/t-sql/functions/last-value-transact-sql
[CUME_DIST]:/sql/t-sql/functions/cume-dist-transact-sql
[PERCENT_RANK]:/sql/t-sql/functions/percent-rank-transact-sql
[CHECKSUM()]:/sql/t-sql/functions/checksum-transact-sql
[BINARY_CHECKSUM()]:/sql/t-sql/functions/binary-checksum-transact-sql
[HAS_PERMS_BY_NAME()]:/sql/t-sql/functions/has-perms-by-name-transact-sql
[NEWID()]:/sql/t-sql/functions/newid-transact-sql
[RAND()]:/sql/t-sql/functions/rand-transact-sql


  

  


