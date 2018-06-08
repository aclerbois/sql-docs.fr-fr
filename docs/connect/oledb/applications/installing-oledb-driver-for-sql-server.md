---
title: L’installation du pilote OLE DB pour SQL Server | Documents Microsoft
description: Installation et désinstallation du pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb|applications
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, uninstalling
- MSOLEDBSQL, installing
- MSOLEDBSQL, uninstalling
- Setup [OLE DB Driver for SQL Server]
- uninstalling OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server], uninstalling OLE DB Driver for SQL Server
- installing OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server, installing
- data access [OLE DB Driver for SQL Server], installing OLE DB Driver for SQL Server
- removing OLE DB Driver for SQL Server
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: 0cbe9e82dac58e3ba4d15f4a608d914c73987af1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-ole-db-driver-for-sql-server"></a>Installation du pilote OLE DB pour SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

Pour installer le pilote OLE DB pour SQL Server, vous devez les installer msoledbsql.msi.
Exécutez le programme d’installation et effectuez vos sélections par défaut. Le pilote OLE DB pour SQL Server peut être installé côte à côte avec les versions antérieures des fournisseurs OLE DB de Microsoft.

Le pilote OLE DB pour SQL Server (msoledbsql.dll, msoledbsqlr.rll) sont installés dans `%SYSTEMROOT%\system32\` . En outre, le x64 msoledbsql.msi installe les fichiers binaires de 32 bits dans `%SYSTEMROOT%\SysWOW64\`.

> [!NOTE]  
> Tous les paramètres de Registre appropriés pour le pilote OLE DB pour SQL Server sont effectuées dans le cadre du processus d’installation.  

Le pilote OLE DB pour les fichiers d’en-tête et de la bibliothèque SQL Server (msoledbsql.h et msoledbsql.lib) sont installés dans `%PROGRAMFILES%\Microsoft SQL Server\Client SDK\OLEDB\180\SDK`. En outre, le x64 msoledbsql.msi installe les fichiers de mêmes dans `%PROGRAMFILES(x86)%\Microsoft SQL Server\Client SDK\OLEDB\180\SDK`.  

Vous pouvez distribuer le pilote OLE DB pour SQL Server via msoledbsql.msi. Vous devrez peut-être installer le pilote OLE DB pour SQL Server lorsque vous déployez une application. Une façon d'installer plusieurs packages dans ce qui paraît à l'utilisateur être une installation unique consiste à utiliser la technologie des programmes de chaînage et d'amorçage. Pour plus d’informations, consultez [création d’un Package de programme d’amorçage personnalisé pour Visual Studio 2005](http://go.microsoft.com/fwlink/?LinkId=115667) et [Ajout de composants requis personnalisés](http://go.microsoft.com/fwlink/?LinkId=115668).  
  
Le x64 msoledbsql.msi installe également la version 32 bits du pilote OLE DB pour SQL Server. Si votre application cible une plateforme autre que celui qu’il a été développée, vous pouvez télécharger des versions de msoledbsql.msi pour x64 et x86.

Lorsque vous appelez msoledbsql.msi, seuls les composants clients sont installés par défaut. Le client sont des composants sont des fichiers qui prennent en charge l’exécution d’une application qui a été développée à l’aide du pilote OLE DB pour SQL Server. Pour installer également les composants SDK, spécifiez `ADDLOCAL=All` dans la ligne de commande. Par exemple :  

`msiexec /i msoledbsql.msi ADDLOCAL=ALL`  

## <a name="silent-install"></a>Installation sans assistance  
 Si vous utilisez l’option, /qn, /qb ou option de /qr avec msiexec, vous devez également spécifier IACCEPTMSOLEDBSQLLICENSETERMS = Oui, pour indiquer explicitement que vous acceptez les termes du contrat de licence utilisateur final. Cette option doit être spécifiée en majuscules.  

## <a name="installing-ole-db-driver-for-sql-server-as-a-dependency"></a>Installation du pilote OLE DB pour SQL Server en tant que dépendance  
Il est important de ne pas désinstaller le pilote OLE DB pour SQL Server jusqu'à ce que toutes les applications dépendantes sont désinstallées. Pour fournir aux utilisateurs un message d’avertissement que votre application dépend de pilote OLE DB pour SQL Server, vous devez utiliser l’option d’installation APPGUID dans votre fichier MSI, comme suit :  

 `msiexec /i msoledbsql.msi APPGUID={0CC618CE-F36A-415E-84B4-FB1BFF6967E1}`  

La valeur passée à APPGUID est votre code de produit spécifique. Un code de produit doit être créé lors de l'utilisation de Microsoft Installer pour regrouper votre programme d'installation d'application.
L’option APPGUID requiert le programme d’installation en cours d’exécution à partir d’une invite de commandes avec élévation de privilèges.

## <a name="see-also"></a>Voir aussi  
 [Génération d’applications avec OLE DB Driver pour SQL Server](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)   
