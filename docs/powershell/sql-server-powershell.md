---
title: SQL Server PowerShell | Microsoft Docs
ms.custom: ''
ms.date: 08/04/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: scripting
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 89b70725-bbe7-4ffe-a27d-2a40005a97e7
caps.latest.revision: 10
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 9eeb40333c719288af19655b61fdebeb88faa370
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="sql-server-powershell"></a>SQL Server PowerShell
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

**[Installer SQL Server PowerShell](download-sql-server-ps-module.md)**

> [!NOTE]
> Il existe deux modules SQL Server PowerShell : **SqlServer** et **SQLPS**. Le module **SQLPS** fait partie de l’installation de SQL Server (à des fins de compatibilité descendante), mais il n’est plus mis à jour. Le module PowerShell le plus récent est **SqlServer**. Le module **SqlServer** contient les versions mises à jour des applets de commande disponibles dans **SQLPS**, ainsi que de nouvelles applets de commande pour prendre en charge les dernières fonctionnalités SQL.  
> Des versions précédentes du module **SqlServer** *étaient* fournies avec SQL Server Management Studio (SSMS), mais uniquement avec les versions 16.x de SSMS. Pour utiliser PowerShell avec SSMS 17.0 et ultérieur, vous devez installer le module **SqlServer** à partir de PowerShell Gallery.
> Pour installer le module **SqlServer**, consultez [Installer SQL Server PowerShell](download-sql-server-ps-module.md).

**Pourquoi le module SQLPS s’appelle-t-il désormais SqlServer ?**

Pour pouvoir expédier les mises à jour SQL PowerShell, nous avons dû changer l’identité du module SQL PowerShell et le wrapper appelé *SQLPS.exe*. En raison de ce changement, il existe désormais deux modules SQL PowerShell : **SqlServer** et **SQLPS**.  

**Mettez à jour vos scripts PowerShell s’ils importent le module SQLPS.**

Si vous avez des scripts PowerShell qui exécutent `Import-Module -Name SQLPS` et que vous souhaitez tirer parti des fonctionnalités du nouveau fournisseur et des nouvelles applets de commande, vous devez utiliser à la place `Import-Module -Name SqlServer`. Le nouveau module est installé dans le dossier `%ProgramFiles%\WindowsPowerShell\Modules\SqlServer`. Il est donc inutile de mettre à jour la variable $env:PSModulePath. Si vous avez des scripts qui utilisent une version de communauté ou tierce d’un module nommé **SqlServer**, utilisez le paramètre Prefix pour éviter toute collision de noms. Le module utilisé par SQL Server Agent est le même. 

  
## <a name="sql-server-powershell-components"></a>Composants de SQL Server PowerShell  
Le module **SqlServer** charge deux composants logiciels enfichables Windows PowerShell :  
  
-   Un fournisseur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , qui active un mécanisme de navigation simple semblable aux chemins d'accès de système de fichiers. Vous pouvez générer des chemins d'accès semblables aux chemins d'accès de système de fichiers, où le lecteur est associé à un modèle SMO ( [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Object) et les nœuds sont basés sur les classes du modèle objet. Vous pouvez ensuite utiliser des commandes familières telles que **cd** et **dir** pour naviguer parmi les chemins d’accès de la même façon que vous naviguez parmi des dossiers dans une fenêtre d’invite de commandes. Vous pouvez utiliser d’autres commandes, telles que **ren** ou **del**, pour exécuter des actions sur les nœuds du chemin d’accès.  
  
-   Un jeu d’applets de commande qui prennent en charge des actions telles que l’exécution d’un script **sqlcmd** contenant des instructions [!INCLUDE[tsql](../includes/tsql-md.md)] ou XQuery.  
  
  
## <a name="sql-server-versions"></a>versions SQL Server  
Les applets de commande SQL PowerShell peuvent servir à gérer des instances d’Azure SQL Database, d’Azure SQL Data Warehouse et de tous les [produits SQL Server pris en charge](https://support.microsoft.com/lifecycle/search/1044).  


## <a name="sql-server-identifiers-that-contain-characters-not-supported-in-powershell-paths"></a>Identificateurs SQL Server contenant des caractères non pris en charge dans les chemins PowerShell  
 
Les applets de commande **Encode-Sqlname** et **Decode-Sqlname** vous aident à spécifier les identificateurs SQL Server qui contiennent des caractères non pris en charge dans les chemins PowerShell. Pour plus d’informations, consultez [Identificateurs SQL Server dans PowerShell](sql-server-identifiers-in-powershell.md).  
  
Utilisez l’applet de commande **Convert-UrnToPath** pour convertir un nom de ressource unique pour un objet du [!INCLUDE[ssDE](../includes/ssde-md.md)] en un chemin pour le fournisseur PowerShell SQL Server. Pour plus d’informations, consultez [Convertir des URN en chemins d’accès de fournisseur SQL Server](https://docs.microsoft.com/powershell/module/sqlserver/Convert-UrnToPath).  
  
## <a name="query-expressions-and-unique-resource-names"></a>Expressions de requête et noms de ressource uniques  

Les expressions de requête sont des chaînes qui utilise une syntaxe semblable à XPath pour spécifier un jeu de critères permettant d'énumérer un ou plusieurs objets dans une hiérarchie de modèle objet. Un nom de ressource unique (URN) est un type spécifique de chaîne d'expression de requête qui identifie de façon unique un objet particulier. Pour plus d’informations, consultez [Expressions de requête et noms URN](query-expressions-and-uniform-resource-names.md).       


## <a name="cmdlet-reference"></a>Informations de référence sur les applets de commande
* [Applets de commande SqlServer](https://docs.microsoft.com/powershell/module/sqlserver)
* [Applets de commande SQLPS](https://docs.microsoft.com/powershell/module/sqlps)
