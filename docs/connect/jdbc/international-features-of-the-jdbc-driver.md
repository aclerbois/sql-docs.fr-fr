---
title: Fonctionnalités internationales du pilote JDBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: bbb74a1d-9278-401f-9530-7b5f45aa79de
caps.latest.revision: 40
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1fd4eb01d7ab8aa2314dfb8f45e0bd08c3b49488
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="international-features-of-the-jdbc-driver"></a>Caractéristiques internationales du pilote JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Les caractéristiques d’internationalisation du [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] incluent les éléments suivants :  
  
-   Prise en charge pour une expérience entièrement localisée dans les mêmes langues que [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]  
  
-   Prise en charge des conversions de langage Java pour les paramètres régionaux sensibles [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] données  
  
-   Prise en charge des langues internationales, quel que soit le système d’exploitation  
  
-   Prise en charge des noms de domaine internationaux (à compter du pilote Microsoft JDBC 6.0 pour SQL Server)  
  
## <a name="handling-of-character-data"></a>Gestion des données caractères  
 Données de caractères dans Java sont traitées en tant qu’Unicode par défaut ; le Java **chaîne** objet représente les données de caractères Unicode. Dans le pilote JDBC, les méthodes getter et setter du flux de données ASCII constituent la seule exception à cette règle ; il s'agit de cas à part, car ces méthodes utilisent des flux d'octets en se basant sur l'hypothèse implicite de pages de codes uniques et bien connues (ASCII).  
  
 En outre, le pilote JDBC fournit la **sendStringParametersAsUnicode** propriété de chaîne de connexion. Cette propriété peut être utilisée pour spécifier que les paramètres préparés pour les données caractères sont envoyés en tant que caractères ASCII ou en tant que jeu de caractères multioctets (MBCS), et non en tant que caractères Unicode. Pour plus d’informations sur la **sendStringParametersAsUnicode** propriété de chaîne de connexion, consultez [définissant les propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md).  
  
### <a name="driver-incoming-conversions"></a>Conversions entrantes du pilote  
 Les données de type texte Unicode provenant du serveur ne doivent pas être converties. Elles sont transmises directement en Unicode. Les données non-Unicode provenant du serveur sont converties en Unicode à partir de la page de codes des données, au niveau base de données ou colonne. Le pilote JDBC utilise les routines de conversion de machine virtuelle Java (Java Virtual Machine, JVM) pour effectuer ces conversions. Ces conversions sont effectuées sur toutes les méthodes getter des flux de données de type chaînes et caractères.  
  
 Si la machine virtuelle Java (JVM) ne prend pas en charge la page de codes appropriée pour les données de la base de données, le pilote JDBC lève l'exception suivante : « Page de codes XXX non prise en charge par l'environnement Java ». Pour résoudre ce problème, vous devez installer la prise en charge complète des caractères internationaux requise par cette machine virtuelle Java. Pour obtenir un exemple, consultez la documentation relative aux encodages pris en charge sur le site Web de Sun Microsystems.  
  
### <a name="driver-outgoing-conversions"></a>Conversions sortantes du pilote  
 Les données caractères allant du pilote au serveur peuvent être au format ASCII ou Unicode. Par exemple, les nouvelles méthodes JDBC 4.0 caractères nationaux, telles que les méthodes setNString, setNCharacterStream et setNClob de [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) et [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classes, toujours envoyer leurs valeurs de paramètres au serveur au format Unicode.  
  
 En revanche, les méthodes de l’API des caractères non nationaux, telles que les méthodes setString, setCharacterStream et setClob de [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) et [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classes envoyer leurs valeurs au serveur au format Unicode uniquement lorsque le **sendStringParametersAsUnicode** propriété est définie sur « true », qui est la valeur par défaut.  
  
## <a name="non-unicode-parameters"></a>Paramètres non Unicode  
 Pour des performances optimales avec **CHAR**, **VARCHAR** ou **LONGVARCHAR** le type de paramètres non Unicode, définissez le **sendStringParametersAsUnicode** propriété sur « false » de type chaîne de connexion et utiliser les méthodes des caractères non nationaux.  
  
## <a name="formatting-issues"></a>Problèmes de mise en forme  
 Pour la date, heure et les devises, toute mise en forme des données localisées est effectuée au niveau du langage Java à l’aide de l’objet de paramètres régionaux ; et les différentes méthodes de mise en forme pour **Date**, **calendrier**, et **nombre** des types de données. Dans le cas, rare, où le pilote JDBC doit passer des données sensibles respectant les paramètres régionaux dans un format localisé, le programme approprié de mise en forme est utilisé avec les paramètres régionaux par défaut de la machine virtuelle Java.  
  
## <a name="collation-support"></a>Prise en charge du classement  
 Le pilote JDBC version 3.0 prend en charge tous les classements pris en charge par [!INCLUDE[ssVersion2000](../../includes/ssversion2000_md.md)], [!INCLUDE[ssVersion2005](../../includes/ssversion2005_md.md)]et les nouveaux classements ou les nouvelles versions de Windows, noms de classements introduites dans [!INCLUDE[ssKatmai](../../includes/sskatmai_md.md)].  
  
 Pour plus d’informations sur les classements, consultez [prise en charge Unicode et du classement](http://go.microsoft.com/fwlink/?LinkId=131366) et [nom de classement Windows (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=131367) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="using-international-domain-names-idn"></a>Utilisation de noms de domaine internationaux (IDN)  
 Le pilote JDBC 6.0 pour SQL Server prend en charge l’utilisation de noms de domaine internationaux (noms de domaines internationaux) et peut convertir un nom de serveur Unicode encodage compatible ASCII (Punycode) à la demande lors d’une connexion.  Si les noms de domaines internationaux sont stockés dans le système DNS (Domain Name System) en tant que chaînes ASCII au format Punycode (spécifié par la RFC 3490), activez la conversion du nom de serveur Unicode en affectant la valeur « True » à la propriété serverNameAsACE.  Sinon, si le service DNS est configuré pour autoriser l’utilisation de caractères Unicode, affectez la valeur « false » (valeur par défaut) à la propriété serverNameAsACE.  Pour les versions antérieures du pilote JDBC, il est également possible de convertir le nom du serveur à l’aide de Punycode [IDN.toASCII de Java](http://docs.oracle.com/javase/8/docs/api/java/net/IDN.html) méthodes avant de définir cette propriété pour une connexion.  
  
> [!NOTE]  
>  La plupart des logiciels de résolution écrits pour les plateformes autres que Windows sont basés sur les normes DSN Internet. Il est donc plus probable qu’ils utilisent le format Punycode pour les noms de domaines internationaux, alors qu’un serveur DNS Windows sur un réseau privé peut être configuré pour autoriser l’utilisation des caractères UTF-8 en fonction du serveur.  Pour plus d’informations, consultez [prise en charge des caractères Unicode](https://technet.microsoft.com/library/cc738403(v=ws.10).aspx).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
