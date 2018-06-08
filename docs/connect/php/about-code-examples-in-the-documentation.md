---
title: À propos des exemples de Code dans la Documentation | Documents Microsoft
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
ms.assetid: 3f035c37-0f2e-47d4-94e0-a10774402e82
caps.latest.revision: 31
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3b3f6112d56dbb8989438730f50b11e733f71ed7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="about-code-examples-in-the-documentation"></a>À propos des exemples de code dans la documentation
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

## <a name="remarks-about-the-code-examples"></a>Remarques sur les exemples de code
Il existe plusieurs points à noter quand vous exécutez les exemples de code dans la documentation [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] :  
  
-   Presque tous les exemples supposent que SQL Server 2008 ou version ultérieure et la base de données AdventureWorks sont installés sur l’ordinateur local.  
  
    Pour plus d’informations sur la façon de télécharger les éditions gratuites et les versions d’évaluation de SQL Server, consultez [SQL Server](http://go.microsoft.com/fwlink/?LinkID=120193).  
  
    Pour plus d’informations sur la façon de télécharger et installer la base de données AdventureWorks, consultez le [page AdventureWorks dans le référentiel Github d’exemples SQL Server](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works).
  
-   Presque tous les exemples de code de cette documentation sont destinés à être exécutés à partir de la ligne de commande, ce qui permet de les tester tous de manière automatisée. Pour plus d’informations sur l’exécution de PHP à partir de la ligne de commande, consultez [Utilisation de PHP à partir de la ligne de commande](http://php.net/manual/en/features.commandline.php).  
  
-   Bien que les exemples sont destinés à être exécutés à partir de la ligne de commande, chaque exemple peut être exécuté en l’appelant à partir d’un navigateur sans apporter de modifications au script. Pour formater la sortie correcte, remplacez chaque « \n » par «\<\/br > » dans chaque exemple avant de l’appeler à partir d’un navigateur.  
  
-   Par souci de concision, la gestion des erreurs appropriée n’est pas effectuée dans tous les exemples. Il est recommandé de vérifier s’il existe des erreurs dans tout appel à une fonction **sqlsrv** ou méthode PDO et de les gérer selon les besoins de l’application.  
  
    Un moyen simple d’obtenir des informations d’erreur quand une erreur s’est produite consiste à quitter le script avec la ligne de code suivante :  
  
    ```  
    die( print_r( sqlsrv_errors(), true));  
    ```  
  
    Ou, si vous utilisez PDO,  
  
    ```  
    print_r ($stmt->errorInfo());  
    die();  
    ```  
  
    Pour plus d’informations sur la gestion des erreurs et avertissements, consultez [Gestion des erreurs et des avertissements](../../connect/php/handling-errors-and-warnings.md).  
  
## <a name="see-also"></a>Voir aussi  
[Vue d’ensemble des pilotes Microsoft SQL Server pour PHP](../../connect/php/overview-of-the-php-sql-driver.md)
  
