---
title: Construction d’instructions SQL pour les curseurs | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-queries
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- cursors [ODBC], statement construction
- ODBC applications, cursors
- SQL Server Native Client ODBC driver, statements
- statements [ODBC], constructing
- ODBC applications, statements
- statements [ODBC], cursors
ms.assetid: 134003fd-9c93-4f5c-a988-045990933b80
caps.latest.revision: 36
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6207d51509b4eed3ebb3ec0db5c40174b7b7ac9b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="constructing-sql-statements-for-cursors"></a>Construction d'instructions SQL pour les curseurs
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client utilise des curseurs de serveur pour implémenter les fonctionnalités de curseur définie dans la spécification ODBC. Une application ODBC contrôle le comportement du curseur à l’aide de [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) pour définir les attributs d’instruction différent. Voici les attributs et leurs valeurs par défaut.  
  
|Attribut|Par défaut|  
|---------------|-------------|  
|SQL_ATTR_CONCURRENCY|SQL_CONCUR_READ_ONLY|  
|SQL_ATTR_CURSOR_TYPE|SQL_CURSOR_FORWARD_ONLY|  
|SQL_ATTR_CURSOR_SCROLLABLE|SQL_NONSCROLLABLE|  
|SQL_ATTR_CURSOR_SENSITIVITY|SQL_UNSPECIFIED|  
|SQL_ATTR_ROW_ARRAY_SIZE|1|  
  
 Lorsque ces options sont définies à leurs valeurs par défaut à la fois une instruction SQL est exécutée, la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client n’utilise pas un curseur côté serveur pour implémenter le jeu de résultats ; au lieu de cela, il utilise un jeu de résultats par défaut. Si une de ces options sont modifiée à partir de leurs valeurs par défaut à la fois une instruction SQL est exécutée, la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client tente d’utiliser un curseur côté serveur pour implémenter le jeu de résultats.  
  
 Les jeux de résultats par défaut prennent en charge toutes les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Il n'y a pas de restrictions concernant les types d'instructions SQL qui peuvent être exécutés lors de l'utilisation d'un jeu de résultats par défaut.  
  
 Les curseurs côté serveur ne prennent pas en charge toutes les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Les curseurs côté serveur ne prennent pas en charge les instructions SQL qui génèrent plusieurs jeux de résultats.  
  
 Les types d'instructions suivants ne sont pas pris en charge par les curseurs côté serveur :  
  
-   Lots  
  
     Les instructions SQL construites à partir de plusieurs instructions SQL SELECT individuelles, par exemple :  
  
    ```  
    SELECT * FROM Authors; SELECT * FROM Titles  
    ```  
  
-   Procédures stockées avec plusieurs instructions SELECT  
  
     Instructions SQL qui exécutent une procédure stockée contenant plusieurs instructions SELECT. Cela inclut les instructions SELECT qui remplissent des paramètres ou des variables.  
  
-   Mots clés  
  
     Instructions SQL contenant les mots clés FOR BROWSE ou INTO.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si une instruction SQL qui remplit l'une de ces conditions est exécutée avec un curseur côté serveur, celui-ci est converti implicitement en un jeu de résultats par défaut. Après avoir **SQLExecDirect** ou **SQLExecute** retourne SQL_SUCCESS_WITH_INFO, le curseur d’attributs reprendront leurs paramètres par défaut.  
  
 Les instructions SQL qui n'appartiennent à aucune des catégories précitées peuvent être exécutées avec tout paramètre d'attribut d'instruction ; elles fonctionnent aussi bien avec un jeu de résultats par défaut qu'avec un curseur côté serveur.  
  
## <a name="errors"></a>Erreurs  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 et versions ultérieures, toute tentative d'exécution d'une instruction qui produit plusieurs jeux de résultats génère SQL_SUCCESS_WITH INFO et le message suivant :  
  
```  
SqlState: 01S02"  
pfNative: 0  
szErrorMsgString: "[Microsoft][SQL Server Native Client][SQL Server]  
               Cursor type changed."  
```  
  
 Applications ODBC qui reçoivent ce message peuvent appeler [SQLGetStmtAttr](../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md) pour déterminer les paramètres de curseur en cours.  
  
 Toute tentative d'exécution d'une procédure avec plusieurs instructions SELECT lors de l'utilisation de curseurs côté serveur génère l'erreur suivante :  
  
```  
SqlState: 42000  
pfNative: 16937  
szErrorMsgString: [Microsoft][SQL Server Native Client][SQL Server]  
               A server cursor is not allowed on a stored procedure  
               with more than one SELECT statement in it. Use a  
               default result set or client cursor.  
```  
  
 Toute tentative d'exécution d'un lot avec plusieurs instructions SELECT lors de l'utilisation de curseurs côté serveur génère l'erreur suivante :  
  
```  
SqlState: 42000  
pfNative: 16938  
szErrorMsgString: [Microsoft][SQL Server Native Client][SQL Server]  
               sp_cursoropen. The statement parameter can only  
               be a single SELECT statement or a single stored   
               procedure.  
```  
  
 Les applications ODBC qui reçoivent ces erreurs doivent réinitialiser tous les attributs d'instructions de curseur à leurs valeurs par défaut avant d'essayer d'exécuter l'instruction.  
  
## <a name="see-also"></a>Voir aussi  
 [L’exécution de requêtes &#40;ODBC&#41;](../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
  
