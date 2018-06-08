---
title: sqlsrv_fetch_array | Documents Microsoft
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
apiname:
- sqlsrv_fetch_array
apitype: NA
helpviewer_keywords:
- sqlsrv_fetch_array
- retrieving data, as an array
- API Reference, sqlsrv_fetch_array
ms.assetid: 69270b9e-0791-42f4-856d-412da39dea63
caps.latest.revision: 52
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3a6df21ff42d0394b153aa97a0b5639fe47beec9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsrvfetcharray"></a>sqlsrv_fetch_array
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère la ligne suivante de données sous forme de tableau indexé numériquement, de tableau associatif ou les deux.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_fetch_array( resource $stmt[, int $fetchType [, row[, ]offset]])  
```  
  
#### <a name="parameters"></a>Paramètres  
*$stmt*: ressource d’instruction correspondant à une instruction exécutée.  
  
*$fetchType* [facultatif] : constante prédéfinie. Ce paramètre peut prendre l’une des valeurs répertoriées dans le tableau suivant :  
  
|Valeur| Description|  
|---------|---------------|  
|SQLSRV_FETCH_NUMERIC|La ligne de données suivante est retournée sous forme de tableau numérique.|  
|SQLSRV_FETCH_ASSOC|La ligne de données suivante est retournée sous forme de tableau associatif. Les clés de tableau sont les noms des colonnes dans le jeu de résultats.|  
|SQLSRV_FETCH_BOTH|La ligne de données suivante est retournée à la fois comme tableau numérique et comme tableau associatif. Ceci est la valeur par défaut.|  
  
*ligne* [facultatif] : ajouté dans la version 1.1. L’une des valeurs suivantes, spécifiant la ligne à laquelle accéder dans un jeu de résultats qui utilise un curseur permettant le défilement. (Lorsque *ligne* est spécifié, *fetchtype* doit être explicitement spécifié, même si vous spécifiez la valeur par défaut.)  
  
-   SQLSRV_SCROLL_NEXT  
-   SQLSRV_SCROLL_PRIOR  
-   SQLSRV_SCROLL_FIRST  
-   SQLSRV_SCROLL_LAST  
-   SQLSRV_SCROLL_ABSOLUTE  
-   SQLSRV_SCROLL_RELATIVE  
  
Pour plus d’informations sur ces valeurs, consultez [Spécification d’un type de curseur et sélection de lignes](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md). La prise en charge du curseur permettant le défilement a été ajoutée dans la version 1.1 du [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
*décalage* [facultatif] : utilisé avec SQLSRV_SCROLL_ABSOLUTE et SQLSRV_SCROLL_RELATIVE pour spécifier la ligne à récupérer. Le premier enregistrement dans le jeu de résultats est 0.  
  
## <a name="return-value"></a>Valeur retournée  
Si une ligne de données est récupérée, un **tableau** est retourné. S’il n’y a plus aucune ligne à récupérer, la valeur **null** est retournée. Si une erreur se produit, la valeur **false** est retournée.  
  
En fonction de la valeur du paramètre *$fetchType* , le **tableau** retourné peut être un **tableau**indexé numériquement, un **tableau**associatif, ou les deux. Par défaut, un **tableau** avec des clés numériques et associatives est retourné. Le type de données d’une valeur dans le tableau retourné sera le type de données PHP par défaut. Pour plus d’informations sur les types de données PHP par défaut, consultez [Default PHP Data Types](../../connect/php/default-php-data-types.md).  
  
## <a name="remarks"></a>Notes  
Si une colonne sans nom est retournée, la clé associative de l’élément de tableau est une chaîne vide (""). Par exemple, considérez cette instruction Transact-SQL qui insère une valeur dans une table de base de données et récupère la clé primaire générée par le serveur :  
  
```
INSERT INTO Production.ProductPhoto (LargePhoto) VALUES (?);  
SELECT SCOPE_IDENTITY()
```
  
Si le jeu de résultats retourné par la `SELECT SCOPE_IDENTITY()` partie de cette instruction est récupérée sous forme de tableau associatif, la clé de la valeur retournée est une chaîne vide (« »), car la colonne retournée n’a pas de nom. Pour éviter ce problème, vous pouvez récupérer le résultat sous forme de tableau numérique, ou spécifier un nom pour la colonne retournée dans l’instruction Transact-SQL. Voici un moyen de spécifier un nom de colonne dans Transact-SQL :  
  
```
SELECT SCOPE_IDENTITY() AS PictureID
```
  
Si un jeu de résultats contient plusieurs colonnes sans nom, la valeur de la dernière colonne sans nom est affectée à la clé de chaîne vide ("").  
  
## <a name="example"></a>Exemple  
L’exemple suivant récupère chaque ligne d’un jeu de résultats sous forme d’objet **array**associatif. L’exemple suppose que le serveur SQL Server et le [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) base de données sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
  
/* Set up and execute the query. */  
$tsql = "SELECT FirstName, LastName  
         FROM Person.Contact  
         WHERE LastName='Alan'";  
$stmt = sqlsrv_query( $conn, $tsql);  
if( $stmt === false)  
{  
     echo "Error in query preparation/execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Retrieve each row as an associative array and display the results.*/  
while( $row = sqlsrv_fetch_array( $stmt, SQLSRV_FETCH_ASSOC))  
{  
      echo $row['LastName'].", ".$row['FirstName']."\n";  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="example"></a>Exemple  
L’exemple suivant récupère chaque ligne d’un jeu de résultats sous forme de tableau indexé numériquement.  
  
L’exemple récupère les informations de produit à partir de la *Purchasing.PurchaseOrderDetail* table de la base de données AdventureWorks pour les produits qui ont une date donnée et la quantité en stock (*StockQty*) inférieur à une valeur spécifiée.  
  
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
  
/* Define the query. */  
$tsql = "SELECT ProductID,  
                UnitPrice,  
                StockedQty   
         FROM Purchasing.PurchaseOrderDetail  
         WHERE StockedQty < 3   
         AND DueDate='2002-01-29'";  
  
/* Execute the query. */  
$stmt = sqlsrv_query( $conn, $tsql);  
if ( $stmt )  
{  
     echo "Statement executed.\n";  
}   
else   
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Iterate through the result set printing a row of data upon each  
iteration.*/  
while( $row = sqlsrv_fetch_array( $stmt, SQLSRV_FETCH_NUMERIC))  
{  
     echo "ProdID: ".$row[0]."\n";  
     echo "UnitPrice: ".$row[1]."\n";  
     echo "StockedQty: ".$row[2]."\n";  
     echo "-----------------\n";  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
La fonction **sqlsrv_fetch_array** retourne toujours des données en fonction des [Default PHP Data Types](../../connect/php/default-php-data-types.md). Pour plus d’informations sur la façon de spécifier le type de données PHP, consultez [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
Si un champ sans nom est récupéré, la clé associative de l’élément de tableau est une chaîne vide (""). Pour plus d’informations, consultez [sqlsrv_fetch_array](../../connect/php/sqlsrv-fetch-array.md).  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Récupération de données](../../connect/php/retrieving-data.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)

[Guide de programmation pour les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)
  
