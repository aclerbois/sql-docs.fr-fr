---
title: Programmation avec SQLXML | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 4d2cc57c-7293-4d92-b8b1-525e2b35f591
caps.latest.revision: 23
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e5cb6d2d44b9988dda28d5e9bd10db0d96467ff6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="programming-with-sqlxml"></a>Programmation à l'aide de SQLXML
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Cette section décrit comment utiliser le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] méthodes API pour stocker et récupérer un document XML de document dans et à partir d’une base de données relationnelle avec **SQLXML** objets.  
  
 Cette section contient également des informations sur les types d'objets SQLXML ; par ailleurs, elle dresse une liste des recommandations et limitations importantes relatives à l'utilisation des objets SQLXML.  
  
## <a name="reading-and-writing-xml-data-with-sqlxml-objects"></a>Lecture et écriture de données XML à l'aide d'objets SQLXML  
 La liste suivante décrit comment utiliser le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] méthodes API pour lire et écrire des données XML à l’aide d’objets SQLXML :  
  
-   Pour créer un objet SQLXML, utilisez le [createSQLXML](../../connect/jdbc/reference/createsqlxml-method-sqlserverconnection.md) méthode de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe. Notez que cette méthode crée un objet SQLXML sans aucune donnée. Pour ajouter **xml** données à un objet SQLXML, appelez l’une des méthodes suivantes qui sont spécifiés dans l’interface SQLXML : setResult, setCharacterStream, setBinaryStream, ou setString.  
  
