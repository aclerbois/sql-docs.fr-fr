---
title: Présentation de prise en charge SSL | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 073f3b9e-8edd-4815-88ea-de0655d0325e
caps.latest.revision: 28
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 089ec1a4a16f9a0568bda9aa584948fd4704ae5f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="understanding-ssl-support"></a>Fonctionnement de la prise en charge SSL
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Lorsque vous vous connectez à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], si l’application demande un chiffrement et l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est configuré pour prendre en charge le chiffrement SSL, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] lance la négociation SSL. L'établissement d'une liaison permet au serveur et au client de négocier le chiffrement et les algorithmes de chiffrement à utiliser pour protéger les données. Une fois la négociation SSL terminée, le client et le serveur peuvent envoyer les données chiffrées de manière sécurisée. Au cours de la négociation SSL, le serveur envoie son certificat de clé publique au client. L'émetteur d'un certificat de clé publique porte le nom d'Autorité de certification. Le client est chargé de vérifier que l'autorité de certification est approuvée par le client.  
  
 Si l’application ne demande pas de chiffrement, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ne forcera pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] pour prendre en charge le chiffrement SSL. Si le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] instance n’est pas configurée pour forcer le chiffrement SSL, une connexion est établie sans chiffrement. Si le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] instance est configurée pour forcer le chiffrement SSL, le pilote active automatiquement le chiffrement SSL lors de l’exécution sur correctement Java Virtual Machine (JVM) configurée, ou sinon la connexion est interrompue et le pilote génère une erreur.  
  
> [!NOTE]  
>  Assurez-vous que la valeur passée à **nom_serveur** correspond exactement au nom DNS ou le nom commun (CN) dans l’autre nom de l’objet dans le certificat de serveur pour une connexion SSL réussisse.  
  
> [!NOTE]  
>  Pour plus d’informations sur la façon de configurer SSL pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], consultez le chiffrement des connexions à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] rubrique dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
## <a name="remarks"></a>Notes  
 Pour permettre aux applications d’utiliser le chiffrement SSL, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] a introduit les propriétés de connexion suivantes, en commençant par la version 1.2 : **chiffrer**, **trustServerCertificate**, **trustStore**, **trustStorePassword**, et **hostNameInCertificate**. Pour plus d’informations, consultez [définissant les propriétés de connexion](../../connect/jdbc/setting-the-connection-properties.md).  
  
 Le tableau suivant résume comment la [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] se comporte de la version pour les scénarios de connexion SSL possibles. Chaque scénario utilise un ensemble différent de propriétés de connexion SSL. Le tableau inclut :  
  
-   **vide**: « la propriété n’existe pas dans la chaîne de connexion »  
  
-   **valeur**: « la propriété existe dans la chaîne de connexion et sa valeur est valide »  
  
-   **n’importe quel**: « Peu importe si la propriété existe dans la chaîne de connexion ou si sa valeur est valide »  
  
> [!NOTE]  
>  Le même comportement s’applique pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] utilisateur et l’authentification Windows intégrée.  
  
