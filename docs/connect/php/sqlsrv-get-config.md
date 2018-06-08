---
title: sqlsrv_get_config | Documents Microsoft
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
- sqlsrv_get_config
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_get_config
- sqlsrv_get_config
ms.assetid: ce2befc2-af98-45bb-8d41-60f1674dccfc
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 433f5dc1d997c4f8756ad48a446aebff977cb908
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlsrvgetconfig"></a>sqlsrv_get_config
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Retourne la valeur actuelle du paramètre de configuration spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqlsrv_get_config( string $setting )  
```  
  
#### <a name="parameters"></a>Paramètres  
*$setting*: paramètre de configuration pour lequel la valeur est retournée. Pour obtenir la liste des paramètres configurables, consultez [sqlsrv_configure](../../connect/php/sqlsrv-configure.md).  
  
## <a name="return-value"></a>Valeur retournée  
Valeur du paramètre spécifié par le paramètre *$setting* . Si un paramètre non valide est spécifié, **false** est retourné et une erreur est ajoutée à la collection d’erreurs.  
  
## <a name="remarks"></a>Notes  
Si **false** est retourné par **sqlsrv_get_config**, vous devez appeler [sqlsrv_errors](../../connect/php/sqlsrv-errors.md) pour déterminer si une erreur s’est produite ou si **false** est la valeur du paramètre spécifié par le paramètre *$setting* .  
  
## <a name="see-also"></a>Voir aussi  
[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[sqlsrv_configure](../../connect/php/sqlsrv-configure.md)  

[sqlsrv_errors](../../connect/php/sqlsrv-errors.md)  
  
