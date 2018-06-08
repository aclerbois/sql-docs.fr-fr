---
title: setBytes (méthode) (long, byte, int, int) | Documents Microsoft
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
- SQLServerBlob.setBytes (long.byte[], int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7def226c-b211-459e-8c1a-08592d75d4a4
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4b18005c16cd62358eb5f269504fc9d80eadb6de
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setbytes-method-long-byte-int-int"></a>setBytes (long, byte, int, int) (méthode)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Enregistre tout ou partie du tableau d'octets spécifié dans l'objet BLOB, en démarrant à la position, au décalage et à la longueur spécifiés, puis retourne le nombre d'octets écrits.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public int setBytes(long pos,  
                    byte[] bytes,  
                    int offset,  
                    int len)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *bons de commande*  
  
 Position (base 1) dans l'objet BLOB à laquelle démarrer l'écriture des données.  
  
 *Octets*  
  
 Tableau d'octets à écrire dans le BLOB.  
  
 *offset*  
  
 Le décalage en octets de tableau auquel commencer la lecture des données à partir de la **octets** tableau.  
  
 *len*  
  
 Nombre d'octets à lire depuis le tableau d'octets dans l'objet BLOB.  
  
## <a name="return-value"></a>Valeur retournée  
 Un **int** contenant le nombre d’octets écrits.  
  
## <a name="exceptions"></a>Exceptions  
 java.sql.SQLException  
  
## <a name="remarks"></a>Notes  
 Cette méthode setBytes est spécifiée par la méthode setBytes dans l’interface java.sql.Blob.  
  
 Les données sont remplacées en démarrant à la position spécifiée et peuvent dépasser la longueur initiale de l'objet BLOB. La spécification d'une valeur position+1 permet d'ajouter des octets. Le passage d'une valeur position+2 ou supérieure (ou inférieure ou égale à zéro) génère une erreur de position. Passage d’une longueur nulle **octets** tableau retourne zéro car aucun octet n’a été écrit.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode setBytes &#40;SQLServerBlob&#41;](../../../connect/jdbc/reference/setbytes-method-sqlserverblob.md)   
 [Méthodes SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [Membres de SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob, classe](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
