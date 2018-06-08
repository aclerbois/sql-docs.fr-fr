---
title: Configuration système requise pour le pilote OLE DB pour SQL Server | Documents Microsoft
description: Configuration requise pour le pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: oledb
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- system requirements [OLE DB Driver for SQL Server]
- data access [OLE DB Driver for SQL Server], system requirements
- OLE DB Driver for SQL Server, system requirements
- MSOLEDBSQL, system requirements
author: pmasl
ms.author: Pedro.Lopes
manager: craigg
ms.openlocfilehash: e109f228d8b902e5c34b4ed5731b80e315fffe3c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="system-requirements-for-ole-db-driver-for-sql-server"></a>Configuration système requise pour le pilote OLE DB pour SQL Server
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

[!INCLUDE[Driver_OLEDB_Download](../../includes/driver_oledb_download.md)]

  Pour utiliser les fonctionnalités d'accès aux données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par exemple MARS, les logiciels suivants doivent être installés :  

-   Pilote OLE DB pour SQL Server sur votre client.  

-   Une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur votre serveur.   

> [!NOTE]  
>  Assurez-vous que vous vous connectez avec les privilèges d'administrateur avant d'installer ce logiciel.  

## <a name="operating-system-requirements"></a>Système d'exploitation requis  
 Pour obtenir la liste des systèmes d’exploitation qui prennent en charge le pilote OLE DB pour SQL Server, consultez [prennent en charge les stratégies pour le pilote OLE DB pour SQL Server](../oledb/applications/support-policies-for-oledb-driver-for-sql-server.md).  

## <a name="sql-server-requirements"></a>Configuration SQL Server requise  
 Pour utiliser le pilote OLE DB pour SQL Server pour accéder aux données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données, vous devez avoir une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installé.  

 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] prend en charge les connexions à partir de toutes les versions de MDAC, Windows Data Access Components et toutes les versions de pilote OLE DB pour SQL Server. Lorsqu'une version cliente plus ancienne se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les types de données de serveur inconnus du client sont mappés à des types compatibles avec la version du client. Pour plus d'informations, consultez Compatibilité des types de données pour les versions du client, plus loin dans cette rubrique.  

## <a name="cross-language-requirements"></a>Configuration multilingue  
 La version anglaise du pilote OLE DB pour SQL Server est prise en charge sur toutes les versions localisées des systèmes d’exploitation pris en charge. Les versions localisées du pilote OLE DB pour SQL Server sont pris en charge sur les systèmes d’exploitation localisés qui sont la même langue que le localisée pilote OLE DB pour SQL Server version. Les versions localisées du pilote OLE DB pour SQL Server sont également prises en charge sur les versions anglaises des systèmes d’exploitation pris en charge tant que les paramètres de langue correspondants sont installés.  

 Pour les mises à niveau :  

-   Les versions anglaises de pilote OLE DB pour SQL Server peuvent être mis à niveau vers n’importe quelle version localisée de pilote OLE DB pour SQL Server.  

-   Les versions localisées du pilote OLE DB pour SQL Server peuvent être mis à niveau vers des versions localisées de pilote OLE DB pour SQL Server de la même langue.  

-   Version localisée du pilote OLE DB pour SQL Server peut être mis à niveau vers la version en langue anglaise de pilote OLE DB pour SQL Server.  

-   Les versions localisées du pilote OLE DB pour SQL Server ne peut pas être mis à niveau localisée pilote OLE DB pour SQL Server versions d’une autre langue localisée.  

## <a name="data-type-compatibility-for-client-versions"></a>Compatibilité des types de données pour les versions du client  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le pilote OLE DB pour SQL Server carte nouveaux types de données pour les types de données plus anciennes qui sont compatibles avec les clients de bas niveau, comme indiqué dans le tableau ci-dessous.  

 Les applications OLE DB et ADO peuvent utiliser le **DataTypeCompatibility** mot clé de chaîne de connexion avec le pilote OLE DB pour SQL Server fonctionne avec les types de données plus anciens. Lorsque **DataTypeCompatibility = 80**, les clients OLE DB seront connecte à l’aide de le [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] version (TDS), plutôt que la version TDS du flux de données tabulaires. Cela signifie que pour [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et types de données ultérieure, conversion de bas niveau ne seront effectuées par le serveur, plutôt que par le pilote OLE DB pour SQL Server. Cela signifie également que les fonctionnalités disponibles sur la connexion seront limitées à l'ensemble de fonctionnalités de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Plutôt que d'essayer de transmettre des requêtes non valides au serveur, une détection des tentatives d'utilisation de nouveaux types de données ou fonctionnalités intervient dès que possible sur les appels d'API et les erreurs sont retournées à l'application appelante.   


 IDBInfo::GetKeywords retourne toujours une liste de mots-clés qui correspond à la version du serveur sur la connexion et n’est pas affectée par **DataTypeCompatibility**.  

|Type de données|SQL Server Native Client<br /><br />SQL Server 2005|SQL Server Native Client 11.0<br /><br /> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|Pilote d’OLE DB pour SQL Server|Windows Data Access Components, MDAC et<br /><br /> Pilote OLE DB pour les applications OLE DB pour SQL Server avec DataTypeCompatibility = 80|  
|---------------|--------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|  
|CLR UDT (\<= 8 Ko)|udt|udt|udt|Varbinary|  
|varbinary(max)|varbinary|varbinary|varbinary|image|  
|varchar(max)|varchar|varchar|varchar|Texte|  
|nvarchar(max)|nvarchar|nvarchar|nvarchar|Ntext|  
|xml|xml|xml|xml|Ntext|  
|CLR UDT (>= 8 Ko)|varbinary|udt|udt|image|  
|date|varchar|date|date|Varchar|  
|datetime2|varchar|datetime2|datetime2|Varchar|  
|datetimeoffset|varchar|datetimeoffset|datetimeoffset|Varchar|  
|time|varchar|time|time|Varchar|  

## <a name="see-also"></a>Voir aussi  
 [Pilote d’OLE DB pour SQL Server](../oledb/oledb-driver-for-sql-server.md)   
 [Installation d’OLE DB Driver pour SQL Server](../oledb/applications/installing-oledb-driver-for-sql-server.md)  
