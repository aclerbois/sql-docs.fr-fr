---
title: Conversions de types de données de présentation | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 98fa7488-aac3-45b4-8aa4-83ed6ab638b4
caps.latest.revision: 34
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e87e223cb64b9bb60008da17841f69bb67dfed10
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-data-type-conversions"></a>Conversions de types de données
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Pour faciliter la conversion de types de données de langage de programmation Java [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] assure les conversions de type de données requis par la spécification JDBC. Pour une flexibilité accrue, tous les types sont convertibles vers et depuis **objet**, **chaîne**, et **byte []** des types de données.  
  
## <a name="getter-method-conversions"></a>Conversions de méthode d'accesseur Get  
 Selon le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données, le graphique suivant contient le mappage des conversions du pilote JDBC pour la méthode get\<Type > () des méthodes de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) classe, ainsi que les conversions prises en charge pour la méthode get\< Type > méthodes de la [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classe.  
  
 ![JDBCGetterConversions](../../connect/jdbc/media/jdbcgetterconversions.gif "JDBCGetterConversions")  
  
 Trois catégories de conversions sont prises en charge par les méthodes d'accesseur Get du pilote JDBC :  
  
-   **Sans perte (x)**: Conversions pour les cas où le type getter est identique ou plus petit que le type de serveur sous-jacent. Par exemple, lors de l’appel getBigDecimal sur une colonne décimale de serveur sous-jacente, aucune conversion n’est nécessaire.  
  
-   **Converti (y)**: les Conversions de types de serveurs numériques en types de langage Java où la conversion est régulière et suit les règles de conversion de langage Java. Pour ces conversions, la précision est toujours tronquée (jamais arrondie) et les dépassements sont gérés comme un opérateur modulo du type de destination, qui est plus petit. Par exemple, l’appel getInt sur un sous-jacent **décimal** colonne qui contient « 1,9999 » retournera « 1 », ou si sous-jacent **décimal** valeur est « 3000000000 », le **int** valeur engendre un dépassement à « -1294967296 ».  
  
-   **Dépendant des données (z)**: les Conversions de types de caractère sous-jacents en types numériques requièrent que les types de caractères contiennent des valeurs qui peuvent être convertis en ce type. Aucune autre conversion n'a lieu. Si la valeur est trop élevée pour le type getter, elle n'est pas valide. Par exemple, si getInt est appelée sur une colonne varchar (50) contenant « 53 », la valeur est retournée comme un **int**; Toutefois, si la valeur sous-jacente est « xyz » ou « 3000000000 », une erreur est générée.  
  
 Si getString est appelée sur une **binaire**, **varbinary**, **varbinary (max)**, ou **image** de type de données de colonne, la valeur est retournée comme un valeur de chaîne hexadécimale.  
  
## <a name="updater-method-conversions"></a>Conversions de méthodes Updater  
 Pour les passé à la mise à jour des données typées Java\<Type > () des méthodes de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) (classe), les conversions suivantes s’appliquent.  
  
 ![JDBCUpdaterConversions](../../connect/jdbc/media/jdbc_jdbcupdatterconversions.gif "JDBCUpdaterConversions")  
  
 Trois catégories de conversions sont prises en charge par les méthodes updater du pilote JDBC :  
  
-   **Sans perte (x)**: Conversions pour les cas où le type updater est identique ou plus petit que le type de serveur sous-jacent. Par exemple, lors de l'appel de updateBigDecimal sur une colonne décimale de serveur sous-jacente, aucune conversion n'est nécessaire.  
  
-   **Converti (y)**: les Conversions de types de serveurs numériques en types de langage Java où la conversion est régulière et suit les règles de conversion de langage Java. Pour ces conversions, la précision est toujours tronquée (jamais arrondie) et le dépassement de capacité est géré en tant que modulo du type de destination (type le plus petit). Par exemple, l’appel updateDecimal sur un sous-jacent **int** colonne qui contient « 1,9999 » retournera « 1 », ou si sous-jacent **décimal** valeur est « 3000000000 », le **int**valeur engendre un dépassement à « -1294967296 ».  
  
