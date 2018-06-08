---
title: Exemple de Type de données SQLXML | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 8f2ff25b-71fd-46d7-b6de-d656095d2aad
caps.latest.revision: 21
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: dfec22f18dd07a9961f7747ad82f2baea9f3ce8e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlxml-data-type-sample"></a>Exemple de type de données SQLXML
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Cela [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] exemple d’application montre comment stocker des données XML dans une base de données relationnelle, comment récupérer des données XML à partir d’une base de données et comment analyser les données XML avec le **SQLXML** type de données Java.  
  
 Les exemples de code de cette section utilisent un analyseur SAX (Simple API for XML). La norme SAX est une norme développée de manière publique pour l'analyse de documents XML en fonction des événements. Elle fournit également une interface de programmation d'applications pour l'utilisation des données XML. Notez que les applications peuvent également utiliser d'autres analyseurs XML, par exemple DOM (Document Object Model), StAX (Streaming API for XML), etc.  
  
 DOM (Document Object Model) fournit une représentation par programme des documents, fragments, nœuds ou éléments node-set XML. Elle fournit également une interface de programmation d'applications pour l'utilisation des données XML. De même, StAX (Streaming API for XML) est une API Java pour l'analyse par extraction du code XML.  
  
> [!IMPORTANT]  
>  Pour pouvoir utiliser l'API de l'analyseur SAX, vous devez importer l'implémentation SAX standard à partir du package javax.xml.  
  
 Le fichier de code pour cet exemple se nomme sqlxmlExample.java et se trouve à l'emplacement suivant :  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \samples\datatypes  
  
## <a name="requirements"></a>Spécifications  
 Pour exécuter cet exemple d'application, vous devez définir l'instruction classpath de façon à inclure le fichier sqljdbc4.jar. Si l'instruction classpath n'a pas d'entrée pour sqljdbc4.jar, l'exemple d'application lève l'exception « Classe introuvable ». Pour plus d’informations sur la façon de définir l’instruction classpath, consultez [à l’aide du pilote JDBC](../../connect/jdbc/using-the-jdbc-driver.md).  
  
 En outre, vous devez accéder à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple pour exécuter cet exemple d’application.  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, l’exemple de code établit une connexion à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] de base de données, puis appelle la méthode createSampleTables.  
  
 La méthode createSampleTables supprime les tables de test, TestTable1 et TestTable2, si elles existent déjà. Elle insère ensuite deux lignes dans TestTable1.  
  
 En outre, l'exemple de code ajoute les trois méthodes décrites ci-après et une classe supplémentaire dont le nom est ExampleContentHandler.  
  
 La classe ExampleContentHandler implémente un gestionnaire de contenu personnalisé qui définit les méthodes pour les événements de l'analyseur.  
  
 La méthode showGetters montre comment analyser les données de l'objet SQLXML à l'aide de l'analyseur SAX, de ContentHandler et de XMLReader. En premier lieu, l'exemple de code crée une instance d'un gestionnaire de contenu personnalisé dont le nom est ExampleContentHandler. Il crée et exécute ensuite une instruction SQL qui retourne un jeu de données à partir de TestTable1. L'exemple de code obtient ensuite un analyseur SAX pour analyser les données XML.  
  
 La méthode showSetters montre comment définir la **xml** colonne à l’aide de SAX, de ContentHandler et de jeu de résultats. Tout d’abord, il crée un objet SQLXML vide à l’aide de la [createSQLXML](../../connect/jdbc/reference/createsqlxml-method-sqlserverconnection.md) méthode de la classe de connexion. Elle obtient ensuite une instance d'un gestionnaire de contenu pour écrire les données dans l'objet SQLXML. L'exemple de code écrit ensuite les données dans TestTable1. Enfin, l’exemple de code effectue une itération dans les lignes de données qui sont contenues dans le jeu de résultats et utilise le [getSQLXML](../../connect/jdbc/reference/getsqlxml-method-sqlserverresultset.md) méthode pour lire les données XML.  
  
 La méthode showTransformer montre comment obtenir des données XML à partir d'une table pour les insérer dans une autre table à l'aide de l'analyseur SAX et du transformateur. En premier lieu, elle récupère l'objet SQLXML source à partir de TestTable1. Ensuite, il crée un objet SQLXML de destination vide à l’aide de la [createSQLXML](../../connect/jdbc/reference/createsqlxml-method-sqlserverconnection.md) méthode de la classe de connexion. Elle met ensuite à jour l'objet SQLXML de destination et écrit les données XML dans TestTable2. Enfin, l’exemple de code effectue une itération dans les lignes de données qui sont contenues dans le jeu de résultats et utilise le [getSQLXML](../../connect/jdbc/reference/getsqlxml-method-sqlserverresultset.md) méthode pour lire les données XML dans TestTable2.  
  
 [!code[JDBC#UsingSQLXML1](../../connect/jdbc/codesnippet/Java/sqlxml-data-type-sample_1.java)]  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation des Types de données &#40;JDBC&#41;](../../connect/jdbc/working-with-data-types-jdbc.md)  
  
  
