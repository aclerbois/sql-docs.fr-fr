---
title: PDO::Prepare | Documents Microsoft
ms.custom: ''
ms.date: 07/10/2017
ms.prod: sql
ms.prod_service: connectivity
ms.component: php
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a8b16fdc-c748-49be-acf2-a6ac7432d16b
caps.latest.revision: 28
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 70bc35baf6121a7a9339064f68d8252b48db22e6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pdoprepare"></a>PDO::prepare
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Prépare une instruction en vue de son exécution.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
PDOStatement PDO::prepare ( $statement [, array(key_pair)] )  
```  
  
#### <a name="parameters"></a>Paramètres  
$*statement*: chaîne qui contient l’instruction SQL.  
  
*key_pair*: un tableau contenant un nom d’attribut et une valeur. Pour plus d'informations, consultez la section Remarques.  
  
## <a name="return-value"></a>Valeur retournée  
En cas de réussite, retourne un objet PDOStatement. En cas d’échec, retourne un objet de PDOException, ou false en fonction de la valeur de PDO::ATTR_ERRMODE.  
  
## <a name="remarks"></a>Notes  
Le [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] n’évalue pas les instructions préparées avant leur exécution.  
  
Le tableau suivant répertorie les possible *key_pair* valeurs.  
  
|Key| Description|  
|-------|---------------|  
|PDO::ATTR_CURSOR|Spécifie le comportement du curseur. La valeur par défaut est PDO::CURSOR_FWDONLY. PDO::CURSOR_SCROLL est un curseur statique.<br /><br />Par exemple, `array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY )`.<br /><br />Si vous utilisez PDO::CURSOR_SCROLL, vous pouvez utiliser PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE, qui est décrit ci-dessous.<br /><br />Consultez [Types de curseurs &#40;pilote PDO_SQLSRV&#41; ](../../connect/php/cursor-types-pdo-sqlsrv-driver.md) pour plus d’informations sur les jeux de résultats et les curseurs dans le pilote PDO_SQLSRV.|  
|PDO::ATTR_EMULATE_PREPARES|Lorsque PDO::ATTR_EMULATE_PREPARES est activé, les espaces réservés dans une instruction préparée est remplacé par les paramètres liés. Une instruction SQL complète avec aucun des espaces réservés est ensuite envoyée à la base de données lors de l’exécution. <br /><br />PDO::ATTR_EMULATE_PREPARES permet de contourner certaines restrictions dans SQL Server. Par exemple, SQL Server ne prend pas en charge des paramètres nommés ou positionnels dans certaines clauses Transact-SQL. En outre, SQL Server a une limite de 2100 paramètres de liaison.<br /><br />Vous pouvez définir l’attribut PDO::ATTR_EMULATE_PREPARES sur true. Par exemple :<br /><br />`PDO::ATTR_EMULATE_PREPARES => true`<br /><br />Par défaut, cet attribut a la valeur false.<br /><br />**Remarque :** La sécurité des requêtes paramétrables n’est pas activée quand vous utilisez `PDO::ATTR_EMULATE_PREPARES => true`. Votre application doit garantir que les données qui sont liées aux paramètres ne contient pas de code Transact-SQL malveillant.<br /><br />**Limitations :**: étant donné que les paramètres ne sont pas liés à l’aide de la fonctionnalité de requête paramétrable de la base de données, paramètres input_output et de sortie ne sont pas pris en charge.|  
|PDO::SQLSRV_ATTR_ENCODING|PDO::SQLSRV_ENCODING_UTF8 (par défaut)<br /><br />PDO::SQLSRV_ENCODING_SYSTEM<br /><br />PDO::SQLSRV_ENCODING_BINARY|  
|PDO::SQLSRV_ATTR_DIRECT_QUERY|Si la valeur est True, spécifie l’exécution de requête directe. False est synonyme d’exécution d’instruction préparée. Pour plus d’informations sur PDO::SQLSRV_ATTR_DIRECT_QUERY, consultez [exécution d’instruction directe et exécution d’instruction préparée dans le pilote PDO_SQLSRV](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|Pour plus d’informations, consultez [PDO::setAttribute](../../connect/php/pdo-setattribute.md).|  
  
Quand vous utilisez PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL, vous pouvez utiliser PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE. Par exemple :  
  
```  
array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL, PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_DYNAMIC));  
```  
  
Le tableau suivant montre les valeurs possibles pour PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE.  
  
|Valeur| Description|  
|---------|---------------|  
|PDO::SQLSRV_CURSOR_BUFFERED|Crée un curseur statique (mis en mémoire tampon) côté client. Pour plus d’informations sur les curseurs côté client, consultez [Types de curseurs &#40;pilote PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).|  
|PDO::SQLSRV_CURSOR_DYNAMIC|Crée un curseur dynamique (sans mise en mémoire tampon) côté serveur, qui vous permet d’accéder aux lignes dans n’importe quel ordre et reflète les modifications dans la base de données.|  
|PDO::SQLSRV_CURSOR_KEYSET_DRIVEN|Crée un curseur de jeu de clés côté serveur. Un curseur de jeu de clés ne met pas à jour le nombre de lignes si une ligne est supprimée de la table (une ligne supprimée est retournée sans valeur).|  
|PDO::SQLSRV_CURSOR_STATIC|Crée un curseur statique côté serveur, qui vous permet d’accéder aux lignes dans n’importe quel ordre mais ne reflète pas les modifications dans la base de données.<br /><br />PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL implique PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_STATIC.|  
  
Vous pouvez fermer un objet PDOStatement en lui affectant la valeur Null.  
  
## <a name="example"></a>Exemple  
Cet exemple montre comment utiliser la méthode PDO::prepare avec des marques et un curseur avant uniquement.  
  
```  
<?php  
$database = "Test";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$col1 = 'a';  
$col2 = 'b';  
  
$query = "insert into Table1(col1, col2) values(?, ?)";  
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );  
$stmt->execute( array( $col1, $col2 ) );  
print $stmt->rowCount();  
echo "\n";  
  
$query = "insert into Table1(col1, col2) values(:col1, :col2)";  
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );  
$stmt->execute( array( ':col1' => $col1, ':col2' => $col2 ) );  
print $stmt->rowCount();  
  
$stmt = null  
?>  
```  
  
## <a name="example"></a>Exemple  
Cet exemple montre comment utiliser la méthode PDO::prepare avec un curseur côté client. Pour voir un exemple montrant un curseur côté serveur, [Types de curseurs &#40;pilote PDO_SQLSRV&#41;](../../connect/php/cursor-types-pdo-sqlsrv-driver.md).  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$query = "select * from Person.ContactType";  
$stmt = $conn->prepare( $query, array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL));  
$stmt->execute();  
  
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
[Classe PDO](../../connect/php/pdo-class.md)

[PDO](http://php.net/manual/book.pdo.php)  
  
