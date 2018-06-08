---
title: Méthode updateBinaryStream (int, java.io.InputStream) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 1db3a975-c108-45d1-8c0d-14a094f391bd
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3682c8418ff107eb7ef7a7c91797a9d859a5f1d8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updatebinarystream-method-int-javaioinputstream"></a>Méthode updateBinaryStream (int, java.io.InputStream)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée avec la valeur de flux binaire.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateBinaryStream(int columnIndex,  
                               java.io.InputStream x)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 Un **int** qui indique l’index de colonne.  
  
 *x*  
  
 Un objet InputStream.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode updateBinaryStream est spécifiée par la méthode updateBinaryStream dans l’interface java.sql.ResultSet.  
  
 À l’aide de cette méthode pour le **image**, **texte**, et **ntext** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] des types de données peuvent avoir un impact sur les performances.  
  
 Cette méthode passe les octets à partir d’un objet InputStream sélectionné [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] colonnes binaires telles que binary, varbinary, varbinary (max), image, xml et udt. La mise à jour de colonnes de caractères n'est pas prise en charge avec cette méthode. Pour mettre à jour des colonnes de type caractère avec un InputStream, utilisez le [updateAsciiStream](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md) (méthode).  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode updateBinaryStream &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatebinarystream-method-sqlserverresultset.md)   
 [Membres de SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