-   **Dépendant des données (z)**: les Conversions de types de données sources sous-jacents en types de données de destination requièrent que les valeurs contenues peuvent être converties en types de destination. Aucune autre conversion n'a lieu. Si la valeur est trop élevée pour le type getter, elle n'est pas valide. Par exemple, si updateString est appelé sur une colonne int qui contient « 53 », la mise à jour réussit ; toutefois, si la valeur String sous-jacente est « foo » ou « 3000000000 », une erreur est générée.  
  
 Lorsque updateString est appelé sur un **binaire**, **varbinary**, **varbinary (max)**, ou **image** de type de données de colonne, il gère la valeur de chaîne en tant que valeur de chaîne hexadécimale.  
  
 Lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est de type de données de colonne **XML**, la valeur de données doit être valide **XML**. Lorsque vous appelez les méthodes updateBlob, updateBinaryStream ou updateBytes, la valeur de données doit être la représentation sous forme de chaîne hexadécimale des caractères XML. Par exemple :  
  
```  
<hello>world</hello> = 0x3C68656C6C6F3E776F726C643C2F68656C6C6F3E   
```  
  
 Notez qu'une marque d'ordre d'octet (BOM, Byte-Order Mark) est obligatoire si les caractères XML correspondent à des encodages de caractères spécifiques.  
  
## <a name="setter-method-conversions"></a>Conversions de méthodes setter  
 Pour les passé à l’ensemble des données typées Java\<Type > () des méthodes de la [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) classe et le [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) (classe), les conversions suivantes s’appliquent.  
  
 ![JDBCSetterConversions](../../connect/jdbc/media/jdbc_jdbcsetterconversions_v2.gif "JDBCSetterConversions")  
  
 Le serveur tente toutes les conversions et retourne des erreurs en cas d'échec.  
  
 Dans le cas de la **chaîne** type de données, si la valeur dépasse la longueur de **VARCHAR**, il est mappé à **LONGVARCHAR**. De même, **NVARCHAR** est mappé à **LONGNVARCHAR** si la valeur dépasse la longueur prise en charge de **NVARCHAR**. Est de même pour **byte []**. Valeurs de plus de **VARBINARY** deviennent **LONGVARBINARY**.  
  
 Deux catégories de conversions sont prises en charge par les méthodes setter du pilote JDBC :  
  
-   **Sans perte (x)**: Conversions pour les cas où le type setter est identique ou plus petit que le type de serveur sous-jacent. Par exemple, lors de l’appel setBigDecimal sur un serveur sous-jacent **décimal** colonne, aucune conversion n’est nécessaire. Pour les conversions de caractères, le Java de numérique à **numérique** type de données est converti en un **chaîne**. Par exemple, l’appel setDouble avec la valeur « 53 » sur une colonne varchar (50) produit une valeur de caractère « 53 » dans cette colonne de destination.  
  
-   **Converti (y)**: les Conversions de Java **numérique** type à un serveur sous-jacent **numérique** type qui est plus petit. Cette conversion est régulière et suit [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] conventions de conversion. La précision est toujours tronquée (jamais arrondie) et tout dépassement de capacité génère une erreur de conversion non prise en charge. Par exemple, à l’aide d’updateDecimal avec la valeur « 1,9999 » sur une sous-jacent entier colonne entraîne un « 1 » dans la colonne de destination ; mais si la valeur est « 3000000000 » est passé, le pilote génère une erreur.  
  
-   **Dépendant des données (z)**: les Conversions de Java **chaîne** type sous-jacent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données dépend des conditions suivantes : le pilote envoie la **chaîne** valeur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] effectue des conversions, si nécessaire. Si le sendStringParametersAsUnicode a la valeur true et sous-jacent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est de type de données **image**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] n’autorise pas la conversion **nvarchar** à **image** et lève une exception SQLServerException. Si le sendStringParametersAsUnicode a la valeur false et sous-jacent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est de type de données **image**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] permet la conversion **varchar** à **image**et ne lève pas d’exception.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] effectue les conversions et retourne les erreurs au pilote JDBC lorsqu’il existe des problèmes.  
  
 Lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est de type de données de colonne **XML**, la valeur de données doit être valide **XML**. Lorsque vous appelez les méthodes updateBlob, updateBinaryStream ou updateBytes, la valeur de données doit être la représentation sous forme de chaîne hexadécimale des caractères XML. Par exemple :  
  
```  
<hello>world</hello> = 0x3C68656C6C6F3E776F726C643C2F68656C6C6F3E   
```  
  
 Notez qu'une marque d'ordre d'octet (BOM, Byte-Order Mark) est obligatoire si les caractères XML correspondent à des encodages de caractères spécifiques.  
  
