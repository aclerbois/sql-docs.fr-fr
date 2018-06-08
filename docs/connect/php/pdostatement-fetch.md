---
title: PDOStatement::fetch | Documents Microsoft
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
ms.assetid: 4368e362-5bda-4da1-8462-33714683c39f
caps.latest.revision: 19
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 6fefb85430b6bf3b98d15a884130e1db1d2cdfa8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pdostatementfetch"></a>PDOStatement::fetch
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère une ligne d’un jeu de résultats.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
mixed PDOStatement::fetch ([ $fetch_style[, $cursor_orientation[, $cursor_offset]]] );  
```  
  
#### <a name="parameters"></a>Paramètres  
$*fetch_style*: symbole (entier) facultatif indiquant le format des données de ligne. Consultez la section Notes pour obtenir la liste des valeurs possibles pour $*fetch_style*. La valeur par défaut est PDO::FETCH_BOTH. $*fetch_style* lors de l’extraction méthode remplace le $*fetch_style* spécifié dans la méthode PDO::query.  
  
$*cursor_orientation*: symbole (entier) facultatif indiquant la ligne à récupérer quand l’instruction prepare spécifie `PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL`. Consultez la section Notes pour obtenir la liste des valeurs possibles pour $*cursor_orientation*. Consultez [PDO::prepare](../../connect/php/pdo-prepare.md) pour obtenir un exemple d’utilisation d’un curseur de défilement.  
  
$*cursor_offset*: symbole (entier) facultatif spécifiant la ligne à récupérer quand $*cursor_orientation* est la valeur PDO::FETCH_ORI_ABS ou PDO::FETCH_ORI_REL et PDO::ATTR_CURSOR a la valeur PDO::CURSOR_SCROLL.  
  
## <a name="return-value"></a>Valeur retournée  
Valeur mixte qui retourne une ligne ou la valeur false.  
  
## <a name="remarks"></a>Notes  
Le curseur s’avance automatiquement à l’appel de fetch. Le tableau suivant contient la liste des $ possible*fetch_style* valeurs.  
  
|$*fetch_style*| Description|  
|-------------------|---------------|  
|PDO::FETCH_ASSOC|Spécifie un tableau indexé par nom de colonne.|  
|PDO::FETCH_BOTH|Spécifie un tableau indexé par nom de colonne et dans l’ordre à partir de 0. Il s’agit du paramètre par défaut.|  
|PDO::FETCH_BOUND|Retourne true et attribue les valeurs comme les spécifie [PDOStatement::bindColumn](../../connect/php/pdostatement-bindcolumn.md).|  
|PDO::FETCH_CLASS|Crée une instance et mappe les colonnes sur des propriétés nommées.<br /><br />Appelez [PDOStatement::setFetchMode](../../connect/php/pdostatement-setfetchmode.md) avant d’appeler fetch.|  
|PDO::FETCH_INTO|Actualise une instance de la classe demandée.<br /><br />Appelez [PDOStatement::setFetchMode](../../connect/php/pdostatement-setfetchmode.md) avant d’appeler fetch.|  
|PDO::FETCH_LAZY|Crée des noms de variable pendant l’accès et crée un objet sans nom.|  
|PDO::FETCH_NUM|Spécifie un tableau indexé dans l’ordre des colonnes à partir de zéro.|  
|PDO::FETCH_OBJ|Spécifie un objet sans nom avec des noms de propriété qui correspondent à des noms de colonne.|  
  
Si le curseur est à la fin du jeu de résultats (la dernière ligne a été récupérée et le curseur a franchi les limites du jeu de résultats) et si le curseur est avant uniquement (PDO::ATTR_CURSOR = PDO::CURSOR_FWDONLY), les appels à fetch ultérieurs échouent.  
  
Si le curseur peut défiler (PDO::ATTR_CURSOR = PDO::CURSOR_SCROLL), fetch le replace dans les limites du jeu de résultats. Le tableau suivant contient la liste des $ possible*cursor_orientation* valeurs.  
  
|$*cursor_orientation*| Description|  
|--------------------------|---------------|  
|PDO::FETCH_ORI_NEXT|Récupère la ligne suivante. Il s'agit du paramètre par défaut.|  
|PDO::FETCH_ORI_PRIOR|Récupère la ligne précédente.|  
|PDO::FETCH_ORI_FIRST|Récupère la première ligne.|  
|PDO::FETCH_ORI_LAST|Récupère la dernière ligne.|  
|PDO::FETCH_ORI_ABS, *num*|Récupère la ligne demandée dans $*cursor_offset* par numéro de ligne.|  
|PDO::FETCH_ORI_REL, *num*|Récupère la ligne demandée dans $*cursor_offset* par position relative à la position actuelle.|  
  
Si la valeur spécifiée pour $*cursor_offset* ou $*cursor_orientation* les résultats dans une position en dehors des limites du jeu de résultats, fetch échoue.  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a>Exemple  
  
```  
<?php  
   $server = "(local)";  
   $database = "AdventureWorks";  
   $conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
   print( "\n---------- PDO::FETCH_CLASS -------------\n" );  
   $stmt = $conn->query( "select * from HumanResources.Department order by GroupName" );  
  
   class cc {  
      function __construct( $arg ) {  
         echo "$arg";  
      }  
  
      function __toString() {  
         return $this->DepartmentID . "; " . $this->Name . "; " . $this->GroupName;  
      }  
   }  
  
   $stmt->setFetchMode(PDO::FETCH_CLASS, 'cc', array( "arg1 " ));  
   while ( $row = $stmt->fetch(PDO::FETCH_CLASS)) {   
      print($row . "\n");   
   }  
  
   print( "\n---------- PDO::FETCH_INTO -------------\n" );  
   $stmt = $conn->query( "select * from HumanResources.Department order by GroupName" );  
   $c_obj = new cc( '' );  
  
   $stmt->setFetchMode(PDO::FETCH_INTO, $c_obj);  
   while ( $row = $stmt->fetch(PDO::FETCH_INTO)) {   
      echo "$c_obj\n";  
   }  
  
   print( "\n---------- PDO::FETCH_ASSOC -------------\n" );  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $result = $stmt->fetch( PDO::FETCH_ASSOC );  
   print_r( $result );  
  
   print( "\n---------- PDO::FETCH_NUM -------------\n" );  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $result = $stmt->fetch( PDO::FETCH_NUM );  
   print_r ($result );  
  
   print( "\n---------- PDO::FETCH_BOTH -------------\n" );  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $result = $stmt->fetch( PDO::FETCH_BOTH );  
   print_r( $result );  
  
   print( "\n---------- PDO::FETCH_LAZY -------------\n" );  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $result = $stmt->fetch( PDO::FETCH_LAZY );  
   print_r( $result );  
  
   print( "\n---------- PDO::FETCH_OBJ -------------\n" );  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $result = $stmt->fetch( PDO::FETCH_OBJ );  
   print $result->Name;  
   print( "\n \n" );  
  
   print( "\n---------- PDO::FETCH_BOUND -------------\n" );  
   $stmt = $conn->query( "select * from Person.ContactType where ContactTypeID < 5 " );  
   $stmt->bindColumn('Name', $name);  
   $result = $stmt->fetch( PDO::FETCH_BOUND );  
   print $name;  
   print( "\n \n" );  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[PDOStatement, classe](../../connect/php/pdostatement-class.md)

[PDO](http://php.net/manual/book.pdo.php)  
  
