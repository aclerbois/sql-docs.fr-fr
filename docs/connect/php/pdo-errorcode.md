---
title: PDO::ErrorCode | Documents Microsoft
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
ms.assetid: 5864b1d8-6814-41cd-a88d-415124484c13
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cf5f96568c52747a188205ba794e10d1c42b07d6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pdoerrorcode"></a>PDO::errorCode
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

PDO::errorCode récupère la valeur SQLSTATE de la dernière opération sur le handle de base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
mixed PDO::errorCode();  
```  
  
## <a name="return-value"></a>Valeur retournée  
PDO::errorCode retourne une valeur SQLSTATE à cinq caractères sous la forme d’une chaîne, ou NULL si aucune opération n’a eu lieu sur le handle de base de données.  
  
## <a name="remarks"></a>Notes  
PDO::ErrorCode dans le pilote PDO_SQLSRV retourne des avertissements sur certaines opérations réussies. Par exemple, sur une connexion réussie, PDO::errorCode retourne « 01000 », ce qui indique SQL_SUCCESS_WITH_INFO.  
  
PDO::errorCode récupère uniquement les codes d’erreur des opérations exécutées directement sur la connexion de base de données. Si vous créez une instance de PDOStatement via PDO::prepare ou PDO::query et une erreur est générée sur l’objet d’instruction, PDO::errorCode ne récupère pas cette erreur. Vous devez appeler PDOStatement::errorCode pour retourner le code d’erreur d’une opération effectuée sur un objet d’instruction particulier.  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a>Exemple  
Dans cet exemple, le nom de la colonne est mal orthographié (`Cityx` au lieu de `City`), à l’origine d’une erreur, qui est ensuite signalée.  
  
```  
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Classe PDO](../../connect/php/pdo-class.md)

[PDO](http://php.net/manual/book.pdo.php)  
  
