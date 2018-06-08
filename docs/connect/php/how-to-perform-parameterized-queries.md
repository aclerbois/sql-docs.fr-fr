---
title: 'Comment : exécuter des requêtes paramétrables | Documents Microsoft'
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.component: php
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- updating data
- parameterized queries
ms.assetid: dc7d0ede-a9b6-4ce2-977e-4d1e7ec2131c
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 60e92bc71cb0be67a9b4680da19b79e01f815353
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="how-to-perform-parameterized-queries"></a>Procédure : exécuter des requêtes paramétrables
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cette rubrique résume et illustre l’utilisation de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] pour exécuter une requête paramétrable.  
  
Les étapes pour exécuter une requête paramétrable sont au nombre de quatre :  
  
1.  Placez des points d’interrogation (?) en tant qu’espaces réservés de paramètre dans la chaîne Transact-SQL qui correspond à la requête à exécuter.  
  
2.  Initialisez ou mettez à jour les variables PHP qui correspondent aux espaces réservés dans la requête Transact-SQL.  
  
3.  Utilisez les variables PHP de l’étape 2 pour créer ou mettre à jour un tableau de valeurs de paramètre qui correspondent aux espaces réservés de paramètre dans la chaîne Transact-SQL. Les valeurs de paramètre dans le tableau doivent être dans le même ordre que les espaces réservés destiné à représenter.
  
4.  Exécutez la requête :  
  
    -   Si vous utilisez le pilote SQLSRV, utilisez [sqlsrv_query](../../connect/php/sqlsrv-query.md) ou [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md)/[sqlsrv_execute](../../connect/php/sqlsrv-execute.md).  
  
    -   Si vous utilisez le pilote PDO_SQLSRV, exécutez la requête avec [PDO::prepare](../../connect/php/pdo-prepare.md) et [PDOStatement::execute](../../connect/php/pdostatement-execute.md). Les rubriques sur [PDO::prepare](../../connect/php/pdo-prepare.md) et [PDOStatement::execute](../../connect/php/pdostatement-execute.md) comportent des exemples de code.  
  
Le reste de cette rubrique traite des requêtes paramétrables qui utilisent le pilote SQLSRV.  
  
> [!NOTE]  
> Les paramètres sont implicitement liés à l’aide de **sqlsrv_prepare**. Cela signifie que si une requête paramétrable est préparée à l’aide de **sqlsrv_prepare** et que des valeurs incluses dans le tableau de paramètres sont mises à jour, les valeurs mises à jour sont utilisées durant la prochaine exécution de la requête. Consultez le deuxième exemple inclus dans cette rubrique pour plus d’informations.  
  
## <a name="example"></a>Exemple  
L’exemple suivant met à jour la quantité d’un ID de produit spécifié dans la table *Production.ProductInventory* de la base de données AdventureWorks. La quantité et l’ID de produit sont des paramètres dans la requête UPDATE.  
  
L’exemple interroge ensuite la base de données pour vérifier que la quantité a été correctement mise à jour. L’ID de produit est un paramètre dans la requête SELECT.  
  
L’exemple part du principe que SQL Server et le [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) base de données sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
  
/* Define the Transact-SQL query.  
Use question marks as parameter placeholders. */  
$tsql1 = "UPDATE Production.ProductInventory   
          SET Quantity = ?   
          WHERE ProductID = ?";  
  
/* Initialize $qty and $productId */  
$qty = 10; $productId = 709;  
  
