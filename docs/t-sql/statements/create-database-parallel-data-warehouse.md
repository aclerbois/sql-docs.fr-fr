---
title: CREATE DATABASE (Parallel Data Warehouse) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 40cacde4-ac72-45f7-9564-d76e2b4a741a
caps.latest.revision: 13
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: a7cef1f977e59e8145a57c806d7edaeb262b1275
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="create-database-parallel-data-warehouse"></a>CREATE DATABASE (Parallel Data Warehouse)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md)]

  Crée une base de données sur une appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Utilisez cette instruction pour créer tous les fichiers associés à une base de données d’appliance, et pour définir les options de croissance automatique et de taille maximale pour les tables de base de données et le journal des transactions.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
CREATE DATABASE database_name   
WITH (   
    [ AUTOGROW = ON | OFF , ]   
    REPLICATED_SIZE = replicated_size [ GB ] ,  
    DISTRIBUTED_SIZE = distributed_size [ GB ] ,  
    LOG_SIZE = log_size [ GB ] )  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Nom de la nouvelle base de données. Pour plus d’informations sur les noms de bases de données autorisés, consultez « Règles de nommage d’objet » et « Noms de bases de données réservés » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 AUTOGROW = ON | **OFF**  
 Spécifie si les paramètres *replicated_size*, *distributed_size* et *log_size* pour cette base de données croissent automatiquement en fonction des besoins au-delà de leur taille spécifiée. La valeur par défaut est **OFF**.  
  
 Si AUTOGROW est ON, *replicated_size*, *distributed_size*, et *log_size* croissent en fonction des besoins (et non par blocs de la taille spécifiée initiale) lors de chaque insertion de données, mise à jour ou autre action nécessitant davantage de stockage que ce qui a déjà été alloué.  
  
 Si AUTOGROW est OFF, les tailles n’augmentent pas automatiquement. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] retourne une erreur en cas de tentative d’exécution d’une action qui exige que *replicated_size*, *distributed_size* ou *log_size* croisse au-delà de sa valeur spécifiée.  
  
 AUTOGROW est ON pour toutes les tailles ou OFF pour toutes les tailles. Par exemple, vous ne pouvez pas définir AUTOGROW ON pour *log_size* sans le définir également pour *replicated_size*.  
  
 *replicated_size* [ GB ]  
 Nombre positif. Définit la taille (en gigaoctets entier ou décimal) pour l’espace total alloué aux tables répliquées et aux données correspondantes *sur chaque nœud de calcul*. Pour plus d’informations sur les exigences liées aux valeurs *replicated_size* minimales et maximales, consultez « Valeurs minimales et maximales » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 Si AUTOGROW est ON, les tables répliquées sont autorisées à croître au-delà de cette limite.  
  
 Si AUTOGROW est OFF, une erreur est retournée si un utilisateur tente de créer une table répliquée, d’insérer des données dans une table répliquée existante, ou de mettre à jour une table répliquée existante d’une manière qui augmenterait la taille au-delà de *replicated_size*.  
  
 *distributed_size* [ GB ]  
 Nombre positif. Taille (en gigaoctets entier ou décimal) pour l’espace total alloué aux tables distribuées et aux données correspondantes *à l’échelle de l’appliance*. Pour plus d’informations sur les exigences liées aux valeurs *distributed_size* minimales et maximales, consultez « Valeurs minimales et maximales » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 Si AUTOGROW est ON, les tables distribuées sont autorisées à croître au-delà de cette limite.  
  
 Si AUTOGROW est OFF, une erreur est retournée si un utilisateur tente de créer une table distribuée, d’insérer des données dans une table distribuée existante, ou de mettre à jour une table distribuée existante d’une manière qui augmenterait la taille au-delà de *replicated_size*.  
  
 *log_size* [ GB ]  
 Nombre positif. Taille (en gigaoctets entier ou décimal) du journal des transactions *à l’échelle de l’appliance*.  
  
 Pour plus d’informations sur les exigences liées aux valeurs *log_size* minimales et maximales, consultez « Valeurs minimales et maximales » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 Si AUTOGROW est ON, le fichier journal est autorisé à croître au-delà de cette limite. Utilisez l’instruction [DBCC SHRINKLOG (Azure SQL Data Warehouse)](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) afin de réduire la taille des fichiers journaux à leur taille d’origine.  
  
 Si AUTOGROW est OFF, une erreur est retournée pour toute action qui augmenterait la taille du journal sur un nœud de calcul au-delà de *log_size*.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **CREATE ANY DATABASE** dans la base de données master ou l’appartenance au rôle serveur fixe **sysadmin**.  
  
 L'exemple suivant fournit l'autorisation de créer une base de données à l'utilisateur de base de données Fay.  
  
