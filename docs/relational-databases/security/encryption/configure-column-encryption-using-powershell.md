---
title: Configurer le chiffrement de colonne à l’aide de PowerShell | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- powershell
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 074c012b-cf14-4230-bf0d-55e23d24f9c8
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: bd835e1bffa767fba40a48fbadd3e82b7f9cf5a1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-column-encryption-using-powershell"></a>Configurer le chiffrement de colonne à l’aide de PowerShell
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Cet article fournit les étapes de définition de la configuration d’Always Encrypted cible pour les colonnes de base de données à l’aide de l’applet de commande [Set-SqlColumnEncryption](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption) (dans le module PowerShell *SqlServer* ). L’applet de commande **Set-SqlColumnEncryption** modifie le schéma de la base de données cible et les données stockées dans les colonnes sélectionnées. Les données stockées dans une colonne peuvent être chiffrées, rechiffrées ou déchiffrées, en fonction des paramètres de chiffrement cible spécifiés pour les colonnes et de la configuration de chiffrement active.
Pour plus d’informations sur la prise en charge d’Always Encrypted dans le module SqlServer PowerShell, consultez [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).

## <a name="prerequisites"></a>Conditions préalables requises

Pour définir la configuration de chiffrement cible, vous devez vérifier ce qui suit :
- Une clé de chiffrement de colonne est configurée dans la base de données (si vous chiffrez ou rechiffrez une colonne). Pour plus d’informations, consultez [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md).
- Vous pouvez accéder à la clé principale de colonne pour chaque colonne que vous souhaitez chiffrer, rechiffrer ou déchiffrer, à partir de l’ordinateur qui exécute les applets de commande PowerShell. 

## <a name="performance-and-availability-considerations"></a>Considérations relatives à la disponibilité et aux performances

Pour appliquer les paramètres de chiffrement cibles spécifiés pour la base de données, l’applet de commande **Set-SqlColumnEncryption** télécharge en toute transparence toutes les données des colonnes contenant les tables cibles, charge les données dans un ensemble de tables temporaires (avec les paramètres cibles chiffrés), et remplace les tables d’origine par les nouvelles versions des tables. Le fournisseur de données .NET Framework pour SQL Server sous-jacent chiffre et/ou déchiffre les données lors du téléchargement et/ou du chargement, selon la configuration de chiffrement actuelle de la colonne cible et les paramètres de chiffrement cibles spécifiés pour les colonnes cibles. Le déplacement des données peut prendre un certain temps, en fonction de la taille des données dans les tables concernées et de la bande passante réseau.

L’applet de commande **Set-SqlColumnEncryption** prend en charge deux approches pour paramétrer la configuration de chiffrement cible : en ligne et hors connexion.

Avec l’approche hors connexion, les tables cibles (et toutes les tables liées aux tables cibles, par exemple toutes les tables avec lesquelles une table cible a des relations de clés étrangères) ne sont pas disponibles pour l’écriture des transactions pendant toute la durée de l’opération. La sémantique des contraintes de clé étrangère (**CHECK** ou **NOCHECK**) est toujours conservée quand vous utilisez l’approche hors connexion.

Avec l’approche en ligne (nécessite le module SqlServer PowerShell 21.x ou version ultérieure), l’opération de copie et de chiffrement, de déchiffrement ou de rechiffrement des données est effectuée de manière incrémentielle. Les applications peuvent lire et écrire des données à partir de et vers les tables cibles tout au long de l’opération de déplacement des données, à l’exception de la dernière itération, dont la durée est limitée par le paramètre **MaxDownTimeInSeconds** (vous pouvez le définir). Pour détecter et traiter les modifications que les applications peuvent effectuer pendant la copie des données, l’applet de commande active le [suivi des modifications](../../track-changes/enable-and-disable-change-tracking-sql-server.md) dans la base de données cible. Par conséquent, l’approche en ligne consomme plus de ressources sur le serveur que l’approche hors connexion. L’opération peut également prendre beaucoup plus de temps avec l’approche en ligne, en particulier si une charge de travail avec d’importantes opérations d’écriture ne s’exécute sur la base de données. L’approche en ligne peut être utilisée pour chiffrer une table à la fois et la table doit avoir une clé primaire. Par défaut, les contraintes de clé étrangère sont recréées avec l’option **NOCHECK** afin de minimiser l’impact sur les applications. Vous pouvez forcer la conservation de la sémantique des contraintes de clé étrangère en spécifiant l’option **KeepCheckForeignKeyConstraints**.

Vous trouverez ici des recommandations pour choisir entre l’approche en ligne ou l’approche hors connexion :

Utilisez l’approche hors connexion :
- pour réduire la durée de l’opération ; 
- pour chiffrer/déchiffrer/chiffrer de nouveau des colonnes de plusieurs tables en même temps ;
- Si le tableau cible n’a pas de clé primaire

Utilisez l’approche en ligne :
- Pour réduire le temps d’arrêt/l’indisponibilité de la base de données pour vos applications

## <a name="security-considerations"></a>Considérations relatives à la sécurité

