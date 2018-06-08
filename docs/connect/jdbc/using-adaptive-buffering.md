---
title: À l’aide de la mise en mémoire tampon adaptative | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 92d4e3be-c3e9-4732-9a60-b57f4d0f7cb7
caps.latest.revision: 53
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ae4120b567d876556c29254eae65d4471ab63924
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-adaptive-buffering"></a>Utilisation de la mise en mémoire tampon adaptative
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  La mise en mémoire tampon adaptative est conçue pour récupérer tout type de données de valeur importante sans la charge mémoire associée aux curseurs côté serveur. Applications peuvent utiliser la fonctionnalité de mise en mémoire tampon adaptative avec toutes les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] qui sont pris en charge par le pilote.  
  
 Normalement, lorsque le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] exécute une requête, il extrait tous les résultats à partir du serveur dans la mémoire de l’application. Bien que cette approche réduit la consommation des ressources sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], elle peut lever un OutOfMemoryError dans l’application JDBC pour les requêtes qui produisent des résultats très volumineux.  
  
 Pour permettre aux applications de gérer des résultats très volumineux, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] fournit la mise en mémoire tampon adaptative. Avec la mise en mémoire tampon adaptative, le pilote récupère les résultats de l’exécution d’instructions à partir de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] que l’application en a besoin, plutôt que simultanément. Le pilote ignore également les résultats dès que l'application ne peut plus y accéder. Voici quelques exemples de situations où la mise en mémoire tampon adaptative peut être utile :  
  
-   **La requête génère un jeu de résultats très volumineux :** l’application peut exécuter une instruction SELECT qui produit plus de lignes que l’application peut stocker en mémoire. Dans les versions précédentes, l’application devait utiliser un curseur côté serveur afin d’éviter une OutOfMemoryError. La mise en mémoire tampon adaptative permet d'effectuer un passage en lecture seule avant uniquement d'un jeu de résultats arbitrairement volumineux sans nécessiter de curseur côté serveur.  
  
-   **La requête produit très volumineux**[SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md)**colonnes ou**[SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md)**les valeurs de paramètre :** L’application peut récupérer une valeur unique (colonne ou paramètre OUT) trop grande pour tenir entièrement dans la mémoire de l’application. Mise en mémoire tampon adaptative permet à l’application cliente récupérer une telle valeur en tant que flux, à l’aide de la getAsciiStream, la getBinaryStream ou les méthodes getCharacterStream. L’application récupère la valeur de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] lorsqu’il lit à partir du flux.  
  
> [!NOTE]  
>  Avec la mise en mémoire tampon adaptative, le pilote JDBC ne met en mémoire tampon que la quantité de données requise. Le pilote ne fournit aucune méthode publique pour contrôler ou limiter la taille de la mémoire tampon.  
  
## <a name="setting-adaptive-buffering"></a>Définition de la mise en mémoire tampon adaptative  
 En commençant par le pilote JDBC version 2.0, le comportement par défaut du pilote est «**adaptive**». En d'autres termes, pour pouvoir obtenir le comportement de mise en mémoire tampon adaptative, votre application n'a pas besoin de requérir explicitement le mode de mise en mémoire tampon adaptative. Dans la version 1.2, toutefois, le mode de mise en mémoire tampon était «**complète**» par défaut et l’application devait requérir explicitement le mode de mise en mémoire tampon adaptative.  
  
 Il existe trois manières pour une application de demander que l'exécution d'instructions utilise la mise en mémoire tampon adaptative :  
  
-   L’application peut définir la propriété de connexion **responseBuffering** « Adaptive ». Pour plus d’informations sur les propriétés de connexion, consultez [définissant les propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md).  
  
-   L’application peut utiliser le [setResponseBuffering](../../connect/jdbc/reference/setresponsebuffering-method-sqlserverdatasource.md) méthode de la [SQLServerDataSource](../../connect/jdbc/reference/sqlserverdatasource-class.md) objet pour définir la réponse mise en mémoire tampon de mode pour toutes les connexions créées par le biais qui [ SQLServerDataSource](../../connect/jdbc/reference/sqlserverdatasource-class.md) objet.  
  
