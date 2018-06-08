---
title: Configurer des clés Always Encrypted à l’aide de PowerShell | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2017
ms.prod: sql
ms.prod_service: security, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 3bdf8629-738c-489f-959b-2f5afdaf7d61
caps.latest.revision: 27
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: b7f4f56828e8c1ec44c58feda21e5f061364b4b9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-always-encrypted-keys-using-powershell"></a>Configurer des clés Always Encrypted à l’aide de PowerShell
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

    
Cet article décrit les étapes de la mise en service de clés pour Always Encrypted à l’aide du [module SqlServer PowerShell](../../../relational-databases/scripting/sql-server-powershell-provider.md). Vous pouvez utiliser PowerShell pour mettre en service des clés Always Encrypted à la fois [avec et sans séparation des rôles](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md#KeyManagementRoles), ce qui permet de contrôler qui a accès aux clés de chiffrement réelles dans le magasin de clés et qui a accès à la base de données. 

Pour obtenir une vue d’ensemble de la gestion des clés Always Encrypted, notamment quelques recommandations de bonnes pratiques détaillées, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).
Pour plus d’informations sur l’utilisation du module SqlServer PowerShell pour Always Encrypted, consultez [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).


## <a name="KeyProvisionWithoutRoles"></a> Mise en service des clés sans séparation des rôles

La méthode de mise en service des clés décrite dans cette section ne prend pas en charge la séparation des rôles entre les administrateurs de la sécurité et les administrateurs de bases de données. Certaines des étapes ci-dessous associent des opérations sur les clés physiques à des opérations sur les métadonnées de clé. Par conséquent, cette méthode de mise en service des clés est recommandée pour les organisations qui utilisent le modèle DevOps, ou si la base de données est hébergée dans le cloud et que le principal objectif est de restreindre l’accès des administrateurs du cloud (mais pas des administrateurs de bases de données) aux données sensibles. Elle n’est pas recommandée si les rivaux potentiels incluent des administrateurs de bases de données ou si ceux-ci ne doivent tout simplement pas avoir accès aux données sensibles.

Avant d’exécuter des étapes qui impliquent l’accès aux clés en texte clair ou au magasin de clés (identifiées dans la colonne **Accède au magasin de clés/aux clés en texte clair** dans le tableau ci-dessous), vérifiez que l’environnement PowerShell s’exécute sur un ordinateur sécurisé qui est différent d’un ordinateur qui héberge votre base de données. Pour plus d’informations, consultez ***Considérations en matière de sécurité pour la gestion des clés***.


