---
title: ALTER DATABASE (Parallel Data Warehouse) | Microsoft Docs
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
ms.assetid: 5751656b-7aae-4152-a314-4c631bea4fc4
caps.latest.revision: 10
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3d199b9822d591c10f1f4d9af232b9f74d7e8a81
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="alter-database-parallel-data-warehouse"></a>ALTER DATABASE (Parallel Data Warehouse)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md)]

  Modifie les options de taille de base de données maximale pour les tables répliquées, les tables distribuées et le journal des transactions dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Cette instruction permet de gérer les allocations de l’espace disque à mesure que la taille d’une base de données augmente ou diminue.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Parallel Data Warehouse  
ALTER DATABASE database_name    
    SET ( <set_database_options>   | <db_encryption_option> )  
[;]  
  
<set_database_options> ::=   
{  
    AUTOGROW = { ON | OFF }  
    | REPLICATED_SIZE = size [GB]  
    | DISTRIBUTED_SIZE = size [GB]  
    | LOG_SIZE = size [GB]  
}  
  
<db_encryption_option> ::=  
    ENCRYPTION { ON | OFF }  
```  
  
## <a name="arguments"></a>Arguments  
 *database_name*  
 Nom de la base de données à modifier. Pour afficher une liste des bases de données sur l’appliance, utilisez [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 AUTOGROW = { ON | OFF }  
 Met à jour l’option AUTOGROW. Quand AUTOGROW est défini sur ON, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] augmente automatiquement l’espace alloué pour les tables répliquées, les tables distribuées et le journal des transactions en fonction des besoins pour s’adapter à la croissance des besoins de stockage. Quand AUTOGROW est défini sur OFF, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] retourne une erreur si des tables, répliquées, des tables distribuées ou le journal des transactions dépasse le paramètre de taille maximale.  
  
 REPLICATED_SIZE = *size* [GB]  
 Spécifie le nouveau nombre maximal de gigaoctets par nœud de calcul pour le stockage de toutes les tables répliquées dans la base de données en cours de modification. Si vous planifiez l’espace de stockage de l’appliance, vous devez multiplier REPLICATED_SIZE par le nombre de nœuds de calcul dans l’appliance.  
  
 DISTRIBUTED_SIZE = *size* [GB]  
 Spécifie le nouveau nombre maximal de gigaoctets par base de données pour le stockage de toutes les tables distribuées dans la base de données en cours de modification. La taille est répartie entre tous les nœuds de calcul dans l’appliance.  
  
 LOG_SIZE = *size* [GB]  
 Spécifie le nouveau nombre maximal de gigaoctets par base de données pour le stockage de tous les journaux des transactions dans la base de données en cours de modification. La taille est répartie entre tous les nœuds de calcul dans l’appliance.  
  
 ENCRYPTION { ON | OFF }  
 Spécifie si la base de données doit être chiffrée (ON) ou non chiffrée (OFF). Le chiffrement peut être configuré uniquement pour [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] quand [sp_pdw_database_encryption](http://msdn.microsoft.com/5011bb7b-1793-4b2b-bd9c-d4a8c8626b6e) a été défini sur **1**. Une clé de chiffrement de base de données doit être créée avant de pouvoir configurer le chiffrement transparent des données. Pour plus d’informations sur le chiffrement des bases de données, consultez [Chiffrement transparent des données &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation ALTER sur la base de données.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Les valeurs de REPLICATED_SIZE, DISTRIBUTED_SIZE et LOG_SIZE peuvent être supérieures, égales ou inférieures aux valeurs actuelles de la base de données.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Les opérations d’augmentation et de réduction sont approximatives. Les tailles réelles obtenues peuvent varier en fonction des paramètres de taille.  
  
 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] n’exécute pas l’instruction ALTER DATABASE comme une opération atomique. Si l’instruction est interrompue pendant l’exécution, les modifications qui ont déjà eu lieu sont conservées.  
  
## <a name="locking-behavior"></a>Comportement de verrouillage  
 Prend un verrou partagé sur l’objet DATABASE. Vous ne pouvez pas modifier une base de données qui est en cours d’utilisation par un autre utilisateur pour une opération de lecture ou d’écriture. Cela inclut les sessions qui ont émis une instruction [USE](http://msdn.microsoft.com/158ec56b-b822-410f-a7c4-1a196d4f0e15) sur la base de données.  
  
## <a name="performance"></a>Performances  
 La réduction de la taille d’une base de données peut prendre beaucoup de temps et consommer beaucoup de ressources système, en fonction de la taille des données réelles contenues dans la base de données et du volume de fragmentation sur le disque. Par exemple, la réduction de la taille d’une base de données peut prendre plusieurs heures ou plus.  
  
## <a name="determining-encryption-progress"></a>Détermination de la progression du chiffrement  
 Pour déterminer la progression du chiffrement transparent des données de base de données sous la forme d’un pourcentage, utilisez la requête suivante :  
  
```  
WITH  
database_dek AS (  
    SELECT ISNULL(db_map.database_id, dek.database_id) AS database_id,  
        dek.encryption_state, dek.percent_complete,  
        dek.key_algorithm, dek.key_length, dek.encryptor_thumbprint,  
        type  
    FROM sys.dm_pdw_nodes_database_encryption_keys AS dek  
    INNER JOIN sys.pdw_nodes_pdw_physical_databases AS node_db_map  
        ON dek.database_id = node_db_map.database_id   
        AND dek.pdw_node_id = node_db_map.pdw_node_id  
    LEFT JOIN sys.pdw_database_mappings AS db_map  
        ON node_db_map .physical_name = db_map.physical_name  
    INNER JOIN sys.dm_pdw_nodes nodes  
        ON nodes.pdw_node_id = dek.pdw_node_id  
    WHERE dek.encryptor_thumbprint <> 0x  
),  
dek_percent_complete AS (  
    SELECT database_dek.database_id, AVG(database_dek.percent_complete) AS percent_complete  
    FROM database_dek  
    WHERE type = 'COMPUTE'  
    GROUP BY database_dek.database_id  
)  
SELECT DB_NAME( database_dek.database_id ) AS name,  
    database_dek.database_id,  
    ISNULL(  
       (SELECT TOP 1 dek_encryption_state.encryption_state  
        FROM database_dek AS dek_encryption_state  
        WHERE dek_encryption_state.database_id = database_dek.database_id  
        ORDER BY (CASE encryption_state  
            WHEN 3 THEN -1  
            ELSE encryption_state  
            END) DESC), 0)  
        AS encryption_state,  
