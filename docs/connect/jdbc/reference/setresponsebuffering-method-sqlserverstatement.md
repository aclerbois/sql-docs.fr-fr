---
title: Méthode setResponseBuffering (SQLServerStatement) | Documents Microsoft
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
- SQLServerStatement.setResponseBuffering(String responseBufferingValue)
apilocation:
- SQLServerStatement.setResponseBuffering(String responseBufferingValue)
apitype: Assembly
ms.assetid: 9f489835-6cda-4c8c-b139-079639a169cf
caps.latest.revision: 24
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 114bba1ad782a3cc14585267407386d30f4ac0b6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setresponsebuffering-method-sqlserverstatement"></a>Méthode setResponseBuffering (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit la réponse mise en mémoire tampon de mode pour cette [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet à la casse **chaîne complète** ou **adaptive**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final void setResponseBuffering(java.lang.String value)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *valeur*  
  
 A **chaîne** qui contient le mode de mise en mémoire tampon de réponse. Le mode valid peut être une des chaînes de casse suivantes : **complète** ou **adaptive**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 **Adaptive** spécifie la mise en mémoire tampon minimale des données lorsque cela est nécessaire.  
  
 **complète** spécifie la lecture de l’ensemble de résultats à partir du serveur en cours d’exécution.  
  
 « Adaptive » est la valeur par défaut du pilote JDBC versions 2.0 et 3.0. la valeur par défaut avant la version 2.0 du pilote JDBC a été complète.  
  
 Le [setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) méthode vous permet de substituer le **responseBuffering** connexion **chaîne** propriété actuel [ SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) objet. Pour plus d’informations sur l’utilisation du mode de mise en mémoire tampon de réponse, consultez [à l’aide de mise en mémoire tampon adaptative](../../../connect/jdbc/using-adaptive-buffering.md).  
  
 Si l’application spécifie une valeur de paramètre non valide pour le [setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) (méthode), un [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md) est levée.  
  
## <a name="see-also"></a>Voir aussi  
 [Membres de SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Classe SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)   
 [Utilisation de la mise en mémoire tampon adaptative](../../../connect/jdbc/using-adaptive-buffering.md)  
  
  
