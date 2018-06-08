---
title: Transparent Data Encryption pour Azure SQL Database et Data Warehouse | Microsoft Docs
description: Vue d’ensemble de Transparent Data Encryption (TDE) pour SQL Database et Data Warehouse. Le document décrit les avantages de cette fonctionnalité, ainsi que les options de configuration disponibles, comme TDE géré par le service et BYOK (Bring Your Own Key).
keywords: ''
author: becczhang
manager: craigg
editor: ''
ms.prod: ''
ms.reviewer: ''
ms.suite: sql
ms.prod_service: sql-database, sql-data-warehouse
ms.service: sql-database
ms.component: security
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.date: 05/08/2018
ms.author: rebeccaz
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: b88dfeac58ef9c00307b2cfee35aca3ea0549f02
ms.sourcegitcommit: feff98b3094a42f345a0dc8a31598b578c312b38
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/11/2018
---
# <a name="transparent-data-encryption-for-sql-database-and-data-warehouse"></a>TDE pour SQL Database et Data Warehouse
[!INCLUDE[appliesto-xx-asdb-asdw-xxx-md](../../../includes/appliesto-xx-asdb-asdw-xxx-md.md)]

TDE vous aide à protéger Azure SQL Database et Azure Data Warehouse contre les menaces d’activités malveillantes. Il effectue un chiffrement et un déchiffrement en temps réel de la base de données, des sauvegardes associées et des fichiers journaux transactionnels au repos, sans qu’il soit nécessaire de modifier l’application. Par défaut, TDE est activé pour toutes les bases de données Azure SQL Database nouvellement déployées, mais il peut être nécessaire de l’activer manuellement pour les bases de données plus anciennes.  

TDE chiffre le stockage d’une base de données entière en utilisant une clé symétrique appelée clé de chiffrement de la base de données. Cette clé de chiffrement de base de données est protégée par le protecteur TDE. Le protecteur est un certificat géré par le service (TDE géré par le service) ou une clé asymétrique stockée dans Azure Key Vault (Bring Your Own Key). Vous définissez le protecteur TDE au niveau du serveur. 

Au démarrage de la base de données, la clé de chiffrement de base de données chiffrée est déchiffrée, puis elle est utilisée pour déchiffrer et rechiffrer les fichiers de la base de données dans le processus du moteur de base de données SQL Server. TDE effectue le chiffrement et le déchiffrement des E/S en temps réel des données au niveau des pages. Chaque page est déchiffrée quand elle est lue en mémoire, puis elle est chiffrée avant d’être écrite sur le disque. Pour obtenir une description générale de TDE, consultez [Transparent data encryption](transparent-data-encryption.md).

Quand SQL Server est exécuté sur une machine virtuelle Azure, il peut également utiliser une clé asymétrique fournie par Key Vault. Les étapes de configuration sont différentes de celles liées à l’utilisation d’une clé asymétrique dans SQL Database. Pour plus d’informations, consultez [Gestion de clés extensible à l’aide d’Azure Key Vault (SQL Server)](extensible-key-management-using-azure-key-vault-sql-server.md).

## <a name="service-managed-transparent-data-encryption"></a>TDE géré par le service

Dans Azure, la configuration par défaut de TDE spécifie que la clé de chiffrement de la base de données est protégée par un certificat de serveur intégré. Chaque serveur est associé à un certificat de serveur intégré unique. Si une base de données est dans une relation de géoréplication, la base de données primaire et la base de données géosecondaire sont toutes les deux protégées par la clé du serveur parent de la base de données primaire. Si deux bases de données sont connectées au même serveur, elles partagent le même certificat intégré. Microsoft fait automatiquement alterner ces certificats au moins tous les 90 jours.

Microsoft déplace et gère également les clés de façon transparente en fonction des besoins pour la géoréplication et les restaurations. 

> [!IMPORTANT]
> Toutes les nouvelles bases de données SQL sont chiffrées par défaut avec TDE géré par le service. Les bases de données antérieures à mai 2017 ou celles qui ont été créées par restauration, géoréplication ou copie de base de données ne sont pas chiffrées par défaut.
>

## <a name="bring-your-own-key"></a>Bring Your Own Key

