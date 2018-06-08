---
title: Lors de la lecture des données de grande taille avec stockées procédures exemple | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 58c76635-a117-4661-8781-d6cb231c5809
caps.latest.revision: 27
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 00387faec988fde6bdfa310ed96723f2bc226bfa
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reading-large-data-with-stored-procedures-sample"></a>Exemple de lecture de données volumineuses avec des procédures stockées
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Cela [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] exemple d’application montre comment récupérer un paramètre OUT volumineux à partir d’une procédure stockée.  
  
 Le fichier de code de cet exemple se nomme executeStoredProcedure.java et se trouve à l'emplacement suivant :  
  
 \<*répertoire d’installation*> \sqljdbc_\<*version*>\\<*langage*> \samples\adaptive  
  
## <a name="requirements"></a>Spécifications  
 Pour exécuter cet exemple d’application, vous devrez accéder à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple. Vous devez également définir l'instruction classpath de façon à inclure le fichier sqljdbc.jar ou sqljdbc4.jar. Si l'instruction classpath n'a pas d'entrée pour sqljdbc.jar ou sqljdbc4.jar, l'exemple d'application lève l'exception usuelle « Classe introuvable ». Pour plus d’informations sur la façon de définir l’instruction classpath, consultez [à l’aide du pilote JDBC](../../connect/jdbc/using-the-jdbc-driver.md).  
  
> [!NOTE]  
>  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit des fichiers de bibliothèques de classes à utiliser en fonction de vos paramètres Java Runtime Environment (JRE) préférés sqljdbc.jar et sqljdbc4.jar. Pour plus d’informations sur le fichier JAR à choisir, consultez [configuration système requise pour le pilote JDBC](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
 Vous devez également créer la procédure stockée suivante dans le [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données exemple :  
  
```  
CREATE PROCEDURE GetLargeDataValue   
  (@Document_ID int,   
   @Document_ID_out int OUTPUT,   
   @Document_Title varchar(50) OUTPUT,  
   @Document_Summary nvarchar(max) OUTPUT)  
  
AS   
BEGIN    
   SELECT @Document_ID_out = DocumentID,   
          @Document_Title = Title,  
          @Document_Summary = DocumentSummary   
    FROM  Production.Document  
    WHERE DocumentID = @Document_ID  
END  
```  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, l’exemple de code établit une connexion à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] base de données. Il crée ensuite des exemples de données et met à jour la table Production.Document en utilisant une requête paramétrable. Ensuite, l’exemple de code obtient le mode de mise en mémoire tampon adaptative à l’aide de la [getResponseBuffering](../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md) méthode de la [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) classe et exécute la procédure stockée GetLargeDataValue. Notez qu'à partir de la version 2.0 du pilote JDBC, la propriété de connexion responseBuffering a la valeur « adaptive » par défaut.  
  
 Enfin, l’exemple de code affiche les données retournées avec les paramètres OUT et montre également comment utiliser le `mark` et `reset` méthodes sur le flux pour relire toute partie des données.  
  
 [!code[JDBC#UsingAdaptiveBuffering2](../../connect/jdbc/codesnippet/Java/reading-large-data-with-_1_1.java)]  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation de données volumineuses](../../connect/jdbc/working-with-large-data.md)  
  
  
