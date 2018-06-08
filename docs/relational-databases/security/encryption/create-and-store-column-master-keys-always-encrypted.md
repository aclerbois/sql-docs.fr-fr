---
title: Créer et stocker des clés principales de colonne (Always Encrypted) | Microsoft Docs
ms.custom: ''
ms.date: 07/01/2016
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: ''
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 856e8061-c604-4ce4-b89f-a11876dd6c88
caps.latest.revision: 26
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4792ca405657dbfafc50a016c2c98ae56d99b4cd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-and-store-column-master-keys-always-encrypted"></a>Créer et stocker des clés principales de colonne (Always Encrypted)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Dans Always Encrypted, les*clés principales de colonne* sont des clés de protection de clés servant à chiffrer les clés de chiffrement de colonne. Les clés principales de colonne doivent être stockées dans un magasin de clés approuvé, et les clés doivent être accessibles aux applications qui doivent chiffrer ou déchiffrer des données, ainsi qu’aux outils servant à la configuration d’Always Encrypted et à la gestion des clés Always Encrypted.

Cet article fournit des informations détaillées sur la sélection d’un magasin de clés et la création de clés principales de colonne pour Always Encrypted. Pour plus d’informations, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

## <a name="selecting-a-key-store-for-your-column-master-key"></a>Sélection d’un magasin de clés pour votre clé principale de colonne

Always Encrypted prend en charge plusieurs magasins de clés pour le stockage des clés principales de colonne Always Encrypted. Les magasins de clés pris en charge varient selon le pilote et la version que vous utilisez.

Il existe deux catégories principales de magasins de clés : les *magasins de clés locaux*et les *magasins de clés centralisés*.

###  <a name="local-or-centralized-key-store"></a>Magasin de clés local ou centralisé ?

* **Magasins de clés locaux** : utilisables uniquement par les applications installées sur des ordinateurs sur lesquels se trouve un magasin de clés local. En d’autres termes, vous devez répliquer le magasin de clés et la clé pour chaque ordinateur qui exécute votre application. Le magasin de certificats Windows est un exemple de magasin de clés local. Lorsque vous utilisez un magasin de clés local, vous devez vérifier que le magasin de clés existe sur chaque ordinateur qui héberge votre application, et que l’ordinateur contient les clés principales de colonne dont votre application a besoin pour accéder aux données protégées à l’aide d’Always Encrypted. Lorsque vous mettez en service une clé principale de colonne pour la première fois, ou lorsque vous modifiez (permutez) la clé, vous devez vérifier que la clé est déployée sur tous les ordinateurs hébergeant vos applications.

* **Magasins de clés centralisés** : diffusent des applications sur plusieurs ordinateurs. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)est un exemple de magasin de clés centralisé. Un magasin de clés centralisé facilite généralement la gestion des clés, car vous n’avez pas besoin de maintenir plusieurs copies de vos clés principales de colonne sur plusieurs ordinateurs. Vous devez vérifier que vos applications sont configurées pour se connecter au magasin de clés centralisé.

### <a name="which-key-stores-are-supported-in-always-encrypted-enabled-client-drivers"></a>Quels sont les magasins de clés pris en charge par les pilotes clients avec Always Encrypted ?

Les pilotes clients avec Always Encrypted sont des pilotes clients SQL Server qui disposent d’une prise en charge intégrée permettant l’intégration d’Always Encrypted aux applications clientes. Les pilotes avec Always Encrypted comprennent certains fournisseurs intégrés pour les magasins de clés les plus courants. Notez que certains pilotes vous permettent également d’implémenter et d’inscrire un fournisseur de magasin de clés principales de colonne personnalisé, afin que vous puissiez utiliser n’importe quel magasin de clés, même s’il n’existe aucun fournisseur intégré pour celui-ci. Lorsque vous devez choisir entre un fournisseur intégré et un fournisseur personnalisé, gardez à l’esprit que l’utilisation d’un fournisseur intégré implique généralement moins de modifications pour vos applications (dans certains cas, seule la modification d’une chaîne de connexion de base de données est nécessaire).

