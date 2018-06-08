---
title: 'Comment : spécifier les Types de données SQL Server lors de l’utilisation du pilote SQLSRV | Documents Microsoft'
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
ms.assetid: 1fcf73cb-5634-4d89-948f-9326f1dbd030
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d3c73c660e0b5468eabfb2878552dd396567144e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="how-to-specify-sql-server-data-types-when-using-the-sqlsrv-driver"></a>Procédure : spécifier des types de données SQL Server quand vous utilisez le pilote SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cette rubrique montre comment utiliser le pilote SQLSRV pour spécifier le type de données SQL Server pour les données envoyées au serveur. Cette rubrique ne s’applique pas à une utilisation du pilote PDO_SQLSRV.  
  
Pour spécifier le type de données SQL Server, vous devez utiliser le tableau *$params* facultatif quand vous préparez ou exécutez une requête qui insère ou met à jour des données. Pour plus d’informations sur la structure et la syntaxe du tableau *$params* , consultez [sqlsrv_query](../../connect/php/sqlsrv-query.md) ou [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md).  
  
Les étapes suivantes récapitulent la manière de spécifier le type de données SQL Server lors de l’envoi de données au serveur :  
  
> [!NOTE]  
> Si aucun type de données SQL Server n’est spécifié, les types par défaut sont utilisés. Pour plus d’informations sur les types de données SQL Server par défaut, consultez [Default SQL Server Data Types](../../connect/php/default-sql-server-data-types.md).  
  
1.  Définissez une requête Transact-SQL qui insère ou met à jour des données. Utilisez des points d’interrogation (?) en tant qu’espaces réservés pour les valeurs de paramètre dans la requête.  
  
2.  Initialisez ou mettez à jour les variables PHP qui correspondent aux espaces réservés dans la requête Transact-SQL.  
  
3.  Construisez le tableau *$params* à utiliser lors de la préparation ou l’exécution de la requête. Notez que chaque élément du tableau *$params* doit également être un tableau quand vous spécifiez le type de données SQL Server.  
  
4.  Spécifiez le type de données SQL Server souhaité à l’aide de la commande appropriée **SQLSRV_SQLTYPE_\***  constante en tant que quatrième paramètre dans chaque sous-tableau de la *$params* tableau. Pour obtenir une liste complète de la **SQLSRV_SQLTYPE_\***  constantes, consultez la section SQLTYPEs de [constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md). Par exemple, dans le code ci-dessous, *$changeDate*, *$rate*et *$payFrequency* sont respectivement spécifiés en tant que types SQL Server **datetime**, **money**et **tinyint** dans le tableau *$params* . Étant donné qu’aucun type SQL Server n’est spécifiée pour *$employeeId* et que ce paramètre est initialisé à un entier, le type SQL Server par défaut **integer** est utilisé.  
  
    ```  
    $employeeId = 5;  
    $changeDate = "2005-06-07";  
    $rate = 30;  
    $payFrequency = 2;  
    $params = array(  
                array($employeeId, null),  
                array($changeDate, null, null, SQLSRV_SQLTYPE_DATETIME),  
                array($rate, null, null, SQLSRV_SQLTYPE_MONEY),  
                array($payFrequency, null, null, SQLSRV_SQLTYPE_TINYINT)  
              );  
    ```  
  
## <a name="example"></a>Exemple  
L’exemple suivant insère des données dans le *HumanResources.EmployeePayHistory* table de base de données AdventureWorks. Les types SQL Server sont spécifiés pour les paramètres *$changeDate*, *$rate*et *$payFrequency* . Le type SQL Server par défaut est utilisé pour le paramètre *$employeeId* . Pour vérifier que les données ont été correctement insérées, les mêmes données sont récupérées et affichées.  
  
Cet exemple suppose que SQL Server et le [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) base de données sont installés sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
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
$tsql1 = "INSERT INTO HumanResources.EmployeePayHistory (EmployeeID,  
                                                        RateChangeDate,  
                                                        Rate,  
                                                        PayFrequency)  
           VALUES (?, ?, ?, ?)";  
  
/* Construct the parameter array. */  
$employeeId = 5;  
$changeDate = "2005-06-07";  
$rate = 30;  
$payFrequency = 2;  
$params1 = array(  
               array($employeeId, null),  
               array($changeDate, null, null, SQLSRV_SQLTYPE_DATETIME),  
               array($rate, null, null, SQLSRV_SQLTYPE_MONEY),  
               array($payFrequency, null, null, SQLSRV_SQLTYPE_TINYINT)  
           );  
  
/* Execute the INSERT query. */  
$stmt1 = sqlsrv_query($conn, $tsql1, $params1);  
if( $stmt1 === false )  
{  
     echo "Error in execution of INSERT.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Retrieve the newly inserted data. */  
/* Define the query. */  
$tsql2 = "SELECT EmployeeID, RateChangeDate, Rate, PayFrequency  
          FROM HumanResources.EmployeePayHistory  
          WHERE EmployeeID = ? AND RateChangeDate = ?";  
  
/* Construct the parameter array. */  
$params2 = array($employeeId, $changeDate);  
  
/*Execute the SELECT query. */  
$stmt2 = sqlsrv_query($conn, $tsql2, $params2);  
if( $stmt2 === false )  
{  
     echo "Error in execution of SELECT.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Retrieve and display the results. */  
$row = sqlsrv_fetch_array( $stmt2 );  
if( $row === false )  
{  
     echo "Error in fetching data.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
echo "EmployeeID: ".$row['EmployeeID']."\n";  
echo "Change Date: ".date_format($row['RateChangeDate'], "Y-m-d")."\n";  
echo "Rate: ".$row['Rate']."\n";  
echo "PayFrequency: ".$row['PayFrequency']."\n";  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt($stmt1);  
sqlsrv_free_stmt($stmt2);  
sqlsrv_close($conn);  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Récupération de données](../../connect/php/retrieving-data.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)

[Guide pratique pour spécifier des types de données PHP](../../connect/php/how-to-specify-php-data-types.md)

[Conversion de types de données](../../connect/php/converting-data-types.md)

[Guide pratique pour envoyer et récupérer des données UTF-8 à l’aide de la prise en charge UTF-8 intégrée](../../connect/php/how-to-send-and-retrieve-utf-8-data-using-built-in-utf-8-support.md)  
  
