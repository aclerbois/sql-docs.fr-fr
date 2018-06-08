---
title: ALTER EXTERNAL DATA SOURCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/09/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ALTER EXTERNAL DATA SOURCE
- ALTER_EXTERNAL_DATA_SOURCE
dev_langs:
- TSQL
helpviewer_keywords:
- polybase, alter external data source statement
- ALTER EXTERNAL DATA SOURCE statement
ms.assetid: a34b9e90-199d-46d0-817a-a7e69387bf5f
caps.latest.revision: 8
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 7ff97e4cc6549b7d441fe7f6124a3b60b1a3041c
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="alter-external-data-source-transact-sql"></a>ALTER EXTERNAL DATA SOURCE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Modifie une source de données externe utilisée pour créer une table externe. La source de données externe peut être Hadoop ou le stockage Blob Azure (WASB).
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Modify an external data source
-- Applies to: SQL Server (2016 or later)
ALTER EXTERNAL DATA SOURCE data_source_name SET
    {   
        LOCATION = 'server_name_or_IP' [,] |
        RESOURCE_MANAGER_LOCATION = <'IP address;Port'> [,] |
        CREDENTIAL = credential_name
    }  
    [;]  

-- Modify an external data source pointing to Azure Blob storage
-- Applies to: SQL Server (starting with 2017)
ALTER EXTERNAL DATA SOURCE data_source_name
    WITH (
        TYPE = BLOB_STORAGE,
        LOCATION = 'https://storage_account_name.blob.core.windows.net'
        [, CREDENTIAL = credential_name ]
    )  
```  
  
## <a name="arguments"></a>Arguments  
 data_source_name Spécifie le nom défini par l’utilisateur de la source de données. Le nom doit être unique.
  
 LOCATION = 'server_name_or_IP' Spécifie le nom du serveur ou une adresse IP.
  
 RESOURCE_MANAGER_LOCATION = '\<IP address;Port>' Spécifie l’emplacement du Gestionnaire de ressources Hadoop. S’il est spécifié, l’optimiseur de requête peut choisir de prétraiter les données d’une requête PolyBase en utilisant les fonctionnalités de calcul d’Hadoop. C’est une décision basée sur les coûts. Appelée pushdown de prédicats, cette opération peut considérablement réduire le volume des données transférées entre Hadoop et SQL, et donc améliorer les performances des requêtes.
  
 CREDENTIAL = Credential_Name Spécifie les informations d’identification nommées. Consultez [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).

TYPE = BLOB_STORAGE   
**S’applique à :** [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].
Pour les opérations en bloc uniquement, `LOCATION` doit être valide dans l’URL vers le stockage Blob Azure. Ne placez pas **/**, le nom du fichier ou les paramètres de signature d’accès partagé à la fin de l’URL `LOCATION`.
Les informations d’identification utilisées doivent être créées avec `SHARED ACCESS SIGNATURE` comme identité. Pour plus d’informations sur les signatures d’accès partagé, consultez [Utilisation des signatures d’accès partagé (SAP)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

  
  
## <a name="remarks"></a>Notes 
 Une seule une source peut être modifiée à la fois. Des demandes simultanées de modifications de la même source provoquent l’attente d’une instruction. Toutefois, différentes sources peuvent être modifiés en même temps. Cette instruction peut s’exécuter en même temps que d’autres instructions.
  
## <a name="permissions"></a>Autorisations  
 Exige l’autorisation ALTER ANY EXTERNAL DATA SOURCE.
 > [!IMPORTANT]  
 >  L’autorisation ALTER ANY EXTERNAL DATA SOURCE accorde à n’importe quel principal la possibilité de créer et de modifier tout objet de source de données externe. Par conséquent, elle permet également d’accéder à toutes les informations d’identification délimitées à la base de données sur la base de données. Cette autorisation doit être considérée comme fournissant des privilèges très élevés, et doit donc être accordée uniquement aux principaux de confiance du système.

  
## <a name="examples"></a>Exemples  
 L’exemple suivant modifie l’emplacement d’une source de données existante ainsi que l’emplacement de son gestionnaire de ressources.
  
```  
ALTER EXTERNAL DATA SOURCE hadoop_eds SET
     LOCATION = 'hdfs://10.10.10.10:8020',
     RESOURCE_MANAGER_LOCATION = '10.10.10.10:8032'
    ;
  
```  

 L’exemple suivant modifie les informations d’identification permettant de se connecter à une source de données existante.
  
```  
ALTER EXTERNAL DATA SOURCE hadoop_eds SET
   CREDENTIAL = new_hadoop_user
    ;
```