-   L’application peut utiliser le [setResponseBuffering](../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) méthode de la [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) classe pour définir la réponse mise en mémoire tampon de mode pour un objet statement particulier.  
  
 Lorsque vous utilisez le pilote JDBC version 1.2, les applications nécessaires pour effectuer un cast de l’objet statement vers une [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) classe à utiliser le [setResponseBuffering](../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) (méthode). Les exemples de code dans le [lors de la lecture des échantillons de données importants](../../connect/jdbc/reading-large-data-sample.md) et [lors de la lecture des données volumineuses avec des exemples de procédures stockées](../../connect/jdbc/reading-large-data-with-stored-procedures-sample.md) illustrent cet ancien mode d’utilisation.  
  
 Toutefois, avec le pilote JDBC version 2.0, les applications peuvent utiliser le [isWrapperFor](../../connect/jdbc/reference/iswrapperfor-method-sqlserverstatement.md) (méthode) et le [unwrap](../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md) méthode pour accéder aux fonctionnalités spécifiques aux fournisseurs sans hypothèse sur la hiérarchie de classe d’implémentation. Par exemple de code, consultez la [mise à jour des exemples de données volumineux](../../connect/jdbc/updating-large-data-sample.md) rubrique.  
  
## <a name="retrieving-large-data-with-adaptive-buffering"></a>Récupération de données volumineuses avec la mise en mémoire tampon adaptative  
 Lorsque des valeurs élevées sont lues une fois à l’aide de la méthode get\<Type > méthodes de flux de données et les colonnes du jeu de résultats et les paramètres OUT CallableStatement sont accessibles dans l’ordre retourné par le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], la mise en mémoire tampon adaptative réduit l’application utilisation de la mémoire lors du traitement des résultats. Lors de l'utilisation de la mise en mémoire tampon adaptative :  
  
-   La méthode get\<Type > diffuser des méthodes définies dans le [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) et [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classes retournent des flux de lecture unique par défaut, bien que les flux de données peut être réinitialisée si marqué par l’application. Si l’application souhaite `reset` le flux, il doit appeler la `mark` tout d’abord de méthode sur ce flux.  
  
-   La méthode get\<Type > Stream des méthodes définies dans le [SQLServerClob](../../connect/jdbc/reference/sqlserverclob-class.md) et [SQLServerBlob](../../connect/jdbc/reference/sqlserverblob-class.md) classes retournent des flux qui peuvent toujours être repositionnés à la position de départ du flux de données sans appel de la `mark` (méthode).  
  
 Lorsque l’application utilise la mise en mémoire tampon adaptative, les valeurs extraites par la méthode get\<Type > méthodes de flux de données peuvent être extraites une seule fois. Si vous essayez d’appeler n’importe quel get\<Type > méthode sur la même colonne ou le paramètre après avoir appelé la méthode get\<Type > méthode de flux de données du même objet, une exception est levée avec le message, « les données a été utilisées et ne sont pas disponibles pour ce colonne ou paramètre ».  
  
> [!NOTE]
> Un appel à ResultSet.close() pendant le traitement d’un jeu de résultats nécessite le pilote JDBC Microsoft pour SQL Server lire et ignorer tous les paquets restants. Cette opération peut prendre de prendre du temps si la requête a retourné un jeu de données volumineux et plus particulièrement la connexion réseau est lente.     
  
## <a name="guidelines-for-using-adaptive-buffering"></a>Consignes pour l'utilisation de la mise en mémoire tampon adaptative  
 Les développeurs doivent respecter les recommandations importantes suivantes afin de réduire l'utilisation de la mémoire par l'application :  
  
-   Évitez d’utiliser la propriété de chaîne de connexion **selectMethod = cursor** pour autoriser l’application de traiter un très grand résultat défini. La fonctionnalité de mise en mémoire tampon adaptative permet aux applications de traiter de très grands jeux de résultats avant uniquement en lecture seule sans utiliser de curseur côté serveur. Notez que lorsque vous définissez **selectMethod = cursor**tout avant uniquement, les jeux de résultats en lecture seule produits par cette connexion sont affectés. En d’autres termes, si votre application traite régulièrement des jeux de résultats de petite avec peu de lignes, créer, lire et fermeture d’un curseur de serveur pour chaque jeu de résultats utilisera plus de ressources sur le côté client et côté serveur que dans le cas où la  **selectMethod** n’est pas définie **curseur**.  
  
-   Lire le texte de grande taille ou des valeurs binaires sous forme de flux en utilisant le getAsciiStream, le getBinaryStream ou les méthodes getCharacterStream au lieu du getBlob ou les méthodes getClob. En commençant par la version 1.2, le [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) classe fournit les nouvelles get\<Type > Stream méthodes à cet effet.  
  
-   Assurez-vous que les colonnes avec des valeurs potentiellement grandes sont placées en dernier dans la liste des colonnes dans une instruction SELECT et que get\<Type > diffuser des méthodes de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) sont utilisés pour accéder aux colonnes dans la ordre de que leur sélection.  
  
