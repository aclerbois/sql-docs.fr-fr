---
title: À l’aide des Types de base de données | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d7044936-5b8c-4def-858c-28a11ef70a97
caps.latest.revision: 73
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ab39b74b4e5a2c243622bbd352a71c943e07a3d6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-basic-data-types"></a>Utilisation des types de données de base
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] utilise les types de données de base JDBC pour convertir le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données dans un format compréhensible par le langage de programmation Java et vice versa. Le pilote JDBC fournit la prise en charge de l’API JDBC 4.0, qui inclut le **SQLXML** type de données et les types de données National (Unicode), tel que **NCHAR**, **NVARCHAR**, **LONGNVARCHAR**, et **NCLOB**.  
  
## <a name="data-type-mappings"></a>Mappages de type de données  
 Le tableau suivant répertorie les mappages par défaut entre la base [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], JDBC et les types de données de langage de programmation Java :  
  
|Types SQL Server|Types JDBC (java.sql.Types)|Types langage Java|  
|----------------------|-----------------------------------|-------------------------|  
|bigint|bigint|long|  
|binary|BINARY|byte[]|  
|bit|BIT|boolean|  
|char|CHAR|Chaîne|  
|date|DATE|java.sql.Date|  
|datetime|TIMESTAMP|java.sql.Timestamp|  
|datetime2|TIMESTAMP|java.sql.Timestamp|  
|datetimeoffset (2)|microsoft.sql.Types.DATETIMEOFFSET|microsoft.sql.DateTimeOffset|  
|Décimal|DECIMAL|java.math.BigDecimal|  
|float|DOUBLE|double|  
|image|LONGVARBINARY|byte[]|  
|int|INTEGER|int|  
|money|DECIMAL|java.math.BigDecimal|  
|NCHAR|CHAR<br /><br /> NCHAR (Java SE 6.0)|Chaîne|  
|ntext|LONGVARCHAR<br /><br /> LONGNVARCHAR (Java SE 6.0)|Chaîne|  
|numérique|NUMERIC|java.math.BigDecimal|  
|nvarchar|VARCHAR<br /><br /> NVARCHAR (Java SE 6.0)|Chaîne|  
|nvarchar(max)|VARCHAR<br /><br /> NVARCHAR (Java SE 6.0)|Chaîne|  
|real|REAL|float|  
|smalldatetime|TIMESTAMP|java.sql.Timestamp|  
|smallint|SMALLINT|short|  
|smallmoney|DECIMAL|java.math.BigDecimal|  
|texte|LONGVARCHAR|Chaîne|  
|time|TIME (1)|java.sql.Time (1)|  
|timestamp|BINARY|byte[]|  
|tinyint|TINYINT|short|  
|udt|VARBINARY|byte[]|  
|uniqueidentifier|CHAR|Chaîne|  
|varbinary|VARBINARY|byte[]|  
|varbinary(max)|VARBINARY|byte[]|  
|varchar|VARCHAR|Chaîne|  
|varchar(max)|VARCHAR|Chaîne|  
|xml|LONGVARCHAR<br /><br /> LONGNVARCHAR (Java SE 6.0)|Chaîne<br /><br /> SQLXML|  
|SQLVARIANT|SQLVARIANT|Objet|  
  
 (1) pour utiliser java.sql.Time avec le temps [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type, vous devez définir le **sendTimeAsDatetime** false à la propriété connexion.  
  
 (2) vous pouvez accéder par programmation aux valeurs de **datetimeoffset** avec [classe DateTimeOffset](../../connect/jdbc/reference/datetimeoffset-class.md).  
  
 Les sections suivantes proposent des exemples d'utilisation du pilote JDBC et des types de données de base. Pour obtenir un exemple plus détaillé montrant comment utiliser les types de base de données dans une application Java, consultez [exemple de Types de données de base](../../connect/jdbc/basic-data-types-sample.md).  
  
## <a name="retrieving-data-as-a-string"></a>Extraction de données en tant que chaîne  
 Si vous devez récupérer des données à partir d’une source de données qui correspond à l’un des types de données de base JDBC pour l’affichage sous forme de chaîne, ou si des données fortement typées ne sont pas obligatoire, vous pouvez utiliser la [getString](../../connect/jdbc/reference/getstring-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) (classe), comme suit :  
  
 [!code[JDBC#UsingBasicDataTypes1](../../connect/jdbc/codesnippet/Java/using-basic-data-types_1.java)]  
  
## <a name="retrieving-data-by-data-type"></a>Extraction de données par type de données  
 Si vous devez extraire des données à partir d’une source de données, et que vous connaissez le type de données sont en cours de récupération, utilisez une de la méthode get\<Type > classe les méthodes de l’objet SQLServerResultSet, également connu sous le *méthodes getter*. Vous pouvez utiliser un nom de colonne ou un index de colonne avec la méthode get\<Type > méthodes, comme suit :  
  
 [!code[JDBC#UsingBasicDataTypes2](../../connect/jdbc/codesnippet/Java/using-basic-data-types_2.java)]  
  
> [!NOTE]  
>  Le getUnicodeStream getBigDecimal avec des méthodes de mise à l’échelle sont déconseillés et ne sont pas pris en charge par le pilote JDBC.  
  
## <a name="updating-data-by-data-type"></a>Mise à jour des données par type de données  
 Si vous devez mettre à jour la valeur d’un champ dans une source de données, utilisez une de la mise à jour\<Type > méthodes de la classe SQLServerResultSet. Dans l’exemple suivant, la [updateInt](../../connect/jdbc/reference/updateint-method-sqlserverresultset.md) méthode est utilisée conjointement avec la [updateRow](../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md) pour mettre à jour les données dans la source de données :  
  
 [!code[JDBC#UsingBasicDataTypes3](../../connect/jdbc/codesnippet/Java/using-basic-data-types_3.java)]  
  
> [!NOTE]  
>  Le pilote JDBC ne peut pas mettre à jour une colonne SQL Server avec un nom de colonne dont la longueur dépasse 127 caractères. Si une mise à jour est tentée sur une colonne dont le nom dépasse 127 caractères, une exception est levée.  
  
## <a name="updating-data-by-parameterized-query"></a>Mise à jour des données par requête paramétrable  
 Si vous devez mettre à jour des données dans une source de données à l’aide d’une requête paramétrable, vous pouvez définir le type de données des paramètres en utilisant l’une de l’ensemble de\<Type > méthodes de la [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) classe, également connu sous le *méthodes setter*. Dans l’exemple suivant, la [prepareStatement](../../connect/jdbc/reference/preparestatement-method-sqlserverconnection.md) méthode est utilisée pour précompiler la requête paramétrable, puis le [setString](../../connect/jdbc/reference/setstring-method-sqlserverpreparedstatement.md) méthode est utilisée pour définir la valeur de chaîne du paramètre avant du [executeUpdate](../../connect/jdbc/reference/executeupdate-method.md) méthode est appelée.  
  
 [!code[JDBC#UsingBasicDataTypes4](../../connect/jdbc/codesnippet/Java/using-basic-data-types_4.java)]  
  
 Pour plus d’informations sur les requêtes paramétrables, consultez [à l’aide d’une instruction SQL avec des paramètres](../../connect/jdbc/using-an-sql-statement-with-parameters.md).  
  
## <a name="passing-parameters-to-a-stored-procedure"></a>Transmission de paramètres à une procédure stockée  
 Si vous devez passer des paramètres typés dans une procédure stockée, vous pouvez définir les paramètres par index ou par nom à l’aide d’un de l’ensemble de\<Type > méthodes de la [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classe. Dans l’exemple suivant, la [prepareCall](../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md) méthode est utilisée pour configurer l’appel à la procédure stockée, puis le [setString](../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md) méthode est utilisée pour définir le paramètre pour l’appel avant le [ executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverstatement.md) méthode est appelée.  
  
 [!code[JDBC#UsingBasicDataTypes5](../../connect/jdbc/codesnippet/Java/using-basic-data-types_5.java)]  
  
> [!NOTE]  
>  Dans cet exemple, un jeu de résultats est retourné avec les résultats de l'exécution de la procédure stockée.  
  
 Pour plus d’informations sur l’utilisation du pilote JDBC avec les procédures stockées et les paramètres d’entrée, consultez [à l’aide d’une procédure stockée avec des paramètres d’entrée](../../connect/jdbc/using-a-stored-procedure-with-input-parameters.md).  
  
## <a name="retrieving-parameters-from-a-stored-procedure"></a>Extraction de paramètres d'une procédure stockée  
 Si vous devez extraire des paramètres d’une procédure stockée, vous devez tout d’abord enregistrer un paramètre out par nom ou index à l’aide de la [registerOutParameter](../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) méthode de la classe SQLServerCallableStatement, puis assignez le retourné paramètre out à une variable appropriée après l’exécution de l’appel à la procédure stockée. Dans l’exemple suivant, la méthode prepareCall est utilisée pour configurer l’appel à la procédure stockée, la méthode registerOutParameter est utilisée pour configurer le paramètre de sortie, puis le [setString](../../connect/jdbc/reference/setstring-method-sqlservercallablestatement.md) méthode est utilisée pour définir le paramètre pour le appel avant l’appel de méthode executeQuery. La valeur retournée par le paramètre de sortie de la procédure stockée est extraite à l’aide de la [getShort](../../connect/jdbc/reference/getshort-method-sqlservercallablestatement.md) (méthode).  
  
 [!code[JDBC#UsingBasicDataTypes6](../../connect/jdbc/codesnippet/Java/using-basic-data-types_6.java)]  
  
> [!NOTE]  
>  Outre le paramètre OUT retourné, un jeu de résultats peut également être retourné avec les résultats de l'exécution de la procédure stockée.  
  
 Pour plus d’informations sur l’utilisation du pilote JDBC avec les procédures stockées et les paramètres de sortie, consultez [à l’aide d’une procédure stockée avec des paramètres Output](../../connect/jdbc/using-a-stored-procedure-with-output-parameters.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation des types de données du pilote JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md)  
  
  
