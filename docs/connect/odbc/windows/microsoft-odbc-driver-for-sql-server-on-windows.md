---
title: Pilote Microsoft ODBC pour SQL Server sur Windows | Documents Microsoft
ms.custom: ''
ms.date: 02/14/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b10cfc22-6a2c-4707-a456-0dcec317982b
caps.latest.revision: 37
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e5d818e4ce5c267432e6e456e11720f546ebaa19
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-odbc-driver-for-sql-server-on-windows"></a>Pilote Microsoft ODBC pour SQL Server sur Windows
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Les pilotes ODBC Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sont des pilotes ODBC autonomes qui fournissent une interface de programmation d’applications (API) implémentant les interfaces ODBC standards dans Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)].

Le pilote Microsoft ODBC pour SQL Server peut servir à créer des applications. Vous pouvez également mettre à niveau vos anciennes applications qui utilisent actuellement un pilote ODBC plus ancien. Le pilote ODBC pour SQL Server prend en charge les connexions à la base de données SQL Azure, entrepôt de données SQL Azure, SQL Server 2017, SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008 et SQL Server 2005.  

## <a name="summary"></a>Résumé

| Version       | Fonctionnalités prises en charge      |
| ------------- |---------------| 
| Pilote ODBC Microsoft 17 pour SQL Server | <ul><li>Always Encrypted prise en charge des API BCP</li><li>Nouvel attribut de chaîne de connexion UseFMTONLY conduit le pilote à utiliser les métadonnées héritées dans les cas spéciaux exigeant des tables temporaires</li>
| Microsoft ODBC Driver 13.1 for SQL Server     | <ul><li>Always Encrypted</li><li>Authentification Azure AD</li><li>Groupes de disponibilité AlwaysOn</li></ul>   | 
| Microsoft ODBC Driver 13 for SQL Server      | <ul><li>Noms de domaine internationaux</li></ul> |
| Microsoft ODBC Driver 11 for SQL Server | <ul><li>Regroupement de connexions prenant en charge les pilotes</li><li>Résilience des connexions</li><li>Exécution asynchrone (méthode d’interrogation)</li></ul> |    

## <a name="documentation"></a>Documentation  
Cette documentation sur Microsoft ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] inclut ce qui suit :  
  
-   [Notes de publication](../../../connect/odbc/windows/release-notes.md)  
-   [Fonctionnalités de Microsoft ODBC Driver for SQL Server sur Windows](../../../connect/odbc/windows/features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md)  
-   [Configuration système requise, installation et fichiers de pilote](../../../connect/odbc/windows/system-requirements-installation-and-driver-files.md)  
-   [Regroupement de connexions prenant en charge le pilote dans le pilote ODBC pour SQL Server](../../../connect/odbc/windows/driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md)  
-   [Exemple d’exécution asynchrone &#40;méthode de notification&#41;](../../../connect/odbc/windows/asynchronous-execution-notification-method-sample.md)  
-   [Résilience de connexion du pilote ODBC Windows](../../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)  
-   [Utilisation du chiffrement intégral avec le pilote ODBC](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
-   [Utilisation d’Azure Active Directory avec ODBC Driver](../../../connect/odbc/using-azure-active-directory.md) 
-   [À l’aide de la résolution IP réseau Transparent](../../../connect/odbc/using-transparent-network-ip-resolution.md)   

## <a name="community"></a>Communauté  
- [Blog de l’équipe Microsoft ODBC Driver for SQL Server](http://blogs.msdn.com/sqlnativeclient/default.aspx)  
- [Forum d'accès aux données SQL Server](http://social.technet.microsoft.com/Forums/en/sqldataaccess/threads)  
  
## <a name="see-also"></a>Voir aussi  
- [À propos de SQL Server Native Client](https://msdn.microsoft.com/sqlserver/ff658532.aspx)   
- [Génération d’Applications avec SQL Server Native Client](../../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)   
- [Forum aux questions sur SQL Server Native Client](https://msdn.microsoft.com/sqlserver/aa937707.aspx)   
- [Guide de référence du programmeur ODBC](../../../odbc/reference/odbc-programmer-s-reference.md)   
- [SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
