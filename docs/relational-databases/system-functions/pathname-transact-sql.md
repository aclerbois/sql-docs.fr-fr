---
title: Chemin d’accès (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- PathName_TSQL
- PathName
dev_langs:
- TSQL
helpviewer_keywords:
- PathName FILESTREAM [SQL Server]
ms.assetid: 6b95ad90-6c82-4a23-9294-a2adb74934a3
caps.latest.revision: 32
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 80fc13baa2d538e054ed88607cd4b45c6477cb6e
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="pathname-transact-sql"></a>PathName (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Retourne le chemin d'accès d'un objet blob FILESTREAM. L’API OpenSqlFilestream utilise ce chemin d’accès pour retourner un handle qu’une application peut utiliser pour travailler avec les données BLOB à l’aide des API Win32. PathName est en lecture seule.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
column_name.PathName ( @option [ , use_replica_computer_name ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *column_name*  
 Est le nom de colonne d’un **varbinary (max)** colonne FILESTREAM. *column_name* doit être un nom de colonne. Il ne peut s'agir d'une expression ou du résultat d'une instruction CAST ou CONVERT.  
  
 Demande de PathName pour une colonne de tout autre type de données ou pour un **varbinary (max)** columnthat n’a pas de provoquer une erreur de compilation de requête de la volonté d’attribut de stockage FILESTREAM.  
  
 *@option*  
 Entier [expression](../../t-sql/language-elements/expressions-transact-sql.md) qui définit comment le composant serveur du chemin d’accès doit être mis en forme. *@option* peut prendre l’une des valeurs suivantes. La valeur par défaut est 0.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0|Retourne le nom de serveur converti au format BIOS, par exemple : `\\SERVERNAME\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F19F7-38EA-4AB0-BB89-E6C545DBD3F9`|  
|1|Retourne le nom de serveur non converti, par exemple : `\\ServerName\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F1`|  
|2|Retourne le chemin d'accès complet du serveur, par exemple : `\\ServerName.MyDomain.com\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F19F7-38EA-4AB0-BB89-E6C545DBD3F9`|  
  
 *use_replica_computer_name*  
 Une valeur de bit qui définit comment le nom du serveur doit être retourné dans un groupe de disponibilité Always On.  
  
 Lorsque la base de données n’appartient pas à un groupe de disponibilité Always On, la valeur de cet argument est ignorée. Le nom d'ordinateur est toujours utilisé dans le chemin d'accès.  
  
 Lorsque la base de données appartient à un groupe de disponibilité Always On de groupe, puis la valeur de *use_replica_computer_name* a les effets suivants sur la sortie de la **chemin d’accès** fonction :  
  
|Valeur| Description|  
|-----------|-----------------|  
|Non spécifié.|La fonction retourne le nom de réseau virtuel (VNN) dans le chemin d'accès.|  
|0|La fonction retourne le nom de réseau virtuel (VNN) dans le chemin d'accès.|  
|1|La fonction retourne le nom d'ordinateur dans le chemin d'accès.|  
  
## <a name="return-type"></a>Type de retour  
 **nvarchar(max)**  
  
## <a name="return-value"></a>Valeur retournée  
 La valeur retournée est le chemin d'accès logique complet ou NETBIOS de l'objet blob. PathName ne retourne pas d'adresse IP. Une valeur NULL est retournée lorsque l'objet blob FILESTREAM n'a pas été créé.  
  
## <a name="remarks"></a>Notes  
 La colonne ROWGUID doit être visible dans toute requête qui appelle PathName.  
  
 Un objet blob FILESTREAM peut être créé uniquement à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-reading-the-path-for-a-filestream-blob"></a>A. Lecture du chemin d'accès d'un objet BLOB FILESTREAM  
 L'exemple suivant attribue `PathName` à une variable `nvarchar(max)`.  
  
```sql  
DECLARE @PathName nvarchar(max);  
SET @PathName = (  
    SELECT TOP 1 photo.PathName()  
    FROM dbo.Customer  
    WHERE LastName = 'CustomerName'  
    );  
```  
  
### <a name="b-displaying-the-paths-for-filestream-blobs-in-a-table"></a>B. Affichage des chemins d'accès des BLOB FILESTREAM dans une table  
 L'exemple suivant crée et affiche les chemins d'accès pour trois objets blob FILESTREAM.  
  
```sql  
-- Create a FILESTREAM-enabled database.  
-- The c:\data directory must exist.  
CREATE DATABASE PathNameDB  
ON  
PRIMARY ( NAME = ArchX1,  
    FILENAME = 'c:\data\archdatP1.mdf'),  
FILEGROUP FileStreamGroup1 CONTAINS FILESTREAM( NAME = ArchX3,  
    FILENAME = 'c:\data\filestreamP1')  
LOG ON  ( NAME = ArchlogX1,  
    FILENAME = 'c:\data\archlogP1.ldf');  
GO  
  
USE PathNameDB;  
GO  
  
-- Create a table, add some records, and  
-- create the associated FILESTREAM  
-- BLOB files.  
  
CREATE TABLE TABLE1  
    (  
        ID int,  
        RowGuidColumn UNIQUEIDENTIFIER  
                      NOT NULL UNIQUE ROWGUIDCOL,  
        FILESTREAMColumn varbinary(MAX) FILESTREAM  
    );  
GO  
  
INSERT INTO TABLE1 VALUES  
 (1, NEWID(), 0x00)  
,(2, NEWID(), 0x00)  
,(3, NEWID(), 0x00);  
GO  
  
SELECT FILESTREAMColumn.PathName() AS 'PathName' FROM TABLE1;  
  
--Results  
--PathName  
------------------------------------------------------------------------------------------------------------  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\DD67C792-916E-4A76-8C8A-4A85DC5DB908  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\2907122B-2560-4CB9-86DC-FBE7ABA1843B  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\922BE0E0-CAB9-4403-90BF-945BD258E4BC  
--  
--(3 row(s) affected)  
GO  
  
--Drop the database to clean up.  
USE master;  
GO  
DROP DATABASE PathNameDB;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Objets binaires volumineux &#40;Objet BLOB&#41; Données &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)   
 [GET_FILESTREAM_TRANSACTION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md)   
 [Accéder à des données FILESTREAM avec OpenSqlFilestream](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)  
  
  
