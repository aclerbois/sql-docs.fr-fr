---
title: Effets des Options ISO | Documents Microsoft
ms.custom: 
ms.date: 03/14/2017
ms.prod: sql-non-specified
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.service: 
ms.component: native-client-odbc-queries
ms.reviewer: 
ms.suite: sql
ms.technology: 
ms.tgt_pltfrm: 
ms.topic: reference
helpviewer_keywords:
- ISO options (ODBC)
- ODBC applications, ISO options
- ODBC applications, statements
- SQL Server Native Client ODBC driver, ISO options
- statements [ODBC], ISO options
ms.assetid: 813f1397-fa0b-45ec-a718-e13fe2fb88ac
caps.latest.revision: 
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 2267290d43ec746fd3f2d11597eeefa498651e62
ms.sourcegitcommit: 9e6a029456f4a8daddb396bc45d7874a43a47b45
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2018
---
# <a name="effects-of-iso-options"></a>Conséquences des options ISO
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Le standard ODBC est étroitement mis en correspondance avec la norme ISO, et les applications ODBC attendent un comportement standard d'un pilote ODBC. Pour que ce comportement se conforme plus étroitement avec qui est défini dans la norme ODBC, le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client utilise toujours les options ISO disponibles dans la version de SQL Server à laquelle il se connecte.  
  
 Lorsque le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client se connecte à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], le serveur détecte que le client utilise le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client et définit alors plusieurs options.  
  
 Le pilote émet ces instructions lui-même ; l'application ODBC n'émet aucune demande. La définition de ces options permet aux applications ODBC utilisant le pilote d'être plus portables parce que le comportement du serveur correspond alors à la norme ISO.  
  
 Les applications basées sur DB-Library n'activent généralement pas ces options. Les sites qui observent une différence comportement entre les clients ODBC ou DB-Library lors de la même instruction SQL en cours d’exécution ne doit pas supposer cela indique un problème avec le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client. Ils doivent d’abord réexécuter l’instruction dans l’environnement de DB-Library avec les mêmes options SET comme serait utilisée par le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client.  
  
 Comme les options SET peuvent être activées et désactivées à tout moment par les utilisateurs et les applications, les développeurs de procédures stockées et de déclencheurs doivent aussi veiller à tester leurs procédures et leurs déclencheurs avec les options SET répertoriées ci-dessus activées et désactivées. Cela garantit que les procédures et les déclencheurs fonctionnent correctement, quelles que soient les options activées d'une connexion donnée lors de l'appel du déclencheur. Les déclencheurs ou les procédures stockées qui requièrent un paramètre particulier pour l'une de ces options doivent émettre une instruction SET au démarrage du déclencheur ou de la procédure stockée. Cette instruction SET n'est active que pour l'exécution du déclencheur ou de la procédure stockée ; à la fin de la procédure ou du déclencheur, la configuration d'origine est restaurée.  
  
 En cas de connexion à une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], une quatrième option SET, CONCAT_NULL_YIELDS_NULL, est également activée. Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client ne définit pas de ces options si AnsiNPW = NO est spécifié dans la source de données ou sur [SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md) ou [SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md).  
  
 Comme les options ISO notées précédemment, le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client n’active pas l’option QUOTED_IDENTIFIER si QuotedID = NO est spécifié dans la source de données ou sur **SQLDriverConnect** ou **SQLBrowseConnect**.  
  
 Pour permettre au pilote de connaître l'état actuel des options SET, les applications ODBC ne doivent pas utiliser l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] SET pour définir ces options. Elles doivent uniquement définir ces options à l'aide de la source de données ou des options de connexion. Si l'application émet des instructions SET, le pilote peut générer des instructions SQL inexactes.  
  
## <a name="see-also"></a>Voir aussi  
 [L’exécution d’instructions &#40; ODBC &#41;](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)   
 [SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)   
 [SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)  
  
  
