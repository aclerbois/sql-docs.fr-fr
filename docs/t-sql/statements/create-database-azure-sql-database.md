---
title: CREATE DATABASE (Azure SQL Database | Microsoft Docs
ms.custom: ''
ms.date: 05/15/2018
ms.prod: ''
ms.prod_service: sql-database
ms.reviewer: ''
ms.service: sql-database
ms.component: t-sql|statements
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SERVICE_OBJECTIVE
- SERVICE_OBJECTIVE_TSQL
- ELASTIC_POOL
- ELASTIC_POOL_TSQL
- EDITION
- EDITION_TSQL
- MAXSIZE
- MAXSIZE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVICE_OBJECTIVE
- ELASTIC_POOL
- EDITION SQL Database
- MAXSIZE SQL Database
ms.assetid: 22b167f7-ae86-490b-adb3-ec02ca1c1508
caps.latest.revision: 62
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: e5d3ff692dc3935bb8791d4c71285b3b7f2a55b7
ms.sourcegitcommit: 02c889a1544b0859c8049827878d66b2301315f8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/17/2018
---
# <a name="create-database-azure-sql-database"></a>CREATE DATABASE (Azure SQL Database)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

  Crée une base de données.  

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]

## <a name="syntax"></a>Syntaxe  

``` 
  
CREATE DATABASE database_name [ COLLATE collation_name ]  
{  
   (<edition_options> [, ...n]) 
}  

[ WITH CATALOG_COLLATION = { DATABASE_DEFAULT | SQL_Latin1_General_CP1_CI_AS }  ]
  
<edition_options> ::= 
{  

  MAXSIZE = { 100 MB | 250 MB | 500 MB | 1 … 1024 … 4096 GB }  
  | ( EDITION = {  'basic' | 'standard' | 'premium' | 'GeneralPurpose' | 'BusinessCritical' } 
  | SERVICE_OBJECTIVE = 
    {  'basic' | 'S0' | 'S1' | 'S2' | 'S3' | 'S4'| 'S6'| 'S7'| 'S9'| 'S12' | 
      | 'P1' | 'P2' | 'P4'| 'P6' | 'P11'  | 'P15'  
      | 'GP_GEN4_1' | 'GP_GEN4_2' | 'GP_GEN4_4' | 'GP_GEN4_8' | 'GP_GEN4_16' | 'GP_GEN4_24' |
      | 'BC_GEN4_1' | 'BC_GEN4_2' | 'BC_GEN4_4' | 'BC_GEN4_8' | 'BC_GEN4_16' | 'BC_GEN4_24' |
      | 'GP_GEN5_2' | 'GP_GEN5_4' | 'GP_GEN5_8' | 'GP_GEN5_16' | 'GP_GEN5_24' | 'GP_GEN5_32' | 'GP_GEN5_48' | 'GP_GEN5_80' |
      | 'BC_GEN5_2' | 'BC_GEN5_4' | 'BC_GEN5_8' | 'BC_GEN5_16' | 'BC_GEN5_24' | 'BC_GEN5_32' | 'BC_GEN5_48' | 'BC_GEN5_80' |
      | { ELASTIC_POOL(name = <elastic_pool_name>) } }  ) 
}  

 [;]  
  

```  

```
To copy a database:  
CREATE DATABASE database_name  
    AS COPY OF [source_server_name.] source_database_name  
    [ ( SERVICE_OBJECTIVE = 
      {  'basic' | 'S0' | 'S1' | 'S2' | 'S3' | 'S4'| 'S6'| 'S7'| 'S9'| 'S12' |  
      | 'GP_GEN4_1' | 'GP_GEN4_2' | 'GP_GEN4_4' | 'GP_GEN4_8' | 'GP_GEN4_16' | 'GP_GEN4_24' |
      | 'BC_GEN4_1' | 'BC_GEN4_2' | 'BC_GEN4_4' | 'BC_GEN4_8' | 'BC_GEN4_16' | 'BC_GEN4_24' |
      | 'GP_GEN5_2' | 'GP_GEN5_4' | 'GP_GEN5_8' | 'GP_GEN5_16' | 'GP_GEN5_24' | 'GP_GEN5_32' | 'GP_GEN5_48' | 'GP_GEN5_80' |
      | 'BC_GEN5_2' | 'BC_GEN5_4' | 'BC_GEN5_8' | 'BC_GEN5_16' | 'BC_GEN5_24' | 'BC_GEN5_32' | 'BC_GEN5_48' | 'BC_GEN5_80' |
        | { ELASTIC_POOL(name = <elastic_pool_name>) } } )  
  ]  
 [;] 
 
```  
  
