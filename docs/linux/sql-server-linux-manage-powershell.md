---
title: Gérer SQL Server sur Linux avec PowerShell | Documents Microsoft
description: Cet article fournit une vue d’ensemble de l’utilisation de PowerShell sur Windows avec SQL Server sur Linux.
author: rothja
ms.author: jroth
manager: craigg
ms.date: 10/02/2017
ms.topic: article
ms.prod: sql
ms.component: ''
ms.suite: sql
ms.custom: sql-linux
ms.technology: linux
ms.assetid: a3492ce1-5d55-4505-983c-d6da8d1a94ad
ms.openlocfilehash: 54892a66cea7643d708bcd7ba5cf1b6b07d4523f
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="use-powershell-on-windows-to-manage-sql-server-on-linux"></a>Utiliser PowerShell sur Windows pour gérer SQL Server sur Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Cet article présente [SQL Server PowerShell](https://msdn.microsoft.com/en-us/library/mt740629.aspx) et présente quelques exemples sur la façon d’utiliser avec 2017 du serveur SQL sur Linux. Prise en charge de PowerShell pour SQL Server est actuellement disponible sur Windows, vous pouvez l’utiliser lorsque vous disposez d’un ordinateur Windows qui peut se connecter à une instance distante de SQL Server sur Linux.

## <a name="install-the-newest-version-of-sql-powershell-on-windows"></a>Installer la version la plus récente de SQL PowerShell sur Windows

[SQL PowerShell](https://msdn.microsoft.com/en-us/library/mt740629.aspx) sur Windows est inclus avec [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md). Lorsque vous travaillez avec SQL Server, vous devez toujours utiliser la version la plus récente de SSMS et SQL PowerShell. La dernière version de SSMS est continuellement mis à jour et optimisées et est actuellement compatible avec SQL Server 2017 Linux on. Pour télécharger et installer la version la plus récente, consultez [télécharger SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md). Pour rester à jour, la dernière version de SSMS vous invite lorsqu’il existe une nouvelle version disponible en téléchargement.

## <a name="before-you-begin"></a>Avant de commencer

Lecture la [problèmes connus](sql-server-linux-release-notes.md) pour SQL Server 2017 sur Linux.

## <a name="launch-powershell-and-import-the-sqlserver-module"></a>Lancez PowerShell et importer le *sqlserver* module

Nous pouvons commencer en lançant PowerShell sur Windows. Ouvrir un *invite de commandes* sur votre ordinateur Windows et le type **PowerShell** pour lancer une nouvelle session Windows PowerShell.

```
PowerShell
```

SQL Server fournit un module Windows PowerShell nommé **SqlServer** que vous pouvez utiliser pour importer les composants de SQL Server (fournisseur SQL Server et applets de commande) dans un environnement PowerShell ou un script.

Copiez et collez la commande suivante à l’invite de PowerShell pour importer le **SqlServer** module dans votre session PowerShell actuelle :

```powershell
Import-Module SqlServer
```

Tapez la commande suivante à l’invite de PowerShell pour vérifier que le **SqlServer** module a été importé correctement :

```powershell
Get-Module -Name SqlServer
```

PowerShell doit afficher des informations similaires à la sortie suivante :

```
ModuleType Version    Name          ExportedCommands
---------- -------    ----          ----------------
Script     0.0        SqlServer
Manifest   20.0       SqlServer     {Add-SqlAvailabilityDatabase, Add-SqlAvailabilityGroupList...
```

## <a name="connect-to-sql-server-and-get-server-information"></a>Se connecter à SQL Server et obtenir des informations sur le serveur

Nous allons utiliser PowerShell sur Windows pour vous connecter à votre instance de SQL Server 2017 sur Linux et afficher plusieurs propriétés du serveur.

Copiez et collez les commandes suivantes à l’invite de PowerShell. Lorsque vous exécutez ces commandes, PowerShell va :
- Afficher le *demande des informations d’identification Windows PowerShell* boîte de dialogue qui vous demande les informations d’identification (*nom d’utilisateur SQL* et *mot de passe SQL*) pour se connecter à votre 2017 de serveur SQL instance sur Linux
- Charger l’assembly de la gestion des objets SMO (SQL Server)
- Créez une instance de la [Server](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.management.smo.server.aspx) objet
- Se connecter à la **Server** et quelques propriétés d’affichage

N’oubliez pas de remplacer **\<your_server_instance\>** avec l’adresse IP ou le nom d’hôte de votre instance de SQL Server 2017 sur Linux.

```powershell
# Prompt for credentials to login into SQL Server
$serverInstance = "<your_server_instance>"
$credential = Get-Credential

# Load the SMO assembly and create a Server object
[System.Reflection.Assembly]::LoadWithPartialName('Microsoft.SqlServer.SMO') | out-null
$server = New-Object ('Microsoft.SqlServer.Management.Smo.Server') $serverInstance

# Set credentials
$server.ConnectionContext.LoginSecure=$false
$server.ConnectionContext.set_Login($credential.UserName)
$server.ConnectionContext.set_SecurePassword($credential.Password)

# Connect to the Server and get a few properties
$server.Information | Select-Object Edition, HostPlatform, HostDistribution | Format-List
# done
```

PowerShell doit afficher des informations similaires à la sortie suivante :

```
Edition          : Developer Edition (64-bit)
HostPlatform     : Linux
HostDistribution : Ubuntu
```
> [!NOTE]
> Si rien n’est affiché pour ces valeurs, la connexion à l’instance de SQL Server cible est plus probablement échoué. Assurez-vous que vous pouvez utiliser les mêmes informations de connexion pour se connecter à partir de SQL Server Management Studio. Passez ensuite en revue les [recommandations en matière de résolution des problèmes de connexion](sql-server-linux-troubleshooting-guide.md#connection).

## <a name="examine-sql-server-error-logs"></a>Examinez les journaux d’erreurs SQL Server

Nous allons utiliser PowerShell sur Windows pour examiner les journaux d’erreurs connect sur votre instance de SQL Server 2017 sur Linux. Nous utiliserons le **Out-GridView** ouvre une applet de commande pour afficher des informations à partir de l’erreur dans un affichage de la vue grille.

Copiez et collez les commandes suivantes à l’invite de PowerShell. Il peuvent prendre quelques minutes pour s’exécuter. Ces commandes comme suit :
- Afficher le *demande des informations d’identification Windows PowerShell* boîte de dialogue qui vous demande les informations d’identification (*nom d’utilisateur SQL* et *mot de passe SQL*) pour se connecter à votre 2017 de serveur SQL instance sur Linux
- Utilisez le **Get-SqlErrorLog** ouvre une applet de commande pour vous connecter à l’instance de SQL Server 2017 sur Linux et récupérer l’erreur depuis **hier**
- Dirigez la sortie vers le **Out-GridView** applet de commande

N’oubliez pas de remplacer **\<your_server_instance\>** avec l’adresse IP ou le nom d’hôte de votre instance de SQL Server 2017 sur Linux.

```powershell
# Prompt for credentials to login into SQL Server
$serverInstance = "<your_server_instance>"
$credential = Get-Credential

# Retrieve error logs since yesterday
Get-SqlErrorLog -ServerInstance $serverInstance -Credential $credential -Since Yesterday | Out-GridView
# done
```
## <a name="see-also"></a>Voir aussi
- [SQL Server PowerShell](../relational-databases/scripting/sql-server-powershell.md)
