---
title: SQLBindParameter | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
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
- SQLBindParameter function
ms.assetid: c302c87a-e7f4-4d2b-a0a7-de42210174ac
caps.latest.revision: 46
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3c078adc30fee659d87e58a1ea6ac44e25e68236
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlbindparameter"></a>SQLBindParameter
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  **SQLBindParameter** peut éliminer la charge de conversion de données lorsqu’il est utilisé pour fournir des données pour le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client, ce qui entraîne des gains de performances significatifs pour les composants client et serveur d’applications. D'autres avantages incluent une perte réduite de précision lors de l'insertion ou de la mise à jour de types de données numériques approximatifs.  
  
> [!NOTE]  
>  Lors de l’insertion **char** et **wchar** dans une colonne d’image, la taille des données passées dans les données de type sont utilisées, par opposition à la taille des données après la conversion au format binaire.  
  
 Si le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client rencontre une erreur sur un élément de tableau unique d'un tableau de paramètres, le pilote continue d'exécuter l'instruction pour les éléments de tableau restants. Si l'application a lié un tableau d'éléments d'état de paramètre pour l'instruction, les lignes des paramètres qui génèrent des erreurs peuvent être déterminées à partir de ce tableau.  
  
 Lors de l'utilisation du pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, spécifiez SQL_PARAM_INPUT lors de la liaison de paramètres d'entrée. Spécifiez seulement SQL_PARAM_OUTPUT ou SQL_PARAM_INPUT_OUTPUT lors de la liaison de paramètres de procédure stockée définis avec le mot clé OUTPUT.  
  
 [SQLRowCount](../../relational-databases/native-client-odbc-api/sqlrowcount.md) n’est pas fiable avec le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client si un élément de tableau d’un tableau de paramètres liés provoque une erreur dans l’exécution des instructions. L'attribut d'instruction ODBC SQL_ATTR_PARAMS_PROCESSED_PTR signale le nombre de lignes traitées avant l'erreur. L'application peut alors parcourir son tableau d'état de paramètre pour découvrir le nombre d'instructions exécutées avec succès, si nécessaire.  
  
## <a name="binding-parameters-for-sql-character-types"></a>Liaison de paramètres pour les types de caractères SQL  
 Si le type de données SQL passé est un type de caractère, *ColumnSize* est la taille en caractères (pas des octets). Si la longueur de la chaîne de données en octets est supérieure à 8000, *ColumnSize* doit être défini sur **SQL_SS_LENGTH_UNLIMITED**, indiquant qu’il n’existe aucune limite à la taille du type SQL.  
  
 Par exemple, si le type de données SQL est **SQL_WVARCHAR**, *ColumnSize* ne doit pas être supérieure à 4000. Si la longueur réelle des données est supérieure à 4 000, puis *ColumnSize* doit être défini sur **SQL_SS_LENGTH_UNLIMITED** afin que **nvarchar (max)** sera utilisé par le pilote.  
  
## <a name="sqlbindparameter-and-table-valued-parameters"></a>SQLBindParameter et paramètres table  
 Comme d’autres types de paramètre, les paramètres table sont liés par SQLBindParameter.  
  
 Une fois qu'un paramètre table a été lié, ses colonnes sont également liées. Pour lier les colonnes, vous appelez [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) pour définir SQL_SOPT_SS_PARAM_FOCUS à l’ordinal du paramètre table incluse. Ensuite, appelez SQLBindParameter pour chaque colonne dans le paramètre table. Pour retourner aux liaisons de paramètre de premier niveau, attribuez à SQL_SOPT_SS_PARAM_FOCUS la valeur 0.  
  
 Pour plus d’informations sur les paramètres de mappage de champs de descripteur pour les paramètres table, consultez [liaison et les valeurs des colonnes et les paramètres Data Transfer of Table-Valued](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md).  
  
 Pour plus d’informations sur les paramètres table, consultez [paramètres table & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlbindparameter-support-for-enhanced-date-and-time-features"></a>Prise en charge de SQLBindParameter pour les fonctionnalités de date et heure améliorées  
 Les valeurs de paramètre des types date/heure sont converties comme décrit dans [Conversions de C en SQL](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-c-to-sql.md). Notez que pour les paramètres de type **temps** et **datetimeoffset** doit avoir *ValueType* spécifié en tant que **SQL_C_DEFAULT** ou **SQL_C_BINARY** si leurs structures correspondantes (**SQL_SS_TIME2_STRUCT** et **SQL_SS_TIMESTAMPOFFSET_STRUCT**) sont utilisés.  
  
 Pour plus d’informations, consultez [Date et heure améliorations & #40 ; ODBC & #41 ;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlbindparameter-support-for-large-clr-udts"></a>Prise en charge SQLBindParameter pour les types CLR volumineux définis par l'utilisateur  
 **SQLBindParameter** prend en charge les types CLR volumineux définis par l’utilisateur (UDT). Pour plus d’informations, consultez [Large CLR User-Defined Types & #40 ; ODBC & #41 ;](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Détails d’implémentation API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [SQLBindParameter, fonction](http://go.microsoft.com/fwlink/?LinkId=59328)  
  
  
