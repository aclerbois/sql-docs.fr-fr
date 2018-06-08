---
title: À l’aide de l’en-tête SQL Server Native Client et les fichiers de bibliothèque | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|applications
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- header files [SQL Server Native Client]
- SQLNCLI, header files
- OLE DB, header files
- library files [SQL Server Native Client]
- SQL Server Native Client, header files
- data access [SQL Server Native Client], header files
- SQL Server Native Client ODBC driver,header files
- data access [SQL Server Native Client], library files
- SQL Server Native Client, library files
- ODBC applications, header files
- SQLNCLI, library files
ms.assetid: 69889a98-7740-4667-aecd-adfc0b37f6f0
caps.latest.revision: 63
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e7fcb33012d766c2585e11468e23a3591a4b2650
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-the-sql-server-native-client-header-and-library-files"></a>Utilisation des fichiers bibliothèques et de l'en-tête SQL Server Native Client
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Les fichiers bibliothèque et d'en-tête [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client sont installés avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Lors du développement d'une application, il importe de copier et d'installer tous les fichiers requis pour le développement sur votre environnement de développement. Pour plus d’informations sur l’installation et la redistribution [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, consultez [l’installation de SQL Server Native Client](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md).  
  
 Les fichiers bibliothèque et d'en-tête [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client sont installés à l'emplacement suivant :  
  
 *%Program Files%* \Microsoft SQL Server\110\SDK  
  
 Le fichier d'en-tête [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (sqlncli.h) peut être utilisé pour ajouter la fonctionnalité d'accès aux données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client à vos applications personnalisées. Le fichier d'en-tête [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client contient l'ensemble des définitions, attributs, propriétés et interfaces nécessaires pour tirer parti des nouvelles fonctionnalités introduites dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
 En plus du fichier d'en-tête [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, il existe également un fichier bibliothèque sqlncli11.lib qui est la bibliothèque d'exportation pour la fonctionnalité BCP (Bulk Copy Program) [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour ODBC.  
  
 Le fichier d'en-tête [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client offre une compatibilité descendante avec les fichiers d'en-tête sqloledb.h et odbcss.h utilisés avec MDAC (Microsoft Data Access Components), mais ne contient pas les CLSID pour SQLOLEDB (le fournisseur OLE DB pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] inclus avec MDAC) ou les symboles pour la fonctionnalité XML (non prise en charge par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client).  
  
 Les applications ODBC ne peuvent pas faire référence à l'en-tête [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (sqlncli.h) et à odbcss.h dans le même programme. Même si vous n'utilisez aucune des fonctionnalités introduites dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], le fichier d'en-tête [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client fonctionnera à la place du fichier odbcss.h plus ancien.  
  
 Les applications OLE DB qui utilisent le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client doivent uniquement référencer sqlncli.h. Si une application utilise MDAC (SQLOLEDB) et le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, elle peut faire référence à sqloledb.h et à sqlncli.h, mais la référence à sqloledb.h doit être placée en premier.  
  
## <a name="using-the-sql-server-native-client-header-file"></a>Utilisation du fichier d'en-tête SQL Server Native Client  
 Pour utiliser le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fichier d’en-tête Native Client, vous devez utiliser un **incluent** instruction dans votre code de programmation C/C++. Les sections suivantes décrivent comment y parvenir pour les applications OLE DB et ODBC.  
  
> [!NOTE]  
>  Les fichiers bibliothèque et d'en-tête [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ne peuvent être compilés qu'à l'aide de Visual Studio C++ 2002 ou version ultérieure.  
  
### <a name="ole-db"></a>OLE DB  
 Pour utiliser le fichier d'en-tête [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client dans une application OLE DB, à l'aide des lignes de code de programmation suivantes :  
  
```  
#define _SQLNCLI_OLEDB_  
include "sqlncli.h";  
```  
  
