---
title: Liaison de paramètres | Documents Microsoft
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
- SQL Server Native Client ODBC driver, parameters
- parameters [SQL Server Native Client], ODBC
- statements [ODBC], parameters
- binding parameters [SQL Server Native Client]
- parameter markers [ODBC]
- unbound parameter markers
- SQLBindParameter function
- ODBC applications, parameters
- bound parameter markers [SQL Server Native Client]
ms.assetid: d6c69739-8f89-475f-a60a-b2f6c06576e2
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6d51c8cb13a4a8bd2e9cc14e8208c6d9e0e0084f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-statement-parameters---binding-parameters"></a>À l’aide des paramètres d’instruction - paramètres de liaison
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Chaque marqueur de paramètre dans une instruction SQL doit être associé, ou lié, à une variable dans l'application avant que l'instruction puisse être exécutée. Cela est effectué en appelant le [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) (fonction). **SQLBindParameter** décrit la variable de programme (adresse, type de données C et ainsi de suite) pour le pilote. Cette fonction identifie également le marqueur de paramètre en indiquant sa valeur ordinale puis décrit les caractéristiques de l'objet SQL qu'il représente (type de données SQL, précision, etc.).  
  
 Les marqueurs de paramètre peuvent être liés ou liés une nouvelle fois à tout moment avant l'exécution d'une instruction. Une liaison de paramètre reste en vigueur jusqu'à ce que l'un des événements suivants se produise :  
  
-   Un appel à [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) avec la *Option* paramètre défini sur SQL_RESET_PARAMS libère tous les paramètres liés au descripteur d’instruction.  
  
-   Un appel à **SQLBindParameter** avec *ParameterNumber* défini sur l’ordinal d’un marqueur de paramètre lié libère automatiquement la liaison précédente.  
  
 Une application peut également lier des paramètres à des tableaux de variables de programme pour traiter une instruction SQL par lots. Il existe deux types de liaison de tableau :  
  
-   Une liaison selon les colonnes est effectuée lorsque chaque paramètre individuel est lié à son propre tableau de variables.  
  
     La liaison est spécifiée en appelant [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) avec *attribut* ayant pour valeur SQL_ATTR_PARAM_BIND_TYPE et *ValuePtr* ayant pour valeur SQL_PARAM_BIND_BY_COLUMN.  
  
-   Une liaison selon les lignes est effectuée lorsque tous les paramètres dans l'instruction SQL sont liés en tant qu'unité à un tableau de structures qui contiennent les variables individuelles pour les paramètres.  
  
     La liaison est spécifiée en appelant **SQLSetStmtAttr** avec *attribut* ayant pour valeur SQL_ATTR_PARAM_BIND_TYPE et *ValuePtr* défini avec la taille de la structure contenant les variables de programme.  
  
 Lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client envoie des caractères ou des paramètres de chaîne binaire au serveur, il remplit les valeurs à la longueur spécifiée dans **SQLBindParameter** *ColumnSize* paramètre. Si une application ODBC 2.x spécifie 0 pour *ColumnSize*, le pilote remplit la valeur du paramètre à la précision du type de données. La précision est 8000 lors d'une connexion à des serveurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], 255 lors d'une connexion à des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *ColumnSize* est exprimée en octets pour les colonnes de type variant.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la définition de noms pour les paramètres de procédure stockée. ODBC 3.5 a également introduit la prise en charge des paramètres nommés utilisés lors de l'appel de procédures stockées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette prise en charge peut être utilisée pour :  
  
-   appeler une procédure stockée et fournir des valeurs pour un sous-ensemble des paramètres définis pour la procédure stockée ;  
  
-   spécifier les paramètres dans un ordre différent dans l'application de l'ordre spécifié quand la procédure stockée a été créée.  
  
 Les paramètres nommés sont uniquement prises en charge avec la [!INCLUDE[tsql](../../includes/tsql-md.md)] **EXECUTE** instruction ou la séquence d’échappement ODBC CALL pour exécuter une procédure stockée.  
  
 Si **SQL_DESC_NAME** est définie pour un paramètre de procédure stockée, tous les paramètres de procédure stockée dans la requête doivent également définir **SQL_DESC_NAME**.  Si des littéraux sont utilisés dans les appels de procédure stockée, où les paramètres ont **SQL_DESC_NAME** ensemble, les littéraux doivent utiliser le format *' nom*=*valeur*», où *nom* est le nom de paramètre de procédure stockée (par exemple, @p1). Pour plus d’informations, consultez [Binding Parameters by Name (Named Parameters)](http://go.microsoft.com/fwlink/?LinkId=167215).  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation de paramètres d’instruction](../../relational-databases/native-client-odbc-queries/using-statement-parameters.md)  
  
  
