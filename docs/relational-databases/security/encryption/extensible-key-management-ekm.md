---
title: Gestion de clés extensible (EKM) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Key Management
- Extensible Key Management
- EKM, described
ms.assetid: 9bfaf500-2d1e-4c02-b041-b8761a9e695b
caps.latest.revision: 46
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: e00a78719febd9a452da5cd85220f2fb091ffa9b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="extensible-key-management-ekm"></a>Gestion de clés extensible (EKM)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fournit des fonctions de chiffrement de données en même temps que la *gestion de clés extensible* (EKM, Extensible Key Management) à l’aide du fournisseur de *l’API Microsoft Cryptography* (MSCAPI) pour le chiffrement et la génération de clés. Les clés de chiffrement pour les données et le chiffrement à clé sont créés dans des conteneurs de clé transitoires, et ils doivent être exportés d'un fournisseur avant d'être stockés dans la base de données. Cette approche permet à la gestion des clés, qui comprend une hiérarchie de clé de chiffrement et une sauvegarde de clé, d'être gérée par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Face à la demande croissante de conformité aux normes et aux problèmes liés à la confidentialité des données, les organisations font appel au chiffrement afin d'apporter une solution de « défense en profondeur ». Cette approche est souvent peu pratique car elle utilise uniquement des outils de gestion de chiffrement de base de données. Les fabricants de matériel fournissent des produits qui prennent en charge la gestion des clés dans l’entreprise à l’aide des *modules de la sécurité matériels* . Les périphériques HSM stockent des clés de chiffrement dans les modules matériels ou logiciels. Il s'agit d'une solution plus sécurisée parce que les clés de chiffrement ne résident pas avec les données de chiffrement.  
  
 Plusieurs fournisseurs offrent HSM pour la gestion des clés et l'accélération du chiffrement. Les périphériques HSM utilisent des interfaces matérielles avec un processus serveur comme intermédiaire entre une application et un HSM. Les fournisseurs implémentent également des fournisseurs MSCAPI sur leurs modules qui peuvent être matériels ou logiciels. MSCAPI offre souvent seulement un sous-ensemble des fonctionnalités offertes par un HSM. Les fournisseurs peuvent également fournir le logiciel de gestion pour HSM, la configuration de clé et l'accès aux clés.  
  
 Les implémentations de modules de sécurité matériels varient selon les fournisseurs, et leur utilisation avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nécessite une interface commune. Si MSCAPI fournit cette interface, elle prend en charge uniquement un sous-ensemble des fonctionnalités HSM. Elle connaît aussi d'autres limitations, telles que l'incapacité à rendre persistantes en mode natif des clés symétriques et une absence de prise en charge orientée session.  
  
 La gestion de clés extensible [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permet aux fournisseurs tiers EKM/HSM d’enregistrer leurs modules dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Une fois enregistrés, les utilisateurs de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peuvent utiliser les clés de chiffrement stockées dans les modules EKM. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peut ainsi accéder aux fonctionnalités de chiffrement avancées offertes par ces modules, telles que le chiffrement et le déchiffrement en bloc, et les fonctions de gestion de clés, telles que le vieillissement de clé et la permutation de clé.  
  
 Quand vous exécutez [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans une machine virtuelle Azure, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peut utiliser des clés stockées dans [Azure Key Vault](http://go.microsoft.com/fwlink/?LinkId=521401). Pour plus d’informations, consultez [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  
  
## <a name="ekm-configuration"></a>Configuration EKM  
 La gestion de clés extensible n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 Par défaut, la gestion de clés extensible est désactivée. Pour activer cette fonctionnalité, utilisez la commande sp_configure avec l'option et la valeur suivantes, comme dans l'exemple ci-après :  
  
```  
sp_configure 'show advanced', 1  
GO  
RECONFIGURE  
GO  
sp_configure 'EKM provider enabled', 1  
GO  
RECONFIGURE  
GO  
```  
  
> [!NOTE]  
>  Si vous utilisez la commande sp_configure pour cette option sur les éditions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui ne prennent pas en charge la gestion de clés extensible, vous recevrez un message d’erreur.  
  
 Pour désactiver la fonctionnalité, affectez-lui la valeur **0**. Pour plus d’informations sur la définition des options de serveur, consultez [sp_configure &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
## <a name="how-to-use-ekm"></a>Comment utiliser EKM  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] La gestion de clés extensible active les clés de chiffrement qui protègent les fichiers de base de données à stocker dans un périphérique off-box tel qu’une carte à puce, un périphérique USB ou un module EKM/HSM. Elle permet aussi la protection des données pour les administrateurs de base de données (sauf les membres du groupe sysadmin). Les données peuvent être chiffrées à l'aide des clés de chiffrement auxquelles seul l'utilisateur de base de données peut accéder sur le module EKM/HSM externe.  
  
 La gestion de clés extensible offre aussi les avantages suivants :  
  
-   Contrôle d'autorisation supplémentaire (activant la séparation des tâches).  
  
-   Performances supérieures pour le chiffrement/déchiffrement basé sur le matériel.  
  
-   Génération de clé de chiffrement externe.  
  
-   Stockage de clé de chiffrement externe (séparation physique des données et des clés).  
  
-   Récupération de clés de chiffrement.  
  
-   Rétention de clé de chiffrement externe (permet la rotation de clé de chiffrement).  
  
-   Récupération simplifiée de clé de chiffrement.  
  
-   Distribution gérable de clé de chiffrement.  
  
-   Suppression de clé de chiffrement sécurisée.  
  
 Vous pouvez utiliser la gestion de clés extensible pour une combinaison de nom d’utilisateur et de mot de passe ou d’autres méthodes définies par le pilote EKM.  
  
> [!CAUTION]  
>  Pour le dépannage, le support technique [!INCLUDE[msCoName](../../../includes/msconame-md.md)] peut requérir la clé de chiffrement du fournisseur EKM. Vous devrez peut-être aussi accéder aux outils ou aux processus du fournisseur pour aider à résoudre un problème.  
  
### <a name="authentication-with-an-ekm-device"></a>Authentification avec un périphérique EKM  
 Un module EKM peut prendre en charge plusieurs types d'authentification. Chaque fournisseur n’expose qu’un seul type d’authentification à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Autrement dit, si le module prend en charge l’authentification de base et l’authentification Windows, il expose l’une ou l’autre, mais pas les deux.  
  
#### <a name="ekm-device-specific-basic-authentication-using-usernamepassword"></a>Authentification de base spécifique au périphérique EKM à l'aide du nom d'utilisateur/mot de passe  
 Pour les modules EKM qui prennent en charge l’authentification de base à l’aide d’une paire *nom d’utilisateur/mot de passe*, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fournit l’authentification transparente à l’aide des informations d’identification. Pour plus d’informations sur les informations d’identification, consultez [Informations d’identification &#40;moteur de base de données&#41;](../../../relational-databases/security/authentication-access/credentials-database-engine.md).  
  
 Une information d’authentification peut être créée pour un fournisseur EKM et mappée à une connexion (comptes Windows et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]) pour accéder à un module EKM sur la base d’une connexion individuelle. Le champ *Identifier* des informations d’identification contient le nom d’utilisateur. Le champ *Secret* contient un mot de passe pour la connexion à un module EKM.  
  
 En l’absence d’une information d’identification mappée à une connexion pour le fournisseur EKM, l’information d’authentification mappée au compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est utilisée.  
  
 Une connexion peut avoir plusieurs informations d'identification mappées à elle, à condition qu'elles soient utilisées pour des fournisseurs EKM distinctifs. Il ne doit y avoir qu'une seule information d'authentification mappée par fournisseur EKM par connexion. La même information d'identification peut être mappée à d'autres connexions.  
  
#### <a name="other-types-of-ekm-device-specific-authentication"></a>Autres types d'authentification spécifique au périphérique EKM  
 Pour les modules EKM qui ont une authentification autre que Windows ou des combinaisons *utilisateur/mot de passe* , l’authentification doit être effectuée indépendamment de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
### <a name="encryption-and-decryption-by-an-ekm-device"></a>Chiffrement et déchiffrement par un périphérique EKM  
 Vous pouvez utiliser les fonctions et fonctionnalités suivantes pour chiffrer et déchiffrer des données à l'aide des clés symétriques et asymétriques :  
  
|Fonction ou fonctionnalité|Référence|  
|-------------------------|---------------|  
|Chiffrement à clé symétrique|[CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)|  
|Chiffrement à clé asymétrique|[CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)|  
|EncryptByKey(key_guid, 'cleartext', …)|[ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbykey-transact-sql.md)|  
|DecryptByKey(ciphertext, …)|[DECRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/decryptbykey-transact-sql.md)|  
|EncryptByAsmKey(key_guid, 'cleartext')|[ENCRYPTBYASYMKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbyasymkey-transact-sql.md)|  
|DecryptByAsmKey(ciphertext)|[DECRYPTBYASYMKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/decryptbyasymkey-transact-sql.md)|  
  
#### <a name="database-keys-encryption-by-ekm-keys"></a>Chiffrement de clés de base de données par les clés EKM  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peut utiliser des clés EKM pour chiffrer d’autres clés dans une base de données. Vous pouvez créer et utiliser à la fois des clés symétriques et asymétriques sur un périphérique EKM. Vous pouvez chiffrer des clés symétriques natives (non-EKM) avec des clés asymétriques EKM.  
  
 L'exemple suivant crée une clé symétrique de base de données et la chiffre à l'aide d'une clé sur un module EKM.  
  
```  
CREATE SYMMETRIC KEY Key1  
WITH ALGORITHM = AES_256  
ENCRYPTION BY EKM_AKey1;  
GO  
--Open database key  
OPEN SYMMETRIC KEY Key1  
DECRYPTION BY EKM_AKey1  
```  
  
 Pour plus d’informations sur les clés de serveur et de base de données dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [SQL Server et clés de chiffrement de base de données &#40;moteur de base de données&#41;](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md).  
  
> [!NOTE]  
>  Vous ne pouvez pas chiffrer une clé EKM avec une autre clé EKM.  
>   
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne prend pas en charge la signature des modules avec les clés asymétriques générées par un fournisseur EKM.  
  
## <a name="related-tasks"></a>Related Tasks  
 [Fournisseur EKM activé (option de configuration de serveur)](../../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md)  
  
 [Activer le chiffrement transparent des données à l’aide de la gestion de clés extensible (EKM)](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
  
 [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-cryptographic-provider-transact-sql.md)   
 [DROP CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-cryptographic-provider-transact-sql.md)   
 [ALTER CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-cryptographic-provider-transact-sql.md)   
 [sys.cryptographic_providers &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-cryptographic-providers-transact-sql.md)   
 [sys.dm_cryptographic_provider_sessions &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-cryptographic-provider-sessions-transact-sql.md)   
 [sys.dm_cryptographic_provider_properties &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-cryptographic-provider-properties-transact-sql.md)   
 [sys.dm_cryptographic_provider_algorithms &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-cryptographic-provider-algorithms-transact-sql.md)   
 [sys.dm_cryptographic_provider_keys &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-cryptographic-provider-keys-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-login-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-asymmetric-key-transact-sql.md)   
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-asymmetric-key-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [OPEN SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/open-symmetric-key-transact-sql.md)   
 [Sauvegarder et restaurer les clés de chiffrement Reporting Services](../../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)   
 [Supprimer et recréer des clés de chiffrement &#40;Gestionnaire de configuration de SSRS&#41;](../../../reporting-services/install-windows/ssrs-encryption-keys-delete-and-re-create-encryption-keys.md)   
 [Ajouter et supprimer des clés de chiffrement pour un déploiement évolutif &#40;Gestionnaire de configuration de SSRS&#41;](../../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md)   
 [Sauvegarder la clé principale du service](../../../relational-databases/security/encryption/back-up-the-service-master-key.md)   
 [Restaurer la clé principale du service](../../../relational-databases/security/encryption/restore-the-service-master-key.md)   
 [Créer une clé principale de base de données](../../../relational-databases/security/encryption/create-a-database-master-key.md)   
 [Sauvegarder une clé primaire de base de données](../../../relational-databases/security/encryption/back-up-a-database-master-key.md)   
 [Restaurer une clé principale de base de données](../../../relational-databases/security/encryption/restore-a-database-master-key.md)   
 [Créer des clés symétriques identiques sur deux serveurs](../../../relational-databases/security/encryption/create-identical-symmetric-keys-on-two-servers.md)  
  
  