> [!NOTE]  
>  La première ligne de code ci-dessus doit être omise si les API OLE DB et ODBC sont utilisées par l'application. En outre, si l’application possède un **incluent** instruction pour sqloledb.h, le **incluent** instruction pour sqlncli.h doit venir après elle.  
  
 Lorsque vous créez une connexion à une source de données via [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, utilisez "SQLNCLI11" comme chaîne du nom de fournisseur.  
  
### <a name="odbc"></a>ODBC  
 Pour utiliser le fichier d'en-tête [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client dans une application ODBC, à l'aide des lignes de code de programmation suivantes :  
  
```  
#define _SQLNCLI_ODBC_  
include "sqlncli.h";  
```  
  
> [!NOTE]  
>  La première ligne de code ci-dessus doit être omise si les API OLE DB et ODBC sont utilisées par l’application. De plus, si l'application possède une instruction `#include` pour odbcss.h, elle doit être supprimée.  
  
 Lors de la création d'une connexion à une source de données via [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, utilisez "SQL Server Native Client 11.0" comme chaîne du nom de pilote.  
  
## <a name="component-names-and-properties-by-version"></a>Noms des composants et propriétés par version  
  
|Propriété|SQL Server Native Client<br /><br /> SQL Server 2005|SQL Server Native Client 10.0<br /><br /> SQL Server 2008|SQL Server Native Client 11.0<br /><br /> [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]|MDAC|  
|--------------|--------------------------------------------------|-------------------------------------------------------|---------------------------------------------------------------|----------|  
|Nom du pilote ODBC|SQL Native Client|SQL Server Native Client 10.0|SQL Server Native Client 11.0|SQL Server|  
|Nom du fichier d'en-tête ODBC|Sqlncli.h|Sqlncli.h|Sqlncli.h|Odbcss.h|  
|DLL du pilote ODBC|Sqlncli.dll|Sqlncl10.dll|Sqlncl11.dll|sqlsrv32.dll|  
|Fichier bibliothèque ODBC pour les API BCP|Sqlncli.lib|Sqlncli10.lib|Sqlncli11.lib|Odbcbcp.lib|  
|DLL ODBC pour les API BCP|Sqlncli.dll|Sqlncli10.dll|Sqlncli11.dll|Odbcbcp.dll|  
|PROGID pour OLE DB|SQLNCLI|SQLNCLI10|SQLNCLI11|SQLOLEDB|  
|Nom du fichier d'en-tête OLE DB|Sqlncli.h|Sqlncli.h|Sqlncli.h|Sqloledb.h|  
|DLL du fournisseur OLE DB|Sqlncli.dll|Sqlncli10.dll|Sqlncli11.dll|Sqloledb.dll|  
  
 sqlncli.h prend en charge plusieurs versions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client via la macro SQLNCLI_VER. Par défaut, SQLNCLI_VER a comme valeur la version la plus récente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. Pour générer une application qui utilise sqlncli10.dll plutôt que sqlncli11.dll, définissez SQLNCLI_VER avec la valeur 10.  
  
## <a name="static-linking-and-bcp-functions"></a>Liaison statique et fonctions BCP  
 Lorsqu'une application utilise des fonctions BCP, il est important pour l'application de spécifier dans la chaîne de connexion le pilote de la même version fourni avec le fichier d'en-tête et la bibliothèque utilisés pour compiler l'application.  
  
 Par exemple, si vous compilez une application à l'aide de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, le fichier de bibliothèque associé (sqlncli11.lib) et le fichier d'en-tête (sqlncli.h) à partir de \Program Files\Microsoft SQL Server\110\SDK, veillez à spécifier (en utilisant ODBC comme exemple) « DRIVER={SQL Server Native Client 11.0} » dans la chaîne de connexion.  
  
 Pour plus d’informations, consultez exécution [effectuant des opérations de copie en bloc](../../../relational-databases/native-client/features/performing-bulk-copy-operations.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Génération d’Applications avec SQL Server Native Client](../../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
  
  
