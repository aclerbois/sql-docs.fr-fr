---
title: sqlsrv_query | Documents Microsoft
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
- sqlsrv_query
apitype: NA
helpviewer_keywords:
- sqlsrv_query
- executing queries
- API Reference, sqlsrv_query
ms.assetid: 9fa7c4c8-4da8-4299-9893-f61815055aa3
caps.latest.revision: 46
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e0c1c14aaeff26111ebb66ce8aa77f9a25b599ba
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34563897"
---
# <a name="sqlsrvquery"></a>sqlsrv_query
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Prépare et exécute une instruction.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_query(resource $conn, string $tsql [, array $params [, array $options]])  
```  
  
#### <a name="parameters"></a>Paramètres  
*$conn*: ressource de connexion associée à l’instruction préparée.  
  
*$tsql*: expression Transact-SQL qui correspond à l’instruction préparée.  
  
*$params* [facultatif] : un **tableau** des valeurs qui correspondent aux paramètres d’une requête paramétrable. Chaque élément du tableau peut être l’un des éléments suivants :
  
-   Une valeur littérale.  
  
-   Une variable PHP.  
  
-   Un objet **array** avec la structure suivante :  
  
    ```  
    array($value [, $direction [, $phpType [, $sqlType]]])  
    ```  
  
    La description de chaque élément du tableau est dans le tableau suivant :  
  
    |Élément|Description|  
    |-----------|---------------|  
    |*$value*|Valeur littérale, variable PHP ou variable PHP par référence.|  
    |*$direction*[facultatif]|Une des valeurs suivantes **SQLSRV_PARAM_\***  les constantes utilisées pour indiquer la direction du paramètre : **SQLSRV_PARAM_IN**, **SQLSRV_PARAM_OUT**, **SQLSRV_PARAM_INOUT**. La valeur par défaut est **SQLSRV_PARAM_IN**.<br /><br />Pour plus d’informations sur les constantes PHP, consultez [constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |*$phpType*[facultatif]|A **SQLSRV_PHPTYPE_\***  constante qui spécifie le type de données PHP de la valeur retournée.<br /><br />Pour plus d’informations sur les constantes PHP, consultez [constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
    |*$sqlType*[facultatif]|A **SQLSRV_SQLTYPE_\***  constante qui spécifie le type de données SQL Server de la valeur d’entrée.<br /><br />Pour plus d’informations sur les constantes PHP, consultez [constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).|  
  
*$options* [facultatif] : tableau associatif qui définit les propriétés de la requête. Les clés prises en charge sont les suivantes :  
  
|Key|Valeurs prises en charge|Description|  
|-------|--------------------|---------------|  
|QueryTimeout|Valeur d'entier positif.|Définit le délai d’expiration de la requête, en secondes. Par défaut, le pilote attend indéfiniment pour obtenir les résultats.|  
|SendStreamParamsAtExec|**true** ou **false**<br /><br />La valeur par défaut est **true**.|Configure le pilote pour envoyer tous les flux de données lors de l’exécution (**true**), ou pour envoyer des données de flux de données en segments (**false**). La valeur par défaut est **true**. Pour plus d’informations, consultez [sqlsrv_send_stream_data](../../connect/php/sqlsrv-send-stream-data.md).|  
|Défilement|SQLSRV_CURSOR_FORWARD<br /><br />SQLSRV_CURSOR_STATIC<br /><br />SQLSRV_CURSOR_DYNAMIC<br /><br />SQLSRV_CURSOR_KEYSET<br /><br />SQLSRV_CURSOR_CLIENT_BUFFERED|Pour plus d’informations sur ces valeurs, consultez [Spécification d’un type de curseur et sélection de lignes](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md).|  
  
## <a name="return-value"></a>Valeur de retour  
Ressource d’instruction. Si l’instruction ne peut pas être créée et/ou exécutée, **false** est retourné.  
  
## <a name="remarks"></a>Notes  
Le **sqlsrv_query** fonction convient bien pour les requêtes à usage unique et doit être le choix par défaut pour exécuter des requêtes, sauf si des circonstances particulières s’appliquent. Cette fonction fournit une méthode simplifiée pour exécuter une requête avec une quantité minimale de code. La fonction **sqlsrv_query** effectue à la fois la préparation et l’exécution des instructions. Vous pouvez l’utiliser pour exécuter des requêtes paramétrables.  
  
Pour plus d’informations, consultez [Procédure : récupérer des paramètres de sortie à l’aide du pilote SQLSRVr](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md).  
  
## <a name="example"></a>Exemple  
Dans l’exemple suivant, une seule ligne est insérée dans la table *Sales.SalesOrderDetail* de la base de données AdventureWorks. L’exemple part du principe que SQL Server et le [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) base de données sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
> [!NOTE]  
> Bien que l’exemple suivant utilise une instruction INSERT pour illustrer l’utilisation de **sqlsrv_query** pour exécuter une seule instruction, le concept s’applique à n’importe quelle instruction Transact-SQL.  
  
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
  
/* Set up the parameterized query. */  
$tsql = "INSERT INTO Sales.SalesOrderDetail   
        (SalesOrderID,   
         OrderQty,   
         ProductID,   
         SpecialOfferID,   
         UnitPrice,   
         UnitPriceDiscount)  
        VALUES   
        (?, ?, ?, ?, ?, ?)";  
  
/* Set parameter values. */  
$params = array(75123, 5, 741, 1, 818.70, 0.00);  
  
/* Prepare and execute the query. */  
$stmt = sqlsrv_query($conn, $tsql, $params);  
if ($stmt) {  
    echo "Row successfully inserted.\n";  
} else {  
    echo "Row insertion failed.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  
?>  
```  
  
## <a name="example"></a>Exemple  
L’exemple suivant met à jour un champ dans le *Sales.SalesOrderDetail* table de base de données AdventureWorks. L’exemple part du principe que SQL Server et le [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) base de données sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
  
/* Set up the parameterized query. */  
$tsql = "UPDATE Sales.SalesOrderDetail   
         SET OrderQty = (?)   
         WHERE SalesOrderDetailID = (?)";  
  
/* Assign literal parameter values. */  
$params = array(5, 10);  
  
/* Execute the query. */  
if (sqlsrv_query($conn, $tsql, $params)) {  
    echo "Statement executed.\n";  
} else {  
    echo "Error in statement execution.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  
  
/* Free connection resources. */  
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
$stmt = sqlsrv_query($conn, "INSERT INTO TestTable (DecimalCol) VALUES (?)", $params);

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

  