```  
USE master;  
GO  
GRANT CREATE ANY DATABASE TO [Fay];  
GO  
```  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Les bases de données sont créées avec le niveau de compatibilité de base de données 120, qui est le niveau de compatibilité pour [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Cela garantit que la base de données pourra utiliser toutes les fonctionnalités de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] utilisées par PDW.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 L’instruction CREATE DATABASE n’est pas autorisée dans une transaction explicite. Pour plus d’informations, consultez [Instructions](../../t-sql/statements/statements.md).  
  
 Pour plus d’informations sur les contraintes minimales et maximales sur les bases de données, consultez « Valeurs minimales et maximales » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].  
  
 Lors de la création d’une base de données, il doit y avoir suffisamment d’espace libre disponible *sur chaque nœud de calcul* pour allouer le total combiné des tailles suivantes :  
  
-   Base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec des tables de la taille de *replicated_table_size*.  
  
-   Base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec des tables de la taille de (*distributed_table_size* / nombre de nœuds de calcul).  
  
-   Journaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la taille de (*log_size* / nombre de nœuds de calcul).  
  
## <a name="locking"></a>Verrouillage  
 Prend un verrou partagé sur l’objet DATABASE.  
  
## <a name="metadata"></a>Métadonnées  
 Une fois cette opération réussie, une entrée pour cette base de données apparaît dans les vues de métadonnées [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) et [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
## <a name="examples-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-basic-database-creation-examples"></a>A. Exemples simples de création de base de données  
 L’exemple suivant crée la base de données `mytest` avec une allocation de stockage de 100 Go par nœud de calcul pour les tables répliquées, 500 Go par appliance pour les tables distribuées et 100 Go par appliance pour le journal des transactions. Dans cet exemple, AUTOGROW est OFF par défaut.  
  
```  
CREATE DATABASE mytest  
   WITH   
   (REPLICATED_SIZE = 100 GB,  
   DISTRIBUTED_SIZE = 500 GB,  
   LOG_SIZE = 100 GB );  
```  
  
 L’exemple suivant crée la base de données `mytest` avec les mêmes paramètres que ci-dessus, sauf que AUTOGROW est ON. Cela permet à la base de données de croître au-delà des paramètres de taille spécifiés.  
  
```  
CREATE DATABASE mytest  
   WITH   
   (AUTOGROW = ON,  
   REPLICATED_SIZE = 100 GB,  
   DISTRIBUTED_SIZE = 500 GB,  
   LOG_SIZE = 100 GB);  
```  
  
### <a name="b-creating-a-database-with-partial-gigabyte-sizes"></a>B. Création d’une base de données avec des valeurs de taille en gigaoctets partielles  
 L’exemple suivant crée la base de données `mytest` avec AUTOGROW OFFn une allocation de stockage de 1,5 Go par nœud de calcul pour les tables répliquées, 5,25 Go par appliance pour les tables distribuées et 10 Go par appliance pour le journal des transactions.  
  
```  
CREATE DATABASE mytest  
   WITH   
   (REPLICATED_SIZE = 1.5 GB,  
   DISTRIBUTED_SIZE = 5.25 GB,  
   LOG_SIZE = 10 GB);  
```  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER DATABASE &#40;Parallel Data Warehouse&#41;](../../t-sql/statements/alter-database-parallel-data-warehouse.md)   
 [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md)  
  
  
