---
title: Azure Active Directory | Documents Microsoft
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
ms.openlocfilehash: bfb4c78f7a32c1205256f7a0d44bd9526fabdc27
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-using-azure-active-directory-authentication"></a>Connectez-vous à l’aide de l’authentification Azure Active Directory
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis) (Azure AD) est une technologie de gestion d’ID utilisateur central qui fonctionne comme une alternative à [l’authentification SQL Server](../../connect/php/how-to-connect-using-sql-server-authentication.md). Azure AD autorise les connexions à la base de données SQL Microsoft Azure et SQL Data Warehouse avec des identités fédérées dans Azure AD à l’aide d’un nom d’utilisateur et mot de passe, l’authentification intégrée Windows ou un jeton d’accès Azure AD ; les pilotes PHP pour SQL Server offrent une prise en charge partielle de ces fonctionnalités.

Pour utiliser Azure AD, utilisez le **authentification** (mot clé). Les valeurs qui **authentification** peut prendre sur sont expliquées dans le tableau suivant.

|Mot clé|Valeurs| Description|
|-|-|-|
|**Authentification**|Pas définie (valeur par défaut)|Mode d’authentification déterminé par les autres mots clés. Pour plus d’informations, consultez [Connection Options](../../connect/php/connection-options.md). |
||`SqlPassword`|S’authentifier directement à une instance de SQL Server (qui peut être une instance Azure) à l’aide d’un nom d’utilisateur et un mot de passe. Le nom d’utilisateur et un mot de passe doivent être passés dans la chaîne de connexion à l’aide de la **UID** et **PWD** mots clés. |
||`ActiveDirectoryPassword`|Authentifier avec une identité Azure Active Directory à l’aide d’un nom d’utilisateur et un mot de passe. Le nom d’utilisateur et un mot de passe doivent être passés dans la chaîne de connexion à l’aide de la **UID** et **PWD** mots clés. |

Le **authentification** mot clé affecte les paramètres de sécurité de connexion. S’il est défini dans la chaîne de connexion, puis par défaut le **Encrypt** (mot clé) est défini sur true, le client demande le chiffrement. En outre, le certificat de serveur sera validé quel que soit le paramètre de chiffrement, sauf si **TrustServerCertificate** est définie sur true. Elle se distingue de l’ancien et moins sécurisée, méthode de connexion, dans lequel le certificat de serveur n’est pas validé, sauf si le chiffrement est demandé spécifiquement dans la chaîne de connexion.

Avant d’utiliser Azure AD avec les pilotes PHP pour SQL Server sur Windows, vérifiez que vous avez installé le [Assistant Microsoft Online Services Sign-In](https://www.microsoft.com/download/details.aspx?id=41950) (non requis pour Linux et Mac OS).

#### <a name="limitations"></a>Limitations

Sous Windows, le pilote ODBC sous-jacent prend en charge plus de valeur pour le **authentification** (mot clé), **ActiveDirectoryIntegrated**, mais les pilotes PHP ne prennent pas en charge cette valeur sur n’importe quelle plateforme et donc également ne prennent pas en charge basée sur les jetons d’authentification Azure AD.

## <a name="example"></a>Exemple

L’exemple suivant montre comment se connecter à l’aide de **SqlPassword** et **ActiveDirectoryPassword**.

```php
    <?php
    // First connect to a local SQL Server instance by setting Authentication to SqlPassword
    $serverName = "myserver.mydomain";

    $connectionInfo = array( "UID"=>$myusername, "PWD"=>$mypassword, "Authentication"=>'SqlPassword' );

    $conn = sqlsrv_connect( $serverName, $connectionInfo );
    if( $conn === false )
    {
        echo "Could not connect with Authentication=SqlPassword.\n";
        print_r( sqlsrv_errors() );
    }
    else
    {
        echo "Connected successfully with Authentication=SqlPassword.\n";
        sqlsrv_close( $conn );
    }

    // Now connect to an Azure SQL database by setting Authentication to ActiveDirectoryPassword
    $azureServer = "myazureserver.database.windows.net";
    $azureDatabase = "myazuredatabase";
    $azureUsername = "myuid";
    $azurePassword = "mypassword";
    $connectionInfo = array( "Database"=>$azureDatabase, "UID"=>$azureUsername, "PWD"=>$azurePassword,
                             "Authentication"=>'ActiveDirectoryPassword' );

    $conn = sqlsrv_connect( $azureServer, $connectionInfo );
    if( $conn === false )
    {
        echo "Could not connect with Authentication=ActiveDirectoryPassword.\n";
        print_r( sqlsrv_errors() );
    }
    else
    {
        echo "Connected successfully with Authentication=ActiveDirectoryPassword.\n";
        sqlsrv_close( $conn );
    }

    ?>
```

L’exemple suivant donne le même résultat que ci-dessus avec le pilote PDO_SQLSRV.

```php
    <?php
    // First connect to a local SQL Server instance by setting Authentication to SqlPassword
    $serverName = "myserver.mydomain";

    $connectionInfo = "Database = $databaseName; Authentication = SqlPassword;";

    try
    {
        $conn = new PDO( "sqlsrv:server = $serverName ; $connectionInfo", $myusername, $mypassword );
        echo "Connected successfully with Authentication=SqlPassword.\n";
        $conn = null;
    }
    catch( PDOException $e )
    {
        echo "Could not connect with Authentication=SqlPassword.\n";
        print_r( $e->getMessage() );
        echo "\n";
    }

    // Now connect to an Azure SQL database by setting Authentication to ActiveDirectoryPassword
    $azureServer = "myazureserver.database.windows.net";
    $azureDatabase = "myazuredatabase";
    $azureUsername = "myuid";
    $azurePassword = "mypassword";
    $connectionInfo = "Database = $azureDatabase; Authentication = ActiveDirectoryPassword;";

    try
    {
        $conn = new PDO( "sqlsrv:server = $azureServer ; $connectionInfo", $azureUsername, $azurePassword );
        echo "Connected successfully with Authentication=ActiveDirectoryPassword.\n";
        $conn = null;
    }
    catch( PDOException $e )
    {
        echo "Could not connect with Authentication=ActiveDirectoryPassword.\n";
        print_r( $e->getMessage() );
        echo "\n";
    }

    ?>
```
## <a name="see-also"></a>Voir aussi  
[Utilisation d’Azure Active Directory avec ODBC Driver](https://docs.microsoft.com/en-us/sql/connect/odbc/using-azure-active-directory)
