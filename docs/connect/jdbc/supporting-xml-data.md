---
title: Prise en charge des données XML | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 32b7217e-1f0c-473d-9a45-176daa81584e
caps.latest.revision: 26
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 56c724017d364f3e581a6f4add22ece0091a2406
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="supporting-xml-data"></a>Prise en charge des données XML
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] fournit un **xml** type de données qui vous permet de stocker des documents XML et des fragments dans un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données. Le **xml** type de données est un type de données intégré dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]et est quelque peu similaire aux autres types intégrés, tels que **int** et **varchar**. Comme d’autres types intégrés, vous pouvez utiliser la **xml** de type de données en tant que : un type de variable, un type de paramètre, un type de retour de fonction ou une colonne de type lorsque vous créez un tableau ; ou dans [!INCLUDE[tsql](../../includes/tsql_md.md)] fonctions CAST et CONVERT. Dans le pilote JDBC, le **xml** type de données peut être mappé comme chaîne, tableau d’octets, flux, CLOB, BLOB ou objet SQLXML. Le mappage en tant que chaîne est le mappage par défaut.  
  
 Le pilote JDBC prend en charge l'API JDBC 4.0, ce qui permet l'introduction de l'interface SQLXML. L'interface SQLXML définit des méthodes d'interaction et de manipulation des données XML. Le **SQLXML** est un type de données JDBC 4.0 et il est mappé à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] **xml** type de données. Par conséquent, pour pouvoir utiliser le type de données SQLXML dans vos applications, vous devez définir l'instruction classpath afin d'inclure le fichier sqljdbc4.jar. Si l'application tente d'utiliser le fichier sqljdbc3.jar lors de l'accès à l'objet SQLXML et à ses méthodes, une exception est levée.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] valide toujours les données XML avant de les stocker dans la colonne de base de données. Les applications peuvent utiliser **SQLXML** de type de données, car le pilote JDBC le mappe à la **xml** automatiquement de type de données. Le **SQLXML** prise en charge est disponible dans sqljdbc4.jar. Consultez [configuration système requise pour le pilote JDBC](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md) pour obtenir la liste des versions JRE prises en charge par le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].  
  
 Les rubriques de cette section décrivent l’interface SQLXML et expliquent comment programmer la **SQLXML** type de données en utilisant les méthodes de l’API JDBC.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Interface SQLXML](../../connect/jdbc/sqlxml-interface.md)|Décrit l'interface SQLXML et ses méthodes.|  
|[Programmation avec SQLXML](../../connect/jdbc/programming-with-sqlxml.md)|Décrit comment utiliser le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] méthodes API permettant de stocker et récupérer des données XML dans une base de données relationnelle avec le **SQLXML** type de données Java. Contient également des informations sur les types d'objets SQLXML ; par ailleurs, elle dresse une liste des recommandations et limitations importantes relatives à l'utilisation des objets SQLXML.|  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation des types de données du pilote JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md)  
  
  
