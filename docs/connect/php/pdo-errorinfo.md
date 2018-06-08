---
title: PDO::ErrorInfo | Documents Microsoft
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
ms.assetid: 9d5481d5-13bc-4388-b3aa-78676c0fc709
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ba820eb4c534fb94302c2aa041e573c591376eda
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pdoerrorinfo"></a>PDO::ErrorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Récupère les informations d’erreur étendues de la dernière opération effectuée sur le handle de base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
array PDO::errorInfo();  
```  
  
## <a name="return-value"></a>Valeur retournée  
Tableau d’informations d’erreur sur la dernière opération effectuée sur le handle de base de données. Le tableau comprend les champs suivants :  
  
-   Le code d’erreur SQLSTATE  
  
-   Le code d’erreur propre au pilote  
  
-   Le message d’erreur propre au pilote  
  
Si aucune erreur, ou si la valeur SQLSTATE n’est pas défini, les champs spécifiques au pilote ont la valeur NULL.  
  
## <a name="remarks"></a>Notes  
PDO::ErrorInfo récupère uniquement les informations d’erreur pour les opérations exécutées directement sur la base de données. Utilisez PDOStatement::errorInfo quand vous créez une instance de PDOStatement à l’aide de PDO::prepare ou PDO::query.  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a>Exemple  
Dans cet exemple, le nom de la colonne est mal orthographié (`Cityx` au lieu de `City`), à l’origine d’une erreur, qui est ensuite signalée.  
  
```  
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
echo "\n";  
print_r ($conn->errorInfo());  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Classe PDO](../../connect/php/pdo-class.md)

[PDO](http://php.net/manual/book.pdo.php)  
  
