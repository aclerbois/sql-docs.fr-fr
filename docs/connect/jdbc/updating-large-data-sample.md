---
title: Mise à jour des exemples de données de grande taille | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 76ecc05f-a77d-40a2-bab9-91a7fcf17347
caps.latest.revision: 27
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8ff0d903d9e4f47580fbce5b4c117a2a339dd22c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updating-large-data-sample"></a>Exemple de mise à jour de données volumineuses
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Cela [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] exemple d’application montre comment mettre à jour une colonne volumineuse dans une base de données.  
  
 Le fichier de code pour cet exemple se nomme updateLargeData.java et se trouve à l'emplacement suivant :  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \samples\adaptive  
  
## <a name="requirements"></a>Spécifications  
 Pour exécuter cet exemple d’application, vous devrez accéder à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple. Vous devez également définir l'instruction classpath de façon à inclure le fichier sqljdbc4.jar. Si l'instruction classpath n'a pas d'entrée pour sqljdbc4.jar, l'exemple d'application lève l'exception usuelle « Classe introuvable ». Pour plus d’informations sur la façon de définir l’instruction classpath, consultez [à l’aide du pilote JDBC](../../connect/jdbc/using-the-jdbc-driver.md).  
  
> [!NOTE]  
>  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit les fichiers de bibliothèque de classes sqljdbc.jar, sqljdbc4.jar, sqljdbc41.jar ou sqljdbc42.jar à utiliser en fonction de vos paramètres Java Runtime Environment (JRE) préférés. Cet exemple utilise le [isWrapperFor](../../connect/jdbc/reference/iswrapperfor-method-sqlserverstatement.md) et [unwrap](../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md) méthodes, qui sont présentés dans l’API JDBC 4.0, d’accéder à la réponse spécifiques au pilote, méthodes de mise en mémoire tampon. Pour pouvoir compiler et exécuter cet exemple, vous devez disposer de la bibliothèque de classes sqljdbc4.jar, qui assure la prise en charge de JDBC 4.0. Pour plus d’informations sur le fichier JAR à choisir, consultez [configuration système requise pour le pilote JDBC](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, l’exemple de code établit une connexion à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données. Ensuite, l’exemple de code crée un objet d’instruction et utilise le [isWrapperFor](../../connect/jdbc/reference/iswrapperfor-method-sqlserverstatement.md) pour vérifier si l’objet d’instruction est un wrapper pour la méthode [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) classe. Le [unwrap](../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md) méthode est utilisée pour accéder à la réponse spécifiques au pilote méthodes de mise en mémoire tampon.  
  
 Ensuite, l’exemple de code définit la réponse mise en mémoire tampon en mode en tant que «**adaptive**» à l’aide de la [setResponseBuffering](../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) méthode de la [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) classe et également montre comment obtenir le mode de mise en mémoire tampon adaptatif.  
  
 Ensuite, il exécute l’instruction SQL et place les données qu’il renvoie dans une mise à jour [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
 Pour finir, l'exemple de code itère son exécution sur les lignes de données contenues dans le jeu de résultats. S’il trouve un document vide résumé, il utilise la combinaison de [updateString](../../connect/jdbc/reference/updatestring-method-sqlserverresultset.md) et [updateRow](../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md) méthodes pour mettre à jour la ligne de données et la remettre à la base de données. S’il existe déjà des données, il utilise le [getString](../../connect/jdbc/reference/getstring-method-sqlserverresultset.md) méthode pour afficher des données qu’il contient.  
  
 Le comportement par défaut du pilote est «**adaptive.**» Toutefois, pour les jeux de résultats d’être mise à jour avant uniquement et lorsque les données dans le jeu de résultats sont supérieure à la mémoire de l’application, l’application doit définir le mode de mise en mémoire tampon adaptative explicitement à l’aide de la [setResponseBuffering](../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) méthode de la [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) classe.  
  
 [!code[JDBC#UsingAdaptiveBuffering3](../../connect/jdbc/codesnippet/Java/updating-large-data-sample_1.java)]  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation de données volumineuses](../../connect/jdbc/working-with-large-data.md)  
  
  
