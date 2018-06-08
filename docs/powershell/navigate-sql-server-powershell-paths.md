---
title: Parcourir les chemins PowerShell SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: scripting
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d68aca48-d161-45ed-9f4f-14122ed30218
caps.latest.revision: 8
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 37be2c3dc4c2307cea99164a5fa955dfbbfdd6d9
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="navigate-sql-server-powershell-paths"></a>Parcourir les chemins PowerShell SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Le fournisseur PowerShell du [!INCLUDE[ssDE](../includes/ssde-md.md)] expose le jeu d'objets dans une instance de SQL Server dans une structure similaire à un chemin d'accès de fichier. Vous pouvez utiliser des applets de commande Windows PowerShell pour naviguer jusqu'au chemin d'accès du fournisseur et créer des lecteurs personnalisés pour raccourcir le chemin d'accès que vous devez taper.  

> [!NOTE]
> Il existe deux modules SQL Server PowerShell : **SqlServer** et **SQLPS**. Le module **SQLPS** fait partie de l’installation de SQL Server (à des fins de compatibilité descendante), mais il n’est plus mis à jour. Le module PowerShell le plus récent est **SqlServer**. Le module **SqlServer** contient les versions mises à jour des applets de commande disponibles dans **SQLPS**, ainsi que de nouvelles applets de commande pour prendre en charge les dernières fonctionnalités SQL.  
> Des versions précédentes du module **SqlServer** *étaient* fournies avec SQL Server Management Studio (SSMS), mais uniquement avec les versions 16.x de SSMS. Pour utiliser PowerShell avec SSMS 17.0 et ultérieur, vous devez installer le module **SqlServer** à partir de PowerShell Gallery.
> Pour installer le module **SqlServer**, consultez [Installer SQL Server PowerShell](download-sql-server-ps-module.md).
  
Windows PowerShell implémente des applets de commande pour parcourir la structure de chemin d'accès qui représentent la hiérarchie des objets pris en charge par un fournisseur PowerShell. Une fois que vous avez accédé à un nœud dans le chemin d'accès, vous pouvez utiliser d'autres applets de commande pour exécuter des opérations de base sur l'objet actif. Étant donné que les applets de commande sont fréquemment utilisées, elles ont des alias canoniques courts. Il existe également un ensemble d'alias qui mappent les applets de commande à des commandes semblables destinées à l'invite de commandes, et un autre ensemble pour les commandes de l'interpréteur de commandes UNIX.  
  
 Le fournisseur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] implémente un sous-ensemble des applets de commande du fournisseur, comme indiqué dans le tableau suivant :  
  
|Applet de commande|Alias canonique|Alias d'applet de commande|Alias de l'interpréteur de commandes UNIX|Description|  
|------------|---------------------|---------------|----------------------|-----------------|  
|**Get-Location**|**gl**|**pwd**|**pwd**|Obtient le nœud actuel.|  
|**Set-Location**|**sl**|**cd, chdir**|**cd, chdir**|Modifie le nœud actuel.|  
|**Get-ChildItem**|**gci**|**dir**|**ls**|Répertorie les objets stockés sur le nœud actuel.|  
|**Get-Item**|**gi**|||Retourne les propriétés de l'élément actif.|  
|**Rename-Item**|**rni**|**rn**|**ren**|Renomme un objet.|  
|**Remove-Item**|**ri**|**del, rd**|**rm, rmdir**|Supprime un objet.|  
  
