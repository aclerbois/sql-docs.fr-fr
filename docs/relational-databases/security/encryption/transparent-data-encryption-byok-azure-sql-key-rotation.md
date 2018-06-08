---
title: PowerShell - Rotation du protecteur TDE - Azure SQL | Microsoft Docs
description: Découvrez comment effectuer une rotation du protecteur TDE (Transparent Data Encryption) pour un serveur Azure SQL Server.
keywords: ''
services: sql-database
documentationcenter: ''
author: becczhang
manager: jhubbard
editor: ''
ms.prod: ''
ms.reviewer: ''
ms.suite: sql
ms.prod_service: sql-database, sql-data-warehouse
ms.service: sql-database
ms.custom: ''
ms.component: security
ms.tgt_pltfrm: ''
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ryzhang26
monikerRange: = azuresqldb-current || = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: cee667b6f92c19c03cd670537d09dc5ccc7b03f3
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="rotate-the-transparent-data-encryption-tde-protector-using-powershell"></a>Effectuer une rotation du protecteur TDE (Transparent Data Encryption) à l’aide de PowerShell 
[!INCLUDE[appliesto-xx-asdb-asdw-xxx-md](../../../includes/appliesto-xx-asdb-asdw-xxx-md.md)]

Ce guide pratique explique comment effectuer une rotation des clés pour un serveur Azure SQL Server qui utilise un protecteur TDE fourni par Azure Key Vault. La rotation du protecteur TDE d’un serveur Azure SQL Server permet de changer la clé asymétrique utilisée pour protéger les bases de données sur le serveur. La rotation des clés est une opération en ligne qui s’effectue normalement en quelques secondes, car elle déchiffre et rechiffre uniquement la clé de chiffrement des données de la base de données, pas la base de données entière.

Ce guide expose les deux options possibles pour effectuer la rotation du protecteur TDE sur le serveur.

> [!NOTE]
> Si vous avez suspendu l’exécution de votre SQL Data Warehouse, vous devez la reprendre avant d’effectuer la rotation des clés.
>

> [!IMPORTANT]
> Après une substitution de clé, **ne supprimez pas** les versions précédentes de la clé.  Quand des clés sont substituées, certaines données peuvent rester chiffrées avec les clés précédentes, par exemple, d’anciennes sauvegardes de la base de données. 
>

## <a name="prerequisites"></a>Conditions préalables requises

- Ce guide pratique suppose que vous utilisez déjà une clé Azure Key Vault comme protecteur TDE pour une base de données ou un entrepôt de données Azure SQL. Consultez [Transparent Data Encryption avec prise en charge de BYOK](transparent-data-encryption-byok-azure-sql.md).
- Azure PowerShell version 3.7.0 ou ultérieure doit être installé et exécuté. 
- [Recommandé mais facultatif] Créez d’abord les éléments de clé du protecteur TDE dans un module de sécurité matériel (HSM) ou un magasin de clés local, puis importez ces éléments dans Azure Key Vault. Pour en savoir plus, consultez [Instructions pour utiliser un module de sécurité matériel (HSM) et Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).

## <a name="option-1-auto-rotation"></a>Option 1 : Rotation automatique

Générez une nouvelle version de la clé du protecteur TDE existante dans Key Vault, en utilisant le même nom de clé et le même Key Vault. Le service Azure SQL commencera à utiliser cette nouvelle version de clé dans un délai de 24 heures. 

Pour créer une nouvelle version du protecteur TDE, utilisez l’applet de commande [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) :

   ```powershell
   Add-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>
   ```

## <a name="option-2-manual-rotation"></a>Option 2 : Rotation manuelle

Utilisez les applets de commande [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey), [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) et [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) pour ajouter une clé entièrement nouvelle, avec un nom de clé différent et/ou un autre Key Vault. 

>[!NOTE]
>La longueur totale du nom Key Vault et du nom de la clé ne doit pas dépasser 94 caractères.
>

   ```powershell
   # Add a new key to Key Vault
   Add-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>   
  
   <# Set the key as the TDE protector for all resources 
   under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```
  
## <a name="other-useful-powershell-cmdlets"></a>Autres applets de commande PowerShell utiles

- Pour passer le protecteur TDE du mode géré par Microsoft au mode BYOK, utilisez l’applet de commande [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector).

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- Pour passer le protecteur TDE du mode BYOK au mode géré par Microsoft, utilisez l’applet de commande [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector).

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type ServiceManaged `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName> 
   ``` 

## <a name="next-steps"></a>Étapes suivantes

- En cas de risque pour la sécurité, découvrez comment supprimer un protecteur TDE potentiellement compromis : [Supprimer une clé potentiellement compromise](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md) 

- Commencez à utiliser TDE avec prise en charge de Bring Your Own Key : [Activer TDE avec votre propre clé Key Vault à l’aide de PowerShell](transparent-data-encryption-byok-azure-sql-configure.md)
