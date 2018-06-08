---
title: ALTER DATABASE (Azure SQL Data Warehouse) | Microsoft Docs
ms.custom: ''
ms.date: 02/15/2018
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
ms.assetid: da712a46-5f8a-4888-9d33-773e828ba845
caps.latest.revision: 20
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 2e89ba1dde52c1b5cb919ff34181d7ca723b6c61
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="alter-database-azure-sql-data-warehouse"></a>ALTER DATABASE (Azure SQL Data Warehouse)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md)]

Modifie le nom, la taille maximale ou l’objectif de service d’une base de données.  
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
ALTER DATABASE database_name  

  MODIFY NAME = new_database_name  
| MODIFY ( <edition_option> [, ... n] )  
  
<edition_option> ::=   
      MAXSIZE = { 
            250 | 500 | 750 | 1024 | 5120 | 10240 | 20480 
          | 30720 | 40960 | 51200 | 61440 | 71680 | 81920 
          | 92160 | 102400 | 153600 | 204800 | 245760 
      } GB  
      | SERVICE_OBJECTIVE = { 
            'DW100' | 'DW200' | 'DW300' | 'DW400' | 'DW500' 
          | 'DW600' | 'DW1000' | 'DW1200' | 'DW1500' | 'DW2000' 
          | 'DW3000' | 'DW6000' | 'DW1000c' | 'DW1500c' | 'DW2000c' 
          | 'DW2500c' | 'DW3000c' | 'DW5000c' | 'DW6000c' | 'DW7500c' 
          | 'DW10000c' | 'DW15000c' | 'DW30000c'
      }  
```  
  
## <a name="arguments"></a>Arguments  
*database_name*  
Spécifie le nom de la base de données à modifier.  

MODIFY NAME = *nouveau_nom_base_de_données*  
Renomme la base de données avec le nom spécifié *nouveau_nom_base_de_données*.  
  
MAXSIZE  
La valeur par défaut est 245 760 Go (240 To).  

**S’applique à :** Niveau de performance Optimisé pour l’élasticité

Taille maximale autorisée pour la base de données. La base de données ne peut pas croître au-delà de MAXSIZE. 

**S’applique à :** Niveau de performance Optimisé pour le calcul

Taille maximale autorisée pour les données rowstore dans la base de données. Les données stockées dans les tables rowstore, dans un deltastore d’index columnstore ou un index non cluster sur un index columnstore cluster ne peuvent pas croître au-delà de MAXSIZE.  Les données compressées au format columnstore n’ont pas de taille limite et ne sont pas restreintes par MAXSIZE. 
  
SERVICE_OBJECTIVE  
Spécifie le niveau de performances. Pour plus d’informations sur les objectifs de service concernant [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)], consultez [Niveaux de performance](https://azure.microsoft.com/documentation/articles/performance-tiers/).  
  
## <a name="permissions"></a>Autorisations  
Nécessite ces autorisations :  
  
-   Connexion au principal de niveau serveur (créée par le processus de provisionnement) ou  
  
-   Membre du rôle de base de données `dbmanager`.  
  
Le propriétaire de la base de données ne peut pas modifier la base de données à moins d'être membre du rôle `dbmanager`.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
La base de données actuelle doit être différente de celle que vous modifiez, par conséquent **ALTER doit être exécuté tout en étant connecté à la base de données master**.  
  
SQL Data Warehouse est défini sur COMPATIBILITY_LEVEL 130 et ne peut pas être modifié. Pour plus d’informations, consultez [Meilleures performances des requêtes avec le niveau de compatibilité 130 dans Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-compatibility-level-query-performance-130/).
  
Pour diminuer la taille d'une base de données, utilisez [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md).  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
Pour exécuter ALTER DATABASE, la base de données doit être en ligne et ne peut pas être dans un état suspendu.  
  
L’instruction ALTER DATABASE doit s’exécuter en mode de validation automatique, qui est le mode de gestion de transaction par défaut. Ce mode est défini dans les paramètres de connexion.  
  
L’instruction ALTER DATABASE ne peut pas faire partie d’une transaction définie par l’utilisateur.

Vous ne pouvez pas changer le classement de la base de données.  
  
## <a name="examples"></a>Exemples  
Avant d’exécuter ces exemples, vérifiez que la base de données que vous modifiez n’est pas la base de données actuelle. La base de données actuelle doit être différente de celle que vous modifiez, par conséquent **ALTER doit être exécuté tout en étant connecté à la base de données master**.  

### <a name="a-change-the-name-of-the-database"></a>A. Changer le nom de la base de données  

```  
ALTER DATABASE AdventureWorks2012  
MODIFY NAME = Northwind;  
```  
  
### <a name="b-change-max-size-for-the-database"></a>B. Changer la taille maximale de la base de données  
  
```  
ALTER DATABASE dw1 MODIFY ( MAXSIZE=10240 GB );  
```  
  
### <a name="c-change-the-performance-level"></a>C. Changer le niveau de performance  
  
```  
ALTER DATABASE dw1 MODIFY ( SERVICE_OBJECTIVE= 'DW1200' );  
```  
  
### <a name="d-change-the-max-size-and-the-performance-level"></a>D. Changer la taille maximale et le niveau de performance  
  
```  
ALTER DATABASE dw1 MODIFY ( MAXSIZE=10240 GB, SERVICE_OBJECTIVE= 'DW1200' );  
```  
  
## <a name="see-also"></a> Voir aussi  
[CREATE DATABASE (Azure SQL Data Warehouse)](../../t-sql/statements/create-database-azure-sql-data-warehouse.md)
[Rubriques de référence pour SQL Data Warehouse ](https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-reference/)  
  
