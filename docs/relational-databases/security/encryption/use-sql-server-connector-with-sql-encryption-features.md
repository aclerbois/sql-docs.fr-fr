---
title: Utiliser le connecteur SQL Server avec les fonctionnalités de chiffrement SQL | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Connector, using
- EKM, with SQL Server Connector
ms.assetid: 58fc869e-00f1-4d7c-a49b-c0136c9add89
caps.latest.revision: 14
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: b22b47922534fc38d72c1d89104a9a301cb3637e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-sql-server-connector-with-sql-encryption-features"></a>Utiliser le connecteur SQL Server avec les fonctionnalités de chiffrement SQL
[!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md](../../../includes/appliesto-xx-asdb-xxxx-xxx-md.md)]
  Les activités de chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] courantes à l’aide d’une clé asymétrique protégée par le coffre Azure Key Vault incluent les trois domaines suivants.  
  
-   Chiffrement TDE (Transparent Data Encryption) à l’aide d’une clé asymétrique dans Azure Key Vault  
  
-   Chiffrement des sauvegardes à l'aide d'une clé asymétrique du coffre de clés  
  
-   Chiffrement au niveau colonne à l’aide d’une clé asymétrique dans le coffre de clés  
  
 Exécutez les parties I à IV de la rubrique [Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)avant de suivre les étapes de cette rubrique.  
 
> [!NOTE]  
>  Les versions 1.0.0.440 et antérieures ont été remplacées et ne sont plus prises en charge dans les environnements de production. Effectuez la mise à niveau vers la version 1.0.1.0 ou ultérieure en accédant au [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=45344) et en utilisant les instructions de la page [Résolution des problèmes et maintenance du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md) sous « Mise à niveau du connecteur SQL Server ».  
  
## <a name="transparent-data-encryption-by-using-an-asymmetric-key-from-azure-key-vault"></a>Chiffrement TDE (Transparent Data Encryption) à l’aide d’une clé asymétrique dans Azure Key Vault  
 Une fois les parties I à IV de la rubrique Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault terminées, utilisez la clé Azure Key Vault pour chiffrer la clé de chiffrement de base de données à l’aide du chiffrement TDE.  
Vous devez créer des informations d’identification et une connexion, puis créer une clé de chiffrement de base de données qui chiffre les données et les journaux dans la base de données. Pour chiffrer une base de données, l’autorisation **CONTROL** sur la base de données est exigée. Le graphique suivant montre la hiérarchie de la clé de chiffrement lors de l’utilisation du coffre Azure Key Vault.  
  
 ![ekm&#45;key&#45;hierarchy&#45;with&#45;akv](../../../relational-databases/security/encryption/media/ekm-key-hierarchy-with-akv.png "ekm-key-hierarchy-with-akv")  
  
1.  **Créer des informations d’identification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour le moteur de base de données à utiliser pour le chiffrement TDE**  
  
     Le moteur de base de données utilise les informations d’identification pour accéder au coffre de clés pendant le chargement de la base de données. Nous vous recommandons de créer un autre **ID client** et une autre **clé secrète** Azure Active Directory dans la Partie I pour le [!INCLUDE[ssDE](../../../includes/ssde-md.md)], afin de limiter les autorisations de coffre de clés accordées.  
  
     Modifiez le script [!INCLUDE[tsql](../../../includes/tsql-md.md)] ci-dessous comme suit :  
  
    -   Modifiez l’argument `IDENTITY` (`ContosoDevKeyVault`) de sorte qu’il pointe vers votre coffre Azure Key Vault.
        - Si vous utilisez **public Azure**, remplacez l’argument `IDENTITY` par le nom de votre coffre Azure Key Vault de la Partie II.
        - Si vous utilisez un **cloud privé Azure** (par exemple, Azure Government, Azure China ou Azure Germany), remplacez l’argument `IDENTITY` par l’URI du coffre renvoyée à l’étape 3 de la Partie II. N’incluez pas « https:// » dans l’URI du coffre.   
  
    -   Remplacez la première partie de l’argument `SECRET` par l’ **ID client** Azure Active Directory mentionné dans la Partie I. Dans cet exemple, l’ **ID client** est `EF5C8E094D2A4A769998D93440D8115D`.  
  
        > [!IMPORTANT]  
        >  Vous devez supprimer les tirets de l’ **ID client**.  
  
    -   Complétez la deuxième partie de l’argument `SECRET` avec la **clé secrète client** mentionnée dans la Partie I. Dans cet exemple, la **clé secrète client** de la Partie I est `Replace-With-AAD-Client-Secret`. La chaîne finale pour l’argument `SECRET` est une longue séquence de lettres et de chiffres, *sans tirets*.  
  
    ```sql  
    USE master;  
    CREATE CREDENTIAL Azure_EKM_TDE_cred   
        WITH IDENTITY = 'ContosoDevKeyVault', -- for public Azure
        -- WITH IDENTITY = 'ContosoDevKeyVault.vault.usgovcloudapi.net', -- for Azure Government
        -- WITH IDENTITY = 'ContosoDevKeyVault.vault.azure.cn', -- for Azure China
        -- WITH IDENTITY = 'ContosoDevKeyVault.vault.microsoftazure.de', -- for Azure Germany   
        SECRET = 'EF5C8E094D2A4A769998D93440D8115DReplace-With-AAD-Client-Secret'   
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;  
    ```  
  
2.  **Créer une connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] pour le chiffrement TDE**  
  
     Créez une connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et ajoutez les informations d’identification mentionnées à l’étape 1. Cet exemple [!INCLUDE[tsql](../../../includes/tsql-md.md)] utilise la même clé que celle importée précédemment.  
  
    ```sql  
    USE master;  
    -- Create a SQL Server login associated with the asymmetric key   
    -- for the Database engine to use when it loads a database   
    -- encrypted by TDE.  
    CREATE LOGIN TDE_Login   
    FROM ASYMMETRIC KEY CONTOSO_KEY;  
    GO   
  
    -- Alter the TDE Login to add the credential for use by the   
    -- Database Engine to access the key vault  
    ALTER LOGIN TDE_Login   
    ADD CREDENTIAL Azure_EKM_TDE_cred ;  
    GO  
    ```  
  
