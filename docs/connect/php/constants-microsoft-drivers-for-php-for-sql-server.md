---
title: Constantes (Microsoft Drivers for PHP for SQL Server) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.component: php
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- constants
ms.assetid: 9727c944-b645-48d6-9012-18dbde35ee3c
caps.latest.revision: 72
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a09ba7744f03fca15bb60db7979d31bb141f75c2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="constants-microsoft-drivers-for-php-for-sql-server"></a>Constantes (Microsoft Drivers for PHP for SQL Server)
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cette rubrique traite des constantes définies par le [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="pdosqlsrv-driver-constants"></a>Constantes de pilote PDO_SQLSRV  
Les constantes répertoriées sur le [site Web PDO](http://php.net/manual/book.pdo.php) sont valides dans le [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
Les éléments suivants décrivent les constantes spécifiques à Microsoft dans le pilote PDO_SQLSRV.  
  
### <a name="transaction-isolation-level-constants"></a>Constantes de niveau d’isolation de la transaction  
La clé **TransactionIsolation** , qui est utilisée avec [PDO::__construct](../../connect/php/pdo-construct.md), accepte l’une des constantes suivantes :  
  
-   PDO::SQLSRV_TXN_READ_UNCOMMITTED  
  
-   PDO::SQLSRV_TXN_READ_COMMITTED  
  
-   PDO::SQLSRV_TXN_REPEATABLE_READ  
  
-   PDO::SQLSRV_TXN_SNAPSHOT  
  
-   PDO::SQLSRV_TXN_SERIALIZABLE  
  
Pour plus d’informations sur la clé **TransactionIsolation** , consultez [Connection Options](../../connect/php/connection-options.md).  
  
### <a name="encoding-constants"></a>Constantes d’encodage  
L’attribut PDO::SQLSRV_ATTR_ENCODING peut être passé à [PDOStatement::setAttribute](../../connect/php/pdostatement-setattribute.md), [PDO::setAttribute](../../connect/php/pdo-setattribute.md), [PDO::prepare](../../connect/php/pdo-prepare.md), [PDOStatement : : bindColumn](../../connect/php/pdostatement-bindcolumn.md), et [PDOStatement::bindParam](../../connect/php/pdostatement-bindparam.md).  
  
Les valeurs pouvant être passées à PDO::SQLSRV_ATTR_ENCODING sont  
  
|Constante de pilote PDO_SQLSRV| Description|  
|-------------------------------|---------------|  
|PDO::SQLSRV_ENCODING_BINARY|Les données sont un flux d’octets bruts retourné à partir du serveur sans encodage ni traduction.<br /><br />Non valide pour PDO::setAttribute.|  
|PDO::SQLSRV_ENCODING_SYSTEM|Les données sont des caractères huit bits comme spécifié dans la page de codes des paramètres régionaux Windows définis sur le système. Les caractères multioctets ou les caractères qui ne sont pas mappent à cette page de codes sont remplacés par un caractère codés sur un point d’interrogation ( ?).|  
|PDO::SQLSRV_ENCODING_UTF8|Les données sont dans l’encodage UTF-8. Il s’agit de l’encodage par défaut.|  
|PDO::SQLSRV_ENCODING_DEFAULT|Utilise PDO::SQLSRV_ENCODING_SYSTEM si cela est spécifié durant la connexion.<br /><br />Utiliser l’encodage de la connexion s’il est spécifié dans une instruction de préparation.|  
  
### <a name="query-timeout"></a>Délai de requête  
L’attribut PDO::SQLSRV_ATTR_QUERY_TIMEOUT est un entier non négatif quelconque représentant le délai d’expiration, en secondes. Zéro (0) est la valeur par défaut, ce qui signifie aucun délai d’expiration.  
  
Vous pouvez spécifier l’attribut PDO::SQLSRV_ATTR_QUERY_TIMEOUT avec [PDOStatement::setAttribute](../../connect/php/pdostatement-setattribute.md), [PDO::setAttribute](../../connect/php/pdo-setattribute.md), et [PDO::prepare](../../connect/php/pdo-prepare.md).  
  
### <a name="direct-or-prepared-execution"></a>Exécution directe ou préparée  
Vous pouvez sélectionner l’exécution de requête directe ou l’exécution d’instruction préparée avec l’attribut PDO::SQLSRV_ATTR_DIRECT_QUERY. Vous pouvez définir PDO::SQLSRV_ATTR_DIRECT_QUERY avec [PDO::prepare](../../connect/php/pdo-prepare.md) ou [PDO::setAttribute](../../connect/php/pdo-setattribute.md). Pour plus d’informations sur PDO::SQLSRV_ATTR_DIRECT_QUERY, consultez [exécution d’instruction directe et exécution d’instruction préparée dans le pilote PDO_SQLSRV](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).  

### <a name="handling-numeric-fetches"></a>Gestion des extractions numérique
L’attribut PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE peut être utilisé pour gérer les extractions numériques à partir de colonnes avec des types numériques SQL (bits, integer, smallint, tinyint, float et real). Lorsque PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE a la valeur true, les résultats à partir d’une colonne d’entiers sont représentés en tant qu’entiers, tandis que SQL flotte réels sont représentées sous forme de nombres à virgule flottante. Cet attribut peut être défini avec [PDOStatement::setAttribute](../../connect/php/pdostatement-setattribute.md). 


## <a name="sqlsrv-driver-constants"></a>SQLSRV  
Les sections qui suivent répertorient les constantes utilisées par le pilote SQLSRV.  
  
### <a name="err-constants"></a>Constantes ERR  
Le tableau suivant répertorie les constantes qui sont utilisées pour spécifier si [sqlsrv_errors](../../connect/php/sqlsrv-errors.md) retourne des erreurs, des avertissements ou les deux.  
  
|Valeur| Description|  
|---------|---------------|  
|SQLSRV_ERR_ALL|Les erreurs et avertissements générés sur le dernier appel de fonction **sqlsrv** sont retournés. Ceci est la valeur par défaut.|  
|SQLSRV_ERR_ERRORS|Les erreurs générées sur le dernier appel de fonction **sqlsrv** sont retournées.|  
|SQLSRV_ERR_WARNINGS|Les avertissements générés sur le dernier appel de fonction **sqlsrv** sont retournés.|  
  
### <a name="fetch-constants"></a>Constantes FETCH  
Le tableau suivant répertorie les constantes utilisées pour spécifier le type de tableau retourné par [sqlsrv_fetch_array](../../connect/php/sqlsrv-fetch-array.md).  
  
|Constante SQLSRV| Description|  
|-------------------|---------------|  
|SQLSRV_FETCH_ASSOC|**sqlsrv_fetch_array** retourne la ligne de données suivante sous forme de tableau associatif.|  
|SQLSRV_FETCH_BOTH|**sqlsrv_fetch_array** retourne la ligne de données suivante sous forme de tableau avec des clés numériques et associatives. Ceci est la valeur par défaut.|  
|SQLSRV_FETCH_NUMERIC|**sqlsrv_fetch_array** retourne la ligne de données suivante sous forme de tableau indexé numériquement.|  
  
### <a name="logging-constants"></a>Constantes de journalisation  
Cette section répertorie les constantes utilisées pour modifier les paramètres de journalisation avec [sqlsrv_configure](../../connect/php/sqlsrv-configure.md). Pour plus d’informations sur la journalisation de l’activité, consultez [Logging Activity](../../connect/php/logging-activity.md).  
  
Le tableau suivant répertorie les constantes que vous pouvez utiliser comme valeur du paramètre **LogSubsystems** :  
  
|Constante SQLSRV (entier équivalent entre parenthèses)| Description|  
|----------------------------------------------------------|---------------|  
|SQLSRV_LOG_SYSTEM_ALL (-1)|Active la journalisation de tous les sous-systèmes.|  
|SQLSRV_LOG_SYSTEM_CONN (2)|Active la journalisation de l’activité de connexion.|  
|SQLSRV_LOG_SYSTEM_INIT (1)|Active la journalisation de l’activité d’initialisation.|  
|SQLSRV_LOG_SYSTEM_OFF (0)|Désactive la journalisation.|  
|SQLSRV_LOG_SYSTEM_STMT (4)|Active la journalisation de l’activité d’instruction.|  
|SQLSRV_LOG_SYSTEM_UTIL (8)|Active la journalisation de l’activité de fonctions d’erreur (tel que **handle_error** et **handle_warning**).|  
  
Le tableau suivant répertorie les constantes que vous pouvez utiliser comme valeur du paramètre **LogSeverity** :  
  
|Constante SQLSRV (entier équivalent entre parenthèses)| Description|  
|----------------------------------------------------------|---------------|  
|SQLSRV_LOG_SEVERITY_ALL (-1)|Spécifie que les erreurs, avertissements et avis doivent être enregistrés.|  
|SQLSRV_LOG_SEVERITY_ERROR (1)|Spécifie que les erreurs doivent être enregistrées.|  
|SQLSRV_LOG_SEVERITY_NOTICE (4)|Spécifie que les avis doivent être enregistrés.|  
|SQLSRV_LOG_SEVERITY_WARNING (2)|Spécifie que les avertissements doivent être enregistrés.|  
  
### <a name="nullable-constants"></a>Constantes nullables  
Le tableau suivant répertorie les constantes que vous pouvez utiliser pour déterminer si une colonne est nullable ou si cette information n’est pas disponible. Vous pouvez comparer la valeur de la clé **Nullable** retournée par [sqlsrv_field_metadata](../../connect/php/sqlsrv-field-metadata.md) pour déterminer l’état nullable de la colonne.  
  
|Constante SQLSRV (entier équivalent entre parenthèses)| Description|  
|----------------------------------------------------------|---------------|  
|SQLSRV_NULLABLE_YES (0)|La colonne accepte la valeur Null.|  
|SQLSRV_NULLABLE_NO (1)|La colonne n’accepte pas la valeur Null.|  
|SQLSRV_NULLABLE_UNKNOWN (2)|On ne sait pas si la colonne accepte la valeur Null.|  
  
### <a name="param-constants"></a>Constantes PARAM  
La liste suivante répertorie les constantes qui permettent de spécifier la direction du paramètre quand vous appelez [sqlsrv_query](../../connect/php/sqlsrv-query.md) ou [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md).  
  
|Constante SQLSRV| Description|  
|-------------------|---------------|  
|SQLSRV_PARAM_IN|Indique un paramètre d’entrée.|  
|SQLSRV_PARAM_INOUT|Indique un paramètre bidirectionnel.|  
|SQLSRV_PARAM_OUT|Indique un paramètre de sortie.|  
  
### <a name="phptype-constants"></a>Constantes PHPTYPE  
Le tableau suivant répertorie les constantes utilisées pour décrire des types de données PHP. Pour plus d’informations sur les types de données PHP, consultez [Types PHP](http://php.net/manual/en/language.types.php).  
  
|Constante SQLSRV|Type de données PHP|  
|-------------------|-----------------|  
|SQLSRV_PHPTYPE_INT|Entier|  
|SQLSRV_PHPTYPE_DATETIME|DateTime|  
|SQLSRV_PHPTYPE_FLOAT|Float|  
|SQLSRV_PHPTYPE_STREAM ($encodage<sup>1</sup>)|STREAM|  
|SQLSRV_PHPTYPE_STRING ($encodage<sup>1</sup>)|Chaîne|  
  
1. **SQLSRV_PHPTYPE_STREAM** et **SQLSRV_PHPTYPE_STRING** acceptent un paramètre qui spécifie l’encodage du flux. Le tableau suivant répertorie les constantes SQLSRV qui sont des paramètres acceptables et fournit une description de l’encodage correspondant.  
  
|Constante SQLSRV| Description|  
|-------------------|---------------|  
|SQLSRV_ENC_BINARY|Les données sont retournées à partir du serveur sous la forme d’un flux d’octets bruts sans encodage ni traduction.|  
|SQLSRV_ENC_CHAR|Les données sont retournées sous forme de caractères huit bits comme spécifié dans la page de codes des paramètres régionaux Windows définis sur le système. Les caractères multioctets ou les caractères qui ne sont pas mappent à cette page de codes sont remplacés par un caractère codés sur un point d’interrogation ( ?).<br /><br />Il s’agit de l’encodage par défaut.|  
|“UTF-8”|Les données sont retournées au format d’encodage UTF-8. Cette constante a été ajoutée dans la version 1.1 du [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]. Pour plus d’informations sur la prise en charge UTF-8, consultez [Comment : envoyer et récupérer UTF-8 données à l’aide intégrée UTF-8 prend en charge](../../connect/php/how-to-send-and-retrieve-utf-8-data-using-built-in-utf-8-support.md).|  
  
> [!NOTE]  
> Lorsque vous utilisez **SQLSRV_PHPTYPE_STREAM** ou **SQLSRV_PHPTYPE_STRING**, l’encodage doit être spécifié. Si vous ne fournissez aucun paramètre, une erreur est retournée.  
  
Pour plus d’informations sur ces constantes, consultez [Procédure : spécifier des types de données PHP](../../connect/php/how-to-specify-php-data-types.md), [Procédure : récupérer des données caractères sous la forme d’un flux à l’aide du pilote SQLSRV](../../connect/php/how-to-retrieve-character-data-as-a-stream-using-the-sqlsrv-driver.md).  
  
### <a name="sqltype-constants"></a>Constantes SQLTYPE  
Le tableau suivant répertorie les constantes utilisées pour décrire des types de données SQL Server. Certaines constantes sont similaires à la fonction et peuvent prendre de paramètres qui correspondent à la précision, échelle et/ou à la longueur.  Lors de la liaison des paramètres, les constantes de type fonction doivent être utilisés. Pour les comparaisons de type, constantes (comme fonction non) standard sont requis. Pour plus d’informations sur les types de données SQL Server, consultez [des Types de données (Transact-SQL).](../../t-sql/data-types/data-types-transact-sql.md) Pour plus d’informations sur la précision, échelle et longueur, consultez [précision, échelle et longueur (Transact-SQL).](../../t-sql/data-types/precision-scale-and-length-transact-sql.md)  
  
|Constante SQLSRV|Type de données SQL Server|  
|-------------------|------------------------|  
|SQLSRV_SQLTYPE_BIGINT|bigint|  
|SQLSRV_SQLTYPE_BINARY|binary|  
|SQLSRV_SQLTYPE_BIT|bit|  
|SQLSRV_SQLTYPE_CHAR|char<sup>5</sup>|  
|SQLSRV_SQLTYPE_CHAR($charCount)|char|  
|SQLSRV_SQLTYPE_DATE|date<sup>4</sup>|  
|SQLSRV_SQLTYPE_DATETIME|datetime|  
|SQLSRV_SQLTYPE_DATETIME2|datetime2<sup>4</sup>|  
|SQLSRV_SQLTYPE_DATETIMEOFFSET|datetimeoffset<sup>4</sup>|  
|SQLSRV_SQLTYPE_DECIMAL|décimal<sup>5</sup>|
|SQLSRV_SQLTYPE_DECIMAL($precision, $scale)|Décimal|  
|SQLSRV_SQLTYPE_FLOAT|float|  
|SQLSRV_SQLTYPE_IMAGE|image<sup>1</sup>|  
|SQLSRV_SQLTYPE_INT|int|  
|SQLSRV_SQLTYPE_MONEY|money| 
|SQLSRV_SQLTYPE_NCHAR|NCHAR<sup>5</sup>|   
|SQLSRV_SQLTYPE_NCHAR($charCount)|NCHAR|  
|SQLSRV_SQLTYPE_NUMERIC|numérique<sup>5</sup>|
|SQLSRV_SQLTYPE_NUMERIC($precision, $scale)|numérique|  
|SQLSRV_SQLTYPE_NVARCHAR|nvarchar<sup>5</sup>|  
|SQLSRV_SQLTYPE_NVARCHAR($charCount)|nvarchar|  
|SQLSRV_SQLTYPE_NVARCHAR(’max’)|nvarchar(MAX)|  
|SQLSRV_SQLTYPE_NTEXT|ntext<sup>2</sup>|  
|SQLSRV_SQLTYPE_REAL|real|  
|SQLSRV_SQLTYPE_SMALLDATETIME|smalldatetime|  
|SQLSRV_SQLTYPE_SMALLINT|smallint|  
|SQLSRV_SQLTYPE_SMALLMONEY|smallmoney|  
|SQLSRV_SQLTYPE_TEXT|text<sup>3</sup>|  
|SQLSRV_SQLTYPE_TIME|time<sup>4</sup>|  
|SQLSRV_SQLTYPE_TIMESTAMP|TIMESTAMP|  
|SQLSRV_SQLTYPE_TINYINT|tinyint|  
|SQLSRV_SQLTYPE_UNIQUEIDENTIFIER|uniqueidentifier|  
|SQLSRV_SQLTYPE_UDT|UDT|  
|SQLSRV_SQLTYPE_VARBINARY|varbinary<sup>5</sup>|  
|SQLSRV_SQLTYPE_VARBINARY($byteCount)|varbinary|  
|SQLSRV_SQLTYPE_VARBINARY(’max’)|varbinary(MAX)|  
|SQLSRV_SQLTYPE_VARCHAR|varchar<sup>5</sup>|  
|SQLSRV_SQLTYPE_VARCHAR($charCount)|varchar|  
|SQLSRV_SQLTYPE_VARCHAR(’max’)|varchar(MAX)|  
|SQLSRV_SQLTYPE_XML|xml|  
  
1.  Il s’agit d’un type hérité mappé sur le type varbinary(max).  
  
2.  Il s’agit d’un type hérité mappé sur le type nvarchar plus récent.  
  
3.  Il s’agit d’un type hérité mappé sur le type varchar plus récent.  
  
4.  La prise en charge de ce type a été ajouté dans la version 1.1 du [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  

5.  Ces constantes doivent être utilisées dans les opérations de comparaison de type et ne pas remplacer les constantes de type fonction avec une syntaxe similaire. Paramètres de liaison, vous devez utiliser l’une des constantes de type fonction.

  
Le tableau suivant répertorie les constantes SQLTYPE qui acceptent des paramètres, ainsi que la plage des valeurs autorisées pour ces paramètres.  
  
|SQLTYPE|Paramètre|Plage autorisée pour le paramètre|  
|-----------|-------------|---------------------------------|  
|SQLSRV_SQLTYPE_CHAR,<br /><br />SQLSRV_SQLTYPE_VARCHAR|charCount|1 - 8000|  
|SQLSRV_SQLTYPE_NCHAR,<br /><br />SQLSRV_SQLTYPE_NVARCHAR|charCount|1 - 4000|  
|SQLSRV_SQLTYPE_BINARY,<br /><br />SQLSRV_SQLTYPE_VARBINARY|byteCount|1 - 8000|  
|SQLSRV_SQLTYPE_DECIMAL,<br /><br />SQLSRV_SQLTYPE_NUMERIC|precision|1 - 38|  
|SQLSRV_SQLTYPE_DECIMAL,<br /><br />SQLSRV_SQLTYPE_NUMERIC|scale|1 - précision|  
  
### <a name="transaction-isolation-level-constants"></a>Constantes de niveau d’isolation de la transaction  
La clé **TransactionIsolation** , qui est utilisé avec [sqlsrv_connect](../../connect/php/sqlsrv-connect.md), accepte l’une des constantes suivantes :  
  
-   SQLSRV_TXN_READ_UNCOMMITTED  
  
-   SQLSRV_TXN_READ_COMMITTED  
  
-   SQLSRV_TXN_REPEATABLE_READ  
  
-   SQLSRV_TXN_SNAPSHOT  
  
-   SQLSRV_TXN_SERIALIZABLE  
  
### <a name="cursor-and-scrolling-constants"></a>Constantes de défilement et curseur  
Les constantes suivantes spécifient le type de curseur que vous pouvez utiliser dans un jeu de résultats :  
  
-   SQLSRV_CURSOR_FORWARD  
  
-   SQLSRV_CURSOR_STATIC  
  
-   SQLSRV_CURSOR_DYNAMIC  
  
-   SQLSRV_CURSOR_KEYSET  
  
-   SQLSRV_CURSOR_CLIENT_BUFFERED  
  
Les constantes suivantes spécifient la ligne à sélectionner dans le jeu de résultats :  
  
-   SQLSRV_SCROLL_NEXT  
  
-   SQLSRV_SCROLL_PRIOR  
  
-   SQLSRV_SCROLL_FIRST  
  
-   SQLSRV_SCROLL_LAST  
  
-   SQLSRV_SCROLL_ABSOLUTE  
  
-   SQLSRV_SCROLL_RELATIVE  
  
Pour plus d’informations sur l’utilisation de ces constantes, consultez [Specifying a Cursor Type and Selecting Rows](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  
  
