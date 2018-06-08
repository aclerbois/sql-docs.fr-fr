---
title: Lier une base de données avec des tables optimisées en mémoire à un pool de ressources | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f222b1d5-d2fa-4269-8294-4575a0e78636
caps.latest.revision: 24
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: b089af44863033eba2c4e0c83b862ebaa918b715
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="bind-a-database-with-memory-optimized-tables-to-a-resource-pool"></a>Lier une base de données avec des tables optimisées en mémoire à un pool de ressources
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Un pool de ressources représente un sous-ensemble de ressources physiques qui peuvent être régies. Par défaut, les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont liées aux ressources du pool de ressources par défaut et consomment ces dernières. Pour empêcher que toutes les ressources de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient consommées par une ou plusieurs tables mémoire optimisées, et que d'autres utilisateurs consomment la mémoire requise par les tables mémoire optimisées, nous vous recommandons de créer un pool de ressources distinct afin de gérer la consommation de mémoire pour la base de données avec des tables mémoire optimisées.  
  
 Une base de données ne peut être liée qu'à un seul pool de ressources. Toutefois, vous pouvez lier plusieurs bases de données au même pool. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet de lier une base de données sans tables optimisées en mémoire à un pool de ressources, mais cela n’a aucun effet. Vous pouvez lier une base de données à un pool de ressources nommé, si, à l'avenir, vous souhaitez créer des tables mémoire optimisées dans la base de données.  
  
 Vous ne pouvez lier une base de données à un pool de ressources que si cette base de données et ce pool de ressources ont été créés au préalable. La liaison prendra effet lors de la prochaine mise en ligne (ONLINE) de la base de données. Pour plus d’informations, consultez [États d’une base de données](../../relational-databases/databases/database-states.md) .  
  
 Pour plus d’informations sur les pools de ressources, consultez [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md).  
  
## <a name="steps-to-bind-a-database-to-a-resource-pool"></a>Étapes pour lier une base de données à un pool de ressources  
  
1.  [Créer la base de données et le pool de ressources](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_CreatePool)  
  
    1.  [Créer la base de données](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_CreateDatabase)  
  
    2.  [Déterminer la valeur minimale pour MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_DeterminePercent)  
  
    3.  [Créer un pool de ressources et configurer la mémoire](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_CreateResourcePool)  
  
2.  [Lier la base de données au pool](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_DefineBinding)  
  
3.  [Confirmer la liaison](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_ConfirmBinding)  
  
4.  [Rendre la liaison effective](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_MakeBindingEffective)  
  
 Autre contenu de cette rubrique  
  
-   [Modifier MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT sur un pool existant](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_ChangeAllocation)  
  
-   [Pourcentage de mémoire disponible pour les tables et index mémoire optimisés](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_PercentAvailable)  
  
##  <a name="bkmk_CreatePool"></a> Créer la base de données et le pool de ressources  
 Vous pouvez créer la base de données et le pool de ressources dans n'importe quel ordre. L'important est de créer les deux avant de procéder à leur liaison.  
  
###  <a name="bkmk_CreateDatabase"></a> Créer la base de données  
 Le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant crée la base de données IMOLTP_DB destinée à contenir une ou plusieurs tables optimisées en mémoire. Le chemin \<lecteur_et_chemin> doit exister avant d’exécuter cette commande.  
  
```sql  
CREATE DATABASE IMOLTP_DB  
GO  
ALTER DATABASE IMOLTP_DB ADD FILEGROUP IMOLTP_DB_fg CONTAINS MEMORY_OPTIMIZED_DATA  
ALTER DATABASE IMOLTP_DB ADD FILE( NAME = 'IMOLTP_DB_fg' , FILENAME = 'c:\data\IMOLTP_DB_fg') TO FILEGROUP IMOLTP_DB_fg;  
GO  
```  
  
###  <a name="bkmk_DeterminePercent"></a> Déterminer la valeur minimale pour MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT  
 Après avoir déterminé les besoins en mémoire des tables mémoire optimisées, vous devez déterminer le pourcentage de mémoire disponible dont vous avez besoin, et définir les pourcentages de mémoire sur cette valeur ou sur une valeur supérieure.  
  
 **Exemple :**   