3.  **Créer la clé de chiffrement de base de données (DEK)**  
  
     La clé DEK chiffre vos données et vos fichiers journaux dans l’instance de base de données et est, à son tour, chiffrée par la clé asymétrique Azure Key Vault. Elle peut être créée à l’aide de n’importe quel algorithme ou longueur de clé pris en charge par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    ```sql  
    USE ContosoDatabase;  
    GO  
  
    CREATE DATABASE ENCRYPTION KEY   
    WITH ALGORITHM = AES_256   
    ENCRYPTION BY SERVER ASYMMETRIC KEY CONTOSO_KEY;  
    GO  
    ```  
  
4.  **Activer le chiffrement TDE**  
  
    ```sql  
    -- Alter the database to enable transparent data encryption.  
    ALTER DATABASE ContosoDatabase   
    SET ENCRYPTION ON;  
    GO  
    ```  
  
     À l’aide de [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], vérifiez que le chiffrement TDE a été activé en vous connectant à votre base de données avec l’Explorateur d’objets. Cliquez avec le bouton droit sur votre base de données, pointez sur **Tâches**, puis cliquez sur **Gérer le chiffrement de base de données**.  
  
     ![ekm&#45;tde&#45;object&#45;explorer](../../../relational-databases/security/encryption/media/ekm-tde-object-explorer.png "ekm-tde-object-explorer")  
  
     Dans la boîte de dialogue **Gérer le chiffrement de base de données** , vérifiez que le chiffrement TDE est activé et quelle clé asymétrique chiffre la clé DEK.  
  
     ![ekm&#45;tde&#45;dialog&#45;box](../../../relational-databases/security/encryption/media/ekm-tde-dialog-box.png "ekm-tde-dialog-box")  
  
     Vous pouvez aussi exécuter le script [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivant. Un état de chiffrement de 3 indique une base de données.  
  
    ```sql  
    USE MASTER  
    SELECT * FROM sys.asymmetric_keys  
  
    -- Check which databases are encrypted using TDE  
    SELECT d.name, dek.encryption_state   
    FROM sys.dm_database_encryption_keys AS dek  
    JOIN sys.databases AS d  
         ON dek.database_id = d.database_id;  
    ```  
  
    > [!NOTE]  
    >  La base de données `tempdb` est chiffrée automatiquement chaque fois qu’une base de données permet le chiffrement TDE.  
  
## <a name="encrypting-backups-by-using-an-asymmetric-key-from-the-key-vault"></a>Chiffrement des sauvegardes à l'aide d'une clé asymétrique du coffre de clés  
 Les sauvegardes chiffrées sont prises en charge à partir de [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)]. L'exemple suivant crée et restaure une sauvegarde chiffrée avec une clé de chiffrement de données protégée par la clé asymétrique dans le coffre de clés.  
Le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] a besoin des informations d’identification lors de l’accès au coffre de clés pendant le chargement de la base de données. Nous vous recommandons de créer un autre ID client et une autre clé secrète Azure Active Directory dans la Partie I pour le moteur de base de données, afin de limiter les autorisations de coffre de clés accordées.  
  
1.  **Créer des informations d’identification SQL Server pour le moteur de base de données à utiliser pour le chiffrement de sauvegarde**  
  
     Modifiez le script [!INCLUDE[tsql](../../../includes/tsql-md.md)] ci-dessous comme suit :  
  
    -   Modifiez l’argument `IDENTITY` (`ContosoDevKeyVault`) de sorte qu’il pointe vers votre coffre Azure Key Vault.
        - Si vous utilisez **public Azure**, remplacez l’argument `IDENTITY` par le nom de votre coffre Azure Key Vault de la Partie II.
        - Si vous utilisez un **cloud privé Azure** (par exemple, Azure Government, Azure China ou Azure Germany), remplacez l’argument `IDENTITY` par l’URI du coffre renvoyée à l’étape 3 de la Partie II. N’incluez pas « https:// » dans l’URI du coffre.    
  
    -   Remplacez la première partie de l’argument `SECRET` par l’ **ID client** Azure Active Directory mentionné dans la Partie I. Dans cet exemple, l’ **ID client** est `EF5C8E094D2A4A769998D93440D8115D`.  
  
        > [!IMPORTANT]  
        >  Vous devez supprimer les tirets de l’ **ID client**.  
  
    -   Complétez la deuxième partie de l’argument `SECRET` avec la **clé secrète client** mentionnée dans la Partie I. Dans cet exemple, la **clé secrète client** de la Partie I est `Replace-With-AAD-Client-Secret`. La chaîne finale pour l’argument `SECRET` est une longue séquence de lettres et de chiffres, *sans tirets*.   
  
        ```sql  
        USE master;  
  
        CREATE CREDENTIAL Azure_EKM_Backup_cred   
            WITH IDENTITY = 'ContosoDevKeyVault', -- for public Azure
            -- WITH IDENTITY = 'ContosoDevKeyVault.vault.usgovcloudapi.net', -- for Azure Government
            -- WITH IDENTITY = 'ContosoDevKeyVault.vault.azure.cn', -- for Azure China
            -- WITH IDENTITY = 'ContosoDevKeyVault.vault.microsoftazure.de', -- for Azure Germany   
            SECRET = 'EF5C8E094D2A4A769998D93440D8115DReplace-With-AAD-Client-Secret'   
        FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;    
        ```  
  
2.  **Créer une connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] pour le chiffrement de sauvegarde**  
  
     Créez une connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à utiliser par le [!INCLUDE[ssDE](../../../includes/ssde-md.md)]pour les sauvegardes de chiffrement, puis ajoutez les informations d’identification mentionnées à l’étape 1. Cet exemple [!INCLUDE[tsql](../../../includes/tsql-md.md)] utilise la même clé que celle importée précédemment.  
  
    > [!IMPORTANT]  
    >  Vous ne pouvez pas utiliser la même clé asymétrique pour le chiffrement de sauvegarde si vous avez déjà utilisé cette clé pour le chiffrement TDE (l’exemple ci-dessus) ou pour le chiffrement au niveau des colonnes (l’exemple suivant).  
  
     Cet exemple utilise la clé asymétrique `CONTOSO_KEY_BACKUP` stockée dans le coffre de clés, qui peut être préalablement importée ou créée pour la base de données MASTER, comme décrit à l’étape 5 de la Partie IV.  
  
    ```sql  
    USE master;  
  
    -- Create a SQL Server login associated with the asymmetric key   
    -- for the Database engine to use when it is encrypting the backup.  
    CREATE LOGIN Backup_Login   
    FROM ASYMMETRIC KEY CONTOSO_KEY_BACKUP;  
    GO   
  
    -- Alter the Encrypted Backup Login to add the credential for use by   
    -- the Database Engine to access the key vault  
    ALTER LOGIN Backup_Login   
    ADD CREDENTIAL Azure_EKM_Backup_cred ;  
    GO  
    ```  
  