|encrypt|trustServerCertificate|hostNameInCertificate|trustStore|trustStorePassword|Comportement|  
|-------------|----------------------------|---------------------------|----------------|------------------------|--------------|  
|false ou blank|n'importe laquelle|n'importe laquelle|n'importe laquelle|n'importe laquelle|Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ne forcera pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] pour prendre en charge le chiffrement SSL. Si le serveur a un certificat auto-signé, le pilote initie l'échange de certificat SSL. Le certificat SSL ne sera pas validé et seules les informations d'identification (dans le paquet de connexion) sont chiffrées.<br /><br /> Si le serveur requiert que le client prenne en charge le chiffrement SSL, le pilote initiera l'échange de certificat SSL. Le certificat SSL ne sera pas validé, mais la communication entière sera chiffrée.|  
|true|true|n'importe laquelle|n'importe laquelle|n'importe laquelle|Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] demande d’utiliser le chiffrement SSL avec le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].<br /><br /> Si le serveur requiert que le client prenne en charge le chiffrement SSL ou si le serveur prend en charge le chiffrement, le pilote initiera l'échange de certificat SSL. Notez que si le **trustServerCertificate** est définie sur « true », le pilote ne validera pas le certificat SSL.<br /><br /> Si le serveur n'est pas configuré pour prendre en charge le chiffrement, le pilote déclenchera une erreur et interrompra la connexion.|  
|true|false ou blank|Vide|Vide|Vide|Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] demande d’utiliser le chiffrement SSL avec le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].<br /><br /> Si le serveur requiert que le client prenne en charge le chiffrement SSL ou si le serveur prend en charge le chiffrement, le pilote initiera l'échange de certificat SSL.<br /><br /> Le pilote utilisera la **nom_serveur** propriété spécifiée sur l’URL de connexion pour valider le certificat SSL du serveur et s’appuient sur les règles de recherche de la fabrique de gestionnaire de confiance pour déterminer le magasin de certificats à utiliser.<br /><br /> Si le serveur n'est pas configuré pour prendre en charge le chiffrement, le pilote déclenchera une erreur et interrompra la connexion.|  
|true|false ou blank|valeur|Vide|Vide|Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] demande d’utiliser le chiffrement SSL avec le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].<br /><br /> Si le serveur requiert que le client prenne en charge le chiffrement SSL ou si le serveur prend en charge le chiffrement, le pilote initiera l'échange de certificat SSL.<br /><br /> Le pilote validera la valeur de l’objet du certificat SSL à l’aide de la valeur spécifiée pour le **hostNameInCertificate** propriété.<br /><br /> Si le serveur n'est pas configuré pour prendre en charge le chiffrement, le pilote déclenchera une erreur et interrompra la connexion.|  
|true|false ou blank|Vide|valeur|valeur|Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] demande d’utiliser le chiffrement SSL avec le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].<br /><br /> Si le serveur requiert que le client prenne en charge le chiffrement SSL ou si le serveur prend en charge le chiffrement, le pilote initiera l'échange de certificat SSL.<br /><br /> Le pilote utilisera la **trustStore** valeur de propriété à rechercher le fichier trustStore de certificat et **trustStorePassword** valeur de propriété pour vérifier l’intégrité du fichier trustStore.<br /><br /> Si le serveur n'est pas configuré pour prendre en charge le chiffrement, le pilote déclenchera une erreur et interrompra la connexion.|  
|true|false ou blank|Vide|Vide|valeur|Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] demande d’utiliser le chiffrement SSL avec le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].<br /><br /> Si le serveur requiert que le client prenne en charge le chiffrement SSL ou si le serveur prend en charge le chiffrement, le pilote initiera l'échange de certificat SSL.<br /><br /> Le pilote utilisera la **trustStorePassword** valeur de propriété pour vérifier l’intégrité du fichier trustStore par défaut.<br /><br /> Si le serveur n'est pas configuré pour prendre en charge le chiffrement, le pilote déclenchera une erreur et interrompra la connexion.|  
|true|false ou blank|Vide|valeur|Vide|Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] demande d’utiliser le chiffrement SSL avec le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].<br /><br /> Si le serveur requiert que le client prenne en charge le chiffrement SSL ou si le serveur prend en charge le chiffrement, le pilote initiera l'échange de certificat SSL.<br /><br /> Le pilote utilisera la **trustStore** valeur de propriété pour rechercher l’emplacement du fichier trustStore.<br /><br /> Si le serveur n'est pas configuré pour prendre en charge le chiffrement, le pilote déclenchera une erreur et interrompra la connexion.|  
|true|false ou blank|valeur|Vide|valeur|Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] demande d’utiliser le chiffrement SSL avec le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].<br /><br /> Si le serveur requiert que le client prenne en charge le chiffrement SSL ou si le serveur prend en charge le chiffrement, le pilote initiera l'échange de certificat SSL.<br /><br /> Le pilote utilisera la **trustStorePassword** valeur de propriété pour vérifier l’intégrité du fichier trustStore par défaut. En outre, le pilote utilisera la **hostNameInCertificate** valeur de propriété à valider le certificat SSL.<br /><br /> Si le serveur n'est pas configuré pour prendre en charge le chiffrement, le pilote déclenchera une erreur et interrompra la connexion.|  
|true|false ou blank|valeur|valeur|Vide|Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] demande d’utiliser le chiffrement SSL avec le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].<br /><br /> Si le serveur requiert que le client prenne en charge le chiffrement SSL ou si le serveur prend en charge le chiffrement, le pilote initiera l'échange de certificat SSL.<br /><br /> Le pilote utilisera la **trustStore** valeur de propriété pour rechercher l’emplacement du fichier trustStore. En outre, le pilote utilisera la **hostNameInCertificate** valeur de propriété à valider le certificat SSL.<br /><br /> Si le serveur n'est pas configuré pour prendre en charge le chiffrement, le pilote déclenchera une erreur et interrompra la connexion.|  
|true|false ou blank|valeur|valeur|valeur|Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] demande d’utiliser le chiffrement SSL avec le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].<br /><br /> Si le serveur requiert que le client prenne en charge le chiffrement SSL ou si le serveur prend en charge le chiffrement, le pilote initiera l'échange de certificat SSL.<br /><br /> Le pilote utilisera la **trustStore** valeur de propriété à rechercher le fichier trustStore de certificat et **trustStorePassword** valeur de propriété pour vérifier l’intégrité du fichier trustStore. En outre, le pilote utilisera la **hostNameInCertificate** valeur de propriété à valider le certificat SSL.<br /><br /> Si le serveur n'est pas configuré pour prendre en charge le chiffrement, le pilote déclenchera une erreur et interrompra la connexion.|  
  
 Si la propriété encrypt a la valeur **true**, le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] utilise le fournisseur de sécurité JSSE par défaut de la machine virtuelle Java pour négocier le chiffrement SSL avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Le fournisseur de sécurité par défaut peut ne pas prendre en charge toutes les fonctionnalités requises pour négocier le chiffrement SSL avec succès. Par exemple, le fournisseur de sécurité par défaut peut prend pas en charge la taille de la clé publique RSA utilisée dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] certificat SSL. Dans ce cas, le fournisseur de sécurité par défaut peut générer une erreur qui forcera le pilote JDBC à mettre fin à la connexion. Pour résoudre ce problème, effectuez l'une des opérations suivantes :  
  
