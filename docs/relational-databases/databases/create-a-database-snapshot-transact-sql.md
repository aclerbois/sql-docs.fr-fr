---
title: Créer un instantané de base de données (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- database snapshots [SQL Server], creating
ms.assetid: 187fbba3-c555-4030-9bdf-0f01994c5230
caps.latest.revision: 56
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 52d7047c32a8ce381fa3d0be590b696b47f15d18
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-database-snapshot-transact-sql"></a>Créer un instantané de base de données (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Pour créer un instantané de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez impérativement utiliser [!INCLUDE[tsql](../../includes/tsql-md.md)]+. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ne prend pas en charge la création d'instantanés de base de données.  
  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 La base de données source, qui peut utiliser n'importe quel mode de récupération, doit respecter les conditions préalables suivantes :  
  
-   L’instance de serveur doit exécuter une édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui prend en charge l’instantané de base de données. Pour plus d’informations sur la prise en charge des captures instantanées de bases de données dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   La base de données source doit être en ligne, à moins que la base de données soit une base de données miroir au sein d'une session de mise en miroir de bases de données.  
  
-   Pour créer une capture instantanée de base de données dans une base de données miroir, la base de données doit être à l’ [état de mise en miroir](../../database-engine/database-mirroring/mirroring-states-sql-server.md)synchronisée.  
  
-   La base de données source ne peut pas être configurée en tant que base de données partagée évolutive.  

- La base de données source ne doit pas comporter de groupe de fichiers MEMORY_OPTIMIZED_DATA. Pour plus d’informations, consultez la page [Fonctionnalités SQL Server non prises en charge pour l’OLTP en mémoire](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md).

>  [!IMPORTANT]
> Pour plus d’informations sur d’autres considérations importantes, consultez [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md)+.  
  
##  <a name="Recommendations"></a> Recommandations  
 Cette section présente les recommandations suivantes :  
  
-   [Recommandation : Dénomination des instantanés de base de données](#Naming)  
  
-   [Recommandation : Limitation du nombre d'instantanés de base de données](#Limiting_Number)  
  
-   [Recommandation : Connexions clientes à un instantané de base de données](#Client_Connections)  
  
####  <a name="Naming"></a> Recommandation : Dénomination des instantanés de base de données  
 Avant de créer des instantanés, il est important de déterminer comment ils seront nommés. Chaque instantané de base de données nécessite un nom de base de données unique. Pour faciliter l'administration, le nom de l'instantané peut intégrer des informations identifiant la base de données, telles que :  
  
-   Nom de la base de données source.  
  
-   Une indication que le nouveau nom désigne un instantané.  
  
-   La date et l'heure de création de l'instantané, un numéro de séquence ou d'autres informations pour distinguer les instantanés consécutifs sur une base de données spécifique.  
  
 Par exemple, envisageons une série d'instantanés de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Trois instantanés quotidiens sont créés à des intervalles de 6 heures, entre 6h00 et 18h00. Chaque instantané quotidien est conservé pendant 24 heures avant d'être supprimé et remplacé par un nouvel instantané du même nom. Notez que chaque instantané indique l'heure, mais non le jour :  
  
```  
AdventureWorks_snapshot_0600  
AdventureWorks_snapshot_1200  
AdventureWorks_snapshot_1800  
```  
  
 Si l'heure de création de ces instantanés quotidiens varie selon les jours, une convention de dénomination moins précise peut être préférable, par exemple :  
  
```  
AdventureWorks_snapshot_morning  
AdventureWorks_snapshot_noon  
AdventureWorks_snapshot_evening  
```  
  
#### <a name="Limiting_Number"></a> Recommandation : Limitation du nombre d'instantanés de base de données  
 La création d'une série d'instantanés dans le temps fournit des instantanés consécutifs de la base de données source. Chaque instantané est conservé jusqu'à ce qu'il soit explicitement supprimé. Chaque instantané continuant à grandir au fur et à mesure que les pages d'origine sont mises à jour, vous voudrez peut-être conserver de l'espace disque en supprimant un instantané plus ancien après en avoir créé un nouveau.  
  

**Remarque** Pour revenir à une capture instantanée de base de données, vous devez supprimer toutes les autres captures instantanées de cette base de données.  
  
####  <a name="Client_Connections"></a> Recommandation : Connexions clientes à un instantané de base de données  
 Pour utiliser un instantané de base de données, les clients ont besoin de savoir où il se trouve. Les utilisateurs peuvent lire un instantané de base de données pendant qu'un autre instantané est créé ou supprimé. Cependant, lorsque vous substituez un nouvel instantané de base de données à un instantané existant, vous devez rediriger les clients vers le nouvel instantané. Les utilisateurs peuvent se connecter manuellement à un instantané de base de données à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Cependant, pour prendre en charge un environnement de production, vous devez créer une solution de programmation qui dirige de façon transparente les clients écrivant des rapports vers le dernier instantané de la base de données.  
  

  
####  <a name="Permissions"></a> Permissions  
 Tout utilisateur ayant la possibilité de créer une base de données peut également créer un instantané de base de données. Toutefois, pour créer un instantané d’une base de données miroir, vous devez être membre du rôle serveur fixe **sysadmin** .  
  
##  <a name="TsqlProcedure"></a> Comment créer un instantané de base de données (en utilisant Transact-SQL)  
 **Pour créer un instantané de base de données**  
  
>  Pour obtenir un exemple de cette procédure, consultez [Exemples (Transact-SQL)](#TsqlExample)plus loin dans cette section.  
  
1.  En vous basant sur la taille actuelle de la base de données source, vérifiez que votre disque dispose de suffisamment d'espace pour en accueillir un instantané. La taille maximale d'un instantané est la taille de la base de données source au moment où l'instantané est créé. Pour plus d’informations, consultez [Afficher la taille du fichier partiellement alloué d’un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md).  
  
2.  Exécutez une instruction CREATE DATABASE sur les fichiers en utilisant la clause AS SNAPSHOT OF. Pour créer un instantané, vous devez spécifier le nom logique de chaque fichier de la base de données source. La syntaxe de base est la suivante :  
  
     CREATE DATABASE *nom_capture_instantanée_base_de_données*  
  
     ON  
  
     (  
  
     NAME =*nom_fichier_logique*,  
  
     FILENAME =’*nom_fichier_se*’  
  
     ) [ ,...*n* ]  
  
     AS SNAPSHOT OF *nom_base_de_données_source*  
  
     [;]  
  
     Où *nom_base_de_données_**source* représente la base de données source, *nom_fichier_logique* le nom logique utilisé dans SQL Server pour référencer le fichier, *nom_fichier_se* le chemin et le nom de fichier utilisés par le système d’exploitation quand vous créez le fichier, et *nom_capture_instantanée_base_de_données* le nom de la capture instantanée à laquelle vous souhaitez restaurer la base de données. Pour obtenir une description complète de cette syntaxe, consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)+.  
  
    > [!NOTE]  
    >  Lorsque vous créez un instantané de base de données, les fichiers journaux, les fichiers hors connexion, les fichiers de restauration et les anciens fichiers ne sont pas autorisés dans l'instruction CREATE DATABASE.  
  
###  <a name="TsqlExample"></a> Exemples (Transact-SQL)  
  
> [!NOTE]  
>  L'extension `.ss` utilisée dans les exemples est arbitraire.  
  
 Cette section contient les exemples suivants :  
  
-   A. [Création d'un instantané sur la base de données AdventureWorks](#Creating_on_AW)  
  
-   B. [Création d'un instantané sur la base de données Sales (Ventes)](#Creating_on_Sales)  
  
####  <a name="Creating_on_AW"></a> A. Création d'un instantané sur la base de données AdventureWorks  
 Cet exemple montre comment créer un instantané de base de données sur la base de données `AdventureWorks` . Le nom de l'instantané, `AdventureWorks_dbss_1800`, et le nom de son fichier partiellement alloué, `AdventureWorks_data_1800.ss`, précisent l'heure de création, 6H00 du soir (18 heures).  
  
```  
CREATE DATABASE AdventureWorks_dbss1800 ON  
( NAME = AdventureWorks_Data, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks_data_1800.ss' )  
AS SNAPSHOT OF AdventureWorks;  
GO  
```  
  
####  <a name="Creating_on_Sales"></a> B. Création d'un instantané sur la base de données Sales (Ventes)  
 Cet exemple montre comment créer un instantané de base de données, `sales_snapshot1200`, sur la base de données `Sales` . Cette base de données a été créée dans l’exemple illustrant la création d’une base de données dotée de groupes de fichiers dans la rubrique [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-sql-server-transact-sql.md).  
  
```  
--Creating sales_snapshot1200 as snapshot of the  
--Sales database:  
CREATE DATABASE sales_snapshot1200 ON  
( NAME = SPri1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SPri1dat_1200.ss'),  
( NAME = SPri2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SPri2dt_1200.ss'),  
( NAME = SGrp1Fi1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\mssql\data\SG1Fi1dt_1200.ss'),  
( NAME = SGrp1Fi2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG1Fi2dt_1200.ss'),  
( NAME = SGrp2Fi1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG2Fi1dt_1200.ss'),  
( NAME = SGrp2Fi2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG2Fi2dt_1200.ss')  
AS SNAPSHOT OF Sales;  
GO  
```  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Afficher un instantané de base de données &#40;SQL Server&#41;](../../relational-databases/databases/view-a-database-snapshot-sql-server.md)  
  
-   [Rétablir une base de données dans l’état d’un instantané de base de données](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md)  
  
-   [Supprimer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md)  
  
  