3.  **Sauvegarder la base de données**  
  
     Sauvegardez la base de données en spécifiant le chiffrement avec la clé asymétrique stockée dans le coffre de clés.
     
     Dans l’exemple ci-dessous, notez que si la base de données a déjà été chiffrée avec TDE et que la clé asymétrique `CONTOSO_KEY_BACKUP` est différente de la clé asymétrique TDE, la sauvegarde sera chiffrée à la fois par la clé asymétrique TDE et par `CONTOSO_KEY_BACKUP`. L’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cible aura besoin des deux clés pour déchiffrer la sauvegarde.
  
    ```sql  
    USE master;  
  
    BACKUP DATABASE [DATABASE_TO_BACKUP]  
    TO DISK = N'[PATH TO BACKUP FILE]'   
    WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,   
    ENCRYPTION(ALGORITHM = AES_256,   
    SERVER ASYMMETRIC KEY = [CONTOSO_KEY_BACKUP]);  
    GO  
    ```  
  
4.  **Restaurer la base de données**  
    
    Pour restaurer une sauvegarde de base de données chiffrée avec TDE, l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cible doit tout d’abord avoir une copie de la clé Key Vault asymétrique utilisée pour le chiffrement. Voici comment procéder :  
    
    - Si la clé asymétrique d’origine utilisée pour le chiffrement TDE n’est plus dans Key Vault, restaurez la sauvegarde de clé Key Vault ou réimportez la clé à partir d’un module HSM local. **Important :** Pour que l’empreinte numérique de la clé corresponde à celle enregistrée dans la sauvegarde de base de données, la clé doit avoir le **même nom que la clé Key Vault**, comme c’était le cas initialement.
    
    - Appliquez les étapes 1 et 2 sur l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cible.
    
    - Une fois que l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cible a accès aux clés asymétriques utilisées pour chiffrer la sauvegarde, restaurez la base de données sur le serveur.
    
     Exemple de code de restauration :  
  
    ```sql  
    RESTORE DATABASE [DATABASE_TO_BACKUP]  
    FROM DISK = N'[PATH TO BACKUP FILE]'   
        WITH FILE = 1, NOUNLOAD, REPLACE;  
    GO  
    ```  
  
     Pour plus d’informations sur les options de sauvegarde, consultez [BACKUP (Transact-SQL)](../../../t-sql/statements/backup-transact-sql.md).  
  
