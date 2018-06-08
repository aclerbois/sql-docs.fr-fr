---
title: SQLDriverConnect | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-api
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLDriverConnect function
ms.assetid: a1e38e2c-3a97-42d1-9c45-a0ca3282ffd1
caps.latest.revision: 60
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6419788b83a20dbfd1037be01723be23f1d277a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqldriverconnect"></a>SQLDriverConnect
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client définit des attributs de connexion qui remplacent ou améliorent les mots clés de chaînes de connexion. Plusieurs mots clés de chaînes de connexion ont des valeurs par défaut définies par le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 Pour obtenir la liste des mots clés disponibles dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client, consultez [Using Connection String Keywords with SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 Pour plus d’informations sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attributs de connexion et les comportements par défaut de pilote, consultez [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md).  
  
 Pour en savoir plus sur les mots clés de chaîne de connexion qui sont valides pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, consultez [Using Connection String Keywords with SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 Lorsque le **SQLDriverConnect *** DriverCompletion* la valeur du paramètre est SQL_DRIVER_PROMPT, SQL_DRIVER_COMPLETE ou SQL_DRIVER_COMPLETE_REQUIRED, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client récupère les valeurs de mot clé de la boîte de dialogue qui s’affiche. Si la valeur de mot clé est transmise à la chaîne de connexion et si l'utilisateur ne modifie pas la valeur pour le mot clé dans la boîte de dialogue, le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client utilise la valeur issue de la chaîne de connexion. Si la valeur n'est pas définie dans la chaîne de connexion et si l'utilisateur ne procède à aucune affectation dans la boîte de dialogue, le pilote utilise la valeur par défaut.  
  
 **SQLDriverConnect** doit comporter une valide *WindowHandle* lorsque des *DriverCompletion* valeur requiert (ou peut nécessiter) l’affichage de la boîte de dialogue de connexion du pilote. Un handle non valide retourne SQL_ERROR.  
  
 Spécifiez le mot clé DRIVER ou DSN. ODBC indique que, de ces deux mots clés, un pilote utilise celui qui est situé le plus à gauche et ignore l'autre si les deux sont spécifiés. Si le pilote est spécifié, ou est le plus à gauche des deux et la **SQLDriverConnect *** DriverCompletion* la valeur du paramètre est SQL_DRIVER_NOPROMPT, le mot clé SERVER et une valeur appropriée.  
  
 Lorsque SQL_DRIVER_NOPROMPT est spécifié, les mots clés d'authentification utilisateur doivent être fournis avec des valeurs. Le pilote garantit que la chaîne « Trusted_Connection=yes » ou les mots clés UID et PWD à la fois sont disponibles.  
  
 Si le *DriverCompletion* la valeur du paramètre est SQL_DRIVER_NOPROMPT ou SQL_DRIVER_COMPLETE_REQUIRED et la langue ou la base de données provient de la chaîne de connexion et n’est pas valide, **SQLDriverConnect** retourne SQL_ERROR.  
  
 Si le *DriverCompletion* la valeur du paramètre est SQL_DRIVER_NOPROMPT ou SQL_DRIVER_COMPLETE_REQUIRED et la langue ou la base de données est fourni à partir des définitions de source de données ODBC et n’est pas valide, **SQLDriverConnect** utilise la langue par défaut ou la base de données pour l’ID d’utilisateur spécifié et retourne SQL_SUCCESS_WITH_INFO.  
  
 Si le *DriverCompletion* la valeur du paramètre est SQL_DRIVER_COMPLETE ou SQL_DRIVER_PROMPT et si la langue ou la base de données est non valide, **SQLDriverConnect** réaffiche la boîte de dialogue.  
  
## <a name="sqldriverconnect-support-for-high-availability-disaster-recovery"></a>SQLDriverConnect prend en charge les fonctionnalités de récupération d'urgence, haute disponibilité  
 Pour plus d’informations sur l’utilisation de **SQLDriverConnect** pour se connecter à un [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] de cluster, consultez [SQL Server Native Client Support for High Availability, Disaster Recovery](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  
  
## <a name="sqldriverconnect-support-for-service-principal-names-spns"></a>Prise en charge de SQLDriverConnect pour les noms de principaux du service (SPN)  
 SQLDDriverConnect utilise la connexion ODBC dès de boîte de dialogue invite est activée. Celle-ci vous permet d'entrer des SPN à la fois pour le serveur principal et pour le partenaire de basculement.  
  
 SQLDriverConnect acceptera les nouveaux mots clés de chaîne de connexion **ServerSPN** et **FailoverPartnerSPN**et reconnaît les nouveaux attributs de connexion SQL_COPT_SS_SERVER_SPN et SQL_COPT_SS_FAILOVER_PARTNER_SPN.  
  
 Lorsqu'une valeur d'attribut de connexion est définie plus d'une fois, toute valeur définie par programme à priorité sur la valeur d'un DSN et sur toute valeur dans une chaîne de connexion. Une valeur dans un DSN est prioritaire par rapport à une valeur dans une chaîne de connexion.  
  
 Lors de l'ouverture d'une connexion, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client définit SQL_COPT_SS_MUTUALLY_AUTHENTICATED et SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD sur la méthode d'authentification employée pour ouvrir la connexion.  
  
 Pour plus d’informations sur les SPN, consultez [noms principaux de Service & #40 ; Noms principaux de service & #41 ; dans les connexions clientes & #40 ; ODBC & #41 ; ](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md).  
  
## <a name="examples"></a>Exemples  
 L’appel suivant illustre le moins de données requises pour **SQLDriverConnect**:  
  
```  
SQLDriverConnect(hdbc, hwnd,  
    (SQLTCHAR*) TEXT("DRIVER={SQL Server Native Client 10};"), SQL_NTS, szOutConn,  
    MAX_CONN_OUT, &cbOutConn, SQL_DRIVER_COMPLETE);  
```  
  
 Les chaînes de connexion suivantes illustrent les données minimales requises lorsque la *DriverCompletion* la valeur du paramètre est SQL_DRIVER_NOPROMPT :  
  
```  
"DSN=Human Resources;Trusted_Connection=yes"  
  
"FILEDSN=HR_FDSN;Trusted_Connection=yes"  
  
"DRIVER={SQL Server Native Client 10};SERVER=(local);Trusted_Connection=yes"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonction SQLDriverConnect](http://go.microsoft.com/fwlink/?LinkId=59340)   
 [Détails d’implémentation API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [SET ANSI_NULLS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md)  
  
  
