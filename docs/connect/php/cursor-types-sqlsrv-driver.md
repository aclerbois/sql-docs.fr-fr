---
title: Types de curseurs (pilote SQLSRV) | Documents Microsoft
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
ms.assetid: 8472d839-8124-4a62-a83c-7e771b0d4962
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ed6b502b0d8b2034624518344c78ed0195dce6b4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cursor-types-sqlsrv-driver"></a>Types de curseurs (pilote SQLSRV)
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Le pilote SQLSRV vous permet de créer un jeu de résultats avec les lignes auxquelles vous pouvez accéder dans n’importe quel ordre, selon le type de curseur.  Cette rubrique traite (mis en mémoire tampon) côté client et côté serveur (sans tampon) les curseurs.  
  
## <a name="cursor-types"></a>Types de curseurs  
Lorsque vous créez un jeu de résultats avec [sqlsrv_query](../../connect/php/sqlsrv-query.md) ou [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md), vous pouvez spécifier le type de curseur. Par défaut, un curseur avant uniquement est utilisé, ce qui vous permet de déplacer une ligne à la fois en commençant à la première ligne du résultat défini jusqu'à ce que vous atteignez la fin du jeu de résultats.  
  
Vous pouvez créer un jeu de résultats avec un curseur de défilement, ce qui vous permet d’accéder à toute ligne dans le jeu de résultats, dans n’importe quel ordre. Le tableau suivant répertorie les valeurs qui peuvent être passés à la **Scrollable** option sqlsrv_query ou sqlsrv_prepare.  
  
|Option| Description|  
|----------|---------------|  
|SQLSRV_CURSOR_FORWARD|Vous permet de déplacer une ligne à la fois en commençant à la première ligne du résultat défini jusqu'à ce que vous atteignez la fin du jeu de résultats.<br /><br />Il s’agit du type de curseur par défaut.<br /><br />[sqlsrv_num_rows](../../connect/php/sqlsrv-num-rows.md) renvoie une erreur pour les jeux de résultats créé avec ce type de curseur.<br /><br />**transférer** est la forme abrégée de SQLSRV_CURSOR_FORWARD.|  
|SQLSRV_CURSOR_STATIC|Permet d’accéder aux lignes dans n’importe quel ordre, mais ne reflète pas les modifications dans la base de données.<br /><br />**statique** est la forme abrégée de SQLSRV_CURSOR_STATIC.|  
|SQLSRV_CURSOR_DYNAMIC|Vous permet de vous accéder aux lignes dans n’importe quel ordre et reflète les modifications dans la base de données.<br /><br />[sqlsrv_num_rows](../../connect/php/sqlsrv-num-rows.md) renvoie une erreur pour les jeux de résultats créé avec ce type de curseur.<br /><br />**dynamique** est la forme abrégée de SQLSRV_CURSOR_DYNAMIC.|  
|SQLSRV_CURSOR_KEYSET|Vous permet de vous accéder aux lignes dans n’importe quel ordre. Toutefois, un curseur keyset ne pas mettre à jour le nombre de lignes si une ligne est supprimée de la table (une ligne supprimée est retournée sans valeurs).<br /><br />**jeu de clés** est la forme abrégée de SQLSRV_CURSOR_KEYSET.|  
|SQLSRV_CURSOR_CLIENT_BUFFERED|Vous permet de vous accéder aux lignes dans n’importe quel ordre. Crée une requête de curseur côté client.<br /><br />**mise en mémoire tampon** est la forme abrégée de SQLSRV_CURSOR_CLIENT_BUFFERED.|  
  
Si une requête génère plusieurs jeux de résultats, le **Scrollable** option s’applique à tous les jeux de résultats.  
  
## <a name="selecting-rows-in-a-result-set"></a>Sélection de lignes dans un jeu de résultats  
Après avoir créé un jeu de résultats, vous pouvez utiliser [sqlsrv_fetch](../../connect/php/sqlsrv-fetch.md), [sqlsrv_fetch_array](../../connect/php/sqlsrv-fetch-array.md), ou [sqlsrv_fetch_object](../../connect/php/sqlsrv-fetch-object.md) pour spécifier une ligne.  
  
Le tableau suivant décrit les valeurs que vous pouvez spécifier dans le *ligne* paramètre.  
  
