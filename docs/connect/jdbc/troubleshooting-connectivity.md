---
title: Résolution des problèmes de connectivité | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: bfba0b49-2e1f-411d-a625-d25fad9ea12d
caps.latest.revision: 23
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5f7f46aa17ca8fba97c4c17d6efdfee8358448a5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshooting-connectivity"></a>Résolution des problèmes de connectivité
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] nécessite que TCP/IP est installé et en cours d’exécution pour communiquer avec votre [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données. Vous pouvez utiliser la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Configuration Manager pour vérifier les protocoles de bibliothèque réseau sont installés.  
  
 Une tentative de connexion à la base de données peut échouer pour plusieurs raisons. Parmi ces raisons :  
  
-   TCP/IP n’est pas activé pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], ou le numéro de port ou de serveur spécifié est incorrect. Vérifiez que [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] écoute avec TCP/IP sur le serveur spécifié et le port. Cela peut être rapporté avec une exception telle que : « Échec de la connexion. La connexion TCP/IP à l'hôte a échoué. » Cela indique l'une des raisons suivantes :  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est installé mais TCP/IP n’a pas été installé en tant que protocole réseau pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] à l’aide de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] utilitaire réseau pour [!INCLUDE[ssVersion2000](../../includes/ssversion2000_md.md)], ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Gestionnaire de Configuration pour [!INCLUDE[ssVersion2005](../../includes/ssversion2005_md.md)] et versions ultérieures.  
  
    -   TCP/IP est installé comme un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] protocole, mais il n’écoute pas sur le port spécifié dans l’URL de connexion de JDBC. Le port par défaut est 1433, mais [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] peut être configuré à l’installation du produit pour écouter sur n’importe quel port. Assurez-vous que [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est à l’écoute sur le port 1433. Autrement, si le port a été modifié, assurez-vous que celui spécifié dans l'URL de connexion de JDBC correspond au port modifié. Pour plus d’informations sur les URL de connexion de JDBC, consultez [générer l’URL de connexion](../../connect/jdbc/building-the-connection-url.md).  
  
    -   L’adresse de l’ordinateur qui est spécifié dans la connexion de JDBC URL ne fait pas référence à un serveur où [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est installé et démarré.  
  
    -   L’opération de mise en réseau de TCP/IP entre le client et le serveur exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] n’est pas opérationnel. Vous pouvez vérifier la connectivité TCP/IP à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] via telnet. Par exemple, à l’invite de commandes, tapez `telnet 192.168.0.0 1433` où 192.168.0.0 est l’adresse de l’ordinateur qui est en cours d’exécution [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et 1433 est le port qu’il écoute. Si vous recevez un message indiquant « Telnet ne peut pas se connecter », TCP/IP n’écoute pas sur ce port pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] connexions. Utilisez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] utilitaire réseau pour [!INCLUDE[ssVersion2000](../../includes/ssversion2000_md.md)], ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Gestionnaire de Configuration pour [!INCLUDE[ssVersion2005](../../includes/ssversion2005_md.md)] et ultérieur pour vous assurer que [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est configuré pour utiliser TCP/IP sur le port 1433.  
  
    -   Le port utilisé par le serveur n'a pas été ouvert dans le pare-feu. Ceci comprend le port utilisé par le serveur ou éventuellement, le port associé à une instance nommée du serveur.  
  
-   Le nom de base de données spécifié est incorrect. Assurez-vous que vous vous connectez à un fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] base de données.  
  
-   Le nom d'utilisateur ou le mot de passe est incorrect. Assurez-vous que vous disposez des valeurs correctes.  
  
-   Lorsque vous utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’authentification, le pilote JDBC requiert que [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est installé avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] l’authentification, ce qui n’est pas la valeur par défaut. Assurez-vous que cette option est incluse lorsque vous installez ou configurez votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Diagnostic des problèmes avec le pilote JDBC](../../connect/jdbc/diagnosing-problems-with-the-jdbc-driver.md)   
 [Connexion à SQL Server avec le pilote JDBC](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)  
  
  
