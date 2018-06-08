---
title: Identificateurs SQL Server dans PowerShell | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: scripting
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Cmdlets [SQL Server], Encode-Sqlname
- PowerShell [SQL Server], identifiers
- Encode-Sqlname cmdlet
- PowerShell [SQL Server], Encode-Sqlname
- Decode-Sqlname cmdlet
- PowerShell [SQL Server], Decode-Sqlname
- identifiers [SQL Server], PowerShell
- Cmdlets [SQL Server], Decode-Sqlname
ms.assetid: 651099b0-33b4-453a-a864-b067f21eb8b9
caps.latest.revision: 24
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2df709e52d78a9d1b6401bfff0dc6e8ecf65148e
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="sql-server-identifiers-in-powershell"></a>Identificateurs SQL Server dans PowerShell
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Le fournisseur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour Windows PowerShell utilise les identificateurs [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dans les chemins Windows PowerShell. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Les identificateurs peuvent contenir des caractères que Windows PowerShell ne prend pas en charge dans les chemins. Vous devez placer ces caractères dans une séquence d’échappement ou leur appliquer un codage spécial lors de l’utilisation des identificateurs dans les chemins Windows PowerShell.  
  
> [!NOTE]
> Il existe deux modules SQL Server PowerShell : **SqlServer** et **SQLPS**. Le module **SQLPS** fait partie de l’installation de SQL Server (à des fins de compatibilité descendante), mais il n’est plus mis à jour. Le module PowerShell le plus récent est **SqlServer**. Le module **SqlServer** contient les versions mises à jour des applets de commande disponibles dans **SQLPS**, ainsi que de nouvelles applets de commande pour prendre en charge les dernières fonctionnalités SQL.  
> Des versions précédentes du module **SqlServer** *étaient* fournies avec SQL Server Management Studio (SSMS), mais uniquement avec les versions 16.x de SSMS. Pour utiliser PowerShell avec SSMS 17.0 et ultérieur, vous devez installer le module **SqlServer** à partir de PowerShell Gallery.
> Pour installer le module **SqlServer**, consultez [Installer SQL Server PowerShell](download-sql-server-ps-module.md).


## <a name="sql-server-identifiers-in-windows-powershell-paths"></a>Identificateurs SQL Server dans les chemins d'accès Windows PowerShell  
 Les fournisseurs Windows PowerShell présentent les hiérarchies de données à l’aide d’une structure de chemin semblable à celle du système de fichiers Windows. Le fournisseur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] implémente les chemins d'accès aux objets [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Pour le [!INCLUDE[ssDE](../includes/ssde-md.md)], le lecteur est défini sur SQLSERVER:, le premier dossier est défini sur \SQL et les objets de base de données sont référencés sous forme de conteneurs et d’éléments. Voici le chemin de la table Vendor dans le schéma Purchasing de la base de données [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] dans une instance par défaut du [!INCLUDE[ssDE](../includes/ssde-md.md)]:  
  
```  
SQLSERVER:\SQL\MyComputer\DEFAULT\Databases\AdventureWorks2012\Tables\Purchasing.Vendor  
```  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Les identificateurs sont les noms des objets [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , tels que des noms de tables ou de colonnes. Il existe deux types d'identificateurs [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] :  
  
-   Les identificateurs standard sont limités à un jeu des caractères qui sont également pris en charge dans les chemins d'accès Windows PowerShell. Ces noms peuvent être utilisés dans les chemins d'accès Windows PowerShell sans être modifiés.  
  
-   Les identificateurs délimités peuvent utiliser des caractères non pris en charge dans les noms de chemins d'accès Windows PowerShell. Ils sont appelés « identificateurs délimités » s'ils sont placés entre crochets et ([NomIdentificateur]) et « identificateurs entre guillemets » s'ils sont placés entre les guillemets ("NomIdentificateur"). Si un identificateur délimité utilise des caractères non pris en charge dans les chemins d'accès Windows PowerShell, les caractères doivent être codés ou placés dans une séquence d'échappement avant d'utiliser l'identificateur comme conteneur ou nom d'élément. L'encodage fonctionne pour tous les caractères. Certains caractères, comme les deux-points (:), ne peuvent pas être placés dans une séquence d'échappement.  
  
## <a name="sql-server-identifiers-in-cmdlets"></a>Identificateurs SQL Server dans les applets de commande  
 Certaines applets de commande [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] contiennent un paramètre dont l'entrée est un identificateur. Les valeurs de paramètres sont généralement fournies sous forme de constantes de chaîne entre guillemets ou dans des variables chaîne. Lorsque les identificateurs sont fournis sous forme de constantes de chaîne ou dans des variables, il n'y a aucun conflit avec l'ensemble des caractères pris en charge par Windows PowerShell.  
  
## <a name="sql-server-identifier-tasks"></a>Tâches des identificateurs SQL Server  
  
|Description de la tâche|Article|  
|----------------------|-----------|  
|Décrit comment spécifier un nom d'instance, y compris le nom de l'ordinateur sur lequel l'instance s'exécute.|[Spécifier des instances dans le fournisseur SQL Server PowerShell](specify-instances-in-the-sql-server-powershell-provider.md)|  
|Décrit comment spécifier le codage hexadécimal des caractères dans les identificateurs délimités qui ne sont pas pris en charge dans les chemins d'accès Windows PowerShell. Décrit également comment décoder les caractères hexadécimaux.|[Encoder et décoder des identificateurs SQL Server](encode-and-decode-sql-server-identifiers.md)|  
|Décrit comment utiliser le caractère d'échappement Windows PowerShell pour les caractères non pris en charge dans les chemins d'accès PowerShell.|[Placer des identificateurs SQL Server dans une séquence d'échappement](escape-sql-server-identifiers.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [fournisseur PowerShell SQL Server](sql-server-powershell-provider.md)   
 [SQL Server PowerShell](sql-server-powershell.md)   
 [Identificateurs de base de données](../relational-databases/databases/database-identifiers.md)  
  
  