|Paramètre| Description|  
|-------------|---------------|  
|SQLSRV_SCROLL_NEXT|Spécifie la ligne suivante. Ceci est la valeur par défaut, si vous ne spécifiez pas le *ligne* paramètre pour un jeu de résultats déroulables.|  
|SQLSRV_SCROLL_PRIOR|Spécifie la ligne avant la ligne actuelle.|  
|SQLSRV_SCROLL_FIRST|Spécifie la première ligne du jeu de résultats.|  
|SQLSRV_SCROLL_LAST|Spécifie la dernière ligne du jeu de résultats.|  
|SQLSRV_SCROLL_ABSOLUTE|Spécifie la ligne spécifiée avec la *offset* paramètre.|  
|SQLSRV_SCROLL_RELATIVE|Spécifie la ligne spécifiée avec la *offset* paramètre à partir de la ligne actuelle.|  
  
## <a name="server-side-cursors-and-the-sqlsrv-driver"></a>Les curseurs côté serveur et le pilote SQLSRV  
L’exemple suivant montre l’effet des curseurs différents. Sur la ligne 33 de l’exemple, vous consultez la première des trois instructions de requête qui spécifient des curseurs différents.  Parmi les instructions de requête sont mis en commentaire. Chaque fois que vous exécutez le programme, utilisez un autre type de curseur pour voir l’effet de la mise à jour de la base de données de ligne 47.  
  
```  
<?php  
$server = "server_name";  
$conn = sqlsrv_connect( $server, array( 'Database' => 'test' ));  
if ( $conn === false ) {  
   die( print_r( sqlsrv_errors(), true ));  
}  
  
$stmt = sqlsrv_query( $conn, "DROP TABLE dbo.ScrollTest" );  
if ( $stmt !== false ) {   
   sqlsrv_free_stmt( $stmt );   
}  
  
$stmt = sqlsrv_query( $conn, "CREATE TABLE ScrollTest (id int, value char(10))" );  
if ( $stmt === false ) {  
   die( print_r( sqlsrv_errors(), true ));  
}  
  
$stmt = sqlsrv_query( $conn, "INSERT INTO ScrollTest (id, value) VALUES(?,?)", array( 1, "Row 1" ));  
if ( $stmt === false ) {  
   die( print_r( sqlsrv_errors(), true ));  
}  
  
$stmt = sqlsrv_query( $conn, "INSERT INTO ScrollTest (id, value) VALUES(?,?)", array( 2, "Row 2" ));  
if ( $stmt === false ) {  
   die( print_r( sqlsrv_errors(), true ));  
}  
  
$stmt = sqlsrv_query( $conn, "INSERT INTO ScrollTest (id, value) VALUES(?,?)", array( 3, "Row 3" ));  
if ( $stmt === false ) {  
   die( print_r( sqlsrv_errors(), true ));  
}  
  
$stmt = sqlsrv_query( $conn, "SELECT * FROM ScrollTest", array(), array( "Scrollable" => 'keyset' ));  
// $stmt = sqlsrv_query( $conn, "SELECT * FROM ScrollTest", array(), array( "Scrollable" => 'dynamic' ));  
// $stmt = sqlsrv_query( $conn, "SELECT * FROM ScrollTest", array(), array( "Scrollable" => 'static' ));  
  
$rows = sqlsrv_has_rows( $stmt );  
if ( $rows != true ) {  
   die( "Should have rows" );  
}  
  
$result = sqlsrv_fetch( $stmt, SQLSRV_SCROLL_LAST );  
$field1 = sqlsrv_get_field( $stmt, 0 );  
$field2 = sqlsrv_get_field( $stmt, 1 );  
echo "\n$field1 $field2\n";  
  
$stmt2 = sqlsrv_query( $conn, "delete from ScrollTest where id = 3" );  
// or  
// $stmt2 = sqlsrv_query( $conn, "UPDATE ScrollTest SET id = 4 WHERE id = 3" );  
if ( $stmt2 !== false ) {   
   sqlsrv_free_stmt( $stmt2 );   
}  
  
$result = sqlsrv_fetch( $stmt, SQLSRV_SCROLL_LAST );  
$field1 = sqlsrv_get_field( $stmt, 0 );  
$field2 = sqlsrv_get_field( $stmt, 1 );  
echo "\n$field1 $field2\n";  
  
sqlsrv_free_stmt( $stmt );  
sqlsrv_close( $conn );  
?>  
```  
  
## <a name="client-side-cursors-and-the-sqlsrv-driver"></a>Curseurs côté client et le pilote SQLSRV  
Curseurs côté client sont une fonctionnalité ajoutée dans la version 3.0 de la [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] qui vous permet de mettre en cache un ensemble de résultats en mémoire. Nombre de lignes est disponible une fois que la requête est exécutée lors de l’utilisation d’un curseur côté client.  
  
