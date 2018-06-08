---
title: La récupération des données en tant que flux à l’aide du pilote SQLSRV | Documents Microsoft
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
ms.assetid: 17dc9129-04cd-430c-b5b3-82824116425d
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 32164c9beb05293249eafef76de29dcf3c356182
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="retrieving-data-as-a-stream-using-the-sqlsrv-driver"></a>Récupération des données sous la forme d’un flux à l’aide du pilote SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

La récupération de données comme un flux de données est disponible uniquement dans le pilote SQLSRV de la [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]et n’est pas disponible dans le pilote PDO_SQLSRV.  
  
[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] exploite les flux pour récupérer de grandes quantités de données. Les rubriques de cette section fournissent des détails sur la façon de récupérer des données sous la forme d’un flux.  
  
Les étapes suivantes récapitulent la manière de récupérer des données sous la forme d’un flux :  
  
1.  Préparer et exécuter une requête Transact-SQL avec [sqlsrv_query](../../connect/php/sqlsrv-query.md) ou la combinaison de [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md)/[sqlsrv_execute](../../connect/php/sqlsrv-execute.md).  
  
2.  Utilisez [sqlsrv_fetch](../../connect/php/sqlsrv-fetch.md) pour passer à la ligne suivante dans le jeu de résultats.  
  
3.  Utilisez [sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md) pour récupérer un champ de la ligne. Spécifiez que les données à récupérer en tant que flux à l’aide de **SQLSRV_PHPTYPE_STREAM (<encoding>)** en tant que troisième paramètre dans l’appel de fonction. Ce tableau répertorie les constantes utilisées pour spécifier les encodages et leurs descriptions :  
  
    |Constante SQLSRV| Description|  
    |-------------------|---------------|  
    |SQLSRV_ENC_BINARY|Les données sont retournées à partir du serveur sous la forme d’un flux d’octets bruts sans encodage ni traduction.|  
    |SQLSRV_ENC_CHAR|Les données sont retournées sous forme de caractères 8 bits comme spécifié dans la page de codes des paramètres régionaux Windows définis sur le système. Les caractères multioctets ou les caractères non mappés dans cette page de codes sont remplacés par un point d’interrogation (?) à un octet.|  
  
> [!NOTE]  
> Certains types de données sont retournés sous forme de flux par défaut. Pour plus d’informations, consultez [Default PHP Data Types](../../connect/php/default-php-data-types.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|---------|---------------|  
|[Types de données avec prise en charge des flux à l’aide du pilote SQLSRV](../../connect/php/data-types-with-stream-support-using-the-sqlsrv-driver.md)|Répertorie les types de données SQL Server qui peuvent être récupérés sous forme de flux.|  
|[Procédure : récupérer des données caractères sous la forme d’un flux à l’aide du pilote SQLSRV](../../connect/php/how-to-retrieve-character-data-as-a-stream-using-the-sqlsrv-driver.md)|Montre comment récupérer des données caractères sous la forme d’un flux.|  
|[Procédure : récupérer des données binaires sous la forme d’un flux à l’aide du pilote SQLSRV](../../connect/php/how-to-retrieve-binary-data-as-a-stream-using-the-sqlsrv-driver.md)|Montre comment récupérer des données binaires sous la forme d’un flux.|  
  
## <a name="see-also"></a>Voir aussi  
[Récupération de données](../../connect/php/retrieving-data.md)

[Constantes &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)  
  
