---
title: Chargement des pilotes Microsoft SQL Server pour PHP | Documents Microsoft
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
- loading the driver
ms.assetid: e5c114c5-8204-49c2-94eb-62ca63f5d3ec
caps.latest.revision: 42
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7890c38877d05a69118a7c61ffe7261d1c9e91e2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="loading-the-microsoft-drivers-for-php-for-sql-server"></a>Chargement des pilotes Microsoft SQL Server pour PHP
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cette page fournit des instructions pour le chargement du [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] dans le processus PHP espace.  
  
Vous pouvez télécharger les pilotes prédéfinis pour votre plateforme à partir de la [Microsoft Drivers for PHP for SQL Server](https://github.com/Microsoft/msphpsql/releases) Github page du projet. Chaque package d’installation contient les fichiers de pilote SQLSRV et PDO_SQLSRV dans des variants et non-thread. Sous Windows, ils sont également disponibles dans les variantes 32 bits et 64 bits. Consultez [configuration système requise pour le Microsoft Drivers for PHP for SQL Server](../../connect/php/system-requirements-for-the-php-sql-driver.md) pour obtenir la liste des fichiers de pilote qui sont contenus dans chaque package. Le fichier de pilote doit correspondre à la version PHP, l’architecture et threadedness de votre environnement de PHP.

Sur Linux et macOS, les pilotes peuvent également être installés à l’aide de PECL, comme dans le [didacticiel d’installation](../../connect/php/installation-tutorial-linux-mac.md).
  
## <a name="moving-the-driver-file-into-your-extension-directory"></a>Déplacement du fichier de pilote dans votre répertoire d’extension  
Le fichier de pilote doit se trouver dans un répertoire où le runtime PHP peut le trouver. Il est plus facile de mettre le fichier de pilote dans votre répertoire d’extensions PHP par défaut - pour trouver le répertoire par défaut, exécutez `php -i | sls extension_dir` sur Windows ou `php -i | grep extension_dir` sur Linux/macOS. Si vous n’utilisez pas le répertoire d’extension par défaut, spécifiez un répertoire dans le fichier de configuration PHP (php.ini), à l’aide de la **extension_dir** option. Par exemple, sur Windows, si vous avez placé le fichier de pilote votre `c:\php\ext` répertoire, ajoutez la ligne suivante au php.ini :
  
```  
extension_dir = "c:\PHP\ext"  
```

## <a name="loading-the-driver-at-php-startup"></a>Chargement du pilote au démarrage de PHP  
Pour charger le pilote SQLSRV au démarrage de PHP, commencez par déplacer un fichier de pilote dans votre répertoire d’extension. Ensuite, procédez comme suit :  
  
1.  Pour activer la **SQLSRV** pilote, modifier **php.ini** en ajoutant la ligne suivante à la section d’extension, la modification du nom de fichier comme il convient :  
  
    Sur Windows : 
    ```  
    extension=php_sqlsrv_72_ts.dll  
    ```  
    Sur Linux, si vous avez téléchargé les fichiers binaires prédéfinis pour votre distribution : 
    ```  
    extension=php_sqlsrv_72_nts.so  
    ```
    Si vous avez compilé le SQLSRV binaire à partir de la source ou avec PECL, il sera à la place nommé sqlsrv.so :
    ```
    extension=sqlsrv.so
    ```
  
2.  Pour activer la **PDO_SQLSRV** pilote, l’extension des objets de données PHP (PDO) doit être disponible, une extension intégrée, soit comme une extension chargée dynamiquement.

    Sous Windows, les binaires PHP prédéfinis fournis avec PDO intégré, il est inutile de modifier php.ini pour la charger. Si, toutefois, vous avez compilé PHP à partir de la source et spécifié une extension PDO distincte doit être créé, il sera nommé `php_pdo.dll`, et vous devez les copier dans votre répertoire d’extension et ajoutez la ligne suivante au php.ini :  
    ```
    extension=php_pdo.dll  
    ```
    Sur Linux, si vous avez installé à l’aide du Gestionnaire de package de votre système, de PHP PDO est probablement installé comme une extension chargée dynamiquement nommée pdo.so. L’extension PDO doit être chargée avant l’extension PDO_SQLSRV, ou le chargement échoue. Les extensions sont généralement chargées à l’aide de fichiers .ini individuels, et ces fichiers sont lues après php.ini. Par conséquent, si pdo.so est chargé par son propre fichier .ini, un fichier distinct du chargement du pilote PDO_SQLSRV après PDO est requis. 

    Pour savoir de quel annuaire sont situés les fichiers spécifiques à l’extension .ini, exécutez `php --ini` et notez le répertoire affiché sous `Scan for additional .ini files in:`. Rechercher le fichier chargé pdo.so--il est probablement préfixé par un nombre, comme 10-pdo.ini. Le préfixe numérique indique l’ordre de chargement des fichiers .ini, tandis que les fichiers qui n’ont pas un préfixe numérique sont chargés par ordre alphabétique. Créez un fichier pour charger le fichier de pilote PDO_SQLSRV appelé 30-pdo_sqlsrv.ini (tout nombre supérieur à celui qui sert de préfixe pdo.ini works) ou pdo_sqlsrv.ini (si pdo.ini n’est pas précédée d’un nombre) et ajoutez la ligne suivante, en modifiant le nom de fichier en tant que le cas :  
    ```
    extension=php_pdo_sqlsrv_72_nts.so
    ```
    Comme avec SQLSRV, si vous avez compilé le PDO_SQLSRV binaire à partir de la source ou avec PECL, il sera à la place nommé pdo_sqlsrv.so :
    ```
    extension=pdo_sqlsrv.so
    ```
    Copiez ce fichier dans le répertoire qui contient les autres fichiers .ini. 

    Si vous avez compilé PHP à partir de la source avec prise en charge PDO intégrée, vous n’avez pas besoin d’un fichier .ini distinct, et vous pouvez ajouter la ligne appropriée ci-dessus à php.ini.
  
3.  Redémarrez le serveur web.  
  
> [!NOTE]  
> Pour déterminer si le pilote a été chargé avec succès, exécutez un script qui appelle [phpinfo()](http://php.net/manual/en/function.phpinfo.php).  
  
Pour plus d’informations sur **php.ini** directives, consultez [Description des principales directives php.ini](http://php.net/manual/en/ini.core.php).  
  
## <a name="see-also"></a>Voir aussi  
[Mise en route avec les pilotes Microsoft PHP pour SQL Server](../../connect/php/getting-started-with-the-php-sql-driver.md)

[Configuration système requise pour les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/system-requirements-for-the-php-sql-driver.md)

[Guide de programmation pour les pilotes Microsoft pour PHP pour SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)

[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Référence d’API du pilote PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)  
  