Avec la prise en charge de la fonctionnalité BYOK, vous pouvez gérer vous-même vos clés TDE, et déterminer qui peut y accéder et quand. Key Vault, qui est le système de gestion des clés externes sur le cloud d’Azure, est le premier service de gestion de clés intégré par TDE à la prise en charge de BYOK. Avec la prise en charge de BYOK, la clé de chiffrement de la base de données est protégée par une clé asymétrique stockée dans Key Vault. La clé asymétrique ne quitte jamais Key Vault. Une fois que le serveur a obtenu les autorisations d’accès à un coffre de clés, il envoie les demandes d’opérations de clé de base à ce coffre via Key Vault. Vous définissez la clé asymétrique au niveau du serveur et toutes les bases de données sur ce serveur en héritent.

Avec la prise en charge de BYOK, vous pouvez désormais contrôler les tâches de gestion des clés, comme les rotations de clés et les autorisations du coffre de clés. Vous pouvez également supprimer des clés et activer l’audit/création de rapports sur toutes les clés de chiffrement. Key Vault fournit une gestion des clés centralisée et utilise des modules de sécurité matériels étroitement surveillés. Key Vault favorise la séparation entre la gestion des clés et celle des données, de façon à respecter la conformité aux normes. Pour en savoir plus sur Key Vault, consultez la [page de documentation de Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).

Pour en savoir plus sur TDE avec prise en charge de BYOK pour Azure SQL Database et Data Warehouse, consultez [Transparent Data Encryption avec prise en charge de BYOK](transparent-data-encryption-byok-azure-sql.md).

Pour commencer à utiliser TDE avec prise en charge de BYOK, consultez le guide pratique [Activer Transparent Data Encryption avec votre propre clé provenant de Key Vault en utilisant PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Déplacer une base de données protégée par TDE

Il n’est pas nécessaire de déchiffrer les bases de données pour effectuer des opérations dans Azure. La base de données cible hérite de façon transparente des paramètres TDE de la base de données source ou de la base de données primaire. Les opérations incluses impliquent :
- La géorestauration
- La restauration en libre-service à un point dans le temps
- La restauration d’une base de données supprimée
- La géoréplication active
- La création d’une copie de base de données

Quand vous exportez une base de données protégée par TDE, le contenu exporté de la base de données n’est pas chiffré. Ce contenu exporté est stocké dans des fichiers BACPAC non chiffrés. Vous devez donc protéger les fichiers BACPAC de façon appropriée et activer TDE une fois l’importation de la nouvelle base de données terminée.

Par exemple, si le fichier BACPAC est exporté à partir d’une instance SQL Server locale, le contenu importé de la nouvelle base de données n’est pas chiffré automatiquement. De la même façon, si le fichier BACPAC est exporté vers une instance SQL Server locale, la nouvelle base de données n’est pas chiffrée automatiquement.

La seule exception est quand vous exportez vers et depuis une base de données SQL. TDE est activé dans la nouvelle base de données, mais le fichier BACPAC lui-même n’est pas chiffré.

## <a name="manage-transparent-data-encryption-in-the-azure-portal"></a>Gérer TDE dans le portail Azure

Pour configurer TDE via le portail Azure, vous devez être connecté en tant que Propriétaire, Contributeur ou Gestionnaire de sécurité SQL Azure. 

Vous définissez TDE au niveau de la base de données. Pour activer TDE sur une base de données, accédez au [portail Azure](https://portal.azure.com) et connectez-vous avec votre compte Administrateur ou Contributeur Azure. Recherchez les paramètres TDE dans votre base de données utilisateur. Par défaut, TDE géré par le service est utilisé. Un certificat TDE est automatiquement généré pour le serveur qui contient la base de données. 

![TDE géré par le service](./media/transparent-data-encryption-azure-sql/service-managed-tde.png)  

Vous définissez la clé principale de TDE, également appelée protecteur TDE, au niveau du serveur. Pour utiliser TDE avec la prise en charge de BYOK et protéger vos bases de données avec une clé de Key Vault, consultez les paramètres TDE de votre serveur. 

![TDE avec prise en charge de BYOK](./media/transparent-data-encryption-azure-sql/tde-byok-support.png) 

## <a name="manage-transparent-data-encryption-by-using-powershell"></a>Gérer TDE avec PowerShell

Pour configurer TDE via PowerShell, vous devez être connecté en tant que Propriétaire, Contributeur ou Gestionnaire de sécurité SQL Azure. 

| Applet de commande | Description |
| --- | --- |
| [Set-AzureRmSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) |Active ou désactive TDE pour une base de données|
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) |Obtient l’état TDE pour une base de données |
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption-Activity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) |Vérifie la progression du chiffrement pour une base de données |
| [Add-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) |Ajoute une clé Key Vault à une instance SQL Server |
| [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) |Obtient la clé Key Vault pour une instance SQL Server  |
| [Set-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) |Définit le protecteur TDE pour une instance SQL Server |
| [Get-AzureRmSqlServerTransparentDataEncryptionProtector](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) |Obtient le protecteur TDE |
| [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) |Supprime une clé Key Vault sur une instance SQL Server |
|  | |

