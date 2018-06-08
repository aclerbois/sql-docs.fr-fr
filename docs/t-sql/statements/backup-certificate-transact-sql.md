---
title: BACKUP CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DUMP_CERTIFICATE_TSQL
- BACKUP CERTIFICATE
- sql13.swb.exportcertificate.f1
- DUMP CERTIFICATE
- BACKUP_CERTIFICATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], certificates
- decryption [SQL Server], certificates
- exporting certificates
- certificates [SQL Server], exporting
- BACKUP CERTIFICATE statement
- backing up certificates [SQL Server]
- decryption [SQL Server]
- cryptography [SQL Server], certificates
ms.assetid: 509b9462-819b-4c45-baae-3d2d90d14a1c
caps.latest.revision: 40
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 13d7e7e0c6ddb7760bb28dfd703904957c4fd820
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="backup-certificate-transact-sql"></a>BACKUP CERTIFICATE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)]

  Permet d'exporter un certificat dans un fichier.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server  
  
BACKUP CERTIFICATE certname TO FILE = 'path_to_file'  
    [ WITH PRIVATE KEY   
      (   
        FILE = 'path_to_private_key_file' ,  
        ENCRYPTION BY PASSWORD = 'encryption_password'   
        [ , DECRYPTION BY PASSWORD = 'decryption_password' ]   
      )   
    ]  
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
BACKUP CERTIFICATE certname TO FILE ='path_to_file'  
      WITH PRIVATE KEY   
      (   
        FILE ='path_to_private_key_file',  
        ENCRYPTION BY PASSWORD ='encryption_password'   
      )   
```  
  
## <a name="arguments"></a>Arguments  
 *path_to_file*  
 Spécifie le chemin d'accès complet, y compris le nom de fichier, du fichier dans lequel le certificat doit être enregistré. Cela peut être un chemin local ou un chemin UNC d’un emplacement réseau. La valeur par défaut est le chemin d'accès au dossier DATA de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *path_to_private_key_file*  
 Spécifie le chemin d'accès complet, y compris le nom de fichier, du fichier dans lequel la clé privée doit être enregistrée. Cela peut être un chemin local ou un chemin UNC d’un emplacement réseau. La valeur par défaut est le chemin d'accès au dossier DATA de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *encryption_password*  
 Mot de passe utilisé pour chiffrer la clé privée avant de l'enregistrer dans le fichier de sauvegarde. Le mot de passe est sujet à des vérifications de la complexité.  
  
 *decryption_password*  
 Mot de passe utilisé pour déchiffrer la clé privée avant de sauvegarder la clé.  
  
## <a name="remarks"></a>Notes   
 Si la clé privée est chiffrée au moyen d'un mot de passe dans la base de données, le mot de passe de déchiffrement doit être spécifié.  
  
 Lorsque vous sauvegardez la clé privée dans un fichier, un chiffrement est nécessaire. Le mot de passe utilisé pour protéger le certificat sauvegardé est différent de celui utilisé pour chiffrer la clé privée du certificat.  
  
 Pour restaurer un certificat sauvegardé, utilisez l’instruction [CREATE CERTIFICATE](../../t-sql/statements/create-certificate-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur le certificat et la connaissance du mot de passe utilisé pour chiffrer la clé privée. Si seule la partie publique du certificat est sauvegardée, l'opération requiert une autorisation sur le certificat et l'autorisation VIEW sur le certificat ne doit pas avoir été refusée à l'appelant.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-exporting-a-certificate-to-a-file"></a>A. Exportation d'un certificat dans un fichier  
 Dans l'exemple ci-dessous, un certificat est exporté dans un fichier.  
  
```  
BACKUP CERTIFICATE sales05 TO FILE = 'c:\storedcerts\sales05cert';  
GO  
```  
  
### <a name="b-exporting-a-certificate-and-a-private-key"></a>B. Exportation d'un certificat et d'une clé privée  
 Dans l'exemple ci-dessous, la clé privée du certificat sauvegardé sera déchiffrée au moyen du mot de passe `997jkhUbhk$w4ez0876hKHJH5gh`.  
  
```  
BACKUP CERTIFICATE sales05 TO FILE = 'c:\storedcerts\sales05cert'  
    WITH PRIVATE KEY ( FILE = 'c:\storedkeys\sales05key' ,   
    ENCRYPTION BY PASSWORD = '997jkhUbhk$w4ez0876hKHJH5gh' );  
GO  
```  
  
### <a name="c-exporting-a-certificate-that-has-an-encrypted-private-key"></a>C. Exportation d'un certificat qui possède une clé privée chiffrée  
 Dans l'exemple ci-dessous, la clé privée du certificat est chiffrée dans la base de données. La clé privée doit être déchiffrée au moyen du mot de passe `9875t6#6rfid7vble7r`. Lorsque le certificat est stocké dans le fichier de sauvegarde, la clé privée est chiffrée au moyen du mot de passe `9n34khUbhk$w4ecJH5gh`.  
  
```  
BACKUP CERTIFICATE sales09 TO FILE = 'c:\storedcerts\sales09cert'   
    WITH PRIVATE KEY ( DECRYPTION BY PASSWORD = '9875t6#6rfid7vble7r' ,  
    FILE = 'c:\storedkeys\sales09key' ,   
    ENCRYPTION BY PASSWORD = '9n34khUbhk$w4ecJH5gh' );  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)  
  
  