Les fournisseurs intégrés disponibles varient selon le pilote, la version du pilote et le système d’exploitation sélectionné.  Consultez la documentation relative à Always Encrypted pour votre pilote afin de connaître les magasins de clés pris en charge de manière autonome et afin de savoir si votre pilote prend en charge les fournisseurs de magasins de clés personnalisés.

- [Développer des applications à l’aide d’Always Encrypted avec le fournisseur de données .NET Framework pour SQL Server](../../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)


### <a name="supported-tools"></a>Outils pris en charge

Vous pouvez utiliser [SQL Server Management Studio](../../../ssms/sql-server-management-studio-ssms.md) et le [module PowerShell SQL Server](https://blogs.technet.microsoft.com/dataplatforminsider/2016/06/30/sql-powershell-july-2016-update) pour configurer Always Encrypted et les clés Always Encrypted. Pour obtenir la liste des magasins de clés pris en charge par ces outils, consultez les rubriques suivantes :

- [Configurer Always Encrypted à l’aide de SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)


## <a name="creating-column-master-keys-in-windows-certificate-store"></a>Génération de clés principales de colonne dans le magasin de certificats Windows    

Une clé principale de colonne peut être un certificat stocké dans le magasin de certificats Windows. Notez qu’un pilote avec Always Encrypted ne vérifie pas les dates d’expiration et les chaînes d’autorité de certification. Un certificat est utilisé simplement comme une paire de clés comprenant une clé publique et une clé privée.

Pour être une clé principale de colonne valide, un certificat doit :
* Être un certificat X.509.
* Être stocké dans l’un des deux emplacements de magasin de certificats : *ordinateur local* ou *utilisateur actuel*. (pour créer un certificat dans l’emplacement de magasin de certificats de l’ordinateur local, vous devez être administrateur de l’ordinateur cible)
* Contenir une clé privée (la longueur minimale recommandée pour les clés d’un certificat est de 2 048 bits).
* Être créé en vue d’un échange de clés.


Plusieurs méthodes permettent de créer un certificat qui soit une clé principale de colonne valide. Cependant, l’option la plus simple consiste à créer un certificat auto-signé.


### <a name="create-a-self-signed-certificate-using-powershell"></a>Créer un certificat auto-signé à l’aide de PowerShell

Utilisez l’applet de commande [New-SelfSignedCertificate](https://technet.microsoft.com/library/hh848633.aspx) pour créer un certificat auto-signé. L’exemple suivant montre comment générer un certificat pouvant être utilisé comme clé principale de colonne pour Always Encrypted.

```
New-SelfSignedCertificate is a Windows PowerShell cmdlet that creates a self-signed certificate. The below examples show how to generate a certificate that can be used as a column master key for Always Encrypted.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048 

# To create a certificate in the local machine certificate store location you need to run the cmdlet as an administrator.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:LocalMachine\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048
```

### <a name="create-a-self-signed-certificate-using-sql-server-management-studio-ssms"></a>Créer un certificat auto-signé à l’aide de SQL Server Management Studio (SSMS)

Pour plus d’informations, consultez [Configurer Always Encrypted à l’aide de SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md).
Pour obtenir un didacticiel pas à pas qui utilise SSMS et stocke les clés Always Encrypted dans le magasin de certificats Windows, consultez le [didacticiel de l’Assistant Always Encrypted (magasin de certificats Windows)](https://azure.microsoft.com/documentation/articles/sql-database-always-encrypted/).


### <a name="making-certificates-available-to-applications-and-users"></a>Mise à disposition des certificats pour les applications et les utilisateurs

Si votre clé principale de colonne est un certificat stocké dans l’emplacement de magasin de certificats *ordinateur local* , vous devez exporter le certificat et la clé privée, et les importer sur tous les ordinateurs qui hébergent les applications censées chiffrer ou déchiffrer des données stockées dans les colonnes chiffrées, ou sur les outils de configuration d’Always Encrypted et de gestion des clés Always Encrypted. En outre, chaque utilisateur doit disposer d’une autorisation de lecture pour le certificat stocké dans le magasin de certificats « ordinateur local » afin de pouvoir utiliser le certificat comme une clé principale de colonne.

Si votre clé principale de colonne est un certificat stocké dans l’emplacement de magasin de certificats *utilisateur actuel* , vous devez exporter le certificat et la clé privée, et les importer dans l’emplacement de magasin de certificats « utilisateur actuel » de tous les comptes d’utilisateurs qui exécutent les applications censées chiffrer ou déchiffrer des données stockées dans les colonnes chiffrées, ou sur les outils de configuration d’Always Encrypted et de gestion des clés Always Encrypted (sur tous les ordinateurs disposant de ces applications et de ces outils). Aucune configuration d’autorisation n’est nécessaire. Une fois connecté à l’ordinateur, l’utilisateur peut accéder à tous les certificats qui se trouvent dans son emplacement de magasin de certificats « utilisateur actuel ».

#### <a name="using-powershell"></a>Utilisation de PowerShell
Utilisez les applets de commande [Import-PfxCertificate](https://msdn.microsoft.com/library/hh848625.aspx) et [Export-PfxCertificate](https://msdn.microsoft.com/library/hh848635.aspx) pour importer et exporter un certificat.

#### <a name="using-microsoft-management-console"></a>Utilisation de Microsoft Management Console 

Pour accorder à un utilisateur une autorisation *en lecture* pour un certificat stocké dans le magasin de certificats « ordinateur local », procédez comme suit :

1.  Ouvrez une invite de commandes et tapez **mmc**.
2.  Dans la console MMC, dans le menu **Fichier** , cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.
3.  Dans la boîte de dialogue **Ajouter/Supprimer un composant logiciel enfichable** , cliquez sur **Ajouter**.
4.  Dans la boîte de dialogue **Ajout d’un composant logiciel enfichable autonome** , cliquez sur **Certificats**, puis sur **Ajouter**.
5.  Dans la boîte de dialogue **Autorité de Certification** , cliquez sur **Le compte de l’ordinateur**, puis sur **Terminer**.
6.  Dans la boîte de dialogue **Ajout d’un composant logiciel enfichable autonome** , cliquez sur **Fermer**.
7.  Dans la boîte de dialogue **Ajouter/Supprimer un composant logiciel enfichable**, cliquez sur **OK**.
8.  Dans le composant logiciel enfichable **Certificats**, recherchez le certificat dans le dossier **Certificats > Personnel**, cliquez avec le bouton droit sur Certificat, pointez sur **Toutes les tâches**, puis cliquez sur **Gérer les clés privées**.
9.  Dans la boîte de dialogue **Sécurité**, ajoutez l’autorisation de lecture pour un compte d’utilisateur, si nécessaire.

## <a name="creating-column-master-keys-in-azure-key-vault"></a>Création de clés principales de colonne dans Azure Key Vault

Azure Key Vault permet de protéger des secrets et des clés de chiffrement. Cet outil est très pratique pour le stockage des clés principales de colonne Always Encrypted, surtout si vos applications sont hébergées dans Azure. Pour créer une clé dans [Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/), vous devez disposer d’un [abonnement Azure](https://azure.microsoft.com/free/) et d’un coffre de clés Azure.

#### <a name="using-powershell"></a>Utilisation de PowerShell

L’exemple suivant crée une clé et un coffre de clés Azure, puis accorde des autorisations à l’utilisateur de votre choix.

```
# Create a column master key in Azure Key Vault.
Login-AzureRmAccount
$SubscriptionId = "<Azure subscription ID>"
$resourceGroup = "<resource group name>"
$azureLocation = "<key vault location>"
$akvName = "<key vault name>"
$akvKeyName = "<column master key name>"
$azureCtx = Set-AzureRMContext -SubscriptionId $SubscriptionId # Sets the context for the below cmdlets to the specified subscription.
New-AzureRmResourceGroup –Name $resourceGroup –Location $azureLocation # Creates a new resource group - skip, if you desire group already exists.
New-AzureRmKeyVault -VaultName $akvName -ResourceGroupName $resourceGroup -Location $azureLocation -SKU premium # Creates a new key vault - skip if your vault already exists.
Set-AzureRmKeyVaultAccessPolicy -VaultName $akvName -ResourceGroupName $resourceGroup -PermissionsToKeys get, create, delete, list, update, import, backup, restore, wrapKey, unwrapKey, sign, verify -UserPrincipalName $azureCtx.Account
$akvKey = Add-AzureKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination HSM
```

#### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)

Pour obtenir un didacticiel pas à pas qui utilise SSMS et stocke les clés Always Encrypted dans Azure Key Vault, consultez le [didacticiel de l’Assistant Always Encrypted (Azure Key Vault)](https://azure.microsoft.com/documentation/articles/sql-database-always-encrypted-azure-key-vault).

### <a name="making-azure-key-vault-keys-available-to-applications-and-users"></a>Mise à disposition des clés Azure Key Vault pour les applications et les utilisateurs

Lorsque vous utilisez une clé Azure Key Vault comme clé principale de colonne, votre application doit s’authentifier auprès d’Azure et l’identité de votre application doit disposer des autorisations suivantes dans le coffre de clés : *get*, *unwrapKey*et *verify*. 

Pour mettre en service les clés de chiffrement de colonne qui sont protégées par une clé principale de colonne stockée dans Azure Key Vault, vous devez disposer des autorisations *get*, *unwrapKey*, *wrapKey*, *sign*et *verify* . En outre, pour créer une nouvelle clé dans un coffre de clés Azure, vous devez disposer de l’autorisation *create* . Pour afficher le contenu du coffre de clés, vous devez disposer de l’autorisation *list* .

#### <a name="using-powershell"></a>Utilisation de PowerShell

Pour permettre aux utilisateurs et aux applications d’accéder aux clés du coffre de clés Azure, vous devez définir la stratégie d’accès au coffre ([Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx)) :

```
$vaultName = "<vault name>"
$resourceGroupName = "<resource group name>"
$userPrincipalName = "<user to grant access to>"
$clientId = "<client Id>"

# grant users permissions to the keys:
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
# grant applications permissions to the keys:
Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list
```

## <a name="creating-column-master-keys-in-hardware-security-modules-using-cng"></a>Création de clés principales de colonne dans des modules de sécurité matériels à l’aide de CNG

Vous pouvez stocker une clé principale de colonne Always Encrypted dans un magasin de clés qui implémente l’API CNG (Cryptography Next Generation). En règle générale, ce type de magasin est un module de sécurité matériel. Un module de sécurité matériel est un périphérique physique qui protège et gère les clés numériques et fournit un traitement du chiffrement. Les modules de sécurité matériels se présentent généralement sous la forme d’une carte enfichable ou d’un périphérique externe qui se connecte directement à un ordinateur (modules de sécurité matériels locaux) ou à un serveur réseau.

Pour rendre un module de sécurité matériel disponible pour les applications d’un ordinateur donné, un fournisseur de stockage de clés (KSP) implémentant le CNG doit être installé et configuré sur l’ordinateur. Un pilote client avec Always Encrypted (fournisseur de clés principales de colonne intégré au pilote) utilise le KSP pour chiffrer et déchiffrer les clés de chiffrement de colonne, protégées par la clé principale de colonne stockée dans le magasin de clés.

Windows inclut le fournisseur de stockage de clés (KSP) des logiciels Microsoft qui peut être utilisé à des fins de test. Consultez [CNG Key Storage Providers](https://msdn.microsoft.com/library/windows/desktop/bb931355.aspx)(Fournisseurs de stockage de clés CNG).

### <a name="creating-column-master-keys-in-a-key-store-using-cngksp"></a>Création de clés principales de colonne dans un magasin de clés à l’aide du CNG ou d’un KSP

Une clé principale de colonne doit être une clé asymétrique (paire de clés publique/privée) utilisant l’algorithme RSA. La longueur de clé minimale recommandée est de 2048 bits.

#### <a name="using-hsm-specific-tools"></a>Utilisation des outils spécifiques aux modules de sécurité matériels
Consultez la documentation de votre module de sécurité matériel.

#### <a name="using-powershell"></a>Utilisation de PowerShell

Vous pouvez utiliser les API .NET pour créer une clé dans un magasin de clés à l’aide du CNG dans PowerShell.


```
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
```

#### <a name="using-sql-server-management-studio"></a>Utilisation de SQL Server Management Studio

Consultez [Provisioning Column Master using SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt757096.aspx#Anchor_2)(Mise en service des clés principales de colonne à l’aide de SQL Server Management Studio (SSMS)).


### <a name="making-cng-keys-available-to-applications-and-users"></a>Mise à disposition des clés CNG pour les applications et les utilisateurs

Consultez la documentation relative au module de sécurité matériel et au fournisseur de stockage de clés pour savoir comment configurer le fournisseur de stockage de clés sur un ordinateur et comment accorder aux applications et aux utilisateurs l’accès au module de sécurité matériel.

## <a name="creating-column-master-keys-in-hardware-security-modules-using-capi"></a>Création de clés principales de colonne dans des modules de sécurité matériels à l’aide de CAPI

Vous pouvez stocker une clé principale de colonne Always Encrypted dans un magasin de clés qui implémente l’API Cryptography (CAPI). En général, ce type de magasin est un module de sécurité matériel, c’est-à-dire un périphérique physique qui protège et gère les clés numériques et fournit un traitement du chiffrement. Les modules de sécurité matériels se présentent généralement sous la forme d’une carte enfichable ou d’un périphérique externe qui se connecte directement à un ordinateur (modules de sécurité matériels locaux) ou à un serveur réseau.

Pour rendre un module de sécurité matériel disponible pour les applications d’un ordinateur donné, un fournisseur de services de chiffrement (CSP) implémentant CAPI doit être installé et configuré sur l’ordinateur. Un pilote client avec Always Encrypted (fournisseur de clés principales de colonne intégré au pilote) utilise le CSP pour chiffrer et déchiffrer les clés de chiffrement de colonne, protégées par la clé principale de colonne stockée dans le magasin de clés. Remarque : CAPI est une API héritée obsolète. Si un fournisseur de stockage de clés est disponible pour votre module de sécurité matériel, vous devez l’utiliser plutôt que le fournisseur de services de chiffrement avec CAPI.

Un fournisseur de services de chiffrement doit prendre en charge l’algorithme RSA à utiliser avec Always Encrypted.

Windows inclut les fournisseurs de services de chiffrement logiciels (sans module de sécurité matériel) suivants qui prennent en charge l’algorithme RSA et peuvent être utilisés à des fins de test : Microsoft Enhanced RSA et AES Cryptographic Provider.

### <a name="creating-column-master-keys-in-a-key-store-using-capicsp"></a>Création de clés principales de colonne dans un magasin de clés à l’aide de CAPI ou d’un CSP

Une clé principale de colonne doit être une clé asymétrique (paire de clés publique/privée) utilisant l’algorithme RSA. La longueur de clé minimale recommandée est de 2048 bits.

#### <a name="using-hsm-specific-tools"></a>Utilisation des outils spécifiques aux modules de sécurité matériels
Consultez la documentation de votre module de sécurité matériel.

#### <a name="using-sql-server-management-studio-ssms"></a>Utilisation de SQL Server Management Studio (SSMS)
Consultez la section « Mise en service des clés principales de colonne » dans la rubrique « Configurer Always Encrypted à l’aide de SQL Server Management Studio ».

 
### <a name="making-cng-keys-available-to-applications-and-users"></a>Mise à disposition des clés CNG pour les applications et les utilisateurs
Consultez la documentation relative au module de sécurité matériel et au fournisseur de services de chiffrement pour savoir comment configurer le fournisseur de services de chiffrement sur un ordinateur et comment accorder aux applications et aux utilisateurs l’accès au module de sécurité matériel.
 
 
## <a name="next-steps"></a>Next Steps  
  
- [Configurer des clés Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md)
- [Permuter des clés Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)
- [Configurer Always Encrypted à l’aide de SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)

  
## <a name="additional-resources"></a>Ressources supplémentaires  

- [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Always Encrypted (moteur de base de données)](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Développer des applications à l’aide d’Always Encrypted avec le fournisseur de données .NET Framework pour SQL Server](../../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)
- [Blog Always Encrypted](https://blogs.msdn.microsoft.com/sqlsecurity/tag/always-encrypted/)
    

