---
title: sqlsrv_prepare | Documents Microsoft
ms.custom: ''
ms.date: 05/22/2018
ms.prod: sql
ms.prod_service: connectivity
ms.component: php
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- sqlsrv_prepare
apitype: NA
helpviewer_keywords:
- executing queries
- API Reference, sqlsrv_prepare
- sqlsrv_prepare
ms.assetid: 8c74c697-3296-4f5d-8fb9-e361f53f19a6
caps.latest.revision: 52
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d05dadf16e96589c2f16b7d31580cab61650b9e6
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34563917"
---
# <a name="sqlsrvprepare"></a>sqlsrv_prepare
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Crée une ressource d’instruction associée à la connexion spécifiée. Cette fonction est utile pour exécuter plusieurs requêtes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_prepare(resource $conn, string $tsql [, array $params [, array $options]])  
```  
  
#### <a name="parameters"></a>Paramètres  
*$conn*: ressource de connexion associée à l’instruction créée.  
  
*$tsql*: expression Transact-SQL qui correspond à l’instruction créée.  
  
*$params* [facultatif] : un **tableau** des valeurs qui correspondent aux paramètres d’une requête paramétrable. Chaque élément du tableau peut être l’un des éléments suivants :
  
-   Valeur littérale.  
  
-   Référence à une variable PHP.  
  
-   Objet **array** avec la structure suivante :  
  
    ```  
    array(&$value [, $direction [, $phpType [, $sqlType]]])  
    ```  
  
    > [!NOTE]  
    > Les variables passées en tant que paramètres de requête doivent être passées par référence plutôt que par valeur. Par exemple, passez `&$myVariable` plutôt que `$myVariable`. Un avertissement PHP est déclenché lorsqu’une requête avec des paramètres par valeur est exécutée.  
  
    Le tableau suivant décrit ces éléments de tableau :  
  
    |Élément|Description|  
    |-----------|---------------|  
    |*&$value*|Valeur littérale ou référence à une variable PHP.|  
    |*$direction*[facultatif]|Une des valeurs suivantes **SQLSRV_PARAM_\***  les constantes utilisées pour indiquer la direction du paramètre : **SQLSRV_PARAM_IN**, **SQLSRV_PARAM_OUT**, **SQLSRV_PARAM_INOUT**. La valeur par défaut est **SQLSRV_PARAM_IN**.<br /><br />Pour plus d’informations sur les constantes PHP, consultez [constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |*$phpType*[facultatif]|A **SQLSRV_PHPTYPE_\***  constante qui spécifie le type de données PHP de la valeur retournée.|  
    |*$sqlType*[facultatif]|A **SQLSRV_SQLTYPE_\***  constante qui spécifie le type de données SQL Server de la valeur d’entrée.|  
  
*$options* [facultatif] : tableau associatif qui définit les propriétés de la requête. Le tableau suivant répertorie les clés prises en charge et les valeurs correspondantes :  
  
|Key|Valeurs prises en charge|Description|  
|-------|--------------------|---------------|  
|QueryTimeout|Valeur d'entier positif.|Définit le délai d’expiration de la requête, en secondes. Par défaut, le pilote attend indéfiniment pour obtenir les résultats.|  
|SendStreamParamsAtExec|**true** ou **false**<br /><br />La valeur par défaut est **true**.|Configure le pilote pour envoyer tous les flux de données lors de l’exécution (**true**), ou pour envoyer des données de flux de données en segments (**false**). La valeur par défaut est **true**. Pour plus d’informations, consultez [sqlsrv_send_stream_data](../../connect/php/sqlsrv-send-stream-data.md).|  
|Défilement|SQLSRV_CURSOR_FORWARD<br /><br />SQLSRV_CURSOR_STATIC<br /><br />SQLSRV_CURSOR_DYNAMIC<br /><br />SQLSRV_CURSOR_KEYSET<br /><br />SQLSRV_CURSOR_CLIENT_BUFFERED|Pour plus d’informations sur ces valeurs, consultez [Spécification d’un type de curseur et sélection de lignes](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).|  
  
## <a name="return-value"></a>Valeur de retour  
Ressource d’instruction. Si la ressource d’instruction ne peut pas être créée, la valeur **false** est retournée.  
  
## <a name="remarks"></a>Notes  
Quand vous préparez une instruction qui utilise des variables comme paramètres, les variables sont liées à l’instruction. Cela signifie que si vous mettez à jour les valeurs des variables, la prochaine fois que vous exécuterez l’instruction elle s’exécutera avec les valeurs de paramètres mises à jour.  
  
La combinaison de **sqlsrv_prepare** et **sqlsrv_execute** sépare la préparation et l’exécution de l’instruction en deux appels de fonction. De plus, elle peut être utilisée pour exécuter des requêtes paramétrables. Cette fonction est idéale pour exécuter une instruction plusieurs fois avec différentes valeurs de paramètres pour chaque exécution.  
  
Pour découvrir d’autres stratégies d’écriture et de lecture de grandes quantités d’informations, consultez [Batches of SQL Statements](../../odbc/reference/develop-app/batches-of-sql-statements.md) et [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
Pour plus d’informations, consultez [Procédure : récupérer des paramètres de sortie à l’aide du pilote SQLSRVr](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md).  
  
## <a name="example"></a>Exemple  
L’exemple suivant prépare et exécute une instruction. L’instruction, lors de l’exécution (voir [sqlsrv_execute](../../connect/php/sqlsrv-execute.md)), met à jour un champ dans le *Sales.SalesOrderDetail* table de base de données AdventureWorks. L’exemple part du principe que SQL Server et le [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) base de données sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Set up Transact-SQL query. */  
$tsql = "UPDATE Sales.SalesOrderDetail   
         SET OrderQty = ?   
         WHERE SalesOrderDetailID = ?";  
  
/* Assign parameter values. */  
$param1 = 5;  
$param2 = 10;  
$params = array(&$param1, &$param2);  
  
/* Prepare the statement. */  
if ($stmt = sqlsrv_prepare($conn, $tsql, $params)) {
    echo "Statement prepared.\n";  
} else {  
    echo "Statement could not be prepared.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Execute the statement. */  
if (sqlsrv_execute($stmt)) {  
    echo "Statement executed.\n";  
} else {  
    echo "Statement could not be executed.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  
?>  
```  
  
