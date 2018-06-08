---
title: Méthode getSubString (SQLServerNClob) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 1d91c930-1bac-4da9-b9a5-ac2cfd31541b
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8740faa9070f785b8610c847f0b5871f0e21dbd4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="getsubstring-method-sqlservernclob"></a>Méthode getSubString (SQLServerNClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère une copie de la sous-chaîne spécifiée dans le **NCLOB** en fonction de la position de départ spécifiée et le nombre de caractères à copier.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.lang.String getSubString(long pos,  
                                  int length)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *bons de commande*  
  
 Premier caractère de la sous-chaîne à extraire. Le premier caractère est à la position 1.  
  
 *length*  
  
 Nombre des caractères consécutifs à copier.  
  
## <a name="return-value"></a>Valeur retournée  
 A **chaîne** qui est la sous-chaîne spécifiée dans le **NCLOB**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getSubString est spécifiée par la méthode getSubString dans l’interface java.sql.NClob.  
  
 La tentative d'obtention de zéro caractère depuis un NCLOB avec une valeur Null ou de longueur zéro retourne une chaîne vide. La tentative d'obtention d'une longueur de caractères quelconque à n'importe quelle position autre que la position 1 dans un NCLOB de longueur zéro entraîne la levée d'une exception de position.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthodes SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-methods.md)   
 [Membres de SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-members.md)   
 [SQLServerNClob, classe](../../../connect/jdbc/reference/sqlservernclob-class.md)  
  
  