-   Assurez-vous que les paramètres avec des valeurs potentiellement grandes sont déclarés en dernier dans la liste des paramètres dans le SQL utilisé pour créer le [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md). En outre, vérifiez que la méthode get\<Type > Stream des méthodes de la [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) permettent d’accéder aux paramètres OUT dans l’ordre qu’ils sont déclarés.  
  
-   Évitez d'exécuter simultanément plusieurs instructions sur la même connexion. L'exécution d'une autre instruction avant le traitement des résultats de l'instruction précédente peut provoquer la mise en mémoire tampon des résultats non traités dans la mémoire d'application.  
  
-   Il existe certains cas où l’utilisation **selectMethod = cursor** au lieu de **responseBuffering = adaptive** serait plus utile, telles que :  
  
    -   Si votre application traite un curseur avant uniquement, en lecture seule jeu de résultats lentement, par exemple la lecture de chaque ligne après une entrée utilisateur, à l’aide de **selectMethod = cursor** au lieu de **responseBuffering = adaptive** peut aider à réduire l’utilisation des ressources par [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
    -   Si votre application traite les deux jeux de résultats plus avant uniquement et en lecture seule en même temps sur la même connexion, à l’aide **selectMethod = cursor** au lieu de **responseBuffering = adaptive** peut vous aider réduire la mémoire requise par le pilote lors du traitement de ces jeux de résultats.  
  
     Dans les deux cas, vous devez tenir compte de la surcharge liée à la création, la lecture et la fermeture des curseurs côté serveur.  
  
 En outre, la liste suivante fournit quelques recommandations pour les jeux de résultats de type avant uniquement pouvant être mis à jour et déroulables :  
  
-   Pour les jeux de résultats déroulables, lors de l’extraction d’un bloc de lignes le pilote toujours lit en mémoire le nombre de lignes indiqué par le [getFetchSize](../../connect/jdbc/reference/getfetchsize-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) de l’objet, même lorsque le mise en mémoire tampon adaptative est activée. Si le défilement provoque une OutOfMemoryError, vous pouvez réduire le nombre de lignes extraites en appelant le [setFetchSize](../../connect/jdbc/reference/setfetchsize-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) pour définir la taille d’extraction à un plus petit nombre de lignes, objet voire une seule 1 ligne, si nécessaire. Si cela n’empêche pas un OutOfMemoryError, évitez d’inclure des colonnes très volumineuses dans les jeux de résultats déroulables.  
  
-   Pour les jeux de résultats actualisable avant uniquement, lors de l’extraction d’un bloc de lignes le pilote normalement lit en mémoire le nombre de lignes indiqué par le [getFetchSize](../../connect/jdbc/reference/getfetchsize-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) objet, même lorsque la mise en mémoire tampon adaptative est activée sur la connexion. Si l’appel de la [suivant](../../connect/jdbc/reference/next-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) les résultats des objets dans une OutOfMemoryError, vous pouvez réduire le nombre de lignes extraites en appelant le [setFetchSize](../../connect/jdbc/reference/setfetchsize-method-sqlserverresultset.md) méthode de la [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) objet pour définir la taille d’extraction à un plus petit nombre de lignes, voire une seule 1 ligne, si nécessaire. Vous pouvez également forcer le pilote à ne mettre en mémoire tampon toutes les lignes en appelant le [setResponseBuffering](../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) méthode de la [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) avec l’objet «**adaptive**« paramètre avant l’exécution de l’instruction. Étant donné que le jeu de résultats n’est pas déroulable, si l’application accède à une valeur de colonne importante à l’aide de la méthode get\<Type > méthodes de flux de données, le pilote ignore la valeur dès que l’application la lit comme il le fait pour le curseur avant uniquement en lecture seule jeux de résultats.  
  
## <a name="see-also"></a>Voir aussi  
 [Amélioration des performances et de la fiabilité avec le pilote JDBC](../../connect/jdbc/improving-performance-and-reliability-with-the-jdbc-driver.md)  
  
  