## <a name="column-level-encryption-by-using-an-asymmetric-key-from-the-key-vault"></a>Chiffrement au niveau colonne à l’aide d’une clé asymétrique dans le coffre de clés  
 L'exemple suivant crée une clé symétrique protégée par la clé asymétrique dans le coffre de clés. La clé symétrique est ensuite utilisée pour chiffrer les données de la base de données.  
  
> [!IMPORTANT]  
>  Vous ne pouvez pas utiliser la même clé asymétrique pour le chiffrement de sauvegarde si vous avez déjà utilisé cette clé pour le chiffrement TDE ou pour le chiffrement de sauvegarde (les exemples précédents).  
  
 Cet exemple utilise la clé asymétrique `CONTOSO_KEY_COLUMNS` stockée dans le coffre de clés, qui peut être préalablement importée ou créée, comme décrit à l’étape 3 de la Partie II de la rubrique [Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md). Pour utiliser cette clé asymétrique dans la base de données `ContosoDatabase` , vous devez réexécuter l’instruction `CREATE ASYMMETRIC KEY` pour fournir à la base de données `ContosoDatabase` avec une référence à la clé.  
  
```sql  
USE [ContosoDatabase];  
GO  
  
-- Create a reference to the key in the key vault  
CREATE ASYMMETRIC KEY CONTOSO_KEY_COLUMNS   
FROM PROVIDER [AzureKeyVault_EKM_Prov]  
WITH PROVIDER_KEY_NAME = 'ContosoDevRSAKey2',  
CREATION_DISPOSITION = OPEN_EXISTING;  
  
-- Create the data encryption key.  
-- The data encryption key can be created using any SQL Server   
-- supported algorithm or key length.  
-- The DEK will be protected by the asymmetric key in the key vault  
  
CREATE SYMMETRIC KEY DATA_ENCRYPTION_KEY  
    WITH ALGORITHM=AES_256  
    ENCRYPTION BY ASYMMETRIC KEY CONTOSO_KEY_COLUMNS;  
  
DECLARE @DATA VARBINARY(MAX);  
  
--Open the symmetric key for use in this session  
OPEN SYMMETRIC KEY DATA_ENCRYPTION_KEY   
DECRYPTION BY ASYMMETRIC KEY CONTOSO_KEY_COLUMNS;  
  
--Encrypt syntax  
SELECT @DATA = ENCRYPTBYKEY  
    (  
    KEY_GUID('DATA_ENCRYPTION_KEY'),   
    CONVERT(VARBINARY,'Plain text data to encrypt')  
    );  
  
-- Decrypt syntax  
SELECT CONVERT(VARCHAR, DECRYPTBYKEY(@DATA));  
  
--Close the symmetric key  
CLOSE SYMMETRIC KEY DATA_ENCRYPTION_KEY;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)   
 [Gestion de clés extensible à l'aide d'Azure Key Vault](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
 [Fournisseur EKM activé (option de configuration de serveur)](../../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md)   
 [Résolution des problèmes et maintenance du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)  
  
  