## <a name="arguments"></a>Arguments  
 Ce diagramme de syntaxe montre les arguments pris en charge dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
*database_name* 
 
Nom de la nouvelle base de données. Ce nom doit respecter les règles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applicables aux identificateurs et être unique sur le serveur SQL qui peut héberger à la fois des bases de données [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et des bases de données [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Pour plus d’informations, consultez [Identificateurs](http://go.microsoft.com/fwlink/p/?LinkId=180386).  
  
*Collation_name*  

Indique le classement par défaut de la base de données. Le nom du classement peut être un nom de classement Windows ou SQL. S'il est omis, le classement par défaut, SQL_Latin1_General_CP1_CI_AS, est affecté à la base de données.  
  
Pour plus d’informations sur les noms de classements Windows et SQL, consultez [COLLATE (Transact-SQL)](http://msdn.microsoft.com/library/ms184391.aspx).  
  
CATALOG_COLLATION  

Spécifie le classement par défaut du catalogue de métadonnées. *DATABASE_DEFAULT* Spécifie que le catalogue de métadonnées utilisé pour les vues et tables système doit être assemblé de façon à correspondre au classement par défaut pour la base de données.  Il s’agit du comportement adopté dans SQL Server. 

*SQL_Latin1_General_CP1_CI_AS* Précise que le catalogue de métadonnées utilisé pour les vues et tables système doit être assemblé par rapport à un classement SQL_Latin1_General_CP1_CI_AS fixe.  Il s’agit du paramètre par défaut dans Azure SQL Database si non spécifié.

EDITION
 
Spécifie la couche de service de la base de données. Les valeurs disponibles sont : 'basic', 'standard', 'premium', 'GeneralPurpose' et 'BusinessCritical'. La prise en charge de 'premiumrs' a été supprimée. Pour poser des questions, utilisez cet alias de messagerie : premium-rs@microsoft.com.
  
 Lorsque l’argument EDITION est spécifié mais que MAXSIZE ne l’est pas, MAXSIZE est défini sur la taille la plus restrictive prise en charge par l’édition.  
  
 MAXSIZE

Spécifie la taille maximale de la base de données. MAXSIZE doit être valide pour l'EDITION (niveau de service) spécifiée. Voici les valeurs de MAXSIZE prises en charge et les valeurs par défaut (D) des niveaux de service.

**Modèle basé sur DTU**

|**MAXSIZE**|**De base**|**S0-S2**|**S3-S12**|**P1-P6**| **P11-P15** | 
|-----------------|---------------|------------------|-----------------|-----------------|-----------------|-----------------| 
|100 Mo|√|√|√|√|√|  
|250 Mo|√|√|√|√|√|
|500 Mo|√|√|√|√|√|
|1 Go|√|√|√|√|√|
|2 Go|√ (D)|√|√|√|√|
|5 Go|Néant|√|√|√|√|
|10 GB|Néant|√|√|√|√|
|20 Go|Néant|√|√|√|√|
|30 Go|Néant|√|√|√|√|
|40 Go|Néant|√|√|√|√|
|50 Go|Néant|√|√|√|√|
|100 Go|Néant|√|√|√|√|
|150 Go|Néant|√|√|√|√|
|200 Go|Néant|√|√|√|√|
|250 Go|Néant|√ (D)|√ (D)|√|√|
|300 Go|Néant|Néant|√|√|√|
|400 Go|Néant|Néant|√|√|√|
|500 Go|Néant|Néant|√|√ (D)|√|
|750 Go|Néant|Néant|√|√|√|
|1 024 Go|Néant|Néant|√|√|√ (D)|
|À partir de 1 024 Go jusqu’à 4 096 Go par incréments de 256 Go* |Néant|Néant|Néant|Néant|√|√|  
  
\* P11 et P15 autorisent MAXSIZE jusqu’à 4 To, 1 024 Go étant la taille par défaut.  P11 et P15 peuvent utiliser jusqu’à 4 To de stockage inclus sans frais supplémentaires. Dans le niveau Premium, une taille supérieure à 1 To est actuellement disponible pour MAXSIZE dans les régions suivantes : Est des États-Unis2, Ouest des États-Unis, Gouvernement des États-Unis - Virginie, Europe de l’Ouest, Centre de l’Allemagne, Asie du Sud-Est, Est du Japon, Est de l’Australie, Centre du Canada et Est du Canada. Pour plus d’informations concernant les limitations des ressources pour le modèle basé sur DTU, consultez [Limites des ressources basées sur DTU](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits).  

La valeur MAXSIZE pour le modèle basé sur DTU, si elle est spécifiée, doit être une valeur valide indiquée dans le tableau ci-dessus pour le niveau de service spécifié.
 
**Modèle basé sur vCore**

**Niveau de service Usage général - Plateforme de calcul de 4e génération**
|MAXSIZE|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP4_24|
|:--- | --: |--: |--: |--: |--: |--:|
|Taille maximale des données (Go)|1024|1024|1536|3072|4096|4096|

**Niveau de service Usage général - Plateforme de calcul de 5e génération**
|MAXSIZE|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_48|GP_Gen5_80|
|:----- | ------: |-------: |-------: |--------: |--------: |---------:|--------: |---------: |
|Taille maximale des données (Go)|1024|1024|1536|3072|4096|4096|4096|4096|


**Niveau de service Critique pour l’entreprise - Plateforme de calcul de 4e génération**
|Niveau de performance|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|
|:--- | --: |--: |--: |--: |--: |--: |
|Taille maximale des données (Go)|1024|1024|1024|1024|1024|1024|

**Niveau de service Critique pour l’entreprise - Plateforme de calcul de 5e génération**
|MAXSIZE|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_48|BC_Gen5_80|
|:----- | ------: |-------: |-------: |--------: |--------: |---------:|--------: |---------: |
|Taille maximale des données (Go)|1024|1024|1024|1024|2048|4096|4096|4096|

Si aucune valeur `MAXSIZE` n’est définie lors de l’utilisation du modèle vCore, la valeur par défaut est de 32 Go. Pour plus d’informations sur les limitations des ressources du modèle basé sur vCore, consultez [Limites des ressources basées sur vCore](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits).
  
Les règles suivantes s'appliquent aux arguments MAXSIZE et EDITION.  
  
- Si EDITION est spécifié, mais MAXSIZE n'est pas spécifié, la valeur par défaut de l'édition est utilisée. Par exemple, si la valeur de EDITION est définie sur Standard, et que MAXSIZE n’est pas spécifié, la valeur de MAXSIZE est automatiquement définie sur 250 Mo.  
- Si MAXSIZE et EDITION ne sont pas spécifiés, la valeur de EDITION est définie sur Standard (S0), et celle de MAXSIZE sur 250 Go.  

SERVICE_OBJECTIVE

Spécifie le niveau de performances. Les valeurs disponibles pour l’objectif de service sont : `S0`, `S1`, `S2`, `S3`, `S4`, `S6`, `S7`, `S9`, `S12`, `P1`, `P2`, `P4`, `P6`, `P11`, `P15`, `GP_GEN4_1`, `GP_GEN4_2`, `GP_GEN4_4`, `GP_GEN4_8`, `GP_GEN4_16`, `GP_GEN4_24`, `BC_GEN4_1` `BC_GEN4_2` `BC_GEN4_4` `BC_GEN4_8` `BC_GEN4_16`, `BC_GEN4_24`, `GP_Gen5_2`, `GP_Gen5_4`, `GP_Gen5_8`, `GP_Gen5_16`, `GP_Gen5_24`, `GP_Gen5_32`, `GP_Gen5_48`, `GP_Gen5_80`, `BC_Gen5_2`,  `BC_Gen5_4`, `BC_Gen5_8`, `BC_Gen5_16`, `BC_Gen5_24`, `BC_Gen5_32`, `BC_Gen5_48`, `BC_Gen5_80`. 

Pour plus d’informations sur les objectifs de service, ainsi que sur la taille, les éditions et les combinaisons d’objectifs de service, consultez [Niveaux de service d’Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers). Si le SERVICE_OBJECTIVE spécifié n’est pas pris en charge par l’EDITION, un message d’erreur s’affiche. Si vous voulez modifier la valeur de SERVICE_OBJECTIVE pour passer d'un niveau de service à un autre (par exemple de S1 à P1), vous devrez également modifier la valeur d'EDITION. Pour plus d’informations sur les objectifs de service, ainsi que sur la taille, les éditions et les combinaisons d’objectifs de service, consultez [Niveaux de service et de performance d’Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-service-tiers/), [Limites des ressources basées sur DTU](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits) et [Limites des ressources basées sur vCore](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits).  La prise en charge des objectifs de service PRS a été supprimée. Pour poser des questions, utilisez cet alias de messagerie : premium-rs@microsoft.com. 
  
ELASTIC_POOL (name = \<elastic_pool_name>)
 
Pour créer une base de données dans un pool de bases de données élastique, définissez SERVICE_OBJECTIVE de la base de données sur ELASTIC_POOL et fournissez le nom du pool. Pour plus d’informations, consultez [Créer et gérer un pool de bases de données élastique SQL Database (préversion)](https://azure.microsoft.com/documentation/articles/sql-database-elastic-pool-portal/).  
  
AS COPY OF [source_server_name.]source_database_name

Pour la copie d'une base de données sur le même serveur ou sur un serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] différent.  
  
*source_server_name*  

Nom du serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] où se trouve la base de données source. Ce paramètre est facultatif lorsque la base de données source et la base de données de destination se trouveront sur le même serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
> [!NOTE]
> l'argument `AS COPY OF` ne prend pas en charge les noms de domaine complets uniques. En d'autres termes, si le nom de domaine complet de votre serveur est `serverName.database.windows.net`, utilisez uniquement `serverName` pendant la copie de base de données.  
  
*source_database_name*

Nom de la base de données copiée.  
  
[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ne prend pas en charge les options et arguments suivants lors de l’utilisation de l’instruction `CREATE DATABASE` :  
  
- Paramètres relatifs à l’emplacement physique de fichiers, tels que \< filespec> et \<filegroup>  
  
- Options d'accès externe, telles que DB_CHAINING et TRUSTWORTHY  
  
- Attachement d'une base de données  
  
- Options de Service Broker, telles que ENABLE_BROKER, NEW_BROKER et ERROR_BROKER_CONVERSATIONS  
  
- Instantané de base de données  
  
Pour plus d'informations sur les arguments et l'instruction `CREATE DATABASE`, consultez [CREATE DATABASE](../../t-sql/statements/create-database-sql-server-transact-sql.md).  
  
## <a name="remarks"></a>Notes 
 
Les bases de données dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ont plusieurs paramètres par défaut définis lors de la création de la base de données. Pour plus d’informations sur ces paramètres par défaut, consultez la liste de valeurs dans [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).  
  
MAXSIZE permet de limiter la taille de la base de données. Si la taille de la base de données atteint sa valeur MAXSIZE, vous recevez un code d’erreur 40544. Lorsque cela se produit, vous ne pouvez pas insérer ou mettre à jour des données, ni créer des objets (tels que des tables, des procédures stockées, des vues et des fonctions). Toutefois, vous pouvez encore lire et supprimer des données, tronquer des tables, supprimer des tables et des index et reconstruire des index. Vous pouvez ensuite mettre à jour MAXSIZE avec une valeur supérieure à votre taille de base de données actuelle ou supprimer certaines données afin de libérer de l'espace de stockage. Vous devrez peut-être patienter jusqu'à quinze minutes avant de pouvoir insérer de nouvelles données.  
  
> [!IMPORTANT]  
>  L'instruction `CREATE DATABASE` doit être la seule instruction dans un lot [!INCLUDE[tsql](../../includes/tsql-md.md)]. 
  
Pour modifier ultérieurement les valeurs de taille, d’édition ou d’objectif de service, utilisez [ALTER DATABASE &#40;Azure SQL Database&#41;](../../t-sql/statements/alter-database-azure-sql-database.md).  

L’argument CATALOG_COLLATION est uniquement disponible lors de la création de base de données. 
  
## <a name="database-copies"></a>Copies de bases de données  

La copie d’une base de données à l’aide de l’instruction `CREATE DATABASE` est une opération asynchrone. Par conséquent, une connexion au serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] n'est pas nécessaire pendant toute la durée du processus de copie. L’instruction `CREATE DATABASE` redonne le contrôle à l’utilisateur après la création de l’entrée dans sys.databases, mais avant que l’opération de copie de base de données soit terminée. Autrement dit, l'instruction `CREATE DATABASE` est renvoyée avec succès lorsque la copie de base de données est encore en cours.  
  
- Surveillance du processus de copie sur un serveur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] : interroger les colonnes `percentage_complete` ou `replication_state_desc` des [dm_database_copies](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md) ou de la colonne `state` dans la vue **sys.databases**. La vue [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md) peut aussi être utilisée, car elle retourne l’état des opérations de base de données, y compris la copie de base de données.  
  
Lorsque le processus de copie est terminé avec succès, la base de données de destination est cohérente avec la base de données source au niveau des transactions.  
  
La syntaxe et les règles sémantiques suivante s'appliquent à votre utilisation de l'argument `AS COPY OF` :  
  
- Le nom du serveur source et le nom du serveur pour la cible de copie peuvent être identiques ou différents. Lorsqu’ils sont identiques, ce paramètre est facultatif et le contexte de serveur de la session active est utilisé par défaut.  
  
- Les noms des bases de données source et de destination doivent être spécifiées, uniques et conformes aux règles applicables aux identificateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Identificateurs](http://go.microsoft.com/fwlink/p/?LinkId=180386).  
  
- L'instruction `CREATE DATABASE` doit être exécutée dans le contexte de la base de données master du serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] où la nouvelle base de données sera créée. 
- Une fois la copie terminée, la base de données de destination doit être gérée comme une base de données indépendante. Vous pouvez exécuter les instructions `ALTER DATABASE` et `DROP DATABASE` contre la nouvelle base de données indépendamment de la base de données source. Vous pouvez également copier la nouvelle base de données vers une autre nouvelle base de données.  
  
- La base de données source est toujours accessible pendant que la copie de base de données est en cours.  
  
 Pour plus d’informations, consultez [Créer une copie de base de données Azure SQL à l’aide de Transact-SQL](https://azure.microsoft.com/documentation/articles/sql-database-copy-transact-sql/).  
  
## <a name="permissions"></a>Autorisations  
 Pour créer une base de données, une connexion doit correspondre à l’un des éléments suivants :  
  
- La connexion du principal au niveau du serveur  
  
- L’administrateur Azure AD pour Azure SQL Server local  
  
- Une connexion qui est membre du rôle de base de données `dbmanager`  
  
 **Exigences supplémentaires relatives à l’utilisation de la syntaxe `CREATE DATABASE ... AS COPY OF` :** la connexion exécutant l’instruction sur le serveur local doit également être au moins `db_owner` sur le serveur source. Si la connexion est basée sur l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la connexion exécutant l’instruction sur le serveur local doit avoir une connexion correspondante sur le serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] source, avec les mêmes nom et mot de passe.  
  
## <a name="examples"></a>Exemples  
Pour un didacticiel de démarrage rapide vous montrant comment se connecter à une base de données SQL Azure à l’aide de SQL Server Management Studio, consultez [Azure SQL Database : Utiliser SQL Server Management Studio pour vous connecter et interroger des données](/azure/sql-database/sql-database-connect-query-ssms).  
  
### <a name="simple-example"></a>Exemple simple  
 Voici un exemple simple de création d’une base de données.  
  
```sql  
CREATE DATABASE TestDB1;  
```  
  
### <a name="simple-example-with-edition"></a>Exemple simple avec une édition  
 Voici un exemple simple de création d’une base de données standard.  
  
```sql  
CREATE DATABASE TestDB2  
( EDITION = 'standard' );  
```  
  
### <a name="example-with-additional-options"></a>Exemple avec des options supplémentaires  
 Voici un exemple utilisant plusieurs options.  
  
```sql  
CREATE DATABASE hito 
COLLATE Japanese_Bushu_Kakusu_100_CS_AS_KS_WS 
( MAXSIZE = 500 MB, EDITION = 'standard', SERVICE_OBJECTIVE = 'S1' ) ;  
```  
  
### <a name="creating-a-copy"></a>Création d’une copie  
 Voici un exemple de création d’une copie de base de données.  
  
```sql  
CREATE DATABASE escuela 
AS COPY OF school;  
```  
  
### <a name="creating-a-database-in-an-elastic-pool"></a>Création d’une base de données dans un pool élastique  
 Crée une base de données dans le pool nommé S3M100 :  
  
```sql  
CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL ( name = S3M100 ) ) ;  
```  
  
### <a name="creating-a-copy-of-a-database-on-another-server"></a>Création d’une copie de base de données sur un autre serveur  
 L’exemple suivant crée une copie de la base de données db_original, nommée db_copy, dans le niveau de performance P2 pour une base de données.  Cette opération est possible, que db_original se trouve dans un pool élastique ou dans un niveau de performance pour une base de données.  
  
```sql  
CREATE DATABASE db_copy 
  AS COPY OF ozabzw7545.db_original ( SERVICE_OBJECTIVE = 'P2' )  ;  
```  
  
 L’exemple suivant crée une copie de la base de données db_original, nommée db_copy, dans le pool élastique nommé ep1.  Cette opération est possible, que db_original se trouve dans un pool élastique ou dans un niveau de performance pour une base de données.  Si db_original se trouve dans un pool élastique avec un nom différent, la création de db_copy est malgré tout effectuée dans ep1.  
  
```sql  
CREATE DATABASE db_copy 
  AS COPY OF ozabzw7545.db_original 
  (SERVICE_OBJECTIVE = ELASTIC_POOL( name = ep1 ) ) ;  
```  

### <a name="create-database-with-specified-catalog-collation-value"></a>Créer la base de données avec la valeur de classement de catalogue spécifiée

L’exemple suivant définit le classement de catalogue sur DATABASE_DEFAULT lors de création de bases de données, rendant le classement de catalogue identique au classement de base de données.

```sql
CREATE DATABASE TestDB3 COLLATE Japanese_XJIS_140  (MAXSIZE = 100 MB, EDITION = ‘basic’)  
  WITH CATALOG_COLLATION = DATABASE_DEFAULT 
```
  
## <a name="see-also"></a>Voir aussi  

-  [sys.dm_database_copies &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)

- [ALTER DATABASE &#40;Azure SQL Database&#41;](alter-database-azure-sql-database.md) 
  
  

