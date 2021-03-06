---
title: PDO::__construct | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: ''
ms.component: php
ms.reviewer: ''
ms.suite: sql
ms.technology:
- drivers
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 3ee53aff-6fe4-44cd-a15b-51770c98c712
caps.latest.revision: ''
author: MightyPen
ms.author: genemi
manager: jhubbard
ms.workload: Inactive
ms.openlocfilehash: 3a1c40e8e31cbba9eb93155c3f81f6dd03452b59
ms.sourcegitcommit: 2e130e9f3ce8a7ffe373d7fba8b09e937c216386
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="pdoconstruct"></a>PDO::__construct
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Crée une connexion à une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] .  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
PDO::__construct($dsn [,$username [,$password [,$driver_options ]]] )  
```  
  
#### <a name="parameters"></a>Paramètres  
*$dsn*: une chaîne qui contient le nom du préfixe (toujours `sqlsrv`), un signe deux-points et le mot clé Server. Par exemple, `"sqlsrv:server=(local)"`. Vous pouvez éventuellement spécifier d’autres mots clés de connexion. Consultez [Connection Options](../../connect/php/connection-options.md) pour obtenir la description du mot clé Server et des autres mots clés de connexion. La totalité de *$dsn* est entre guillemets, si bien que chaque mot clé de connexion ne doit pas être individuellement mis entre guillemets.  
  
*$username*: facultatif. Chaîne qui contient le nom de l’utilisateur. Pour vous connecter en utilisant l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , spécifiez l’ID de connexion. Pour vous  connecter avec l’authentification Windows, spécifiez `""`.  
  
*$password*: facultatif. Chaîne qui contient le mot de passe de l’utilisateur. Pour vous connecter en utilisant l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , spécifiez le mot de passe. Pour vous  connecter avec l’authentification Windows, spécifiez `""`.  
  
*$driver_options*: facultatif. Vous pouvez spécifier les attributs du Gestionnaire de pilotes PDO et [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] attributs de pilotes spécifique : PDO::SQLSRV_ATTR_ENCODING, PDO::SQLSRV_ATTR_DIRECT_QUERY. Un attribut non valide ne génère pas d’exception. Les attributs non valides lèvent des exceptions quand ils sont spécifiés avec [PDO::setAttribute](../../connect/php/pdo-setattribute.md).  
  
## <a name="return-value"></a>Valeur retournée  
Retourne un objet PDO. En cas d’échec, retourne un objet PDOException.  
  
## <a name="exceptions"></a>Exceptions  
PDOException  
  
## <a name="remarks"></a>Notes  
Vous pouvez fermer un objet de connexion en affectant à l’instance la valeur Null.  
  
Une fois la connexion, PDO::errorCode affiche 01000 au lieu de 00000.  
  
Si PDO::__construct échoue pour une raison quelconque, une exception est levée, même si PDO::ATTR_ERRMODE a la valeur PDO::ERRMODE_SILENT.  
  
La prise en charge de PDO a été ajoutée dans la version 2.0 de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a>Exemple  
Cet exemple montre comment se connecter à un serveur en utilisant l’authentification Windows et spécifier une base de données.  
  
```  
<?php  
   $c = new PDO( "sqlsrv:Server=(local) ; Database = AdventureWorks ", "", "", array(PDO::SQLSRV_ATTR_DIRECT_QUERY => true));   
  
   $query = 'SELECT * FROM Person.ContactType';   
   $stmt = $c->query( $query );   
   while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ) {   
      print_r( $row );   
   }  
   $c = null;   
?>  
```  
  
## <a name="example"></a>Exemple  
Cet exemple montre comment se connecter à un serveur en spécifiant la base de données plus tard.  
  
```  
<?php  
   $c = new PDO( "sqlsrv:server=(local)");  
  
   $c->exec( "USE AdventureWorks");  
   $query = 'SELECT * FROM Person.ContactType';  
   $stmt = $c->query( $query );  
   while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){  
      print_r( $row );  
   }  
   $c = null;  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Classe PDO](../../connect/php/pdo-class.md)

[PDO](http://php.net/manual/book.pdo.php)  
  