Curseurs côté client doivent être utilisés pour les jeux de résultats de petite à moyenne taille. Utilisez les curseurs côté serveur pour les jeux de résultats volumineux.  
  
Une requête retourne false si la mémoire tampon n’est pas suffisamment grande pour contenir le jeu de résultats entier. Vous pouvez augmenter la taille de mémoire tampon jusqu'à la limite de mémoire PHP.  
  
À l’aide du pilote SQLSRV, vous pouvez configurer la taille de la mémoire tampon qui contient le jeu de résultats avec le paramètre ClientBufferMaxKBSize pour [sqlsrv_configure](../../connect/php/sqlsrv-configure.md). [sqlsrv_get_config](../../connect/php/sqlsrv-get-config.md) retourne la valeur de ClientBufferMaxKBSize. Vous pouvez également définir la taille maximale de mémoire tampon dans le fichier php.ini avec sqlsrv. ClientBufferMaxKBSize (par exemple, sqlsrv. ClientBufferMaxKBSize = 1024).  
  
L’exemple suivant montre :  
  
-   Nombre de lignes est toujours disponible avec un curseur côté client.  
  
-   Utilisation de curseurs côté client et d’instructions par lots.  
  
```  
<?php  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
  
if ( $conn === false ) {  
   echo "Could not connect.\n";  
   die( print_r( sqlsrv_errors(), true));  
}  
  
$tsql = "select * from HumanResources.Department";  
  
// Execute the query with client-side cursor.  
$stmt = sqlsrv_query($conn, $tsql, array(), array("Scrollable"=>"buffered"));  
if (! $stmt) {  
   echo "Error in statement execution.\n";  
   die( print_r( sqlsrv_errors(), true));  
}  
  
// row count is always available with a client-side cursor  
$row_count = sqlsrv_num_rows( $stmt );  
echo "\nRow count = $row_count\n";  
  
// Move to a specific row in the result set.  
$row = sqlsrv_fetch($stmt, SQLSRV_SCROLL_FIRST);  
$EmployeeID = sqlsrv_get_field( $stmt, 0);  
echo "Employee ID = $EmployeeID \n";  
  
// Client-side cursor and batch statements  
$tsql = "select top 2 * from HumanResources.Employee;Select top 3 * from HumanResources.EmployeeAddress";  
  
$stmt = sqlsrv_query($conn, $tsql, array(), array("Scrollable"=>"buffered"));  
if (! $stmt) {  
   echo "Error in statement execution.\n";  
   die( print_r( sqlsrv_errors(), true));  
}  
  
$row_count = sqlsrv_num_rows( $stmt );  
echo "\nRow count for first result set = $row_count\n";  
  
$row = sqlsrv_fetch($stmt, SQLSRV_SCROLL_FIRST);  
$EmployeeID = sqlsrv_get_field( $stmt, 0);  
echo "Employee ID = $EmployeeID \n";  
  
sqlsrv_next_result($stmt);  
  
$row_count = sqlsrv_num_rows( $stmt );  
echo "\nRow count for second result set = $row_count\n";  
  
$row = sqlsrv_fetch($stmt, SQLSRV_SCROLL_LAST);  
$EmployeeID = sqlsrv_get_field( $stmt, 0);  
echo "Employee ID = $EmployeeID \n";  
?>  
```  
  
L’exemple suivant montre un curseur côté client à l’aide de [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md).  
  
```  
<?php  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
  
if ( $conn === false ) {  
   echo "Could not connect.\n";  
   die( print_r( sqlsrv_errors(), true));  
}  
  
$tsql = "select * from HumanResources.Employee";  
$stmt = sqlsrv_prepare( $conn, $tsql, array(), array("Scrollable"=>SQLSRV_CURSOR_CLIENT_BUFFERED));  
  
if (! $stmt ) {  
   echo "Statement could not be prepared.\n";  
   die( print_r( sqlsrv_errors(), true));  
}  
  
sqlsrv_execute( $stmt);  
  
$row_count = sqlsrv_num_rows( $stmt );  
if ($row_count)  
   echo "\nRow count = $row_count\n";  
  
$row = sqlsrv_fetch($stmt, SQLSRV_SCROLL_FIRST);  
if ($row ) {  
   $EmployeeID = sqlsrv_get_field( $stmt, 0);  
   echo "Employee ID = $EmployeeID \n";  
}  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Spécification d’un type de curseur et sélection de lignes](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md)  
  