L’applet de commande **Set-SqlColumnEncryption** , qui permet de configurer le chiffrement pour des colonnes de base de données, gère les clés Always Encrypted et les données stockées dans des colonnes de base de données. Ainsi, il est important que vous exécutiez cette applet de commande sur un ordinateur sécurisé. Si votre base de données est dans SQL Server, exécutez l’applet de commande à partir d’un ordinateur autre que celui qui héberge votre instance de SQL Server. L’objectif principal d’Always Encrypted étant de garantir la sécurité des données sensibles chiffrées même si le système de base de données est compromis, l’exécution d’un script PowerShell qui traite des clés et/ou des données sensibles sur l’ordinateur SQL Server peut réduire ou annuler les avantages de la fonctionnalité.

Tâche  |Article  |Accède au magasin de clés/aux clés en texte brut  |Accède à la base de données   
---|---|---|---
Étape 1. Démarrer un environnement PowerShell et importer le module SqlServer. | [Importer le module SQL Server](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | non | non
Étape 2. Se connecter à votre serveur et à la base de données. | [Connexion à une base de données](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | non | Oui
Étape 3. S’authentifier auprès d’Azure, si votre clé principale de colonne (qui protège la clé de chiffrement de colonne, soumise à permutation) est stockée dans Azure Key Vault. | [Add-SqlAzureAuthenticationContext](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Oui | non
Étape 4. Créer un tableau d’objets SqlColumnEncryptionSettings : un pour chaque colonne de base de données que vous souhaitez chiffrer, rechiffrer ou déchiffrer. SqlColumnMasterKeySettings est un objet qui existe en mémoire (dans PowerShell). Il spécifie le schéma de chiffrement cible pour une colonne. | [New-SqlColumnEncryptionSettings](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings) | non | non
Étape 5. Définir la configuration de chiffrement souhaitée, spécifiée dans le tableau d’objets SqlColumnMasterKeySettings que vous avez créé à l’étape précédente. Une colonne est chiffrée, rechiffrée ou déchiffrée en fonction des paramètres cible spécifiés et de la configuration de chiffrement active de la colonne.| [Set-SqlColumnEncryption](https://docs.microsoft.com/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)<br><br>**Remarque :** cette étape peut prendre longtemps. Vos applications ne pourront pas accéder aux tables pendant toute la durée de l’opération ou pendant une partie de l’opération, en fonction de l’approche sélectionnée (en ligne ou hors ligne). | Oui | Oui

## <a name="encrypt-columns-using-offline-approach---example"></a>Chiffrer des colonnes à l’aide de l’approche hors connexion - Exemple

L’exemple ci-dessous illustre la définition de la configuration de chiffrement cible pour deux colonnes.  Si l’une des colonnes n’est pas encore chiffrée, son chiffrement est effectué. Si l’une des colonnes est déjà chiffrée à l’aide d’une clé et/ou d’un type de chiffrement différent, elle est déchiffrée puis rechiffrée avec la clé et/ou le type cible spécifié.


```
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

# Encrypt the selected columns (or re-encrypt, if they are already encrypted using keys/encrypt types, different than the specified keys/types.
$ces = @()
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.SSN" -EncryptionType "Deterministic" -EncryptionKey "CEK1"
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.BirthDate" -EncryptionType "Randomized" -EncryptionKey "CEK1"
Set-SqlColumnEncryption -InputObject $database -ColumnEncryptionSettings $ces -LogFileDirectory .
```
 
## <a name="encrypt-columns-using-online-approach---example"></a>Chiffrer des colonnes à l’aide de l’approche en ligne - Exemple

L’exemple ci-dessous illustre la définition de la configuration de chiffrement cible pour deux colonnes via l’approche en ligne. Si l’une des colonnes n’est pas encore chiffrée, son chiffrement est effectué. Si l’une des colonnes est déjà chiffrée à l’aide d’une clé et/ou d’un type de chiffrement différent, elle est déchiffrée puis rechiffrée avec la clé et/ou le type cible spécifié.

```
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

# Encrypt the selected columns (or re-encrypt, if they are already encrypted using keys/encrypt types, different than the specified keys/types.
$ces = @()
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.SSN" -EncryptionType "Deterministic" -EncryptionKey "CEK1"
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.BirthDate" -EncryptionType "Randomized" -EncryptionKey "CEK1"
Set-SqlColumnEncryption -InputObject $database -ColumnEncryptionSettings $ces -UseOnlineApproach -MaxDowntimeInSeconds 180 -LogFileDirectory .
```
## <a name="decrypt-columns---example"></a>Déchiffrer des colonnes - Exemple

L’exemple suivant montre comment déchiffrer toutes les colonnes qui sont actuellement chiffrées dans une base de données.


```
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

# Find all encrypted columns, and create a SqlColumnEncryptionSetting object for each column.
$ces = @()
$tables = $database.Tables
for($i=0; $i -lt $tables.Count; $i++){
    $columns = $tables[$i].Columns
    for($j=0; $j -lt $columns.Count; $j++) {
        if($columns[$j].isEncrypted) {
            $threeColPartName = $tables[$i].Schema + "." + $tables[$i].Name + "." + $columns[$j].Name 
            $ces += New-SqlColumnEncryptionSettings -ColumnName $threeColPartName -EncryptionType "Plaintext" 
        }
    }
}

# Decrypt all columns.
Set-SqlColumnEncryption -ColumnEncryptionSettings $ces -InputObject $database -LogFileDirectory .
```
 

## <a name="additional-resources"></a>Ressources supplémentaires
- [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
- [Always Encrypted (moteur de base de données)](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)