Pour cet exemple, nous allons supposer que vos calculs ont déterminé que les tables et les index mémoire optimisés ont besoin de 16 Go de mémoire. Supposons que vous disposez de 32 Go de mémoire allouée.  
  
 À première vue, il semblerait que vous deviez définir MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT à 50 (16 est 50 % de 32).  Cependant, cela ne permettrait pas de fournir à vos tables mémoire optimisées suffisamment de mémoire. En observant la table ci-dessous ([Pourcentage de mémoire disponible pour les tables et index mémoire optimisés](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_PercentAvailable)), nous voyons que si vous disposez de 32 Go de mémoire allouée, seulement 80 % est disponible pour les tables et les index optimisés en mémoire.  Par conséquent, il convient de calculer les pourcentages minimum et maximum en fonction de la mémoire, et non de la mémoire allouée.  
  
 `memoryNeedeed = 16`   
 `memoryCommitted = 32`   
 `availablePercent = 0.8`   
 `memoryAvailable = memoryCommitted * availablePercent`   
 `percentNeeded = memoryNeeded / memoryAvailable`  
  
 Chiffres réels :   
`percentNeeded = 16 / (32 * 0.8) = 16 / 25.6 = 0.625`  
  
 Ainsi, vous avez besoin au moins de 62,5 % de la mémoire disponible pour obtenir les 16 Go requis pour vos tables et index mémoire optimisés.  Étant donné que les valeurs de MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT doivent être des entiers, nous devons les définir sur au moins 63 %.  
  
