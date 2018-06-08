---
title: Fonctionnalités du pilote Microsoft ODBC pour SQL Server sur Windows | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
caps.latest.revision: 22
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9e2b7c107a40a60a1da5ed891d7a26a7c85011db
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="features-of-the-microsoft-odbc-driver-for-sql-server-on-windows"></a>Fonctionnalités de Microsoft ODBC Driver for SQL Server sur Windows
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

    
## <a name="microsoft-odbc-driver-131-for-sql-server-on-windows"></a>Microsoft ODBC Driver 13.1 for SQL Server sur Windows

ODBC Driver 13.1 for SQL Server contient toutes les fonctionnalités de la version précédente (11) et ajoute la prise en charge pour l’authentification Always Encrypted et Azure Active Directory lorsqu’il est utilisé conjointement avec Microsoft SQL Server 2016.  
  
Le Chiffrement intégral permet aux clients de chiffrer des données sensibles dans des applications clientes et de ne jamais révéler les clés de chiffrement à SQL Server. À cette fin, un pilote de Chiffrement intégral installé sur l’ordinateur client chiffre et déchiffre automatiquement les données sensibles dans l’application cliente SQL Server. Le pilote chiffre les données dans les colonnes sensibles avant de les transmettre à SQL Server et il réécrit automatiquement les requêtes pour que la sémantique de l’application soit conservée. De même, il déchiffre de manière transparente les données stockées dans les colonnes de base de données chiffrées qui figurent dans les résultats de la requête. Pour plus d’informations, consultez [à l’aide d’Always Encrypted avec le pilote ODBC](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md).
 
Azure Active Directory permet aux utilisateurs, de droits DBA et les programmeurs d’applications à utiliser l’authentification Azure Active Directory en tant que mécanisme de connexion à la base de données SQL Microsoft Azure et Microsoft SQL Server 2016 à l’aide des identités dans Azure Active Directory (Azure AD ). Pour plus d’informations, consultez [à l’aide de Azure Active Directory avec le pilote ODBC](../../../connect/odbc/using-azure-active-directory.md), et [la connexion à la base de données SQL ou SQL données entrepôt par à l’aide d’authentification Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/sql-database-aad-authentication/).   
  
## <a name="microsoft-odbc-driver-11-for-sql-server-on-windows"></a>Microsoft® ODBC Driver 11 for SQL Server® dans Windows  

ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] contient toutes les fonctionnalités du pilote ODBC [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] Native Client fournies avec [!INCLUDE[ssSQL11](../../../includes/sssql11_md.md)]. Pour plus d’informations, consultez [Programmation de SQL Server Native Client](http://msdn.microsoft.com/library/ms130892.aspx). Le pilote ODBC [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] Native Client est basé sur le pilote ODBC fourni avec le système d’exploitation Windows. Pour plus d’informations, consultez [Windows Data Access Components SDK](http://msdn.microsoft.com/library/aa968814(VS.85).aspx).  
  
Cette version du pilote ODBC pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] contient les nouvelles fonctionnalités suivantes :  
  
### <a name="bcpexe-l-option-for-specifying-a-login-timeout"></a>bcp.exe – l, option permettant de spécifier un délai d’attente de connexion
 
L’option – l spécifie le nombre de secondes avant une `bcp.exe` connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] arrive à expiration lorsque vous essayez de vous connecter à un serveur. Le délai de connexion par défaut est de 15 secondes. Le délai de connexion doit être un nombre compris entre 0 et 65534. Si la valeur fournie n'est pas numérique ou n'est pas comprise dans cet intervalle, `bcp.exe` génère un message d'erreur. La valeur 0 indique un délai infini. Un délai d’attente de connexion de moins de 10 secondes (environ) n’est pas fiable.  
  
### <a name="driver-aware-connection-pooling"></a>Regroupement de connexions prenant en charge les pilotes  
Le pilote ODBC pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] prend en charge [le regroupement de connexions prenant en charge les pilotes](http://msdn.microsoft.com/library/hh405031(VS.85).aspx). Pour plus d’informations, consultez [Driver-Aware Connection Pooling in the ODBC Driver for SQL Server](../../../connect/odbc/windows/driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md).  
  
### <a name="asynchronous-execution-notification-method"></a>Exécution asynchrone (méthode de notification)  
Le pilote ODBC pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] prend en charge [exécution asynchrone (méthode de Notification)](http://msdn.microsoft.com/library/hh405038(VS.85).aspx). Pour obtenir un exemple d’utilisation, consultez [exécution asynchrone &#40;méthode de Notification&#41; exemple](../../../connect/odbc/windows/asynchronous-execution-notification-method-sample.md).  
  
### <a name="connection-resiliency"></a>Résilience des connexions
Pour vous assurer que les applications restent connectées à Microsoft Azure SQL Database, le pilote ODBC sur Windows peut restaurer les connexions inactives. Pour plus d’informations, consultez [Connection Resiliency in the Windows ODBC Driver](../../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md).  
  
## <a name="behavior-changes"></a>Modifications de comportement

Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] Native Client, le `-y0` est définie sur `sqlcmd.exe` sortie seront tronquées à 1 Mo si la largeur d’affichage est 0.
  
À compter d’ODBC Driver 11 pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)], il n’existe aucune limite quant à la quantité de données qui peuvent être récupérées dans une seule colonne lorsque `–y0` est spécifié. `sqlcmd.exe` diffuse désormais des colonnes aussi grands que 2 Go ([!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] maximale du type de données).  
  
Une autre différence est que le fait de spécifier `-h` et `-y0` génère désormais une erreur signalant que les options sont incompatibles. `-h`, qui spécifie le nombre de lignes à imprimer entre les en-têtes de colonne et n’a jamais été compatible avec `-y0`, a été ignorée même si aucun en-tête n’était imprimé.
  
Notez que `-y0` peut entraîner des problèmes de performances sur le serveur et le réseau, en fonction de la taille des données retournées.

## <a name="see-also"></a>Voir aussi  
[Pilote Microsoft ODBC pour SQL Server sur Windows](../../../connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)  