dek_percent_complete.percent_complete,  
database_dek.key_algorithm, database_dek.key_length, database_dek.encryptor_thumbprint  
FROM database_dek  
INNER JOIN dek_percent_complete   
    ON dek_percent_complete.database_id = database_dek.database_id  
WHERE type = 'CONTROL';  
```  
  
 Pour obtenir un exemple complet illustrant toutes les étapes de l’implémentation de TDE, consultez [Chiffrement transparent des données &#40;TDE&#41;](../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
## <a name="examples-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-altering-the-autogrow-setting"></a>A. Modification du paramètre AUTOGROW  
 Définissez AUTOGROW sur ON pour la base de données `CustomerSales`.  
  
```  
ALTER DATABASE CustomerSales  
    SET ( AUTOGROW = ON );  
```  
  
### <a name="b-altering-the-maximum-storage-for-replicated-tables"></a>B. Modification du stockage maximal pour les tables répliquées  
 L’exemple suivant définit la limite de stockage des tables répliquées sur 1 Go pour la base de données `CustomerSales`. Il s’agit de la limite de stockage par nœud de calcul.  
  
```  
ALTER DATABASE CustomerSales  
    SET ( REPLICATED_SIZE = 1 GB );  
```  
  
### <a name="c-altering-the-maximum-storage-for-distributed-tables"></a>C. Modification du stockage maximal pour les tables distribuées  
 L’exemple suivant définit la limite de stockage des tables distribuées sur 1 000 GB (un téraoctet) pour la base de données `CustomerSales`. Il s’agit de la limite de stockage combiné sur l’ensemble de l’appliance pour tous les nœuds de calcul, et non pas la limite de stockage par nœud de calcul.  
  
```  
ALTER DATABASE CustomerSales  
    SET ( DISTRIBUTED_SIZE = 1000 GB );  
```  
  
### <a name="d-altering-the-maximum-storage-for-the-transaction-log"></a>D. Modification du stockage maximal pour le journal des transactions  
 L’exemple suivant met à jour la base de données `CustomerSales` pour avoir une taille maximale du journal de transactions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de 10 Go pour l’appliance.  
  
```  
ALTER DATABASE CustomerSales  
    SET ( LOG_SIZE = 10 GB );  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE DATABASE &#40;Parallel Data Warehouse&#41;](../../t-sql/statements/create-database-parallel-data-warehouse.md)   
 [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md)  
  
  