-   Configurer le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] avec un certificat de serveur qui a une clé publique RSA inférieure  
  
-   Configurer la machine virtuelle Java pour utiliser un autre fournisseur de sécurité JSSE dans le «\<java-home > / lib/security/java.security « fichier de propriétés de sécurité  
  
-   Utiliser une autre machine virtuelle Java  
  
## <a name="validating-server-ssl-certificate"></a>Validation du certificat SSL de serveur  
 Au cours de la négociation SSL, le serveur envoie son certificat de clé publique au client. Le client ou pilote JDBC doit confirmer que le certificat de serveur est publié par une autorité de certification approuvée par le client. Le pilote requiert que le certificat de serveur réponde aux conditions suivantes :  
  
-   Le certificat a été publié par une autorité de certification approuvée.  
  
-   Le certificat doit être publié pour l'authentification de serveur.  
  
-   Le certificat n'a pas expiré.  
  
-   Le nom commun (CN) dans l’objet ou un nom DNS dans l’autre nom de l’objet du certificat correspond exactement à la **nom_serveur** valeur spécifiée dans la chaîne de connexion ou, si spécifié, le  **hostNameInCertificate** valeur de propriété.  
  
-   Un nom DNS peut comprendre des caractères génériques. Mais le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ne prend pas en charge la correspondance générique. Autrement dit, abc.com ne correspondra pas à *.com mais \*.com correspond à \*. com.  
  
## <a name="see-also"></a>Voir aussi  
 [À l’aide du chiffrement SSL](../../connect/jdbc/using-ssl-encryption.md)   
 [Sécurisation des applications de pilote JDBC](../../connect/jdbc/securing-jdbc-driver-applications.md)  
  
  