## <a name="example"></a>Exemple  
L’exemple suivant montre comment préparer une instruction puis la réexécuter avec des valeurs de paramètres différentes. L’exemple met à jour la colonne *OrderQty* de la table *Sales.SalesOrderDetail* dans la base de données AdventureWorks. Une fois les mises à jour effectuées, la base de données est interrogée pour vérifier que les mises à jour ont réussi. L’exemple part du principe que SQL Server et le [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) base de données sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
     echo "Could not connect.\n";  
     die(print_r(sqlsrv_errors(), true));  
}  
  
/* Define the parameterized query. */  
$tsql = "UPDATE Sales.SalesOrderDetail  
         SET OrderQty = ?  
         WHERE SalesOrderDetailID = ?";  
  
/* Initialize parameters and prepare the statement. Variables $qty  
and $id are bound to the statement, $stmt1. */  
$qty = 0; $id = 0;  
$stmt1 = sqlsrv_prepare($conn, $tsql, array(&$qty, &$id));  
if ($stmt1) {  
    echo "Statement 1 prepared.\n";  
} else {  
    echo "Error in statement preparation.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Set up the SalesOrderDetailID and OrderQty information. This array  
maps the order ID to order quantity in key=>value pairs. */  
$orders = array(1=>10, 2=>20, 3=>30);  
  
/* Execute the statement for each order. */  
foreach ($orders as $id => $qty) {  
    // Because $id and $qty are bound to $stmt1, their updated  
    // values are used with each execution of the statement.   
    if (sqlsrv_execute($stmt1) === false) {  
        echo "Error in statement execution.\n";  
        die(print_r(sqlsrv_errors(), true));  
    }  
}  
echo "Orders updated.\n";  
  
/* Free $stmt1 resources.  This allows $id and $qty to be bound to a different statement.*/  
sqlsrv_free_stmt($stmt1);  
  
/* Now verify that the results were successfully written by selecting   
the newly inserted rows. */  
$tsql = "SELECT OrderQty   
         FROM Sales.SalesOrderDetail   
         WHERE SalesOrderDetailID = ?";  
  
/* Prepare the statement. Variable $id is bound to $stmt2. */  
$stmt2 = sqlsrv_prepare($conn, $tsql, array(&$id));  
if ($stmt2) {  
    echo "Statement 2 prepared.\n";  
} else {  
    echo "Error in statement preparation.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Execute the statement for each order. */  
foreach (array_keys($orders) as $id)  
{  
    /* Because $id is bound to $stmt2, its updated value   
    is used with each execution of the statement. */  
    if (sqlsrv_execute($stmt2)) {  
        sqlsrv_fetch($stmt2);  
        $quantity = sqlsrv_get_field($stmt2, 0);  
        echo "Order $id is for $quantity units.\n";  
    } else {  
        echo "Error in statement execution.\n";  
        die(print_r(sqlsrv_errors(), true));  
    }  
}  
  
/* Free $stmt2 and connection resources. */  
sqlsrv_free_stmt($stmt2);  
sqlsrv_close($conn);  
?>  
```  
  
> [!NOTE]
> Il est recommandé d’utiliser des chaînes en tant qu’entrées lors de la liaison de valeurs à un [colonne decimal ou numeric](https://docs.microsoft.com/en-us/sql/t-sql/data-types/decimal-and-numeric-transact-sql) pour vérifier la précision et l’exactitude que PHP est limitée à la précision pour [nombres à virgule flottante](http://php.net/manual/en/language.types.float.php). Va de même pour des colonnes bigint, en particulier lorsque les valeurs sont en dehors de la plage d’un [entier](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).

## <a name="example"></a>Exemple  
Cet exemple de code montre comment lier une valeur décimale en tant que paramètre d’entrée.  

```
<?php
$serverName = "(local)";
$connectionInfo = array("Database"=>"YourTestDB");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  

// Assume TestTable exists with a decimal field 
$input = "9223372036854.80000";
$params = array($input);
$stmt = sqlsrv_prepare($conn, "INSERT INTO TestTable (DecimalCol) VALUES (?)", $params);
sqlsrv_execute($stmt);

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Guide pratique pour exécuter des requêtes paramétrables](../../connect/php/how-to-perform-parameterized-queries.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)

[Guide pratique pour envoyer des données sous forme de flux](../../connect/php/how-to-send-data-as-a-stream.md)

[Utilisation de paramètres directionnels](../../connect/php/using-directional-parameters.md)

[Récupération de données](../../connect/php/retrieving-data.md)

[Mise à jour des données &#40;pilotes Microsoft SQL Server pour PHP&#41;](../../connect/php/updating-data-microsoft-drivers-for-php-for-sql-server.md)

