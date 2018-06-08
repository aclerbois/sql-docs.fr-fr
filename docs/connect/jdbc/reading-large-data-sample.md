---
title: Lors de la lecture des données volumineuses exemple | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 6c986144-3854-4352-8331-e79eccbefc28
caps.latest.revision: 28
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0037ee678766bad6786a6feb14e0bf569f9448a0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reading-large-data-sample"></a>Exemple de lecture de données volumineuses
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Cela [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] exemple d’application montre comment récupérer une grande valeur de colonne unique à partir d’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données à l’aide de la [getCharacterStream](../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md) (méthode).  
  
 Le fichier de code pour cet exemple se nomme readLargeData.java et se trouve à l'emplacement suivant :  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \samples\adaptive  
  
## <a name="requirements"></a>Spécifications  
 Pour exécuter cet exemple d’application, vous devrez accéder à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple. Vous devez également définir l'instruction classpath de façon à inclure le fichier sqljdbc.jar ou sqljdbc4.jar. Si l'instruction classpath n'a pas d'entrée pour sqljdbc.jar ou sqljdbc4.jar, l'exemple d'application lève l'exception usuelle « Classe introuvable ». Pour plus d’informations sur la façon de définir l’instruction classpath, consultez [à l’aide du pilote JDBC](../../connect/jdbc/using-the-jdbc-driver.md).  
  
> [!NOTE]  
>  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit des fichiers de bibliothèques de classes à utiliser en fonction de vos paramètres Java Runtime Environment (JRE) préférés sqljdbc.jar et sqljdbc4.jar. Pour plus d’informations sur le fichier JAR à choisir, consultez [configuration système requise pour le pilote JDBC](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, l’exemple de code établit une connexion à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données. Il crée ensuite des exemples de données et met à jour la table Production.Document en utilisant une requête paramétrable.  
  
 En outre, l’exemple de code montre comment obtenir le mode de mise en mémoire tampon adaptatif à l’aide de la [getResponseBuffering](../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md) méthode de la [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) classe. Notez qu'à partir de la version 2.0 du pilote JDBC, la propriété de connexion responseBuffering a la valeur « adaptive » par défaut.  
  
 Puis, à l’aide d’une instruction SQL avec la [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) de l’objet, l’exemple de code exécute l’instruction SQL et place les données qu’il renvoie dans un [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) objet.  
  
 Enfin, l’exemple de code effectue une itération dans les lignes de données qui sont contenues dans le jeu de résultats et utilise le [getCharacterStream](../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md) méthode pour accéder à certaines des données qu’il contient.  
  
 [!code[JDBC#UsingAdaptiveBuffering1](../../connect/jdbc/codesnippet/Java/reading-large-data-sample_1.java)]  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation de données volumineuses](../../connect/jdbc/working-with-large-data.md)  
  
  
