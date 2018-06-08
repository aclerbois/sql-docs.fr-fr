---
title: Types de curseurs (pilote PDO_SQLSRV) | Documents Microsoft
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
ms.assetid: 49ea6a6e-78d4-40f8-85eb-180b527f0537
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7cba6032b58594c106f0f1c3a88f174588f3d351
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cursor-types-pdosqlsrv-driver"></a>Types de curseurs (pilote PDO_SQLSRV)
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Le pilote PDO_SQLSRV vous permet de créer des jeux de résultats déroulables avec l’une de plusieurs curseurs.  
  
Pour plus d’informations sur la façon de spécifier un curseur à l’aide du pilote PDO_SQLSRV et pour obtenir des exemples de code, consultez [PDO::prepare](../../connect/php/pdo-prepare.md).  
  
## <a name="pdosqlsrv-and-server-side-cursors"></a>PDO_SQLSRV et les curseurs côté serveur  
Avant la version 3.0 de la [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], le pilote PDO_SQLSRV autorisées vous permet de créer un jeu de résultats avec un curseur avant uniquement ou statique de côté serveur. À compter de la version 3.0 de la [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)], keyset et les curseurs dynamiques sont également disponibles.  
  
Vous pouvez indiquer le type de curseur côté serveur à l’aide de PDO::prepare ou PDOStatement::setAttribute pour sélectionner un type de curseur :  
  
-   PDO::ATTR_CURSOR = &GT; PDO::CURSOR_FWDONLY  
  
-   PDO::ATTR_CURSOR = &GT; PDO::CURSOR_SCROLL  
  
Vous pouvez demander un curseur keyset ou dynamic en spécifiant PDO::ATTR_CURSOR = > PDO::CURSOR_SCROLL et puis passer la valeur appropriée pour PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE. Les valeurs possibles que vous pouvez passer à PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE sont :  
  
-   PDO::SQLSRV_CURSOR_BUFFERED  
  
-   PDO::SQLSRV_CURSOR_DYNAMIC  
  
-   PDO::SQLSRV_CURSOR_KEYSET_DRIVEN  
  
-   PDO::SQLSRV_CURSOR_STATIC  
  
## <a name="pdosqlsrv-and-client-side-cursors"></a>PDO_SQLSRV et les curseurs côté Client  
Curseurs côté client ont été ajoutées dans la version 3.0 de la [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] qui vous permet de mettre en cache un ensemble de résultats en mémoire. Un avantage est que le nombre de lignes est disponible après l’exécution d’une requête.  
  
Curseurs côté client doivent être utilisés pour les jeux de résultats de petite à moyenne taille. Jeux de résultats volumineux doivent utiliser des curseurs côté serveur.  
  
Une requête retourne false si la mémoire tampon n’est pas suffisamment grande pour contenir un ensemble de résultats lors de l’utilisation d’un curseur côté client. Vous pouvez augmenter la taille de mémoire tampon jusqu'à la limite de mémoire PHP.  
  
Vous pouvez configurer la taille de la mémoire tampon qui contient le jeu de résultats avec l’attribut PDO::SQLSRV_ATTR_CLIENT_BUFFER_MAX_KB_SIZE de [PDO::setAttribute](../../connect/php/pdo-setattribute.md) ou [PDOStatement::setAttribute](../../connect/php/pdostatement-setattribute.md). Vous pouvez également définir la taille maximale de mémoire tampon dans le fichier php.ini avec pdo_sqlsrv.client_buffer_max_kb_size (par exemple, pdo_sqlsrv.client_buffer_max_kb_size = 1024).  
  
Vous indiquez que vous souhaitez un curseur côté client à l’aide de PDO::prepare ou PDOStatement::setAttribute et que vous sélectionnez le PDO::ATTR_CURSOR = > PDO::CURSOR_SCROLL les type de curseur.  Vous spécifiez ensuite PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE = > PDO::SQLSRV_CURSOR_BUFFERED.  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$query = "select * from Person.ContactType";  
$stmt = $conn->prepare( $query, array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL, PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_BUFFERED));  
$stmt->execute();  
print $stmt->rowCount();  
  
echo "\n";  
  
while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){  
   print "$row[Name]\n";  
}  
echo "\n..\n";  
  
$row = $stmt->fetch( PDO::FETCH_BOTH, PDO::FETCH_ORI_FIRST );  
print_r($row);  
  
$row = $stmt->fetch( PDO::FETCH_ASSOC, PDO::FETCH_ORI_REL, 1 );  
print "$row[Name]\n";  
  
$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_NEXT );  
print "$row[1]\n";  
  
$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_PRIOR );  
print "$row[1]..\n";  
  
$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_ABS, 0 );  
print_r($row);  
  
$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_LAST );  
print_r($row);  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Spécification d’un type de curseur et sélection de lignes](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md)  
  
