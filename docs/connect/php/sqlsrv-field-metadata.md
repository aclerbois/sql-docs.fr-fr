---
title: sqlsrv_field_metadata | Documents Microsoft
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
apiname:
- sqlsrv_field_metadata
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_field_metadata
- sqlsrv_field_metadata
ms.assetid: c02f6942-0484-4567-a78e-fe8aa2053536
caps.latest.revision: 34
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 43cb1b1a028d645c6c1de6e89c292c475eb00dc5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsrvfieldmetadata"></a>sqlsrv_field_metadata
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère les métadonnées des champs d’une instruction préparée. Pour plus d’informations sur la préparation d’une instruction, consultez [sqlsrv_query](../../connect/php/sqlsrv-query.md) ou [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md). Notez que **sqlsrv_field_metadata** peut être appelée sur une instruction préparée, avant ou après l’exécution.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_field_metadata( resource $stmt)  
```  
  
#### <a name="parameters"></a>Paramètres  
*$stmt*: ressource d’instruction pour laquelle des métadonnées de champ sont recherchées.  
  
## <a name="return-value"></a>Valeur retournée  
**Tableau** de tableaux ou **false**. Le tableau se compose d’un tableau pour chaque champ dans le jeu de résultats. Chaque sous-tableau possède des clés décrites dans le tableau ci-dessous. En cas d’erreur lors de l’extraction des métadonnées de champ, **false** est retourné.  
  
|Key| Description|  
|-------|---------------|  
|Nom|Nom de la colonne à laquelle le champ correspond.|  
|Type|Valeur numérique qui correspond à un type SQL.|  
|Taille|Nombre de caractères des champs de type caractère (char(n), varchar(n), nchar(n), nvarchar(n), XML). Nombre d’octets des champs de type binaire (binary(n), varbinary(n), UDT). **NULL** pour les autres types de données SQL Server.|  
|Précision|Précision des types dont la précision varie (réel, numérique, décimal, DateHeure2, datetimeoffset et Heure). **NULL** pour les autres types de données SQL Server.|  
|Échelle|Échelle des types dont l’échelle varie (numérique, décimal, DateHeure2, datetimeoffset et Heure). **NULL** pour les autres types de données SQL Server.|  
|Nullable|Valeur énumérée indiquant si la colonne est nullable (**SQLSRV_NULLABLE_YES**), la colonne n’est pas nullable (**SQLSRV_NULLABLE_NO**), ou il n’est pas connu si la colonne est nullable ( **SQLSRV_NULLABLE_UNKNOWN**).|  
  
Le tableau suivant fournit plus d’informations sur les clés pour chaque sous-tableau (consultez la documentation de SQL Server pour plus d’informations sur ces types) :  
  
|Type de données de SQL Server 2008|Type|Précision minimale/maximale|Échelle minimale/maximale|Taille|  
|-----------------------------|--------|----------------------|------------------|--------|  
|bigint|SQL_BIGINT (-5)|||8|  
|binary|SQL_BINARY (-2)|||0 < *n* < 8000 <sup>1</sup>|  
|bit|SQL_BIT (-7)||||  
|char|SQL_CHAR (1)|||0 < *n* < 8000 <sup>1</sup>|  
|date|SQL_TYPE_DATE (91)|10/10|0/0||  
|datetime|SQL_TYPE_TIMESTAMP (93)|23/23|3/3||  
|datetime2|SQL_TYPE_TIMESTAMP (93)|19/27|0/7||  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET (-155)|26/34|0/7||  
|Décimal|SQL_DECIMAL (3)|1/38|0/valeur de précision||  
|float|SQL_FLOAT (6)|4/8|||  
|image|SQL_LONGVARBINARY (-4)|||2 Go|  
|int|SQL_INTEGER (4)||||  
|money|SQL_DECIMAL (3)|19/19|4/4||  
|NCHAR|SQL_WCHAR (-8)|||0 < *n* < 4000 <sup>1</sup>|  
|ntext|SQL_WLONGVARCHAR (-10)|||1 Go|  
|numérique|SQL_NUMERIC (2)|1/38|0/valeur de précision||  
|nvarchar|SQL_WVARCHAR (-9)|||0 < *n* < 4000 <sup>1</sup>|  
|real|SQL_REAL (7)|4/4|||  
|smalldatetime|SQL_TYPE_TIMESTAMP (93)|16/16|0/0||  
|smallint|SQL_SMALLINT (5)|||2 octets|  
|Smallmoney|SQL_DECIMAL (3)|10/10|4/4||  
|texte|SQL_LONGVARCHAR (-1)|||2 Go|  
|time|SQL_SS_TIME2 (-154)|8/16|0/7||  
|TIMESTAMP|SQL_BINARY (-2)|||8 octets|  
|tinyint|SQL_TINYINT (-6)|||1 octet|  
|udt|SQL_SS_UDT (-151)|||variable|  
|uniqueidentifier|SQL_GUID (-11)|||16|  
|varbinary|SQL_VARBINARY (-3)|||0 < *n* < 8000 <sup>1</sup>|  
|varchar|SQL_VARCHAR (12)|||0 < *n* < 8000 <sup>1</sup>|  
|xml|SQL_SS_XML (-152)|||0|  
  
(1) Zéro (0) indique que la taille maximale est autorisée.  
  
La clé Nullable peut avoir la valeur oui ou non.  
  
## <a name="example"></a>Exemple  
L’exemple suivant crée une ressource d’instruction, puis récupère et affiche les métadonnées de champ. L’exemple part du principe que SQL Server et le [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) base de données sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Prepare the statement. */  
$tsql = "SELECT ReviewerName, Comments FROM Production.ProductReview";  
$stmt = sqlsrv_prepare( $conn, $tsql);  
  
/* Get and display field metadata. */  
foreach( sqlsrv_field_metadata( $stmt) as $fieldMetadata)  
{  
      foreach( $fieldMetadata as $name => $value)  
      {  
           echo "$name: $value\n";  
      }  
      echo "\n";  
}  
  
/* Note: sqlsrv_field_metadata can be called on any statement  
resource, pre- or post-execution. */  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Constantes &#40;pilotes Microsoft SQL Server pour PHP&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)  

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)  
  
