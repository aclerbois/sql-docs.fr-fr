---
title: Sys.dm_tran_version_store (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_tran_version_store_TSQL
- sys.dm_tran_version_store
- dm_tran_version_store
- dm_tran_version_store_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_version_store dynamic management view
ms.assetid: 7ab44517-0351-4f91-bdd9-7cf940f03c51
caps.latest.revision: 32
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7de332c09b586b1c3a7feba4097a940ba9f29a83
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmtranversionstore-transact-sql"></a>sys.dm_tran_version_store (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Retourne une table virtuelle qui affiche tous les enregistrements de version dans la banque des versions. **Sys.dm_tran_version_store** n’est pas efficace, car elle interroge le magasin de versions entier, et la banque des versions peut être très volumineuse.  
  
 Chaque enregistrement avec contrôle de version est stocké sous forme de données binaires, avec des informations de suivi ou d'état. À l'instar des enregistrements des tables de la base de données, ceux de la banque des versions sont stockés dans des pages de 8 192 octets. Si un enregistrement excède ces 8 192 octets, il est réparti sur deux enregistrements.  
  
 Comme l'enregistrement avec contrôle de version est stocké sous forme binaire, cela ne pose pas de problème avec les différents classements des différentes bases de données. Utilisez **sys.dm_tran_version_store** pour rechercher les versions précédentes des lignes dans la représentation binaire, tels qu’ils existent dans la banque des versions.  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_tran_version_store  
```  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**transaction_sequence_num**|**bigint**|Numéro de séquence de la transaction qui produit la version de l'enregistrement.|  
|**version_sequence_num**|**bigint**|Numéro de séquence de l'enregistrement avec version. Cette valeur est unique dans la transaction produisant la version.|  
|**database_id**|**int**|ID de base de données de l'enregistrement avec contrôle de version.|  
|**rowset_id**|**bigint**|ID d'ensemble de lignes de l'enregistrement.|  
|**status**|**tinyint**|Indique si un enregistrement avec version a été réparti sur deux enregistrements. Si la valeur est 0, l'enregistrement est stocké sur une seule page. Si la valeur est 1, l'enregistrement est réparti sur deux enregistrements, lesquels sont stockés sur deux pages différentes.|  
|**min_length_in_bytes**|**smallint**|Longueur minimale de l'enregistrement, en octets.|  
|**record_length_first_part_in_bytes**|**smallint**|Longueur de la première partie de l'enregistrement avec contrôle de version, en octets.|  
|**record_image_first_part**|**varbinary(8000)**|Image binaire de la première partie de l'enregistrement avec contrôle de version.|  
|**record_length_second_part_in_bytes**|**smallint**|Longueur de la deuxième partie de l'enregistrement avec contrôle de version, en octets.|  
|**record_image_second_part**|**varbinary(8000)**|Image binaire de la deuxième partie de l'enregistrement avec contrôle de version.|  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre un scénario de test dans lequel quatre transactions simultanées, chacune étant identifiée par un numéro de séquence de transaction, sont exécutées dans une base de données où les options ALLOW_SNAPSHOT_ISOLATION et READ_COMMITTED_SNAPSHOT sont définies à ON. Les transactions suivantes sont exécutées :  
  
-   XSN-57 est une opération Update exécutée avec le niveau d'isolement sérialisable.  
  
-   XSN-58 est identique à XSN-57.  
  
-   XSN-59 est une opération Select exécutée avec le niveau d'isolement d'instantané.  
  
-   XSN-60 est identique à XSN-59.  
  
 La requête suivante est exécutée :  
  
```  
SELECT  
    transaction_sequence_num,  
    version_sequence_num,  
    database_id rowset_id,  
    status,  
    min_length_in_bytes,  
    record_length_first_part_in_bytes,  
    record_image_first_part,  
    record_length_second_part_in_bytes,  
    record_image_second_part  
  FROM sys.dm_tran_version_store;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_sequence_num version_sequence_num database_id  
------------------------ -------------------- -----------  
57                      1                    9             
57                      2                    9             
57                      3                    9             
58                      1                    9             
  
rowset_id            status min_length_in_bytes  
-------------------- ------ -------------------  
72057594038321152    0      12                   
72057594038321152    0      12                   
72057594038321152    0      12                   
72057594038386688    0      16                   
  
record_length_first_part_in_bytes  
---------------------------------  
29                                 
29                                 
29                                 
33                                 
  
record_image_first_part                                               
--------------------------------------------------------------------  
0x50000C0073000000010000000200FCB000000001000000270000000000          
0x50000C0073000000020000000200FCB000000001000100270000000000          
0x50000C0073000000030000000200FCB000000001000200270000000000          
0x500010000100000002000000030000000300F800000000000000002E0000000000  
  
record_length_second_part_in_bytes record_image_second_part  
---------------------------------- ------------------------  
0                                  NULL  
0                                  NULL  
0                                  NULL  
0                                  NULL  
```  
  
 Le résultat indique que XSN-57 a créé trois versions de ligne à partir d'une table et que XSN-58 a créé une version de ligne à partir d'une autre table.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
