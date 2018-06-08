---
title: PowerShell - Supprimer un protecteur TDE - Azure SQL | Microsoft Docs
description: Ce guide pratique explique comment gérer un protecteur TDE potentiellement compromis pour une base de données ou un entrepôt de données Azure SQL qui utilise la fonctionnalité TDE avec prise en charge de BYOK (Apportez votre propre clé).
keywords: ''
services: sql-database
documentationcenter: ''
author: becczhang
manager: craigg
ms.prod: ''
ms.reviewer: ''
ms.suite: sql
ms.prod_service: sql-database, sql-data-warehouse
ms.service: sql-database
ms.custom: ''
ms.component: security
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.date: 08/07/2017
ms.author: rebeccaz
monikerRange: = azuresqldb-current || = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: d83d696accde5d192c151703bb461edae06bc084
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="remove-a-transparent-data-encryption-tde-protector-using-powershell"></a>Supprimer un protecteur TDE (Transparent Data Encryption) à l’aide de PowerShell
[!INCLUDE[appliesto-xx-asdb-asdw-xxx-md](../../../includes/appliesto-xx-asdb-asdw-xxx-md.md)]

## <a name="prerequisites"></a>Conditions préalables requises
- Vous devez avoir un abonnement Azure et être administrateur de cet abonnement.
- Azure PowerShell version 4.2.0 ou ultérieure doit être installé et exécuté. 
- Ce guide pratique suppose que vous utilisez déjà une clé Azure Key Vault comme protecteur TDE pour une base de données ou un entrepôt de données Azure SQL. Pour en savoir plus, consultez [Transparent Data Encryption avec prise en charge de BYOK](transparent-data-encryption-byok-azure-sql.md).

## <a name="overview"></a>Vue d'ensemble
Ce guide pratique explique comment gérer un protecteur TDE potentiellement compromis pour une base de données ou un entrepôt de données Azure SQL qui utilise la fonctionnalité TDE avec prise en charge de BYOK (Apportez votre propre clé). Pour en savoir plus sur la prise en charge de BYOK pour TDE, consultez la [page de présentation](transparent-data-encryption-byok-azure-sql.md). 

Effectuez les procédures suivantes uniquement dans des cas exceptionnels ou dans des environnements de test. Lisez ce guide pratique avec soin, car la suppression de protecteurs TDE activement utilisés dans Azure Key Vault peut entraîner une  **perte de données**. 

Si vous suspectez une clé d’être compromise, par exemple, un service ou un utilisateur y accède de façon non autorisée, supprimez cette clé.

N’oubliez pas qu’après la suppression du protecteur TDE dans Key Vault, **toutes les connexions aux bases de données chiffrées du serveur seront bloquées, et toutes ces bases de données seront mises hors connexion et seront supprimées dans un délai de 24 heures**. De plus, les anciennes sauvegardes chiffrées avec la clé compromise ne seront plus accessibles.

Ce guide pratique expose deux approches possibles en fonction du résultat souhaité après la réponse à un incident :
- Garder les bases de données et entrepôts de données Azure SQL **accessibles**
- Rendre les bases de données et entrepôts de données Azure SQL **inaccessibles**

## <a name="to-keep-the-encrypted-resources-accessible"></a>Garder les ressources chiffrées accessibles
1. [Créez une clé dans Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/add-azurekeyvaultkey?view=azurermps-4.1.0). Assurez-vous de créer cette autre clé dans un coffre Key Vault séparé du protecteur TDE potentiellement compromis, car le contrôle d’accès est provisionné au niveau du coffre. 
2. Ajoutez la nouvelle clé au serveur à l’aide des applets de commande [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) et [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector), puis définissez cette clé comme nouveau protecteur TDE du serveur.

   ```powershell
   # Add the key from Key Vault to the server  
   Add-AzureRmSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>
   
   # Set the key as the TDE protector for all resources under the server
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault -KeyId <KeyVaultKeyId> 
   ```

3. Vérifiez que le nouveau protecteur TDE a été mis à jour sur le serveur et tous les réplicas à l’aide de l’applet de commande [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector). 

   >[!NOTE]
   > La propagation du nouveau protecteur TDE dans toutes les bases de données et bases de données secondaires sur le serveur peut prendre plusieurs minutes.
   >

   ```powershell
   Get-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

4. Effectuez une [sauvegarde de la nouvelle clé](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey) dans Key Vault.

   ```powershell
   <# -OutputFile parameter is optional; 
   if removed, a file name is automatically generated. #>
   Backup-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -OutputFile <DesiredBackupFilePath>
   ```
 
5. Supprimez la clé compromise dans Key Vault à l’aide de l’applet de commande [Remove-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/remove-azurekeyvaultkey). 

   ```powershell
   Remove-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName>
   ```
 
6. Pour restaurer ultérieurement une clé dans Key Vault, utilisez l’applet de commande [Restore-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey) :
   ```powershell
   Restore-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -InputFile <BackupFilePath>
   ```
 
## <a name="to-make-the-encrypted-resources-inaccessible"></a>Rendre les ressources chiffrées inaccessibles
1. Supprimez les bases de données qui sont chiffrées avec la clé potentiellement compromise.
Les fichiers de base de données et les fichiers journaux sont automatiquement sauvegardés. Vous pouvez ainsi à tout moment effectuer une restauration limitée dans le temps de la base de données (à condition de fournir la clé). Supprimez les bases de données avant de supprimer un protecteur TDE actif pour éviter la perte éventuelle des données des transactions effectuées au cours des 10 dernières minutes. 
2. Sauvegardez les éléments de clé du protecteur TDE dans Key Vault.
3. Supprimez la clé potentiellement compromise dans Key Vault.

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment effectuer une rotation du protecteur TDE d’un serveur conformément aux exigences de sécurité : [Effectuer une rotation du protecteur TDE (Transparent Data Encryption) à l’aide de PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md)

- Commencez à utiliser TDE avec prise en charge de Bring Your Own Key : [Activer TDE avec votre propre clé Key Vault à l’aide de PowerShell](transparent-data-encryption-byok-azure-sql-configure.md)
