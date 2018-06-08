---
title: CREATE DATABASE (Azure SQL Data Warehouse | Microsoft Docs
ms.custom: ''
ms.date: 02/14/20178
ms.prod: ''
ms.prod_service: sql-data-warehouse
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.component: t-sql|statements
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 621d348cface10d459693fc64b261fc2fa2ddf04
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="create-database-azure-sql-data-warehouse"></a>CREATE DATABASE (Azure SQL Data Warehouse)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md)]

Crée une base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
CREATE DATABASE database_name [ COLLATE collation_name ]  
(  
    [ MAXSIZE = { 
          250 | 500 | 750 | 1024 | 5120 | 10240 | 20480 | 30720 
        | 40960 | 51200 | 61440 | 71680 | 81920 | 92160 | 102400 
        | 153600 | 204800 | 245760 
      } GB ,
    ]  
    EDITION = 'datawarehouse',  
    SERVICE_OBJECTIVE = { 
         'DW100' | 'DW200' | 'DW300' | 'DW400' | 'DW500' | 'DW600' 
        | 'DW1000' | 'DW1200' | 'DW1500' | 'DW2000' | 'DW3000' | 'DW6000' 
        | 'DW1000c' | 'DW1500c' | 'DW2000c' | 'DW2500c' | 'DW3000c' | 'DW5000c' 
        | 'DW6000c' | 'DW7500c' | 'DW10000c' | 'DW15000c' | 'DW30000c'
    }  
)  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
*database_name*  
Nom de la nouvelle base de données. Ce nom doit respecter les règles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applicables aux identificateurs et être unique sur le serveur SQL qui peut héberger à la fois des bases de données [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et des bases de données [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Pour plus d’informations, consultez [Identificateurs](http://go.microsoft.com/fwlink/p/?LinkId=180386).  
  
*collation_name*  
Indique le classement par défaut de la base de données. Le nom du classement peut être un nom de classement Windows ou SQL. S’il est omis, le classement par défaut, SQL_Latin1_General_CP1_CI_AS, est affecté à la base de données.  
  
Pour plus d’informations sur les noms de classements Windows et SQL, consultez [COLLATE (Transact-SQL)](http://msdn.microsoft.com/library/ms184391.aspx).  
  
*EDITION*  
Spécifie la couche de service de la base de données. Pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] utilisez 'datawarehouse'.  
  
*MAXSIZE*  
La valeur par défaut est 245 760 Go (240 To).  

**S’applique à :** Niveau de performance Optimisé pour l’élasticité

Taille maximale autorisée pour la base de données. La base de données ne peut pas croître au-delà de MAXSIZE. 

**S’applique à :** Niveau de performance Optimisé pour le calcul

Taille maximale autorisée pour les données rowstore dans la base de données. Les données stockées dans les tables rowstore, dans un deltastore d’index columnstore ou un index non cluster sur un index columnstore cluster ne peuvent pas croître au-delà de MAXSIZE.  Les données compressées au format columnstore n’ont pas de taille limite et ne sont pas restreintes par MAXSIZE.
  
SERVICE_OBJECTIVE  
Spécifie le niveau de performances. Pour plus d’informations sur les objectifs de service concernant [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], consultez [Niveaux de performance](https://azure.microsoft.com/documentation/articles/performance-tiers/).  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
Utilisez [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/databasepropertyex-transact-sql.md) pour afficher les propriétés de la base de données.  
  
Utilisez [ALTER DATABASE &#40;Azure SQL Data Warehouse&#41;](../../t-sql/statements/alter-database-azure-sql-data-warehouse.md) pour changer la taille maximale ou les valeurs des objectifs de service par la suite.   

SQL Data Warehouse est défini sur COMPATIBILITY_LEVEL 130 et ne peut pas être modifié. Pour plus d’informations, consultez [Meilleures performances des requêtes avec le niveau de compatibilité 130 dans Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-compatibility-level-query-performance-130/).
  
## <a name="permissions"></a>Autorisations  
Autorisations nécessaires :  
  
-   Connexion au principal de niveau serveur, créé par le processus de déploiement ou  
  
-   Membre du rôle de base de données `dbmanager`.  
  
## <a name="error-handling"></a>Gestion des erreurs  
Si la taille de la base de données atteint la valeur MAXSIZE, vous recevez un code d’erreur 40544. Lorsque cela se produit, vous ne pouvez pas insérer ou mettre à jour des données, ni créer des objets (tels que des tables, des procédures stockées, des vues et des fonctions). Vous pouvez toujours lire et supprimer des données, tronquer des tables, supprimer des tables et des index, et reconstruire des index. Vous pouvez ensuite mettre à jour MAXSIZE avec une valeur supérieure à votre taille de base de données actuelle ou supprimer certaines données afin de libérer de l'espace de stockage. Vous devrez peut-être patienter jusqu'à quinze minutes avant de pouvoir insérer de nouvelles données.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
Vous devez être connecté à la base de données master pour créer une base de données.  
  
L'instruction `CREATE DATABASE` doit être la seule instruction dans un lot [!INCLUDE[tsql](../../includes/tsql-md.md)].

Vous ne pouvez pas modifier le classement de la base de données après la création de celle-ci.   
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]  
  
### <a name="a-simple-example"></a>A. Exemple simple  
Voici un exemple simple de création d’une base de données de l’entrepôt de données. La base de données est créée avec la plus petite taille maximale (10 240 Go), le classement par défaut (SQL_Latin1_General_CP1_CI_AS) et la plus petite puissance de calcul (DW100).  
  
```  
CREATE DATABASE TestDW  
(EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');  
```  
  
### <a name="b-create-a-data-warehouse-database-with-all-the-options"></a>B. Créer une base de données de l’entrepôt de données avec toutes les options  
Voici un exemple de création d’un entrepôt de données de 10 téraoctets en utilisant toutes les options.  
  
```  
CREATE DATABASE TestDW COLLATE Latin1_General_100_CI_AS_KS_WS  
(MAXSIZE = 10240 GB, EDITION = 'datawarehouse', SERVICE_OBJECTIVE = 'DW1000');  
```  
  
## <a name="see-also"></a> Voir aussi  
[ALTER DATABASE &#40;Azure SQL Data Warehouse&#40;](../../t-sql/statements/alter-database-azure-sql-data-warehouse.md)
[CREATE TABLE &#40;Azure SQL Data Warehouse&#41;](../../t-sql/statements/create-table-azure-sql-data-warehouse.md) 
[DROP DATABASE &#40;Transact-SQL&#40;](../../t-sql/statements/drop-database-transact-sql.md) 
  

