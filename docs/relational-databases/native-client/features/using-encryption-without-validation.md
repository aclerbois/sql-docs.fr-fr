---
title: Utilisation du chiffrement sans Validation | Documents Microsoft
ms.custom: ''
ms.date: 12/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: native-client|features
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- data access [SQL Server Native Client], encryption
- cryptography [SQL Server Native Client]
- SQLNCLI, encryption
- encryption [SQL Server Native Client]
- SQL Server Native Client, encryption
ms.assetid: f4c63206-80bb-4d31-84ae-ccfcd563effa
caps.latest.revision: 18
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 8ae3205cf4a530b958582f9efdd4644930950a76
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-encryption-without-validation"></a>Utilisation du chiffrement sans validation
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] chiffre toujours les paquets réseau associés à l’ouverture de session. Si aucun certificat n'a été fourni sur le serveur à son démarrage, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] génère un certificat auto-signé qui est utilisé pour chiffrer les paquets d’ouverture de session.  

Les certificats auto-signés ne garantissent pas la sécurité. Le protocole de transfert chiffré est basé sur NT LAN Manager (NTLM). Il est fortement recommandé que vous configuriez un certificat vérifiable sur SQL Server pour la connectivité sécurisée. Sécurité TLS (Transport Layer) peuvent être apportées sécurisé uniquement avec la validation de certificat.

Les applications peuvent également demander le chiffrement de tout le trafic réseau en utilisant des mots clés de chaîne de connexion ou des propriétés de connexion. Les mots clés sont « Encrypt » pour ODBC et OLE DB lors de l’utilisation d’une chaîne de fournisseur avec **IDbInitialize::Initialize**, ou « Use Encryption for Data » pour ADO et OLE DB lors de l’utilisation d’une chaîne d’initialisation avec **IDataInitialize**. Cela peut également être configuré par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l’aide du Gestionnaire de Configuration le **forcer le chiffrement du protocole** option et en configurant le client pour demander des connexions chiffrées. Par défaut, le chiffrement de tout le trafic réseau pour une connexion requiert la fourniture d'un certificat sur le serveur. En définissant votre client faire confiance au certificat sur le serveur, vous risquez d’être vulnérable aux attaques de man-in-the-middle. Si vous déployez un certificat vérifiable sur le serveur, assurez-vous de modifier les paramètres du client sur le certificat de confiance à la valeur FALSE.

Pour plus d’informations sur les mots clés de chaîne de connexion, consultez [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 Pour activer le chiffrement à utiliser lorsqu’aucun certificat n’a pas été fourni sur le serveur, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager peut être utilisée pour définir les deux le **forcer le chiffrement du protocole** et **faire confiance au certificat serveur** options. Dans ce cas, le chiffrement utilise un certificat de serveur auto-signé sans validation si aucun certificat vérifiable n'a été fourni sur le serveur.  
  
 Les applications peuvent également utiliser le mot clé « TrustServerCertificat » ou son attribut de connexion associé pour garantir que le chiffrement est réalisé. Les paramètres de l'application ne réduisent jamais le niveau de sécurité défini par le Gestionnaire de configuration du client [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], mais peuvent le renforcer. Par exemple, si **forcer le chiffrement du protocole** n’est pas définie pour le client, une application peut demander elle-même le chiffrement. Pour garantir le chiffrement même si aucun certificat de serveur n'a été fourni, une application peut demander le chiffrement et « TrustServerCertificate ». Toutefois, si« TrustServerCertificate » n'est pas activé dans la configuration client, un certificat de serveur fourni est toujours requis. Le tableau ci-dessous décrit l'ensembles des scénarios :  
  
|Paramètre client Forcer le chiffrement du protocole|Paramètre client Faire confiance au certificat de serveur|Chaîne de connexion/attribut de connexion Encrypt/Use Encryption for Data|Chaîne de connexion/attribut de connexion Trust Server Certificate|Résultat|  
|----------------------------------------------|---------------------------------------------|------------------------------------------------------------------------------|----------------------------------------------------------------------|------------|  
|non|Néant|Non (par défaut)|Ignoré|Aucun chiffrement ne se produit.|  
|non|Néant|Oui|Non (par défaut)|Le chiffrement se produit uniquement s'il existe un certificat de serveur vérifiable ; sinon, la tentative de connexion échoue.|  
|non|Néant|Oui|Oui|Le chiffrement se produit toujours, mais peut utiliser un certificat de serveur auto-signé.|  
|Oui|non|Ignoré|Ignoré|Le chiffrement se produit uniquement s'il existe un certificat de serveur vérifiable ; sinon, la tentative de connexion échoue.|  
|Oui|Oui|Non (par défaut)|Ignoré|Le chiffrement se produit toujours, mais peut utiliser un certificat de serveur auto-signé.|  
|Oui|Oui|Oui|Non (par défaut)|Le chiffrement se produit uniquement s'il existe un certificat de serveur vérifiable ; sinon, la tentative de connexion échoue.|  
|Oui|Oui|Oui|Oui|Le chiffrement se produit toujours, mais peut utiliser un certificat de serveur auto-signé.|  
||||||

> [!CAUTION]
> Le tableau précédent fournit uniquement un guide sur le comportement du système sous différentes configurations. Pour la connectivité sécurisée, assurez-vous que le client et serveur exigent le chiffrement. Vérifiez également que le serveur dispose d’un certificat vérifiable et que le **TrustServerCertificate** sur le client est défini sur FALSE.

## <a name="sql-server-native-client-ole-db-provider"></a>Fournisseur OLE DB SQL Server Native Client  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Client fournisseur OLE DB natif prend en charge le chiffrement sans validation via l’ajout de la propriété de l’initialisation SSPROP_INIT_TRUST_SERVER_CERTIFICATE données source qui est implémentée dans le jeu de propriétés DBPROPSET_SQLSERVERDBINIT. De plus, un nouveau mot clé de chaîne de connexion, « TrustServerCertificate» , a été ajouté. Il accepte les valeurs Oui ou non ; non est la valeur par défaut. Lors de l'utilisation de composants du service, il accepte les valeurs true ou false ; false étant la valeur par défaut.  
  
 Pour plus d’informations sur les améliorations apportées au jeu de propriétés DBPROPSET_SQLSERVERDBINIT, consultez [propriétés d’initialisation et d’autorisation](../../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md).  
  
## <a name="sql-server-native-client-odbc-driver"></a>Pilote ODBC SQL Server Native Client  
 Le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pilote ODBC Native Client prend en charge le chiffrement sans validation via des ajouts à la [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) et [SQLGetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) fonctions. SQL_COPT_SS_TRUST_SERVER_CERTIFICATE a été ajouté pour accepter SQL_TRUST_SERVER_CERTIFICATE_YES ou SQL_TRUST_SERVER_CERTIFICATE_NO ; SQL_TRUST_SERVER_CERTIFICATE_NO étant la valeur par défaut. De plus, un nouveau mot clé de chaîne de connexion, « TrustServerCertificate », a été ajouté. Il accepte les valeurs « yes » ou « no » ; «no » étant la valeur par défaut.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités de SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
  
  
