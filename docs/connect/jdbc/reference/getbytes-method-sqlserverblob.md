---
title: Méthode getBytes (SQLServerBlob) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLServerBlob.getBytes
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bea1b810-b5c1-466d-bdc4-561468214632
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7a872d042c0a96123cccffac5c5a6f8e2b686c4d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getbytes-method-sqlserverblob"></a>Méthode getBytes (SQLServerBlob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Obtient les données de BLOB sous forme de tableau d'octets.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public byte[] getBytes(long pos,  
                       int length)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *bons de commande*  
  
 Position de départ à 1 (et non 0).  
  
 *length*  
  
 Longueur des données à obtenir.  
  
## <a name="return-value"></a>Valeur retournée  
 A **octets** tableau contenant les données demandées.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getBytes est spécifiée par la méthode getBytes dans l’interface java.sql.Blob.  
  
 Si vous ont une valeur null ou une longueur de zéro objets BLOB et essayer d’obtenir exactement zéro octet à la position 1, vide **octets** tableau est retourné (tableau d’octets de longueur 0).  
  
 Si vous avez un BLOB de valeur Null ou de longueur zéro et que vous essayez d'obtenir une longueur d'octets quelconque à une position autre que la position 1, une exception de position est levée.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [Membres de SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob, classe](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
