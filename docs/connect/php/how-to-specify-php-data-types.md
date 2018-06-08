---
title: 'Comment : spécifier les Types de données PHP | Documents Microsoft'
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
- converting data types
- streaming data
ms.assetid: fee6e6b8-aad9-496b-84a2-18d2950470a4
caps.latest.revision: 32
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3fdd7d2eee34303c466da0445233109b707c6645
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="how-to-specify-php-data-types"></a>Procédure : spécifier des types de données PHP
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Quand vous utilisez le pilote PDO_SQLSRV, vous pouvez spécifier le type de données PHP lors de la récupération des données à partir du serveur avec PDOStatement::bindColumn et PDOStatement::bindParam. Pour plus d’informations, consultez [PDOStatement::bindColumn](../../connect/php/pdostatement-bindcolumn.md) et [PDOStatement::bindParam](../../connect/php/pdostatement-bindparam.md) .  
  
Les étapes suivantes récapitulent la manière de spécifier des types de données PHP lors de la récupération des données à partir du serveur à l’aide du pilote SQLSRV :  
  
1.  Configurer et exécuter une requête Transact-SQL avec [sqlsrv_query](../../connect/php/sqlsrv-query.md) ou la combinaison de [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md)/[sqlsrv_execute](../../connect/php/sqlsrv-execute.md).  
  
2.  Rendez une ligne de données disponible pour la lecture avec [sqlsrv_fetch](../../connect/php/sqlsrv-fetch.md).  
  
3.  Récupérez les données de champ d’une ligne retournée à l’aide de [sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md) avec le type de données PHP souhaité spécifié en tant que troisième paramètre facultatif. Si le troisième paramètre facultatif n’est pas spécifié, les données sont retournées selon les types PHP par défaut. Pour plus d’informations sur les types de retour PHP par défaut, consultez [Default PHP Data Types](../../connect/php/default-php-data-types.md).  
  
    Pour plus d’informations sur les constantes utilisées pour spécifier le type de données PHP, consultez la section PHPTYPE de [constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).  
  
## <a name="example"></a>Exemple  
L’exemple suivant récupère des lignes de la table *Production.ProductReview* de la base de données AdventureWorks. Dans chaque ligne retournée, le *ReviewDate* champ est récupéré sous forme de chaîne et le *commentaires* champ est récupéré sous la forme d’un flux de données. Les données de flux apparaissent à l’aide de la fonction [fpassthru](http://php.net/manual/en/function.fpassthru.php) PHP.  
  
L’exemple part du principe que SQL Server et le [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) base de données sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
```  
<?php  
/*Connect to the local server using Windows Authentication and specify  
the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Set up the Transact-SQL query. */  
$tsql = "SELECT ReviewerName,   
                ReviewDate,  
                Rating,   
                Comments   
         FROM Production.ProductReview   
         WHERE ProductID = ?   
         ORDER BY ReviewDate DESC";  
  
/* Set the parameter value. */  
$productID = 709;  
$params = array( $productID);  
  
/* Execute the query. */  
$stmt = sqlsrv_query($conn, $tsql, $params);  
if( $stmt === false )  
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Retrieve and display the data. The first and third fields are  
retrieved according to their default types, strings. The second field  
is retrieved as a string with 8-bit character encoding. The fourth  
field is retrieved as a stream with 8-bit character encoding.*/  
while ( sqlsrv_fetch( $stmt))  
{  
   echo "Name: ".sqlsrv_get_field( $stmt, 0 )."\n";  
   echo "Date: ".sqlsrv_get_field( $stmt, 1,   
                       SQLSRV_PHPTYPE_STRING( SQLSRV_ENC_CHAR))."\n";  
   echo "Rating: ".sqlsrv_get_field( $stmt, 2 )."\n";  
   echo "Comments: ";  
   $comments = sqlsrv_get_field( $stmt, 3,   
                            SQLSRV_PHPTYPE_STREAM(SQLSRV_ENC_CHAR));  
   fpassthru( $comments);  
   echo "\n";   
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
Dans l’exemple, la récupération du deuxième champ (*ReviewDate*) comme une chaîne préserve la précision à la milliseconde du type de données DATETIME SQL Server. Par défaut, le type de données DATETIME SQL Server est récupéré sous la forme d’un objet DateTime PHP dans lequel la précision à la milliseconde est perdue.  
  
La récupération du quatrième champ (*commentaires*) comme un flux de données est à des fins de démonstration. Par défaut, le type de données SQL Server nvarchar(3850) est récupérée sous la forme d’une chaîne, ce qui est acceptable dans la plupart des situations.  
  
> [!NOTE]  
> La fonction [sqlsrv_field_metadata](../../connect/php/sqlsrv-field-metadata.md) fournit un moyen d’obtenir des informations de champ, ainsi que des informations de type, avant d’exécuter une requête.  
  
## <a name="see-also"></a>Voir aussi  
[Récupération de données](../../connect/php/retrieving-data.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)

[Guide pratique pour récupérer des paramètres de sortie à l’aide du pilote SQLSRV](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md)

[How to: Retrieve Input and Output Parameters Using the SQLSRV Driver](../../connect/php/how-to-retrieve-input-and-output-parameters-using-the-sqlsrv-driver.md)  
  