###  <a name="bkmk_CreateResourcePool"></a> Créer un pool de ressources et configurer la mémoire  
 Lors de la configuration de tables optimisées en mémoire, la planification des capacités doit être effectuée sur MIN_MEMORY_PERCENT, et non sur MAX_MEMORY_PERCENT.  Consultez [ALTER RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-pool-transact-sql.md) pour des informations sur MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT. Ce paramètre fournit une disponibilité de mémoire plus prédictible pour les tables optimisées en mémoire, car MIN_MEMORY_PERCENT sollicite la mémoire d'autres pools de ressources pour s'assurer qu'il est servi. Pour garantir que la mémoire est disponible et éviter les conditions OOM (mémoire insuffisante), les valeurs de MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT doivent être identiques. Consultez [Pourcentage de mémoire disponible pour les tables et index mémoire optimisés](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md#bkmk_PercentAvailable) ci-dessous pour obtenir le pourcentage de la mémoire disponible pour les tables optimisées en mémoire en fonction de la quantité de mémoire allouée.  
  
 Pour plus d’informations sur le fonctionnement dans un environnement de machines virtuelles, consultez [Meilleures pratiques : utilisation de l’OLTP en mémoire dans un environnement de machine virtuelle](http://msdn.microsoft.com/library/27ec7eb3-3a24-41db-aa65-2f206514c6f9) .  
  
 Le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant crée un pool de ressources nommé Pool_IMOLTP avec la moitié de la mémoire disponible.  Une fois le pool créé, Resource Governor est reconfiguré afin d'inclure Pool_IMOLTP.  
  
```sql  
-- set MIN_MEMORY_PERCENT and MAX_MEMORY_PERCENT to the same value  
CREATE RESOURCE POOL Pool_IMOLTP   
  WITH   
    ( MIN_MEMORY_PERCENT = 63,   
    MAX_MEMORY_PERCENT = 63 );  
GO  
  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
##  <a name="bkmk_DefineBinding"></a> Lier la base de données au pool  
 Utilisez la fonction système `sp_xtp_bind_db_resource_pool` pour lier la base de données au pool de ressources. La fonction accepte deux paramètres : le nom de la base de données et le nom du pool de ressources.  
  
 Le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant définit une liaison de la base de données IMOLTP_DB au pool de ressources Pool_IMOLTP. La liaison ne devient effective que lorsque vous mettez la base de données en ligne.  
  
```sql  
EXEC sp_xtp_bind_db_resource_pool 'IMOLTP_DB', 'Pool_IMOLTP'  
GO  
```  
  
 La fonction système sp_xtp_bind_db_resource_pool accepte deux paramètres de chaîne : database_name et pool_name.  
  
##  <a name="bkmk_ConfirmBinding"></a> Confirmer la liaison  
 Confirmez la liaison, en prenant soin de noter l'ID du pool de ressources pour IMOLTP_DB. Cette valeur ne doit pas être NULL.  
  
```sql  
SELECT d.database_id, d.name, d.resource_pool_id  
FROM sys.databases d  
GO  
```  
  
##  <a name="bkmk_MakeBindingEffective"></a> Rendre la liaison effective  
 Vous devez mettre la base de données hors ligne, puis en ligne, après sa liaison au pool de ressources afin que la liaison prenne effet. Si votre base de données était déjà liée à un autre pool de ressources, cela supprime la mémoire allouée du pool précédent afin que les allocations de mémoire pour vos index et votre table mémoire optimisée émanent désormais du nouveau pool de ressources lié à la base de données.  
  
```sql  
USE master  
GO  
  
ALTER DATABASE IMOLTP_DB SET OFFLINE  
GO  
ALTER DATABASE IMOLTP_DB SET ONLINE  
GO  
  
USE IMOLTP_DB  
GO  
```  
  
 Maintenant, la base de données est liée au pool de ressources.  
  
##  <a name="bkmk_ChangeAllocation"></a> Modifier MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT sur un pool existant  
 Si vous ajoutez de la mémoire supplémentaire au serveur ou si la quantité de mémoire requise pour vos tables mémoire optimisées change, il peut être nécessaire de remplacer la valeur de MIN_MEMORY_PERCENT et de MAX_MEMORY_PERCENT. Les étapes suivantes vous montrent comment modifier la valeur de MIN_MEMORY_PERCENT et de MAX_MEMORY_PERCENT sur un pool de ressources. Consultez la section ci-dessous, pour des conseils sur les valeurs à utiliser pour MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT.  Pour plus d’informations, consultez la rubrique [Meilleures pratiques : utilisation de l’OLTP en mémoire dans un environnement de machine virtuelle](http://msdn.microsoft.com/library/27ec7eb3-3a24-41db-aa65-2f206514c6f9) .  
  
1.  Utilisez `ALTER RESOURCE POOL` pour modifier à la fois la valeur de MIN_MEMORY_PERCENT et de MAX_MEMORY_PERCENT.  
  
2.  Utilisez `ALTER RESURCE GOVERNOR` pour reconfigurer Resource Governor avec les nouvelles valeurs.  
  
 **Exemple de code**  
  
```sql  
ALTER RESOURCE POOL Pool_IMOLTP  
WITH  
     ( MIN_MEMORY_PERCENT = 70,  
       MAX_MEMORY_PERCENT = 70 )   
GO  
  
-- reconfigure the Resource Governor  
ALTER RESOURCE GOVERNOR RECONFIGURE  
GO  
```  
  
##  <a name="bkmk_PercentAvailable"></a> Pourcentage de mémoire disponible pour les tables et index mémoire optimisés  
 Si vous mappez une base de données avec des tables mémoire optimisées et une charge de travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au même pool de ressources, Resource Governor définit un seuil interne pour l'utilisation de l' [!INCLUDE[hek_2](../../includes/hek-2-md.md)] afin que les utilisateurs du pool ne rencontrent aucun conflit au niveau de l'utilisation du pool. D'une manière générale, le seuil d'utilisation de l' [!INCLUDE[hek_2](../../includes/hek-2-md.md)] représente environ 80 % du pool. Le tableau suivant montre les seuils réels pour différentes capacités de mémoire.  
  
 Lorsque vous créez un pool de ressources dédié pour la base de données [!INCLUDE[hek_2](../../includes/hek-2-md.md)] , vous devez estimer la quantité de mémoire physique dont vous avez besoin pour les tables en mémoire compte tenu de la croissance des versions de ligne et des données. Après avoir estimé la mémoire nécessaire, créez un pool de ressources avec un pourcentage de la mémoire cible allouée à l’instance SQL comme indiqué par la colonne « committed_target_kb » dans la DMV `sys.dm_os_sys_info` (reportez-vous à [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)). Par exemple, créez un pool de ressources P1 avec 40 % de la mémoire totale disponible sur l'instance. Parmi ces 40 %, le moteur [!INCLUDE[hek_2](../../includes/hek-2-md.md)] obtient un pourcentage plus petit pour enregistrer les données [!INCLUDE[hek_2](../../includes/hek-2-md.md)] .  Cela a pour but de s'assurer que [!INCLUDE[hek_2](../../includes/hek-2-md.md)] ne consomme pas toute la mémoire de ce pool.  Cette valeur de pourcentage inférieur dépend de la mémoire allouée cible. Le tableau suivant décrit la mémoire disponible pour la base de données [!INCLUDE[hek_2](../../includes/hek-2-md.md)] dans un pool de ressources (nommé ou par défaut) avant qu'une situation d'insuffisance de mémoire soit déclenchée.  
  
|Mémoire validée cible|Pourcentage disponible pour les tables en mémoire|  
|-----------------------------|---------------------------------------------|  
|<= 8 Go|70|  
|<= 16 Go|75 %|  
|<= 32 Go|80 %|  
|\<= 96 Go|85 %|  
|>96 Go|90 %|  
  
 Par exemple, si votre « mémoire allouée cible » est de 100 Go et vous estimez que les tables et les index mémoire optimisés ont besoin de 60 Go de mémoire, créez un pool de ressources avec MAX_MEMORY_PERCENT = 67 (60 Go nécessaires/0,90 = 66 667 Go – arrondi à 67 Go ; 67 Go/100 Go installés = 67 %) pour vous assurer que vos d'objets [!INCLUDE[hek_2](../../includes/hek-2-md.md)] ont les 60 Go dont ils ont besoin.  
  
 Une fois qu'une base de données a été liée à un pool de ressources nommé, utilisez la requête suivante pour afficher les allocations de mémoire sur les différents pools de ressources.  
  
```sql  
SELECT pool_id  
     , Name  
     , min_memory_percent  
     , max_memory_percent  
     , max_memory_kb/1024 AS max_memory_mb  
     , used_memory_kb/1024 AS used_memory_mb   
     , target_memory_kb/1024 AS target_memory_mb  
   FROM sys.dm_resource_governor_resource_pools  
```  
  
 Les résultats indiquent que la mémoire consommée par les objets mémoire optimisés est de 1356 Mo dans le pool de ressources, PoolIMOLTP, avec une limite supérieure de 2307 Mo. Cette limite supérieure contrôle la mémoire totale pouvant être consommée par les objets mémoire optimisés système et utilisateur mappés à ce pool.  
  
 **Résultat de l'exemple**   
Ce résultat concerne la base de données et les tables créées précédemment.  
  
```  
pool_id     Name        min_memory_percent max_memory_percent max_memory_mb used_memory_mb target_memory_mb  
----------- ----------- ------------------ ------------------ ------------- -------------- ----------------   
1           internal    0                  100                3845          125            3845  
2           default     0                  100                3845          32             3845  
259         PoolIMOLTP 0                  100                3845          1356           2307  
```  
  
 Pour plus d’informations consultez [sys.dm_resource_governor_resource_pools (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).  
  
 Si vous ne liez pas votre base de données à un pool de ressources, elle est liée au pool « par défaut ». Étant donné que le pool de ressources par défaut est utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la plupart des autres allocations, vous ne pourrez pas surveiller la mémoire consommée par les tables mémoire optimisées à l'aide de la DMV sys.dm_resource_governor_resource_pools précisément pour la base de données.  
  
## <a name="see-also"></a> Voir aussi  
 [sys.sp_xtp_bind_db_resource_pool &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-bind-db-resource-pool-transact-sql.md)   
 [sys.sp_xtp_unbind_db_resource_pool &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-unbind-db-resource-pool-transact-sql.md)   
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [Créer un pool de ressources](../../relational-databases/resource-governor/create-a-resource-pool.md)   
 [Modifier les paramètres de pool de ressources](../../relational-databases/resource-governor/change-resource-pool-settings.md)   
 [Supprimer un pool de ressources](../../relational-databases/resource-governor/delete-a-resource-pool.md)  
  
  
