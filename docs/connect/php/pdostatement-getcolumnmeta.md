---
title: PDOStatement::getColumnMeta | Documents Microsoft
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
ms.assetid: c92a21cc-8e53-43d0-a4bf-542c77c100c9
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 954bc27f3bfaaf14a9ccb9fd7b97ffee7b9f274c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pdostatementgetcolumnmeta"></a>PDOStatement::getColumnMeta
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère des métadonnées pour une colonne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
array PDOStatement::getColumnMeta ( $column );  
```  
  
#### <a name="parameters"></a>Paramètres  
*$conn*: (entier) numéro de base zéro de la colonne dont vous souhaitez récupérer les métadonnées.  
  
## <a name="return-value"></a>Valeur retournée  
Tableau associatif (clé et valeur) contenant les métadonnées pour la colonne. Consultez la section Notes pour obtenir une description des champs du tableau.  
  
## <a name="remarks"></a>Notes  
Le tableau suivant décrit les champs du tableau retourné par getColumnMeta.  
  
|NAME|VALUES|  
|--------|----------|  
|native_type|Spécifie le type PHP de la colonne. Toujours une chaîne.|  
|driver:decl_type|Spécifie le type SQL utilisé pour représenter la valeur de la colonne dans la base de données. Si la colonne dans le jeu de résultats est le résultat d’une fonction, cette valeur n’est pas retournée par PDOStatement::getColumnMeta.|  
|flags|Spécifie les indicateurs définis pour cette colonne. Toujours 0.|  
|NAME|Spécifie le nom de la colonne dans la base de données.|  
|table|Spécifie le nom de la table qui contient la colonne dans la base de données. Toujours vide.|  
|len|Spécifie la longueur de la colonne.|  
|precision|Spécifie la précision numérique de cette colonne.|  
|pdo_type|Spécifie le type de cette colonne comme représenté par les constantes PDO::PARAM_*. Toujours PDO::PARAM_STR (2).|  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a>Exemple  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$stmt = $conn->query("select * from Person.ContactType");  
$metadata = $stmt->getColumnMeta(2);  
var_dump($metadata);  
  
print $metadata['sqlsrv:decl_type'] . "\n";  
print $metadata['native_type'] . "\n";  
print $metadata['name'];  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[PDOStatement, classe](../../connect/php/pdostatement-class.md)

[PDO](http://php.net/manual/book.pdo.php)  
  