## <a name="manage-transparent-data-encryption-by-using-transact-sql"></a>Gérer TDE avec Transact-SQL

Connectez-vous à la base de données en tant qu’administrateur ou membre du rôle **dbmanager** dans la base de données MASTER.

| Command | Description |
| --- | --- |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) | SET ENCRYPTION ON/OFF chiffre ou déchiffre une base de données. |
| [sys.dm_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Retourne des informations sur l’état de chiffrement d’une base de données et de ses clés de chiffrement de base de données associées |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Retourne des informations sur l’état de chiffrement de chaque nœud d’entrepôt de données et des clés de chiffrement de base de données associées | 
|  | |

Vous ne pouvez pas passer du protecteur TDE à une clé de Key Vault en utilisant Transact-SQL. Utilisez PowerShell ou le portail Azure.

## <a name="manage-transparent-data-encryption-by-using-the-rest-api"></a>Gérer TDE avec l’API REST
 
Pour configurer TDE via le portail Azure, vous devez être connecté en tant que Propriétaire, Contributeur ou Gestionnaire de sécurité SQL Azure. 

| Command | Description |
| --- | --- |
|[Créer ou mettre à jour un serveur](/rest/api/sql/servers/createorupdate)|Ajoute une identité Azure Active Directory à une instance SQL Server (utilisée pour accorder l’accès à Key Vault)|
|[Créer ou mettre à jour une clé serveur](/rest/api/sql/serverkeys/createorupdate)|Ajoute une clé Key Vault à une instance SQL Server|
|[Supprimer une clé serveur](/rest/api/sql/serverkeys/delete)|Supprime une clé Key Vault sur une instance SQL Server|
|[Obtenir des clés serveur](/rest/api/sql/serverkeys/get)|Obtient une clé Key Vault spécifique auprès d’une instance SQL Server|
|[Afficher les clés serveur par serveur](/rest/api/sql/serverkeys/listbyserver)|Obtient la clé Key Vault pour une instance SQL Server |
|[Créer ou mettre à jour le protecteur de chiffrement](/rest/api/sql/encryptionprotectors/createorupdate)|Définit le protecteur TDE pour une instance SQL Server|
|[Obtenir le protecteur de chiffrement](/rest/api/sql/encryptionprotectors/get)|Obtient le protecteur TDE pour une instance SQL Server|
|[Afficher les protecteurs de chiffrement par serveur](/rest/api/sql/encryptionprotectors/listbyserver)|Obtient le protecteur TDE pour une instance SQL Server |
|[Créer ou mettre à jour la configuration de TDE](/rest/api/sql/transparentdataencryptions/createorupdate)|Active ou désactive TDE pour une base de données|
|[Obtenir la configuration de TDE](/rest/api/sql/transparentdataencryptions/get)|Obtient la configuration TDE pour une base de données|
|[Afficher le résultat de la configuration de TDE](/rest/api/sql/transparentdataencryptionactivities/ListByConfiguration)|Obtient le résultat du chiffrement pour une base de données|

## <a name="next-steps"></a>Étapes suivantes

- Pour obtenir une description générale de TDE, consultez [Transparent data encryption](transparent-data-encryption.md).
- Pour en savoir plus sur TDE avec prise en charge de BYOK pour Azure SQL Database et Data Warehouse, consultez [Transparent Data Encryption avec prise en charge de BYOK](transparent-data-encryption-byok-azure-sql.md).
- Pour commencer à utiliser TDE avec prise en charge de BYOK, consultez le guide pratique [Activer Transparent Data Encryption avec votre propre clé provenant de Key Vault en utilisant PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).
- Pour plus d’informations sur Key Vault, consultez la [page de documentation de Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