-   Pour récupérer l’objet SQLXML proprement dit, utilisez les méthodes de getSQLXML de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) classe ou la [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classe.  
  
-   Pour récupérer le **xml** données d’un objet SQLXML, utilisez une des méthodes suivantes qui sont spécifiés dans l’interface SQLXML : getSource, getCharacterStream, getBinaryStream, ou getString.  
  
-   Pour mettre à jour le **xml** des données dans un objet SQLXML, utilisez le [updateSQLXML](../../connect/jdbc/reference/updatesqlxml-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) classe.  
  
-   Pour stocker un objet SQLXML dans une colonne de table de base de données de type **xml**, utilisez les méthodes setSQLXML de la [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) classe ou la [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md)classe.  
  
 L’exemple de code dans [exemple de Type de données SQLXML](../../connect/jdbc/sqlxml-data-type-sample.md) montre comment effectuer ces tâches d’API courantes.  
  
## <a name="readable-and-writable-sqlxml-objects"></a>Objets SQLXML accessibles en lecture et en écriture  
 Le tableau suivant répertorie les types d'objets SQLXML pris en charge par les méthodes setter, getter et updater fournies par l'API JDBC. Les colonnes du tableau font référence aux éléments suivants :  
  
-   Le **nom de la méthode** colonne répertorie les méthodes getter, setter et updater prises en charge dans l’API JDBC.  
  
-   Le **objet SQLXML Getter** colonne représente un objet SQLXML, qui est créé par le le [getSQLXML](../../connect/jdbc/reference/getsqlxml-method-sqlservercallablestatement.md) méthode de la [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classe ou le [getSQLXML](../../connect/jdbc/reference/getsqlxml-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) classe.  
  
-   Le **objet SQLXML Setter** colonne représente un objet SQLXML, qui est créé par le [createSQLXML](../../connect/jdbc/reference/createsqlxml-method-sqlserverconnection.md) méthode de la [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) classe. Notez que les méthodes setter ci-dessous acceptent uniquement un objet SQLXML créé par le [createSQLXML](../../connect/jdbc/reference/createsqlxml-method-sqlserverconnection.md) (méthode).  
  
|Nom de la méthode|Objet SQLXML getter<br /><br /> (Accessible en lecture)|Objet SQLXML setter<br /><br /> (Accessible en écriture)|  
|-----------------|-------------------------------------------|-------------------------------------------|  
|CallableStatement.setSQLXML()|Non pris en charge|Pris en charge|  
|CallableStatement.setObject()|Non pris en charge|Pris en charge|  
|PreparedStatement.setSQLXML()|Non pris en charge|Pris en charge|  
|PreparedStatement.setObject()|Non pris en charge|Pris en charge|  
|ResultSet.updateSQLXML()|Non pris en charge|Pris en charge|  
|ResultSet.updateObject()|Non pris en charge|Pris en charge|  
|ResultSet.getSQLXML()|Pris en charge|Non pris en charge|  
|CallableStatement.getSQLXML()|Pris en charge|Non pris en charge|  
  
 Comme indiqué dans le tableau ci-dessus, les méthodes SQLXML setter ne fonctionnent pas avec les objets SQLXML accessibles en lecture ; de même, les méthodes getter ne fonctionnent pas avec les objets SQLXML accessibles en écriture.  
  
 Si l’application appelle la méthode setObject en spécifiant une échelle ou un paramètre de longueur avec un objet SQLXML, le paramètre de mise à l’échelle ou la longueur est ignoré.  
  
## <a name="guidelines-and-limitations-when-using-sqlxml-objects"></a>Recommandations et limitations relatives à l'utilisation des objets SQLXML  
 Les applications peuvent utiliser des objets SQLXML pour lire et écrire les données XML dans la base de données. La liste suivante fournit des informations sur les limitations et recommandations spécifiques à l'utilisation d'objets SQLXML :  
  
-   Un objet SQLXML ne peut être valide que pendant la durée de la transaction au cours de laquelle il a été créé.  
  
-   Un objet SQLXML reçu à partir d'une méthode getter ne peut être utilisé que pour lire des données.  
  
-   Un objet SQLXML créé par l'objet de connexion ne peut être utilisé que pour écrire des données.  
  
-   L'application ne peut appeler qu'une seule méthode getter sur un objet SQLXML accessible en lecture pour lire des données. Une fois la méthode getter appelée, toutes les autres méthodes getter ou setter sur le même objet SQLXML échouent.  
  
-   L’application peut appeler uniquement la méthode free sur l’objet SQLXML une fois qu’il est lu ou écrit dans. Toutefois, il est toujours possible de traiter la source ou le flux retourné tant que la colonne ou le paramètre sous-jacent est actif. Si la colonne ou le paramètre sous-jacent devient inactif, la source ou le flux associé à l'objet SQLXML est fermé. Si la colonne ou le paramètre sous-jacent n'est plus valide, les données sous-jacentes ne sont pas disponibles pour les méthodes getter du flux, de SAX (Simple API for XML) et de StAX (Streaming API for XML).  
  
-   L'application ne peut appeler qu'une seule méthode setter sur un objet SQLXML accessible en écriture. Une fois la méthode setter appelée, toutes les autres méthodes setter ou getter sur le même objet SQLXML échouent.  
  
-   Pour définir des données sur l'objet SQLXML, l'application doit utiliser la méthode setter appropriée ainsi que les fonctions correspondantes dans l'objet retourné.  
  
-   Les méthodes getSQLXML de la [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classe et le [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) classe retourne **null** données si la colonne sous-jacente est **null**.  
  
-   Les objets setter peuvent être valides via la connexion dans laquelle ils ont été créés.  
  
-   Les applications ne sont pas autorisées pour définir un **null** à l’aide de méthodes setter fournies par l’interface SQLXML. Les applications peuvent définir une chaîne vide ("") à l'aide des méthodes setter fournies dans l'interface SQLXML. Pour définir un **null** valeur, les applications doivent appeler l’une des opérations suivantes :  
  
    -   Les méthodes setNull de la [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classe et [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) classe.  
  
    -   Les méthodes setObject de la [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classe et [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) classe.  
  
    -   Les méthodes setSQLXML de la [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classe et [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) classe avec un **null** la valeur du paramètre.  
  
-   Lors de l'utilisation de documents XML, il est recommandé de privilégier les analyseurs SAX (Simple API for XML) et StAX (Streaming API for XML) aux analyseurs DOM (Document Object Model) pour des raisons de performances.  
  
 Les analyseurs XML ne peuvent pas gérer les valeurs vides. Toutefois, SQL Server permet aux applications de récupérer et de stocker des valeurs vides dans les colonnes de base de données de type de données XML. En d'autres termes, lors de l'analyse des données XML, si la valeur sous-jacente est vide, une exception est levée par l'analyseur. Pour les sorties DOM, le pilote JDBC intercepte cette exception et génère une erreur. Pour les sorties SAX et Stax, l'erreur provient directement de l'analyseur.  
  
## <a name="adaptive-buffering-and-sqlxml-support"></a>Prise en charge de la mise en mémoire tampon adaptative et de SQLXML  
 Les flux de données aux formats binaire et caractère retournés par l'objet SQLXML obéissent aux modes de mise en mémoire tampon adaptative ou de mise en mémoire tampon complète. Par ailleurs, si les analyseurs XML ne sont pas des flux, ils n'obéissent pas aux paramètres de mise en mémoire tampon adaptative ou de mise en mémoire tampon complète. Pour plus d’informations sur la mise en mémoire tampon adaptative, consultez [à l’aide de mise en mémoire tampon adaptative](../../connect/jdbc/using-adaptive-buffering.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en charge des données XML](../../connect/jdbc/supporting-xml-data.md)  
  
  
