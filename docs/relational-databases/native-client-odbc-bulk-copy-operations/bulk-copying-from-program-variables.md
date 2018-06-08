---
title: Variables de programme à partir de la copie en bloc | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client-odbc-bulk-copy-operations
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC], program variables
- bcp_bind function
- mapping data types [ODBC]
- SQL Server Native Client ODBC driver, bulk copy
- data types [ODBC], bulk copying
- ODBC, bulk copy operations
- program variables [ODBC]
ms.assetid: e4284a1b-7534-4b34-8488-b8d05ed67b8c
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 5e0bcb033e4e99bdba8dc7167d3823e103c9cfd9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bulk-copying-from-program-variables"></a>Copie en bloc à partir de variables de programme
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  Vous pouvez effectuer une copie en bloc directement à partir de variables de programme. Après avoir alloué des variables pour stocker les données d’une ligne et l’appel [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) pour démarrer la copie en bloc, appelez [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) pour chaque colonne spécifier l’emplacement et le format de la variable de programme à associer à la colonne. Remplissez chaque variable avec des données, puis appelez [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) pour envoyer une ligne de données sur le serveur. Répétez le processus de remplissage des variables et d’appel **bcp_sendrow** jusqu'à ce que toutes les lignes ont été envoyés au serveur, puis appelez [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md) pour spécifier que l’opération est terminée.  
  
 Le **bcp_bind *** pData* paramètre contient l’adresse de la variable liée à la colonne. Les données de chaque colonne peuvent être stockées de l'une des deux manières suivantes :  
  
-   Allocation d'une variable destinée à contenir les données.  
  
-   Allocation d'une variable indicateur suivie immédiatement de la variable de données.  
  
 La variable indicateur indique la longueur des données des colonnes de longueur variable et indique des valeurs NULL si la colonne autorise ces valeurs. Si seule une variable de données est utilisée, l’adresse de cette variable est stockée dans le **bcp_bind *** pData* paramètre. Si une variable indicateur est utilisée, l’adresse de la variable indicateur est stockée dans le **bcp_bind *** pData* paramètre. Les fonctions de copie en bloc calculent l’emplacement de la variable de données en ajoutant le **bcp_bind *** cbIndicator* et *pData* paramètres.  
  
 **bcp_bind** prend en charge trois méthodes pour traiter les données de longueur variable :  
  
-   Utilisez *cbData* avec uniquement une variable de données. La longueur des données dans *cbData*. Chaque fois que la longueur des données en bloc copiés les modifications, appelez [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md)réinitialiser *cbData*. Si une des deux autres méthodes est utilisée, spécifiez SQL_VARLEN_DATA pour *cbData*. Si toutes les valeurs de données qui est fournis pour une colonne sont NULL, spécifiez SQL_NULL_DATA pour *cbData*.  
  
-   Utilisation de variables indicateur. Dès qu'une nouvelle valeur de données est déplacée dans la variable de données, stockez la longueur de la valeur dans la variable indicateur. Si un des deux autres méthodes est utilisé, spécifiez 0 pour *cbIndicator*.  
  
-   Utilisation de pointeurs de terminateur. Charge le **bcp_bind *** pTerm* paramètre avec l’adresse du modèle binaire qui termine les données. Si un des deux autres méthodes est utilisé, spécifiez la valeur NULL pour *pTerm*.  
  
 Les trois de ces méthodes peuvent être utilisés sur le même **bcp_bind** appeler, auquel cas la spécification qui donne la plus petite quantité de données à copier est utilisée.  
  
 Le **bcp_bind *** type* identificateurs de type de données paramètre utilise DB-Library, pas les identificateurs de type de données ODBC. Les identificateurs de type de données DB-Library sont définis dans sqlncli.h pour une utilisation avec ODBC **bcp_bind** (fonction).  
  
 Les fonctions de copie en bloc ne prennent pas en charge tous les types de données ODBC C. Par exemple, les fonctions de copie en bloc ne prennent pas en charge la structure ODBC SQL_C_TYPE_TIMESTAMP, alors utilisez [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) ou [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) pour convertir des données ODBC SQL_TYPE_TIMESTAMP en variable SQL_C_CHAR. Si vous utilisez ensuite **bcp_bind** avec un *type* paramètre ayant la valeur sqlcharacter pour lier la variable à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **datetime** colonne, les fonctions de copie en bloc convertissent la clause escape timestamp dans la variable de caractère au format datetime approprié.  
  
 Le tableau suivant répertorie les types de données recommandés à utiliser lors du mappage d’un type de données SQL ODBC à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données.  
  
