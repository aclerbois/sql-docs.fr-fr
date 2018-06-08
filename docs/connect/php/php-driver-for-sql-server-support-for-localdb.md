---
title: Prise en charge de la base de données locale | Documents Microsoft
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.prod_service: drivers
ms.component: php
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d315ad6a-0d50-4093-80c2-2f11217237c2
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 92be589df4038d152f07445d7cd5fa4ffafc935b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="support-for-localdb"></a>Prise en charge de la base de données locale

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

LocalDB est une version légère de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] qui est disponible depuis [!INCLUDE[ssSQL11](../../includes/sssql11_md.md)]. Cette rubrique explique comment se connecter à une base de données dans une instance LocalDB.

## <a name="remarks"></a>Notes

Pour plus d’informations sur la base de données locale, y compris comment installer LocalDB et configurer votre instance de base de données locale, consultez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] rubrique de la documentation en ligne sur [!INCLUDE[ssSQL11](../../includes/sssql11_md.md)] Express LocalDB.

En bref, LocalDB vous permet de :

-   Utiliser **sqllocaldb.exe i** pour déterminer le nom de l'instance par défaut.

-   Utiliser le mot clé de chaîne de connexion **AttachDBFilename** pour spécifier le fichier de base de données auquel le serveur doit se rattacher. Lorsque vous utilisez **AttachDBFilename**, si vous ne spécifiez pas le nom de la base de données avec le mot clé de chaîne de connexion **Database** , la base de données est supprimée de l'instance LocalDB lorsque l'application se ferme.

-   Spécifiez une instance de la base de données locale dans votre chaîne de connexion. Par exemple, Voici un exemple de chaîne de connexion SQLSRV :

    ```php
    $conn = sqlsrv_connect( '(localdb)\\v11.0',
        array( 'Database'=>'myData'));

    $conn = sqlsrv_connect( '(localdb)\\v11.0',
        array('AttachDBFileName'=>'c:\\myData.MDF','Database'=>'myData'));

    $conn = sqlsrv_connect( '(localdb)\\v11.0',
        array('AttachDBFileName'=>'c:\\myData.MDF'));
    ```

    L’élément suivant est un exemple de chaîne de connexion PDO_SQLSRV :  

    ```php
    $conn = new PDO( 'sqlsrv:server=(localdb)\\v11.0;'
        . 'Database=myData', NULL, NULL);

    $conn = new PDO( 'sqlsrv:server=(localdb)\\v11.0;'
        . 'AttachDBFileName=c:\\myData.MDF;Database=myData ',
        NULL, NULL);

    $conn = new PDO( 'sqlsrv:server=(localdb)\\v11.0;'
        . 'AttachDBFileName=c:\\myData.MDF', NULL, NULL);  
    ```

Si nécessaire, vous pouvez créer une instance LocalDB avec sqllocaldb.exe. Vous pouvez également utiliser sqlcmd.exe pour ajouter et modifier des bases de données dans une instance LocalDB. Par exemple, `sqlcmd -S (localdb)\v11.0`. (Lors de l’exécution dans IIS, vous devez exécuter sous le compte approprié pour obtenir les mêmes résultats que lorsque vous exécutez à la ligne de commande, consultez [à l’aide de LocalDB avec IIS complet, partie 2 : la propriété d’Instance](http://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-2-instance-ownership.aspx) pour plus d’informations.)

Chaînes de connexion exemple à l’aide du pilote SQLSRV qui se connectent à une base de données dans une base de données locale d’une instance appelée myInstance nommée sont les suivants :

```php
$conn = sqlsrv_connect( '(localdb)\\myInstance',
    array( 'Database'=>'myData'));
```

Chaînes de connexion exemple à l’aide du pilote PDO_SQLSRV qui se connectent à une base de données dans une base de données locale d’une instance appelée myInstance nommée sont les suivants :  
  
```php
$conn = new PDO( 'sqlsrv:server=(localdb)\\myInstance;'
    . 'database=myData', NULL, NULL);
```

Pour obtenir des instructions sur l’installation de base de données locale, consultez le [LocalDB documentation](../../database-engine/configure-windows/sql-server-2016-express-localdb.md). Si vous utilisez sqlcmd.exe pour modifier les données dans votre instance de base de données locale, vous devez le [utilitaire sqlcmd](../../tools/sqlcmd-utility.md).

## <a name="see-also"></a>Voir aussi

[Connexion au serveur](../../connect/php/connecting-to-the-server.md)
