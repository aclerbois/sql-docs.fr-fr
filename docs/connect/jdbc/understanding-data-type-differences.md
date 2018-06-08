---
title: Différences entre les types de données de présentation | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ab8fa00f-cb16-47e2-94b8-3a76f56c2b84
caps.latest.revision: 33
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5395b915d2f261813495d364730d0442a8139334
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-data-type-differences"></a>Différences entre les types de données
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Il existe plusieurs différences entre les types de données de le langage de programmation Java et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données. Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] vous aide à maîtriser les différences entre les différents types de conversions.  
  
## <a name="character-types"></a>Types de caractères  
 Les types de données de chaîne de caractères JDBC sont **CHAR**, **VARCHAR**, et **LONGVARCHAR**. Le pilote JDBC prend en charge l'API JDBC 4.0. Dans JDBC 4.0, les types de données de chaîne de caractères JDBC peuvent également être **NCHAR**, **NVARCHAR**, et **LONGNVARCHAR**. Ces nouveaux types de chaînes de caractères conservent les types de caractères natifs Java au format Unicode et rendent inutiles les conversions ANSI vers Unicode ou Unicode vers ANSI.  
  
|Type| Description|  
|----------|-----------------|  
|Longueur fixe|Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **char** et **nchar** des types de données sont mappés directement aux JDBC **CHAR** et **NCHAR** types. Il s'agit de types de longueur fixe avec un remplissage fourni par le serveur au cas où la colonne a SET ANSI_PADDING ON. Remplissage est toujours activé pour **nchar**, mais pour **char**, dans le cas où les colonnes char du serveur ne sont pas complétées, le pilote JDBC ajoute le remplissage.|  
|Longueur variable|Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **varchar** et **nvarchar** types mappent directement aux types JDBC **VARCHAR** et **NVARCHAR** types, respectivement.|  
|Long|Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **texte** et **ntext** types mappent aux types JDBC **LONGVARCHAR** et **LONGNVARCHAR** type, respectivement. Ces types sont déconseillés depuis [!INCLUDE[ssVersion2005](../../includes/ssversion2005_md.md)], vous devez utiliser des types de valeur élevée, **varchar (max)** ou **nvarchar (max)** à la place.<br /><br /> À l’aide de la mise à jour\<Type numérique > et [updateObject (int, java.lang.Object)](../../connect/jdbc/reference/updateobject-method-int-java-lang-object.md) méthodes échoue sur **texte** et **ntext** colonnes de serveur. Toutefois, à l’aide de la [setObject](../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md) méthode avec un type de conversion de caractère spécifiée est prise en charge par rapport à **texte** et **ntext** colonnes de serveur.|  
  
## <a name="binary-string-types"></a>Types de chaînes binaires  
 Les types de chaînes binaires JDBC sont **binaire**, **VARBINARY**, et **LONGVARBINARY**.  
  
|Type| Description|  
|----------|-----------------|  
|Longueur fixe|Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **binaire** est directement mappée à JDBC de type **binaire** type. Il s'agit d'un type de longueur fixe avec une marge fournie par le serveur au cas où la colonne a SET ANSI_PADDING ON. Si les colonnes char du serveur ne disposent pas de marge, le pilote JDBC ajoute la marge.<br /><br /> Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **timestamp** type est un JDBC **binaire** type avec une longueur fixe de 8 octets.|  
|Longueur variable|Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **varbinary** type correspond à JDBC **VARBINARY** type.<br /><br /> Le **udt** tapez [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] mappe à JDBC comme un **VARBINARY** type.|  
|Long|Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **image** type correspond à JDBC **LONGVARBINARY** type. Ce type est déconseillé à compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005_md.md)], vous devez utiliser un type de valeur élevée, **varbinary (max)** à la place.|  
  
## <a name="exact-numeric-types"></a>Types numériques exacts  
 Les types numériques exacts JDBC sont mappés directement aux types SQL Server correspondants.  
  
|Type| Description|  
|----------|-----------------|  
|BIT|JDBC **bits** type représente un bit unique qui peut être 0 ou 1. Correspond à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **bits** type.|  
|TINYINT|JDBC **TINYINT** type représente un octet unique. Correspond à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **tinyint** type.|  
|SMALLINT|JDBC **SMALLINT** type représente un entier 16 bits signé. Correspond à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **smallint** type.|  
|INTEGER|JDBC **entier** type représente un entier signé de 32 bits. Correspond à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **int** type.|  
|bigint|JDBC **BIGINT** type représente un entier signé de 64 bits. Correspond à un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **bigint** type.|  
|NUMERIC|JDBC **numérique** type représente une valeur décimale à précision fixe qui contient des valeurs de précision identique. Le **numérique** type est mappé sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **numérique** type.|  
|DECIMAL|JDBC **décimal** type représente une valeur décimale à précision fixe qui conserve des valeurs au moins les précision spécifiée. Le **décimal** type est mappé sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **décimal** type.<br /><br /> JDBC **décimal** type correspond également à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **money** et **smallmoney** types, qui sont des types décimaux à précision fixe spécifiques stockés dans 8 et 4 octets, respectivement.|  
  
## <a name="approximate-numeric-types"></a>Types numériques approximatifs  
 Types numériques approximatifs JDBC sont **réel**, **DOUBLE**, et **FLOAT**.  
  
|Type| Description|  
|----------|-----------------|  
|REAL|JDBC **réel** type a sept chiffres de précision (précision unique) et mappe directement à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **réel** type.|  
|DOUBLE|JDBC **DOUBLE** type a 15 chiffres de précision (précision double) et correspond à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **float** type. JDBC **FLOAT** type est un synonyme de **DOUBLE**. Car il peut y avoir confusion entre **FLOAT** et **DOUBLE**, **DOUBLE** est préféré.|  
  
## <a name="datetime-types"></a>Types de date/d'heure  
 JDBC **TIMESTAMP** type est mappé sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **datetime** et **smalldatetime** types. Le **datetime** type est stocké dans deux entiers de 4 octets. Le **smalldatetime** type conserve les mêmes informations (date et heure), mais avec une précision inférieure, dans deux petits entiers de 2 octets.  
  
> [!NOTE]  
>  Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **timestamp** type est un type de chaîne binaire de longueur fixe. Il ne correspond à un des types de temps JDBC : **DATE**, **temps**, ou **TIMESTAMP**.  
  
## <a name="custom-type-mapping"></a>Mappage de type personnalisé  
 La fonctionnalité de mappage de type personnalisé de JDBC qui utilise les interfaces SQLData JDBC avancée types (UDT, Struct, etc.). n'est pas implémentée dans le pilote JDBC.  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation des types de données du pilote JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md)  
  
  