> [!IMPORTANT]  
>  Certains identificateurs (noms d'objets) [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] contiennent des caractères que Windows PowerShell ne prend pas en charge dans les noms de chemins d'accès. Pour plus d'informations sur l'utilisation de noms qui contiennent ces caractères, consultez [SQL Server Identifiers in PowerShell](sql-server-identifiers-in-powershell.md).  
  
### <a name="sql-server-information-returned-by-get-childitem"></a>Informations de SQL Server retournées par Get-ChildItem  
 Les informations retournées par **Get-ChildItem** (ou ses alias **dir** et **ls** ) dépendent de votre emplacement dans un chemin SQLSERVER:.  
  
|Emplacement de chemin d'accès|Résultats de Get-ChildItem|  
|-------------------|----------------------------|  
|SQLSERVER:\SQL|Retourne le nom de l'ordinateur local. Si vous avez utilisé SMO ou WMI pour vous connecter aux instances du [!INCLUDE[ssDE](../includes/ssde-md.md)] sur d’autres ordinateurs, ces ordinateurs sont également répertoriés.|  
|SQLSERVER:\SQL\\*nom_ordinateur*|Liste des instances du [!INCLUDE[ssDE](../includes/ssde-md.md)] sur l'ordinateur.|  
|SQLSERVER:\SQL\\*nom_ordinateur*\\*nom_instance*|Liste des types d'objets de niveau supérieur dans l'instance, tels que les points de terminaison, les certificats et les bases de données.|  
|Nœud de classes d'objets, tels que Databases|Liste des objets de ce type, telle que la liste des bases de données : MASTER, Model, AdventureWorks20008R2.|  
|Nœud de noms d’objets, comme AdventureWorks2012|Liste des types d'objets contenus dans l'objet. Par exemple, une base de données répertorierait des types d'objets tels que les tables et les vues.|  
  
 Par défaut, **Get-ChildItem** ne répertorie pas d’objets système. Utilisez le paramètre *Force* pour afficher les objets système, tels que les objets dans le schéma **sys** .  
  
### <a name="custom-drives"></a>Lecteurs personnalisés  
 Windows PowerShell permet aux utilisateurs de définir des lecteurs virtuels appelés lecteurs PowerShell, qui sont mappés aux nœuds de démarrage d’une instruction de chemin. Ils sont généralement utilisés pour raccourcir les chemins d'accès fréquemment tapés. Les chemins SQLSERVER: peuvent devenir longs, occupant beaucoup de place dans la fenêtre Windows PowerShell et nécessitant beaucoup de saisie. Si vous devez effectuer beaucoup de travail sur un nœud de chemin d'accès particulier, vous pouvez définir un lecteur Windows PowerShell personnalisé mappé à ce nœud.  
  
## <a name="use-powershell-cmdlet-aliases"></a>Utiliser des alias d'applets de commande PowerShell  
 **Utiliser un alias d'applet de commande**  
  
-   Au lieu de taper un nom d'applet de commande complet, tapez un alias plus court ou un alias mappé à une commande d'invite de commandes familière.  
  
### <a name="alias-example-powershell"></a>Exemple d'alias (PowerShell)  
 Par exemple, vous pouvez utiliser l'un des jeux d'applets de commande ou ensembles d'alias suivants pour récupérer la liste des instances [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] disponibles si vous accédez au dossier SQLSERVER:\SQL et si vous demandez la liste des éléments enfants de ce dossier :  
  
```  
## Shows using the full cmdet name.  
Set-Location SQLSERVER:\SQL  
Get-ChildItem  
  
## Shows using canonical aliases.  
sl SQLSERVER:\SQL  
gci  
  
## Shows using command prompt aliases.  
cd SQLSERVER:\SQL  
dir  
  
## Shows using Unix shell aliases.  
cd SQLSERVER:\SQL  
ls  
```  
  
## <a name="use-get-childitem"></a>Utiliser Get-ChildItem  
 **Renvoyer des informations à l'aide de Get-Childitem**  
  
1.  Accédez au nœud pour lequel vous souhaitez obtenir une liste d’enfants.  
  
2.  Exécutez Get-Childitem pour obtenir la liste.  
  
### <a name="get-childitem-example-powershell"></a>Exemple de Get-ChildItem (PowerShell)  
 Ces exemples illustrent les informations retournées par Get-Childitem pour différents nœuds dans un chemin d'accès de fournisseur SQL Server.  
  
```  
## Return the current computer and any computer  
## to which you have made a SQL or WMI connection.  
Set-Location SQLSERVER:\SQL  
Get-ChildItem  
  
## List the instances of the Database Engine on the local computer.  
  
Set-Location SQLSERVER:\SQL\localhost  
Get-ChildItem  
  
## Lists the categories of objects available in the  
## default instance on the local computer.  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT  
Get-ChildItem  
  
## Lists the databases from the local default instance.  
## The force parameter is used to include the system databases.  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
Get-ChildItem -force  
```  
  
## <a name="create-a-custom-drive"></a>Créer un lecteur personnalisé  
 **Créer et utiliser un lecteur personnalisé**  
  
1.  Utilisez **New-PSDrive** pour définir un lecteur personnalisé. Utilisez le paramètre **Root** pour spécifier le chemin d'accès représenté par le nom du lecteur personnalisé.  
  
2.  Faites référence au nom du lecteur personnalisé dans les applets de commande de navigation de chemin comme **Set-Location**.  
  
### <a name="custom-drive-example-powershell"></a>Exemple de lecteur personnalisé (PowerShell)  
 Cet exemple crée un lecteur virtuel nommé AWDB qui mappe au nœud pour une copie déployée de l’exemple de base de données AdventureWorks2012. Le lecteur virtuel est ensuite utilisé pour naviguer jusqu'à une table dans la base de données.  
  
```  
## Create a new virtual drive.  
New-PSDrive -Name AWDB -Root SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012  
  
## Use AWDB: to navigate to a specific table.  
Set-Location AWDB:\Tables\Purchasing.Vendor  
```  
  
## <a name="see-also"></a> Voir aussi  
 [fournisseur PowerShell SQL Server](sql-server-powershell-provider.md)   
 [Utiliser des chemins d'accès PowerShell SQL Server](work-with-sql-server-powershell-paths.md)   
 [SQL Server PowerShell](sql-server-powershell.md)  
  
  