## <a name="conversions-on-setobject"></a>Conversions sur setObject  
  
> [!NOTE]  
>  Pilotes Microsoft JDBC 4.2 (et versions ultérieures) pour SQL Server prend en charge JDBC 4.1 et 4.2. Pour plus d’informations sur les versions 4.1 et 4.2 les mappages de type de données et les conversions, consultez [conformité JDBC 4.1 pour le pilote JDBC](../../connect/jdbc/jdbc-4-1-compliance-for-the-jdbc-driver.md) et [mise en conformité de JDBC 4.2 pour le pilote JDBC](../../connect/jdbc/jdbc-4-2-compliance-for-the-jdbc-driver.md), outre les informations ci-dessous.  
  
 Pour les données passées à la setObject typées Java (\<Type >) les méthodes de la [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) (classe), les conversions suivantes s’appliquent.  
  
 ![JDBCSetObjectConversions](../../connect/jdbc/media/jdbc_jdbcsetobjectconversions.gif "JDBCSetObjectConversions")  
  
 La méthode setObject sans type cible spécifié utilise le mappage par défaut. Dans le cas de la **chaîne** type de données, si la valeur dépasse la longueur de **VARCHAR**, il est mappé à **LONGVARCHAR**. De même, **NVARCHAR** est mappé à **LONGNVARCHAR** si la valeur dépasse la longueur prise en charge de **NVARCHAR**. Est de même pour **byte []**. Valeurs de plus de **VARBINARY** deviennent **LONGVARBINARY**.  
  
 Trois catégories de conversions sont prises en charge par les méthodes setObject du pilote JDBC :  
  
-   **Sans perte (x)**: Conversions pour les cas où le type setter est identique ou plus petit que le type de serveur sous-jacent. Par exemple, lors de l’appel setBigDecimal sur un serveur sous-jacent **décimal** colonne, aucune conversion n’est nécessaire. Pour les conversions de caractères, le Java de numérique à **numérique** type de données est converti en un **chaîne**. Par exemple, l’appel setDouble avec la valeur « 53 » sur une colonne varchar (50) génère une valeur de caractère « 53 » dans cette colonne de destination.  
  
-   **Converti (y)**: les Conversions de Java **numérique** type à un serveur sous-jacent **numérique** type qui est plus petit. Cette conversion est régulière et suit [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] conventions de conversion. La précision est toujours tronquée (jamais arrondie) et les dépassements génèrent une erreur de conversion non prise en charge. Par exemple, à l’aide d’updateDecimal avec la valeur « 1,9999 » sur une sous-jacent entier colonne entraîne un « 1 » dans la colonne de destination ; mais si la valeur est « 3000000000 » est passé, le pilote génère une erreur.  
  
-   **Dépendant des données (z)**: les Conversions de Java **chaîne** type sous-jacent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données dépend des conditions suivantes : le pilote envoie la **chaîne** valeur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] effectue des conversions, si nécessaire. Si la propriété de connexion sendStringParametersAsUnicode a la valeur true et sous-jacent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est de type de données **image**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] n’autorise pas la conversion **nvarchar** à **image** et lève une exception SQLServerException. Si le sendStringParametersAsUnicode a la valeur false et sous-jacent [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est de type de données **image**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] permet la conversion **varchar** à **image**et ne lève pas d’exception.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] effectue l’essentiel des conversions de jeu et retourne les erreurs au pilote JDBC lorsqu’il existe des problèmes. Conversions côté client constituent l’exception et sont effectuées uniquement dans le cas de **date**, **temps**, **timestamp**, **booléenne**et  **Chaîne** valeurs.  
  
 Lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est de type de données de colonne **XML**, la valeur de données doit être valide **XML**. Lors de l'appel des méthodes setObject(byte[], SQLXML), setObject(inputStream, SQLXML) ou setObject(Blob, SQLXML), la valeur de données doit être la représentation sous forme de chaîne hexadécimale des caractères XML. Par exemple :  
  
```  
<hello>world</hello> = 0x3C68656C6C6F3E776F726C643C2F68656C6C6F3E   
```  
  
 Notez qu'une marque d'ordre d'octet (BOM, Byte-Order Mark) est obligatoire si les caractères XML correspondent à des encodages de caractères spécifiques.  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation des types de données du pilote JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md)  
  
  
