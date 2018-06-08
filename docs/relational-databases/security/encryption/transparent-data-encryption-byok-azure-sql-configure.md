---
title: 'PowerShell et CLI : Activer le chiffrement TDE SQL à l’aide de votre propre clé Azure Key Vault | Microsoft Docs'
description: Découvrez comment configurer Azure SQL Database et Data Warehouse pour commencer à utiliser TDE (Transparent Data Encryption) pour le chiffrement au repos à l’aide de PowerShell ou CLI.
keywords: ''
documentationcenter: ''
author: aliceku
manager: craigg
editor: ''
ms.prod: ''
ms.reviewer: ''
ms.suite: sql
ms.prod_service: sql-database, sql-data-warehouse
ms.service: sql-database
ms.component: security
ms.tgt_pltfrm: ''
ms.devlang: azurecli, powershell
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: aliceku
monikerRange: = azuresqldb-current || = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 7b930486b624244ea0863ee1fd059fafd1e8598f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-using-your-own-key-from-azure-key-vault"></a>PowerShell et CLI : Activer TDE à l’aide de votre propre clé Azure Key Vault

[!INCLUDE[appliesto-xx-asdb-asdw-xxx-md](../../../includes/appliesto-xx-asdb-asdw-xxx-md.md)]

Ce guide pratique explique comment utiliser une clé Azure Key Vault avec la fonctionnalité Transparent Data Encryption (TDE) pour SQL Database ou Data Warehouse. Pour en savoir plus sur TDE avec prise en charge de Bring Your Own Key (BYOK), consultez [Transparent Data Encryption (TDE) avec prise en charge de Bring Your Own Key pour Azure SQL](transparent-data-encryption-byok-azure-sql.md). 

## <a name="prerequisites-for-powershell"></a>Prérequis pour PowerShell

