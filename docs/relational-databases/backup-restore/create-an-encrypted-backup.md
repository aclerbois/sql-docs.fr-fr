---
title: Créer une sauvegarde chiffrée | Microsoft Docs
ms.custom: ''
ms.date: 08/04/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.suite: sql
ms.technology: backup-restore
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e29061d3-c2ab-4d98-b9be-8e90a11d17fe
caps.latest.revision: 17
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d963909a74b3d82bc9f851eff18ccf48036b9657
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-an-encrypted-backup"></a>Créer une sauvegarde chiffrée
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit les étapes nécessaires pour créer une sauvegarde chiffrée à l'aide de Transact-SQL.  Par exemple, à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], consultez [Créer une sauvegarde complète de base de données (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md). 
  
## <a name="backup-to-disk-with-encryption"></a>Sauvegarde sur disque avec chiffrement  
 **Configuration requise :**  
  
-   Accès à un disque local ou à un stockage disposant de l'espace approprié pour créer une sauvegarde de la base de données.  
  
-   Une clé principale de base de données pour la base de données master, et un certificat ou une clé asymétrique disponible sur l'instance de SQL Server. Pour les conditions et les autorisations de chiffrement, consultez [Backup Encryption](../../relational-databases/backup-restore/backup-encryption.md).  
  
 Utilisez la procédure suivante pour créer une sauvegarde chiffrée d'une base de données sur un disque local. Cet exemple utilise une base de données utilisateur appelée MyTestDB.  
  
1.  **Créez une clé principale de base de données pour la base de données master :** Choisissez un mot de passe pour chiffrer la copie de la clé principale qui sera enregistrée dans la base de données. Connectez-vous au moteur de base de données, ouvrez une nouvelle fenêtre de requête, copiez et collez l'exemple, puis cliquez sur **Exécuter**.  
  
    ```  
    -- Creates a database master key.   
    -- The key is encrypted using the password "<master key password>"  
    USE master;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master key password>';  
    GO  
  
    ```  
  
2.  **Créez une sauvegarde du certificat :** Créez un certificat dans la base de données MASTER. Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    Use Master  
    GO  
    CREATE CERTIFICATE MyTestDBBackupEncryptCert  
       WITH SUBJECT = 'MyTestDB Backup Encryption Certificate';  
    GO  
  
    ```  
  
3.  **Sauvegardez la base de données :** spécifiez l'algorithme de chiffrement et le certificat à utiliser. Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    BACKUP DATABASE [MyTestDB]  
    TO DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\MyTestDB.bak'  
    WITH  
      COMPRESSION,  
      ENCRYPTION   
       (  
       ALGORITHM = AES_256,  
       SERVER CERTIFICATE = MyTestDBBackupEncryptCert  
       ),  
      STATS = 10  
    GO  
  
    ```  
  
 Pour obtenir un exemple de chiffrement d’une sauvegarde protégée par une gestion de clés extensible, consultez [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  
  
### <a name="backup-to-windows-azure-storage-with-encryption"></a>Sauvegarder dans le stockage Windows Azure avec chiffrement  
 Si vous créez une sauvegarde dans le stockage Windows Azure à l'aide de l'option **Sauvegarde SQL Server vers une URL** , les étapes de chiffrement sont identiques, mais vous devez utiliser l'URL de destination et les informations d'identification SQL pour l'authentification dans le stockage Windows Azure. Si vous voulez configurer [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] avec les options de chiffrement, consultez [Activation de la sauvegarde managée SQL Server sur Microsoft Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md).  
  
 **Configuration requise :**  
  
-   Un compte de stockage Windows et un conteneur. Pour plus d'informations, consultez [Lesson 1: Create Windows Azure Storage Objects](http://msdn.microsoft.com/library/74edd1fd-ab00-46f7-9e29-7ba3f1a446c5).  
  
-   Une clé principale de base de données pour la base de données master, et un certificat ou une clé asymétrique sur l'instance de SQL Server. Pour les conditions et les autorisations de chiffrement, consultez [Backup Encryption](../../relational-databases/backup-restore/backup-encryption.md).  
  
1.  **Créez les informations d'identification SQL Server :** Pour créer des informations d'identification SQL Server, connectez-vous au moteur de base de données, ouvrez une nouvelle fenêtre de requête, copiez et collez l'exemple suivant, puis cliquez sur **Exécuter**.  
  
    ```  
    CREATE CREDENTIAL mycredential   
    WITH IDENTITY= 'mystorageaccount' – this is the name of the storage account you specified when creating a storage account    
    , SECRET = '<storage account access key>' – this should be either the Primary or Secondary Access Key for the storage account  
    ```  
  
2.  **Créer une clé principale de base de données :** Choisissez un mot de passe pour chiffrer la copie de la clé principale qui sera stockée dans la base de données. Connectez-vous au moteur de base de données, ouvrez une nouvelle fenêtre de requête, copiez et collez l'exemple, puis cliquez sur **Exécuter**.  
  
    ```  
    -- Creates a database master key.  
    -- The key is encrypted using the password "<master key password>"  
    USE Master;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master key password>';  
    GO  
  
    ```  
  
3.  **Créez une sauvegarde du certificat :** créez un certificat de sauvegarde dans la base de données MASTER. Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE Master;  
    GO  
    CREATE CERTIFICATE MyTestDBBackupEncryptCert  
       WITH SUBJECT = 'MyTestDBBackupEncryptCert ';  
    GO  
  
    ```  
  
4.  **Sauvegardez la base de données :** spécifiez l'algorithme de chiffrement et le certificat à utiliser. Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    BACKUP DATABASE [MyTestDB]  
    TO URL = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\MyTestDB.bak'  
    WITH  
      CREDENTIAL 'mycredential' – this is the name of the credential created in the first step.  
      ,COMPRESSION  
      ,ENCRYPTION   
       (  
       ALGORITHM = AES_256,  
       SERVER CERTIFICATE = MyTestDBBackupEncryptCert  
       ),  
      STATS = 10  
    GO  
  
    ```  
  
  
