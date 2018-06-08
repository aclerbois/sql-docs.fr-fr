---
title: sqlsrv_client_info | Documents Microsoft
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
apiname:
- sqlsrv_client_info
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_client_info
- sqlsrv_client_info
ms.assetid: 3e2d3679-436a-45d8-8bdc-7c633b65a720
caps.latest.revision: 47
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2ea94ad6f635a438fc9df0546039137261f3c77e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsrvclientinfo"></a>sqlsrv_client_info
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Retourne des informations sur la pile de connexion et de client.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_client_info( resource $conn)  
```  
  
#### <a name="parameters"></a>Paramètres  
*$conn*: ressource de connexion par laquelle le client est connecté.  
  
## <a name="return-value"></a>Valeur retournée  
Tableau associatif avec les clés décrites dans le tableau ci-dessous, ou **false** si la ressource de connexion est Null.  
  
**Pour PHP pour SQL Server versions 3.2 et 3.1**:  
  
|Key| Description|  
|-------|---------------|  
|DriverDllName|MSODBCSQL11.DLL (ODBC Driver 11 for SQL Server)|  
|DriverODBCVer|Version ODBC (xx.yy)|  
|DriverVer|Version de la DLL ODBC Driver 11 for SQL Server :<br /><br />xx.yy.zzzz ([!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 3.2 ou 3.1)|  
|ExtensionVer|Version php_sqlsrv.dll :<br /><br />3.2.xxxx.x (pour [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 3.2)<br /><br />3.1.xxxx.x (pour [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 3.1)|  
  
**Pour PHP pour SQL Server versions 3.0 et 2.0**:  
  
|Key| Description|  
|-------|---------------|  
|DriverDllName|SQLNCLI10. DLL ([!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 2.0)|  
|DriverODBCVer|Version ODBC (xx.yy)|  
|DriverVer|Version de la DLL SQL Server Native Client :<br /><br />10.50.xxx ([!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 2.0)|  
|ExtensionVer|Version php_sqlsrv.dll :<br /><br />2.0.xxxx.x ([!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] version 2.0)|  
  
## <a name="example"></a>Exemple  
L’exemple suivant écrit des informations de client dans la console quand l’exemple est exécuté à partir de la ligne de commande. L’exemple part du principe que SQL Server est installé sur l’ordinateur local. Toute la sortie est écrite dans la console quand l’exemple est exécuté à partir de la ligne de commande.  
  
```  
<?php  
/*Connect to the local server using Windows Authentication and   
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$conn = sqlsrv_connect( $serverName);  
  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
if( $client_info = sqlsrv_client_info( $conn))  
{  
       foreach( $client_info as $key => $value)  
      {  
              echo $key.": ".$value."\n";  
      }  
}  
else  
{  
       echo "Client info error.\n";  
}  
  
/* Close connection resources. */  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[À propos des exemples de code dans la documentation](../../connect/php/about-code-examples-in-the-documentation.md)  
  
