---
title: Allocation d’un descripteur de connexion | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-communication
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- ODBC applications, passwords
- ODBC applications, connections
- handles [SQL Server Native Client]
- expiration [SQL Server Native Client]
- passwords [SQL Server], modifying
- SQL Server Native Client ODBC driver, connection handles
- connection handles [SQL Server Native Client]
- modifying passwords
- SQLAllocHandle function
ms.assetid: 471d8a31-199c-4f92-bb10-004fc7733b35
caps.latest.revision: 32
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: ab24d328ed8c6e68b5828b67f92c55004a184f45
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="allocating-a-connection-handle"></a>Allocation d'un handle de connexion
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Pour que l'application puisse se connecter à une source de données ou à un pilote, elle doit auparavant allouer un handle de connexion. Cela est effectué en appelant **SQLAllocHandle** avec la *HandleType* paramètre la valeur SQL_HANDLE_DBC et *InputHandle* pointant vers un handle d’environnement initialisé.  
  
 Les caractéristiques de la connexion sont contrôlées en définissant des attributs de connexion. Par exemple, étant donné que les transactions se produisent au niveau de la connexion, le niveau d'isolation de la transaction est un attribut de connexion. De la même façon, le délai d'expiration de la connexion (c'est-à-dire le nombre de secondes s'écoulant avant l'expiration de la tentative de connexion) est un attribut de connexion.  
  
 Attributs de connexion sont définis avec [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md), et leurs paramètres actuels sont récupérés avec [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md). Si **SQLSetConnectAttr** est appelée avant une tentative de connexion, le Gestionnaire de pilotes ODBC stocke les attributs dans la structure de connexion et les définit dans le pilote dans le cadre du processus de connexion. Certains attributs de connexion doivent être définis avant toute tentative de connexion de l'application ; d'autres peuvent être définis une fois la connexion établie. Par exemple, SQL_ATTR_ODBC_CURSORS doit être défini avant l'établissement d'une connexion, mais SQL_ATTR_AUTOCOMMIT peut être défini une fois la connexion établie.  
  
 Les applications s'exécutant sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7.0 ou version ultérieure peuvent améliorer leurs performances en réinitialisant la taille du paquet réseau du flux TDS (Tabular Data Stream). La taille du paquet par défaut est définie au niveau du serveur avec la valeur 4 Ko. Une taille de paquet comprise entre 4 Ko et 8 Ko offre généralement les meilleures performances. Si les tests indiquent qu'une taille de paquet différente offre de meilleures performances, l'application peut réinitialiser la taille du paquet. Les applications ODBC pour ce faire avant de se connecter en appelant **SQLSetConnectAttr** avec l’option SQL_ATTR_PACKET_SIZE. Certaines applications fonctionnent mieux avec une taille de paquet plus grande, mais les gains en termes de performances sont généralement minimes pour les tailles de paquet supérieures à 8 Ko.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client a un nombre d’attributs de connexion étendus qu’une application peut utiliser pour améliorer ses fonctionnalités. Certains de ces attributs contrôlent les mêmes options que celles qui peuvent être spécifiées dans les sources de données et qui sont utilisées pour remplacer n'importe quelle option définie dans une source de données. Par exemple, si une application utilise des identificateurs entre guillemets, elle peut attribuer à l'attribut SQL_COPT_SS_QUOTED_IDENT spécifique au pilote la valeur SQL_QI_ON pour faire en sorte que cette option soit toujours définie indépendamment du paramètre dans une source de données quelconque.  
  
## <a name="see-also"></a>Voir aussi  
 [Communication avec SQL Server &#40;ODBC&#41;](../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
  
  