- Vous devez avoir un abonnement Azure et être administrateur de cet abonnement.
- [Recommandé mais facultatif] Vous devez avoir un module de sécurité matériel (HSM) ou un magasin de clés local à utiliser pour la création d’une copie locale des éléments de clé du protecteur TDE.
- Azure PowerShell version 4.2.0 ou ultérieure doit être installé et exécuté. 
- Vous devez avoir créé un conteneur Azure Key Vault et une clé pour TDE.
   - [Instructions PowerShell à partir de Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started)
   - [Instructions pour utiliser un module de sécurité matériel (HSM) et Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started#a-idhsmaif-you-want-to-use-a-hardware-security-module-hsm)
 - Le coffre de clés doit avoir la propriété suivante pour être utilisé pour TDE :
   - [soft-delete](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-ovw-soft-delete)
   - [Guide pratique pour utiliser la suppression réversible Key Vault avec Azure Power​Shell](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-soft-delete-powershell) 
- La clé doit avoir les attributs suivants pour être utilisée pour TDE :
   - Pas de date d’expiration
   - Non désactivée
   - Configurée avec les autorisations *get*, *wrap key*, *unwrap key*

## <a name="step-1-assign-an-azure-ad-identity-to-your-server"></a>Étape 1. Affecter une identité Azure AD à votre serveur 

Si vous disposez d’un serveur existant, ajoutez une identité Azure AD à ce serveur de la manière suivante :

   ```powershell
   $server = Set-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -AssignIdentity
   ```

Si vous créez un serveur, utilisez l’applet de commande [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) avec la balise -Identity pour ajouter une identité Azure AD au moment de la création du serveur :

   ```powershell
   $server = New-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -Location <RegionName> `
   -ServerName <LogicalServerName> `
   -ServerVersion "12.0" `
   -SqlAdministratorCredentials <PSCredential> `
   -AssignIdentity 
   ```

## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>Étape 2. Accorder des autorisations Key Vault à votre serveur

Utilisez l’applet de commande [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) pour autoriser votre serveur à accéder au coffre de clés et à utiliser une clé de ce coffre pour TDE.

   ```powershell
   Set-AzureRmKeyVaultAccessPolicy  `
   -VaultName <KeyVaultName> `
   -ObjectId $server.Identity.PrincipalId `
   -PermissionsToKeys get, wrapKey, unwrapKey
   ```

## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Étape 3. Ajouter la clé Key Vault au serveur et définir le protecteur TDE

- Utilisez l’applet de commande [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) pour ajouter la clé Key Vault au serveur.
- Utilisez l’applet de commande [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) pour définir la clé comme protecteur TDE pour toutes les ressources du serveur.
- Utilisez l’applet de commande [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) pour vérifier que le protecteur TDE a été configuré de manière appropriée.

> [!Note]
> La longueur totale du nom Key Vault et du nom de la clé ne doit pas dépasser 94 caractères.
> 

>[!Tip]
>Voici un exemple d’ID de la clé de Key Vault : https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>

   ```powershell
   <# Add the key from Key Vault to the server #>
   Add-AzureRmSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> 

   <# To confirm that the TDE protector was configured as intended: #>
   Get-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> 
   ```

## <a name="step-4-turn-on-tde"></a>Étape 4. Activer TDE 

Utilisez l’applet de commande [Set-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) pour activer TDE.

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `
   -State "Enabled"
   ```

TDE est maintenant activé pour l’entrepôt de données ou la base de données, avec une clé de chiffrement dans Key Vault.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Étape 5. Vérifier l’état et l’activité du chiffrement

Utilisez l’applet de commande [Get-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) pour obtenir l’état de chiffrement et l’applet de commande [Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) pour vérifier la progression du chiffrement pour un entrepôt de données ou une base de données.

   ```powershell
   # Get the encryption state
   Get-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `

   <# Check the encryption progress for a database or data warehouse #>
   Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName>  
   ```

## <a name="other-useful-powershell-cmdlets"></a>Autres applets de commande PowerShell utiles

- Utilisez l’applet de commande [Set-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) pour désactiver TDE.

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -DatabaseName <DatabaseName> `
   -State "Disabled”
   ```
 
- Utilisez l’applet de commande [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) pour retourner la liste des clés Key Vault ajoutées au serveur.

   ```powershell
   <# KeyId is an optional parameter, to return a specific key version #>
   Get-AzureRmSqlServerKeyVaultKey `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```
 
- Utilisez l’applet de commande [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) pour supprimer une clé Key Vault sur le serveur.

   ```powershell
   <# The key set as the TDE Protector cannot be removed. #>
   Remove-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>   
   ```
 
## <a name="troubleshooting"></a>Dépannage

En cas de problème, vérifiez les points suivants :
- Si le conteneur Key Vault est introuvable, vérifiez que vous avez l’abonnement approprié à l’aide de l’applet de commande [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

   ```powershell
   Get-AzureRmSubscription `
   -SubscriptionId <SubscriptionId>
   ```

- Si la nouvelle clé ne peut pas être ajoutée au serveur ou si elle ne peut pas être définie comme protecteur TDE, vérifiez les points suivants :
   - La clé ne doit pas avoir de date d’expiration.
   - La clé doit avoir les autorisations *get*, *wrap key* et *unwrap key*.

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment effectuer une rotation du protecteur TDE d’un serveur conformément aux exigences de sécurité : [Effectuer une rotation du protecteur TDE (Transparent Data Encryption) à l’aide de PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- En cas de risque pour la sécurité, découvrez comment supprimer un protecteur TDE potentiellement compromis : [Supprimer une clé potentiellement compromise](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md). 

## <a name="prerequisites-for-cli"></a>Prérequis pour CLI

- Vous devez avoir un abonnement Azure et être administrateur de cet abonnement.
- [Recommandé mais facultatif] Vous devez avoir un module de sécurité matériel (HSM) ou un magasin de clés local à utiliser pour la création d’une copie locale des éléments de clé du protecteur TDE.
- Interface de ligne de commande CLI version 2.0 ou ultérieure. Pour installer la version la plus récente et vous connecter à votre abonnement Azure, consultez [Installer et configurer l’interface de ligne de commande multiplateforme Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest). 
- Vous devez avoir créé un conteneur Azure Key Vault et une clé pour TDE.
   - [Gérer Key Vault à l’aide de CLI 2.0](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-manage-with-cli2)
   - [Instructions pour utiliser un module de sécurité matériel (HSM) et Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started#a-idhsmaif-you-want-to-use-a-hardware-security-module-hsm)
 - Le coffre de clés doit avoir la propriété suivante pour être utilisé pour TDE :
   - [soft-delete](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-ovw-soft-delete)
   - [Guide pratique pour utiliser la suppression réversible Key Vault avec l’interface CLI](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-soft-delete-cli) 
- La clé doit avoir les attributs suivants pour être utilisée pour TDE :
   - Pas de date d’expiration
   - Non désactivée
   - Configurée avec les autorisations *get*, *wrap key*, *unwrap key*
   
## <a name="step-1-create-a-server-and-assign-an-azure-ad-identity-to-your-server"></a>Étape 1. Créer un serveur et affecter une identité Azure AD à votre serveur
      cli
      # create server (with identity) and database
      az sql server create -n "ServerName" -g "ResourceGroupName" -l "westus" -u "cloudsa" -p "YourFavoritePassWord99@34" -I 
      az sql db create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 
      

 
## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>Étape 2. Accorder des autorisations Key Vault à votre serveur
      cli
      # create key vault, key and grant permission
      az keyvault create -n "VaultName" -g "ResourceGroupName" 
      az keyvault key create -n myKey -p software --vault-name "VaultName" 
      az keyvault set-policy -n "VaultName" --object-id "ServerIdentityObjectId" -g "ResourceGroupName" --key-permissions wrapKey unwrapKey get list 
      

 
## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Étape 3. Ajouter la clé Key Vault au serveur et définir le protecteur TDE
  
     cli
     # add server key and update encryption protector
      az sql server key create -g "ResourceGroupName" -s "ServerName" -t "AzureKeyVault" -u "FullVersionedKeyUri 
      az sql server tde-key update -g "ResourceGroupName" -s "ServerName" -t AzureKeyVault -u "FullVersionedKeyUri" 
      
  
  > [!Note]
> La longueur totale du nom Key Vault et du nom de la clé ne doit pas dépasser 94 caractères.
> 

>[!Tip]
>Voici un exemple d’ID de la clé de Key Vault : https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>
  
## <a name="step-4-turn-on-tde"></a>Étape 4. Activer TDE 
      cli
      # enable encryption
      az sql db tde create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" --status Enabled 
      

TDE est maintenant activé pour l’entrepôt de données ou la base de données, avec une clé de chiffrement dans Key Vault.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Étape 5. Vérifier l’état et l’activité du chiffrement

     cli
      # get encryption scan progress
      az sql db tde show-activity -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

      # get whether encryption is on or off
      az sql db tde show-configuration -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

