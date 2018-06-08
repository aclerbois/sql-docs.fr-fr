---
title: PDO::Query | Documents Microsoft
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
ms.assetid: f6f5e6d4-8ca9-4f06-89ed-de65ad3952a2
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fb23a63d77461cb13784c515bd0638af277e63b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pdoquery"></a>PDO::Query
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Exécute une requête SQL et retourne un jeu de résultats sous la forme d’un objet PDOStatement.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
PDOStatement PDO::query ($statement[, $fetch_style);  
```  
  
#### <a name="parameters"></a>Paramètres  
*$statement*: instruction SQL à exécuter.  
  
*$fetch_style*: instructions facultatives sur l’exécution de la requête. Pour plus d’informations, consultez la section Notes. $*fetch_style* dans PDO::query peut être remplacé par $*fetch_style* dans PDO::fetch.  
  
## <a name="return-value"></a>Valeur retournée  
Si l’appel réussit, PDO::query retourne un objet PDOStatement. Si l’appel échoue, PDO::query génère un objet PDOException ou retourne la valeur false, selon le paramètre de PDO::ATTR_ERRMODE.  
  
## <a name="exceptions"></a>Exceptions  
PDOException.  
  
## <a name="remarks"></a>Notes  
Une requête exécutée avec PDO::query peut s’exécuter soit une instruction préparée ou directement, selon le paramètre de PDO::SQLSRV_ATTR_DIRECT_QUERY. Pour plus d’informations, consultez [Exécution d’instruction directe et exécution d’instruction préparée dans le pilote PDO_SQLSRV](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).  
  
PDO::SQLSRV_ATTR_QUERY_TIMEOUT affecte également le comportement de PDO::exec ; Pour plus d’informations, consultez [PDO::setAttribute](../../connect/php/pdo-setattribute.md).  
  
Vous pouvez spécifier les options suivantes pour $*fetch_style*.  
  
|style| Description|  
|---------|---------------|  
|PDO::FETCH_COLUMN, *num*|Recherche les données dans la colonne spécifiée. La première colonne de la table est la colonne 0.|  
|À PDO::FETCH_CLASS, '*classname*', tableau ( *arglist* )|Crée une instance d’une classe et attribue des noms de colonne aux propriétés de la classe. Si le constructeur de classe accepte un ou plusieurs paramètres, vous pouvez également passer un *arglist*.|  
|À PDO::FETCH_CLASS, '*classname*'|Assigne des noms de colonne aux propriétés dans une classe existante.|  
  
Appelez PDOStatement::closeCursor pour libérer les ressources de base de données associées à l’objet PDOStatement avant de rappeler PDO::query.  
  
Vous pouvez fermer un objet PDOStatement en lui affectant la valeur Null.  
  
Si toutes les données d’un jeu de résultats ne sont pas récupérées, l’appel suivant de PDO::query n’échoue pas.  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a>Exemple  
Cet exemple illustre plusieurs requêtes.  
  
```  
<?php  
$database = "AdventureWorks";  
$conn = new PDO( "sqlsrv:server=(local) ; Database = $database", "", "");  
$conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );  
$conn->setAttribute( PDO::SQLSRV_ATTR_QUERY_TIMEOUT, 1 );  
  
$query = 'select * from Person.ContactType';  
  
// simple query  
$stmt = $conn->query( $query );  
while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){  
   print_r( $row['Name'] ."\n" );  
}  
  
echo "\n........ query for a column ............\n";  
  
// query for one column  
$stmt = $conn->query( $query, PDO::FETCH_COLUMN, 1 );  
while ( $row = $stmt->fetch() ){  
   echo "$row\n";  
}  
  
echo "\n........ query with a new class ............\n";  
$query = 'select * from HumanResources.Department order by GroupName';  
// query with a class  
class cc {  
   function __construct( $arg ) {  
      echo "$arg";  
   }  
  
   function __toString() {  
      return $this->DepartmentID . "; " . $this->Name . "; " . $this->GroupName;  
   }  
}  
  
$stmt = $conn->query( $query, PDO::FETCH_CLASS, 'cc', array( "arg1 " ));  
  
while ( $row = $stmt->fetch() ){  
   echo "$row\n";  
}  
  
echo "\n........ query into an existing class ............\n";  
$c_obj = new cc( '' );  
$stmt = $conn->query( $query, PDO::FETCH_INTO, $c_obj );  
while ( $stmt->fetch() ){  
   echo "$c_obj\n";  
}  
  
$stmt = null;  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Classe PDO](../../connect/php/pdo-class.md)

[PDO](http://php.net/manual/book.pdo.php)  
  
