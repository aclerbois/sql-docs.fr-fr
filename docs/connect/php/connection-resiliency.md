---
title: Résilience des connexions inactives
ms.date: 07/13/2017
ms.prod: sql
ms.prod_service: connectivity
ms.component: php
ms.suite: sql
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: david-puglielli
ms.author: v-dapugl
manager: v-hakaka
ms.openlocfilehash: b2ffbf3ef57db31fcfd3a714fe9e2f6e0565237f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="idle-connection-resiliency"></a>Résilience des connexions inactives
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[Résilience des connexions](https://msdn.microsoft.com/library/dn632678.aspx) est le principe qu’une interruption de connexion inactive peut être rétablie, dans certaines contraintes. Si une connexion à Microsoft SQL Server échoue, la résilience des connexions permet au client tente automatiquement de rétablir la connexion. Résilience des connexions est une propriété de la source de données ; seul SQL Server 2014 et versions ultérieur et base de données SQL Azure prend en charge la résilience des connexions.

Résilience des connexions est implémentée avec deux mots clés de connexion qui peuvent être ajoutées aux chaînes de connexion : **ConnectRetryCount** et **ConnectRetryInterval**.

|Mot clé|Valeurs|Par défaut| Description|
|-|-|-|-|
|**ConnectRetryCount**| Entier compris entre 0 et 255 (inclus)|1|Le nombre maximal de tentatives de rétablissement d’une connexion interrompue avant d’abandonner. Par défaut, une seule tentative pour rétablir une connexion lorsque rompu. Une valeur de 0 signifie qu’aucune reconnexion ne sera tentée.|
|**ConnectRetryInterval**| Entier compris entre 1 et 60 (inclus)|1| Durée, en secondes, entre les tentatives de rétablir une connexion. L’application tente de se reconnecter immédiatement lors de la détection d’une connexion interrompue et qu’il attendra puis **ConnectRetryInterval** secondes avant de réessayer. Ce mot clé est ignoré si **ConnectRetryCount** est égal à 0.

Si le produit de **ConnectRetryCount** multipliée par **ConnectRetryInterval** est supérieure à **LoginTimeout**, puis le client cesse tente de se connecter une seule fois  **LoginTimeout** est atteinte ; sinon, elle continuera d’essayer de se reconnecter tant que **ConnectRetryCount** est atteinte.

#### <a name="remarks"></a>Notes

Résilience de connexion s’applique lors de la connexion est inactive. Les erreurs qui se produisent pendant l’exécution d’une transaction, par exemple, ne déclenche pas les tentatives de reconnexion : ne pourront pas comme prévu. Les cas suivants, appelés des États de session non récupérable, ne déclenchent pas les tentatives de reconnexion :

* tables temporaires ;
* Curseurs globaux et locaux
* Verrous de transaction de niveau session et le contexte de transaction
* Verrous d’application
* EXECUTE AS / rétablir le contexte de sécurité
* Gère OLE automation
* Handles XML préparés
* Indicateurs de trace

## <a name="example"></a>Exemple

Le code suivant se connecte à une base de données et exécute une requête. La connexion est interrompue par l’arrêt de la session et une requête est effectuée à l’aide de la connexion interrompue. Cet exemple utilise le [AdventureWorks](https://msdn.microsoft.com/library/ms124501%28v=sql.100%29.aspx) base de données exemple.

Dans cet exemple, nous spécifions un curseur mis en mémoire tampon avant de s’arrêter la connexion. Si nous ne spécifiez pas un curseur mis en mémoire tampon, la connexion n’est pas rétablie car serait un curseur côté serveur actif et par conséquent, la connexion ne serait pas inactive lorsque rompu. Toutefois, dans ce cas, nous pouvons appeler sqlsrv_free_stmt() avant de s’arrêter la connexion pour videz pas le curseur, et la connexion est rétablie avec succès.

```php
<?php
// This function breaks the connection by determining its session ID and killing it.
// A separate connection is used to break the main connection because a session
// cannot kill itself. The sleep() function ensures enough time has passed for KILL
// to finish ending the session.
function BreakConnection( $conn, $conn_break )
{
    $stmt1 = sqlsrv_query( $conn, "SELECT @@SPID" );
    if ( sqlsrv_fetch( $stmt1 ) )
    {
        $spid=sqlsrv_get_field( $stmt1, 0 );
    }

    $stmt2 = sqlsrv_prepare( $conn_break, "KILL ".$spid );
    sqlsrv_execute( $stmt2 );
    sleep(1);
}

// Connect to the local server using Windows authentication and specify
// AdventureWorks as the database in use. Specify values for
// ConnectRetryCount and ConnectRetryInterval as well.
$databaseName = 'AdventureWorks2014';
$serverName = '(local)';
$connectionInfo = array( "Database"=>$databaseName, "ConnectRetryCount"=>10, "ConnectRetryInterval"=>10 );

$conn = sqlsrv_connect( $serverName, $connectionInfo );
if( $conn === false)  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}

// A separate connection that will be used to break the main connection $conn
$conn_break = sqlsrv_connect( $serverName, array( "Database"=>$databaseName) );

// Create a statement to retrieve the contents of a table
$stmt1 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Employee",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt1 === false )
{
     echo "Error in statement 1.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 1 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt1 );
    echo $rowcount." rows in result set.\n";
}

// Now break the connection $conn
BreakConnection( $conn, $conn_break );

// Create another statement. The connection will be reestablished.
$stmt2 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Department",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt2 === false )
{
     echo "Error in statement 2.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 2 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt2 );
    echo $rowcount." rows in result set.\n";
}

sqlsrv_close( $conn );
sqlsrv_close( $conn_break );
?>
```
Sortie attendue :
```
Statement 1 successful.
290 rows in result set.
Statement 2 successful.
16 rows in result set.
```

## <a name="see-also"></a>Voir aussi
[Résilience de connexion du pilote ODBC Windows](https://docs.microsoft.com/en-us/sql/connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver)