Tâche  |Article  |Accède au magasin de clés/aux clés en texte brut  |Accède à la base de données   
---------|---------|---------|---------
Étape 1. Créer une clé principale de colonne dans un magasin de clés.<br><br>**Remarque :** Le module SqlServer PowerShell ne prend pas en charge cette étape. Pour accomplir cette tâche à partir d’une ligne de commande, utilisez les outils qui sont spécifiques à votre magasin de clés sélectionné. |[Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md) | Oui | non     
Étape 2.  Démarrer un environnement PowerShell et importer le module SqlServer PowerShell.  |   [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)   |    non    | non         
Étape 3.  Se connecter à votre serveur et à la base de données.     |     [Se connecter à une base de données](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase)    |    non     | Oui         
Étape 4.  Créer un objet *SqlColumnMasterKeySettings* qui contient des informations sur l’emplacement de votre clé principale de colonne. SqlColumnMasterKeySettings est un objet qui existe en mémoire (dans PowerShell). Utilisez l’applet de commande qui est spécifique à votre magasin de clés.   |     [New-SqlAzureKeyVaultColumnMasterKeySettings](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)        |   non      | non         
Étape 5.  Créer les métadonnées relatives à la clé principale de colonne dans votre base de données.      |    [New-SqlColumnMasterKey](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Remarque :** En arrière-plan, l’applet de commande émet l’instruction [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) pour créer des métadonnées de clé.|    non     |    Oui
Étape 6.  S’authentifier auprès d’Azure, si votre clé principale de colonne est stockée dans Azure Key Vault. | [Add-SqlAzureAuthenticationContext](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)    |  Oui   | non         
Étape 7.  Générer une clé de chiffrement de colonne, la chiffrer avec la clé principale de colonne et créer les métadonnées de clé de chiffrement de colonne dans la base de données.     |    [New-SqlColumnEncryptionKey](https://docs.microsoft.com/en-us/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)<br><br>**Remarque :** Utilisez une variation de l’applet de commande qui génère et chiffre en interne une clé de chiffrement de colonne.<br><br>**Remarque :** En arrière-plan, l’applet de commande émet l’instruction [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) pour créer des métadonnées de clé.  | Oui | Oui
  

## <a name="windows-certificate-store-without-role-separation-example"></a>Magasin de certificats Windows sans séparation des rôles (exemple)

Ce script est un exemple de bout en bout de génération d’une clé principale de colonne qui est un certificat dans le magasin de certificats Windows, de génération et de chiffrement d’une clé de chiffrement de colonne, et de création de métadonnées de clé dans une base de données SQL Server.


```
# Create a column master key in Windows Certificate Store.
$cert1 = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$connection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection
$connection.ConnectionString = $connStr
$connection.Connect()
$server = New-Object Microsoft.SqlServer.Management.Smo.Server($connection)
$database = $server.Databases[$databaseName]

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert1.Thumbprint

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings


# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName  -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="azure-key-vault-without-role-separation-example"></a>Azure Key Vault sans séparation des rôles (exemple)

Ce script est un exemple de bout en bout de mise en service et de configuration d’Azure Key Vault, de génération d’une clé principale de colonne dans le coffre, de génération et de chiffrement d’une clé de chiffrement de colonne, et de création de métadonnées de clé dans une base de données SQL Azure.


```
# Create a column master key in Azure Key Vault.
Login-AzureRmAccount
$SubscriptionId = "<Azure SubscriptionId>"
$resourceGroup = "<resource group name>"
$azureLocation = "<datacenter location>"
$akvName = "<key vault name>"
$akvKeyName = "<key name>"
$azureCtx = Set-AzureRMConteXt -SubscriptionId $SubscriptionId # Sets the context for the below cmdlets to the specified subscription.
New-AzureRmResourceGroup –Name $resourceGroup –Location $azureLocation # Creates a new resource group - skip, if you desire group already exists.
New-AzureRmKeyVault -VaultName $akvName -ResourceGroupName $resourceGroup -Location $azureLocation # Creates a new key vault - skip if your vault already exists.
Set-AzureRmKeyVaultAccessPolicy -VaultName $akvName -ResourceGroupName $resourceGroup -PermissionsToKeys get, create, delete, list, update, import, backup, restore, wrapKey,unwrapKey, sign, verify -UserPrincipalName $azureCtx.Account
$akvKey = Add-AzureKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination "Software"

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database (Azure SQL database).
$serverName = "<Azure SQL server name>.database.windows.net"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Authentication = Active Directory Integrated"
$connection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection
$connection.ConnectionString = $connStr
$connection.Connect()
$server = New-Object Microsoft.SqlServer.Management.Smo.Server($connection)
$database = $server.Databases[$databaseName] 

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlAzureKeyVaultColumnMasterKeySettings -KeyURL $akvKey.ID

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Authenticate to Azure
Add-SqlAzureAuthenticationContext -Interactive

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="cngksp-without-role-separation-example"></a>CNG/KSP sans séparation des rôles (exemple)

Le script ci-dessous est un exemple de bout en bout de génération d’une clé principale de colonne dans un magasin de clés qui implémente l’API CNG (Cryptography Next Generation), de génération et de chiffrement d’une clé de chiffrement de colonne, et de création de métadonnées de clé dans une base de données SQL Server.

L’exemple s’appuie sur le magasin de clés qui utilise le fournisseur de stockage de clés (KSP) des logiciels Microsoft. Vous pouvez choisir de modifier l’exemple pour utiliser un autre magasin, tel que votre module de sécurité matériel. Pour ce faire, vous devez vérifier que le fournisseur du magasin de clés qui implémente CNG pour votre appareil est correctement installé sur votre ordinateur. Vous devez remplacer « Microsoft Software Key Storage Provider » par le nom du fournisseur KSP de l’appareil.


```
# Create a column master key in a key store that has a CNG provider, a.k.a key store provider (KSP).
$cngProviderName = "Microsoft Software Key Storage Provider" # If you have an HSM, you can use a KSP for your HSM instead of a Microsoft KSP
$cngAlgorithmName = "RSA"
$cngKeySize = 2048 # Recommended key size for Always Encrypted column master keys
$cngKeyName = "AlwaysEncryptedKey" # Name identifying your new key in the KSP
$cngProvider = New-Object System.Security.Cryptography.CngProvider($cngProviderName)
$cngKeyParams = New-Object System.Security.Cryptography.CngKeyCreationParameters
$cngKeyParams.provider = $cngProvider
$cngKeyParams.KeyCreationOptions = [System.Security.Cryptography.CngKeyCreationOptions]::OverwriteExistingKey
$keySizeProperty = New-Object System.Security.Cryptography.CngProperty("Length", [System.BitConverter]::GetBytes($cngKeySize), [System.Security.Cryptography.CngPropertyOptions]::None);
$cngKeyParams.Parameters.Add($keySizeProperty)
$cngAlgorithm = New-Object System.Security.Cryptography.CngAlgorithm($cngAlgorithmName)
$cngKey = [System.Security.Cryptography.CngKey]::Create($cngAlgorithm, $cngKeyName, $cngKeyParams)

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$connection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection
$connection.ConnectionString = $connStr
$connection.Connect()
$server = New-Object Microsoft.SqlServer.Management.Smo.Server($connection)
$database = $server.Databases[$databaseName]

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCngColumnMasterKeySettings -CngProviderName $cngProviderName -KeyName $cngKeyName

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="KeyProvisionWithRoles"></a> Mise en service des clés avec séparation des rôles

Cette section présente les étapes de la configuration du chiffrement où les administrateurs de la sécurité n’ont pas accès à la base de données et les administrateurs de bases de données n’ont pas accès au magasin de clés ni aux clés en texte clair.


### <a name="security-administrator"></a>Administrateur de la sécurité

Avant d’exécuter des étapes qui impliquent l’accès aux clés en texte clair ou au magasin de clés (identifiées dans la colonne **Accède au magasin de clés/aux clés en texte clair** dans le tableau ci-dessous), vérifiez les points suivants :
1.  L’environnement PowerShell s’exécute sur une machine sécurisée qui est différente d’un ordinateur qui héberge votre base de données.
2.  Les administrateurs de bases de données de votre organisation n’ont aucun accès à l’ordinateur (ce serait en opposition avec l’objectif de la séparation des rôles).

Pour plus d’informations, consultez [Considérations en matière de sécurité pour la gestion des clés](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md#SecurityForKeyManagement).


Tâche  |Article  |Accède au magasin de clés/aux clés en texte brut  |Accède à la base de données  
---------|---------|---------|---------
Étape 1. Créer une clé principale de colonne dans un magasin de clés.<br><br>**Remarque :** Le module SqlServer ne prend pas en charge cette étape. Pour accomplir cette tâche à partir d’une ligne de commande, vous devez utiliser les outils propres au type de votre magasin de clés.     | [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)  |    Oui    | non 
Étape 2.  Démarrer une session PowerShell et importer le module SqlServer.      |     [Importer le module SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule)     | non | non         
Étape 3.  Créer un objet *SqlColumnMasterKeySettings* qui contient des informations sur l’emplacement de votre clé principale de colonne. *SqlColumnMasterKeySettings* est un objet qui existe en mémoire (dans PowerShell). Utilisez l’applet de commande qui est spécifique à votre magasin de clés. |      [New-SqlAzureKeyVaultColumnMasterKeySettings](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)   | non         | non         
Étape 4.  S’authentifier auprès d’Azure, si votre clé principale de colonne est stockée dans Azure Key Vault |    [Add-SqlAzureAuthenticationContext](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)    |Oui|non         
Étape 5.  Générer une clé de chiffrement de colonne, la chiffrer avec la clé principale de colonne pour produire une valeur chiffrée de la clé de chiffrement de colonne.     |   [New-SqlColumnEncryptionKeyEncryptedValue](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)     |    Oui    | non        
Étape 6.  Fournir l’emplacement de la clé principale de colonne (nom du fournisseur et chemin d’accès à la clé principale de colonne) et une valeur chiffrée de la clé de chiffrement de colonne à l’administrateur de base de données.  | Consultez les exemples ci-dessous.        |   non      | non         

### <a name="dba"></a>Administrateur de base de données 

Les administrateurs de bases de données utilisent les informations qu’ils reçoivent de l’administrateur de la sécurité (étape 6 ci-dessus) pour créer et gérer les métadonnées de clé Always Encrypted dans la base de données.

Tâche  |Article  |Accède aux clés en texte clair  |Accède à la base de données   
---------|---------|---------|---------
Étape 1.  Obtenir l’emplacement de la clé principale de colonne et la valeur chiffrée de la clé de chiffrement de colonne de votre administrateur de la sécurité. |Consultez les exemples ci-dessous. | non | non
Étape 2.  Démarrer un environnement PowerShell et importer le module SqlServer.  | [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)  | non | non
Étape 3.  Se connecter à votre serveur et à une base de données. | [Se connecter à une base de données](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | non | Oui
Étape 4.  Créer un objet SqlColumnMasterKeySettings qui contient des informations sur l’emplacement de votre clé principale de colonne. SqlColumnMasterKeySettings est un objet qui existe en mémoire. | New-SqlColumnMasterKeySettings | non | non
Étape 5. Créer les métadonnées relatives à la clé principale de colonne dans votre base de données | [New-SqlColumnMasterKey](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br>**Remarque :** En arrière-plan, l’applet de commande émet l’instruction [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) pour créer des métadonnées de clé principale de colonne. | non | Oui
Étape 6. Créer les métadonnées de clé de chiffrement de colonne dans la base de données. | New-SqlColumnEncryptionKey<br>**Remarque :** Les administrateurs de bases de données utilisent une variation de l’applet de commande qui crée uniquement des métadonnées de clé de chiffrement de colonne.<br>En arrière-plan, l’applet de commande émet l’instruction [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) pour créer des métadonnées de clé de chiffrement de colonne. | non | Oui
  
## <a name="windows-certificate-store-with-role-separation-example"></a>Magasin de certificats Windows avec séparation des rôles (exemple)

### <a name="security-administrator"></a>Administrateur de la sécurité


```
# Create a column master key in Windows Certificate Store.
$storeLocation = "CurrentUser"
$certPath = "Cert:" + $storeLocation + "\My"
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation $certPath -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module
Import-Module "SqlServer"

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Generate a column encryption key, encrypt it with the column master key to produce an encrypted value of the column encryption key.
$encryptedValue = New-SqlColumnEncryptionKeyEncryptedValue -TargetColumnMasterKeySettings $cmkSettings

# Share the location of the column master key and an encrypted value of the column encryption key with a DBA, via a CSV file on a share drive
$keyDataFile = "Z:\keydata.txt"
"KeyStoreProviderName, KeyPath, EncryptedValue" > $keyDataFile
$cmkSettings.KeyStoreProviderName + ", " + $cmkSettings.KeyPath + ", " + $encryptedValue >> $keyDataFile

# Read the key data back to verify
$keyData = Import-Csv $keyDataFile
$keyData.KeyStoreProviderName
$keyData.KeyPath
$keyData.EncryptedValue 
```

### <a name="dba"></a>Administrateur de base de données

```
# Obtain the location of the column master key and the encrypted value of the column encryption key from your Security Administrator, via a CSV file on a share drive.
$keyDataFile = "Z:\keydata.txt"
$keyData = Import-Csv $keyDataFile

# Import the SqlServer module
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$connection = New-Object Microsoft.SqlServer.Management.Common.ServerConnection
$connection.ConnectionString = $connStr
$connection.Connect()
$server = New-Object Microsoft.SqlServer.Management.Smo.Server($connection)
$database = $server.Databases[$databaseName]

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $keyData.KeyStoreProviderName -KeyPath $keyData.KeyPath

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a  column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName -EncryptedValue $keyData.EncryptedValue
```  
 
## <a name="next-steps"></a>Next Steps    

- [Configurer le chiffrement de colonne à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-column-encryption-using-powershell.md)    
- [Permuter des clés Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)
    
## <a name="additional-resources"></a>Ressources supplémentaires    
    
- [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
- [Always Encrypted (moteur de base de données)](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted (développement client)](../../../relational-databases/security/encryption/always-encrypted-client-development.md)
- [Blog sur Always Encrypted](https://blogs.msdn.microsoft.com/sqlsecurity/tag/always-encrypted/)