|Type de données ODBC SQL|Type de données ODBC C|bcp_bind *type* paramètre|Type de données SQL Server|  
|-----------------------|----------------------|--------------------------------|--------------------------|  
|SQL_CHAR|SQL_C_CHAR|SQLCHARACTER|**character**<br /><br /> **char**|  
|SQL_VARCHAR|SQL_C_CHAR|SQLCHARACTER|**varchar**<br /><br /> **variable de caractère**<br /><br /> **char varying**<br /><br /> **sysname**|  
|SQL_LONGVARCHAR|SQL_C_CHAR|SQLCHARACTER|**texte**|  
|SQL_WCHAR|SQL_C_WCHAR|SQLNCHAR|**nchar**|  
|SQL_WVARCHAR|SQL_C_WCHAR|SQLNVARCHAR|**nvarchar**|  
|SQL_WLONGVARCHAR|SQL_C_WCHAR|SQLNTEXT|**ntext**|  
|SQL_DECIMAL|SQL_C_CHAR|SQLCHARACTER|**decimal**<br /><br /> **dec**<br /><br /> **money**<br /><br /> **smallmoney**|  
|SQL_NUMERIC|SQL_C_NUMERIC|SQLNUMERICN|**numeric**|  
|SQL_BIT|SQL_C_BIT|SQLBIT|**bit**|  
|SQL_TINYINT (signé)|SQL_C_SSHORT|SQLINT2|**smallint**|  
|SQL_TINYINT (non signé)|SQL_C_UTINYINT|SQLINT1|**tinyint**|  
|SQL_SMALL_INT (signé)|SQL_C_SSHORT|SQLINT2|**smallint**|  
|SQL_SMALL_INT (non signé)|SQL_C_SLONG|SQLINT4|**int**<br /><br /> **entier**|  
|SQL_INTEGER (signé)|SQL_C_SLONG|SQLINT4|**int**<br /><br /> **entier**|  
|SQL_INTEGER (non signé)|SQL_C_CHAR|SQLCHARACTER|**decimal**<br /><br /> **dec**|  
|SQL_BIGINT (signé et non signé)|SQL_C_CHAR|SQLCHARACTER|**bigint**|  
|SQL_REAL|SQL_C_FLOAT|SQLFLT4|**real**|  
|SQL_FLOAT|SQL_C_DOUBLE|SQLFLT8|**float**|  
|SQL_DOUBLE|SQL_C_DOUBLE|SQLFLT8|**float**|  
|SQL_BINARY|SQL_C_BINARY|SQLBINARY|**binaire**<br /><br /> **timestamp**|  
|SQL_VARBINARY|SQL_C_BINARY|SQLBINARY|**varbinary**<br /><br /> **Variable binaire**|  
|SQL_LONGVARBINARY|SQL_C_BINARY|SQLBINARY|**image**|  
|SQL_TYPE_DATE|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_TYPE_TIME|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_TYPE_TIMESTAMP|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_GUID|SQL_C_GUID|SQLUNIQUEID|**uniqueidentifier**|  
|SQL_INTERVAL_|SQL_C_CHAR|SQLCHARACTER|**char**|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne pas avoir signé **tinyint**non signé **smallint**, non signé ou **int** des types de données. Pour empêcher la perte de valeurs de données lors de la migration de ces types de données, créez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table avec le type de données plus grand entier suivant. Pour empêcher les utilisateurs d'ajouter ultérieurement des valeurs en dehors de la plage autorisée par le type de données d'origine, appliquez une règle à la colonne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de manière à limiter les valeurs autorisées à la plage prise en charge par le type de données dans la source d'origine :  
  
```  
CREATE TABLE Sample_Ints(STinyIntCol   SMALLINT,  
USmallIntCol INT)  
GO  
CREATE RULE STinyInt_Rule  
AS   
@range >= -128 AND @range <= 127  
GO  
CREATE RULE USmallInt_Rule  
AS   
@range >= 0 AND @range <= 65535  
GO  
sp_bindrule STinyInt_Rule, 'Sample_Ints.STinyIntCol'  
GO  
sp_bindrule USmallInt_Rule, 'Sample_Ints.USmallIntCol'  
GO  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge les types de données intervalle directement. Une application peut, toutefois, stocker des séquences d’échappement d’intervalle sous forme de chaînes de caractères dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] colonne de type caractère. L'application peut les lire pour une utilisation ultérieure, mais elles ne peuvent pas être utilisées dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Les fonctions de copie en bloc peuvent être utilisées pour charger des données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ont été lues à partir d’une source de données ODBC. Utilisez [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) pour lier les colonnes d’un jeu de résultats aux variables de programme, puis utilisez **bcp_bind** pour lier ces mêmes variables de programme pour une opération de copie en bloc. Appel de [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) ou **SQLFetch** extrait ensuite une ligne de données à partir de la source de données ODBC dans les variables de programme et l’appel [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) copie en bloc les données des variables de programme à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Une application peut utiliser le [bcp_colptr](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colptr.md) fonction dès qu’il a besoin de modifier l’adresse de la variable de données spécifiée à l’origine dans le **bcp_bind** *pData* paramètre. Une application peut utiliser le [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md) fonction dès qu’il a besoin de modifier la longueur de données spécifiée à l’origine dans le **bcp_bind *** cbData* paramètre.  
  
 Vous ne pouvez pas lire les données à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans des variables de programme à l’aide de la copie en bloc ; il n’existe une fonction similaire à « bcp_readrow ». Vous pouvez seulement envoyer des données de l'application au serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution d’opérations de copie en bloc &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
  