/* Execute the statement with the specified parameter values. */  
$stmt1 = sqlsrv_query( $conn, $tsql1, array($qty, $productId));  
if( $stmt1 === false )  
{  
     echo "Statement 1 could not be executed.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Free statement resources. */  
sqlsrv_free_stmt( $stmt1);  
  
/* Now verify the updated quantity.  
Use a question mark as parameter placeholder. */  
$tsql2 = "SELECT Quantity   
          FROM Production.ProductInventory  
          WHERE ProductID = ?";  
  
/* Execute the statement with the specified parameter value.  
Display the returned data if no errors occur. */  
$stmt2 = sqlsrv_query( $conn, $tsql2, array($productId));  
if( $stmt2 === false )  
{  
     echo "Statement 2 could not be executed.\n";  
     die( print_r(sqlsrv_errors(), true));  
}  
else  
{  
     $qty = sqlsrv_fetch_array( $stmt2);  
     echo "There are $qty[0] of product $productId in inventory.\n";  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt2);  
sqlsrv_close( $conn);  
?>  
```  
  
L’exemple précédent utilise la fonction **sqlsrv_query** pour exécuter des requêtes. Cette fonction est parfaite pour exécuter des requêtes à usage unique car elle effectue à la fois la préparation et l’exécution. La combinaison de **sqlsrv_prepare**/**sqlsrv_execute** est préférable à la réexécution d’une requête avec différentes valeurs de paramètre. Pour voir un exemple de réexécution d’une requête avec des valeurs de paramètre différentes, consultez l’exemple qui suit.  
  
## <a name="example"></a>Exemple  
L’exemple suivant illustre la liaison implicite des variables quand vous utilisez la fonction **sqlsrv_prepare** . L’exemple montre comment insérer plusieurs commandes client dans la table *Sales.SalesOrderDetail* . Le *$params* est lié à l’instruction (*$stmt*) lorsque **sqlsrv_prepare** est appelée. Avant chaque exécution d’une requête qui insère une nouvelle commande client dans la table, le tableau *$params* est mis à jour avec de nouvelles valeurs correspondant aux détails de cette commande client. L’exécution suivante de la requête utilise les nouvelles valeurs de paramètre.  
  
L’exemple part du principe que SQL Server et le [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) base de données sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
  
$tsql = "INSERT INTO Sales.SalesOrderDetail (SalesOrderID,   
                                             OrderQty,   
                                             ProductID,   
                                             SpecialOfferID,   
                                             UnitPrice)  
         VALUES (?, ?, ?, ?, ?)";  
  
/* Each sub array here will be a parameter array for a query.  
The values in each sub array are, in order, SalesOrderID, OrderQty,  
 ProductID, SpecialOfferID, UnitPrice. */  
$parameters = array( array(43659, 8, 711, 1, 20.19),  
                     array(43660, 6, 762, 1, 419.46),  
                     array(43661, 4, 741, 1, 818.70)  
                    );  
  
/* Initialize parameter values. */  
$orderId = 0;  
$qty = 0;  
$prodId = 0;  
$specialOfferId = 0;  
$price = 0.0;  
  
/* Prepare the statement. $params is implicitly bound to $stmt. */  
$stmt = sqlsrv_prepare( $conn, $tsql, array( &$orderId,  
                                             &$qty,  
                                             &$prodId,  
                                             &$specialOfferId,  
                                             &$price));  
if( $stmt === false )  
{  
     echo "Statement could not be prepared.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Execute a statement for each set of params in $parameters.  
Because $params is bound to $stmt, as the values are changed, the  
new values are used in the subsequent execution. */  
foreach( $parameters as $params)  
{  
     list($orderId, $qty, $prodId, $specialOfferId, $price) = $params;  
     if( sqlsrv_execute($stmt) === false )  
     {  
          echo "Statement could not be executed.\n";  
          die( print_r( sqlsrv_errors(), true));  
     }  
     else  
     {  
          /* Verify that the row was successfully inserted. */  
          echo "Rows affected: ".sqlsrv_rows_affected( $stmt )."\n";  
     }  
}  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Conversion de types de données](../../connect/php/converting-data-types.md)

[Considérations de sécurité pour les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/security-considerations-for-php-sql-driver.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)

[sqlsrv_rows_affected](../../connect/php/sqlsrv-rows-affected.md)  
  
