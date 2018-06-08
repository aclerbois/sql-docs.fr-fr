---
title: Résolution des problèmes et maintenance du connecteur SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 04/05/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Connector, appendix
ms.assetid: 7f5b73fc-e699-49ac-a22d-f4adcfae62b1
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a8f4b4a73139a698d481b65e1ebe93b524e2d863
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-connector-maintenance-amp-troubleshooting"></a>Résolution des problèmes et maintenance du connecteur SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Des informations supplémentaires sur le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont fournies dans cette rubrique. Pour plus d’informations sur le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md), [Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault ](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md) et [Utiliser le connecteur SQL Server avec les fonctionnalités de chiffrement SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).  
  
  
##  <a name="AppendixA"></a> A. Instructions de maintenance du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
  
### <a name="key-rollover"></a>Substitution de clé  
  
> [!IMPORTANT]  
>  Le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] exige que le nom de clé utilise uniquement les caractères « a-z », « A-Z », « 0-9 » et « - », avec une limite de 26 caractères.   
> Des versions de clés différentes sous le même nom de clé dans Azure Key Vault ne fonctionnent pas avec le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour faire pivoter une clé Azure Key Vault qui est utilisée par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], une nouvelle clé portant un nouveau nom de la clé doit être créée.  
  
 En général, les clés asymétriques de serveur pour le chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doivent faire l’objet d’une gestion des versions une fois par an ou une fois tous les deux ans. Bien que le coffre de clés prenne en charge la gestion des versions des clés, les clients ne doivent pas utiliser cette fonctionnalité pour implémenter la gestion des versions. Le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut pas traiter les modifications apportées à la version des clés du coffre de clés. Pour implémenter la gestion des versions des clés, le client doit créer une clé dans le coffre de clés, puis chiffrer à nouveau la clé de chiffrement de données dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
 Dans le cas du chiffrement transparent des données, voici comment procéder :  
  
-   **Dans PowerShell** : Créez une clé asymétrique (avec un nom différent de celui de votre clé asymétrique TDE) dans le coffre de clés.  
  
    ```powershell  
    Add-AzureRmKeyVaultKey -VaultName 'ContosoDevKeyVault' `  
      -Name 'Key2' -Destination 'Software'  
    ```  
  
-   **À l’aide de [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] ou sqlcmd.exe :** Utilisez les instructions suivantes, comme indiqué à l’étape 3 de la section 3.  
  
     Importez la nouvelle clé asymétrique.  
  
    ```sql  
    USE master  
    CREATE ASYMMETRIC KEY [MASTER_KEY2]   
    FROM PROVIDER [EKM]   
    WITH PROVIDER_KEY_NAME = 'Key2',   
    CREATION_DISPOSITION = OPEN_EXISTING   
    GO  
    ```  
  
     Créez une connexion à associer à la nouvelle clé asymétrique (comme indiqué dans les instructions sur le chiffrement transparent des données).  
  
    ```sql  
    USE master  
    CREATE LOGIN TDE_Login2   
    FROM ASYMMETRIC KEY [MASTER_KEY2]  
    GO  
    ```  
  
     Créez des informations d’identification à associer à la connexion.  
  
    ```sql  
    CREATE CREDENTIAL Azure_EKM_TDE_cred2  
        WITH IDENTITY = 'ContosoDevKeyVault',   
       SECRET = 'EF5C8E094D2A4A769998D93440D8115DAADsecret123456789=’   
    FOR CRYPTOGRAPHIC PROVIDER EKM;  
  
    ALTER LOGIN TDE_Login2  
    ADD CREDENTIAL Azure_EKM_TDE_cred2;  
    GO  
    ```  
  
     Choisissez la base de données dont vous souhaitez chiffrer à nouveau la clé de chiffrement de base de données.  
  
    ```sql  
    USE [database]  
    GO  
    ```  
  
     Rechiffrez la clé de chiffrement de base de données.  
  
    ```sql  
    ALTER DATABASE ENCRYPTION KEY   
    ENCRYPTION BY SERVER ASYMMETRIC KEY [MASTER_KEY2];  
    GO  
    ```  
  
### <a name="upgrade-of-includessnoversionincludesssnoversion-mdmd-connector"></a>Mise à niveau du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

Les versions 1.0.0.440 et antérieures ont été remplacées et ne sont plus prises en charge dans les environnements de production. Les versions 1.0.1.0 et les versions antérieures ne sont pas prises en charge dans les environnements de production. Utilisez les instructions suivantes pour effectuer une mise à niveau vers la dernière version disponible sur le [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=45344).

Si vous utilisez actuellement la version 1.0.1.0 ou une version plus récente, procédez comme suit pour mettre à jour vers la dernière version du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Ces instructions évitent de redémarrer l’instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .
 
1. Installez la version la plus récente du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=45344). Dans l’Assistant programme d’installation, enregistrez le nouveau fichier DLL sous un chemin d’accès différent du chemin d’accès au fichier de DLL du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d’origine. Par exemple, le nouveau chemin d’accès du fichier pourrait être : `C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\<latest version number>\Microsoft.AzureKeyVaultService.EKM.dll`
 
2. Dans l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], exécutez la commande Transact-SQL suivante pour faire pointer votre instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vers votre nouvelle version du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :

    ``` 
    ALTER CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov   
    FROM FILE =   
    'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\<latest version number>\Microsoft.AzureKeyVaultService.EKM.dll'
    GO  
    ```

Si vous utilisez actuellement la version 1.0.0.440 ou une version plus récente, procédez comme suit pour mettre à jour vers la dernière version du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .
  
1.  Arrêtez l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
2.  Arrêtez le service du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
3.  Désinstallez le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l’aide de la fonctionnalité Programmes et fonctionnalités de Windows.  
  
     Vous pouvez également renommer le dossier dans lequel se trouve le fichier DLL. Le nom par défaut du dossier est «[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour Microsoft Azure Key Vault ».  
  
4.  Installez la version la plus récente du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à partir du Centre de téléchargement Microsoft.  
  
5.  Redémarrez l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
6.  Exécutez l’instruction suivante pour modifier le fournisseur EKM afin de commencer à utiliser la version la plus récente du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Assurez-vous que le chemin du fichier pointe vers l’emplacement où vous avez téléchargé la version la plus récente. Cette étape peut être ignorée si la nouvelle version est installée dans le même emplacement que la version d’origine. 
  
    ```sql  
    ALTER CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov   
    FROM FILE =   
    'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';  
    GO  
    ```  
  
7.  Vérifiez que les bases de données qui utilisent le chiffrement TDE sont accessibles.  
  
8.  Après avoir vérifié que la mise à jour fonctionne, vous pouvez supprimer le dossier de l’ancien connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si vous avez choisi de le renommer au lieu de le désinstaller à l’étape 3.  
  
### <a name="rolling-the-includessnoversionincludesssnoversion-mdmd-service-principal"></a>Modification du principal du service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilise des principaux du service créés dans Azure Active Directory comme informations d’identification pour accéder au coffre de clés.  Le principal du service a un ID client et une clé d’authentification.  Des informations d’identification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont configurées avec le **nom du coffre**, l’ **ID client**et la **clé d’authentification**.  La **clé d’authentification** est valide un certain temps (1 ou 2 ans).   Avant l’expiration de cette période, une nouvelle clé doit être générée pour le principal du service dans Azure AD.  Ensuite, les informations d’identification doivent être modifiées dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].    [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] gère un cache pour les informations d’identification de la session en cours ; ainsi, si des informations d’identification sont modifiées, [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] doit être redémarré.  
  
### <a name="key-backup-and-recovery"></a>Sauvegarde et récupération des clés  
Le coffre de clés doit être sauvegardé régulièrement. Si une clé asymétrique dans le coffre est perdue, elle peut être restaurée à partir d’une sauvegarde. La clé doit être restaurée à l’aide du même nom qu’avant, ce que fera la commande PowerShell Restore (voir les étapes ci-dessous).  
Si le coffre a été perdu, vous devez recréer un coffre et restaurer la clé asymétrique dans le coffre en utilisant le même nom qu’avant. Le nom du coffre peut être différent (ou le même qu’avant). En outre, vous devez définir les autorisations d’accès sur le nouveau coffre de manière à accorder au principal du service SQL Server l’accès nécessaire pour les scénarios de chiffrement SQL Server, puis paramétrer les informations d’identification SQL Server afin qu’elles reflètent le nom du nouveau coffre.  
Voici un récapitulatif des étapes :  
  
* Sauvegarder la clé de coffre (à l’aide de l’applet de commande PowerShell Backup-AzureKeyVaultKey).  
* En cas de défaillance du coffre, créer un coffre dans la même région géographique*. L’utilisateur qui effectue cette création doit se trouver dans le même répertoire par défaut que le programme d’installation du principal du service pour SQL Server.  
* Restaurer la clé dans le nouveau coffre (à l’aide de l’applet de commande PowerShell Restore-AzureKeyVaultKey : cette opération restaure la clé en utilisant le même nom qu’avant). S’il existe déjà une clé portant le même nom, la restauration échoue.  
* Accorder des autorisations au principal du service SQL Server pour utiliser ce nouveau coffre.  
* Modifier les informations d’identification SQL Server utilisées par le moteur de base de données afin qu’elles reflètent le nom du nouveau coffre (le cas échéant).  
  
Les sauvegardes des clés peuvent être restaurées entre les régions Azure, tant qu’elles restent dans la même région géographique ou le même cloud national : États-Unis, Canada, Japon, Australie, Inde, Asie et Pacifique du sud, Europe, Brésil, Chine, Administration américaine ou Allemagne.  
  
  
##  <a name="AppendixB"></a> B. Forum Aux Questions (FAQ)  
### <a name="on-azure-key-vault"></a>À propos d’Azure Key Vault  
  
**Comment fonctionnent les opérations de clé avec Azure Key Vault ?**  
 La clé asymétrique dans le coffre de clés permet de protéger les clés de chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Seule la partie publique de la clé asymétrique sort du coffre ; la partie privée n’est jamais exportée par le coffre. Toutes les opérations de chiffrement effectuées à l’aide de la clé asymétrique ont lieu dans le service Azure Key Vault et sont protégées par la sécurité de ce service.  
  
 **Qu’est-ce qu’un URI de clé ?**  
 Dans Azure Key Vault, chaque clé a un URI, que vous pouvez utiliser pour référencer la clé dans votre application. Utilisez le format `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey` pour obtenir la version actuelle, et le format `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87` pour obtenir une version spécifique.  
  
### <a name="on-configuring-includessnoversionincludesssnoversion-mdmd"></a>À propos de la configuration de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

**Quels sont les points de terminaison auxquels le connecteur SQL Server doit pouvoir accéder ?** Le connecteur communique avec deux points de terminaison, qui doivent figurer dans la liste verte. Le seul port requis pour la communication sortante à ces autres services est le port 443 pour Https :
-  login.microsoftonline.com/*:443
-  *.vault.azure.net/*:443
  
**Quels sont les niveaux d’autorisation minimaux exigés pour chaque étape de configuration dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]?**  
 Même si vous pouvez effectuer toutes les étapes de configuration comme membre du rôle serveur fixe sysadmin, [!INCLUDE[msCoName](../../../includes/msconame-md.md)] vous encourage à réduire les autorisations que vous utilisez. La liste suivante définit le niveau d’autorisation minimal pour chaque action.  
  
-   Pour créer un fournisseur de services de chiffrement, l’autorisation `CONTROL SERVER` ou l’appartenance au rôle de serveur fixe **sysadmin** est exigée.  
  
-   Pour modifier une option de configuration et exécuter l’instruction `RECONFIGURE` , vous devez disposer de l’autorisation de niveau serveur `ALTER SETTINGS` . L’autorisation `ALTER SETTINGS` est implicitement détenue par les rôles serveur fixes sysadmin et **serveradmin** .  
  
-   Pour créer des informations d’identification, l’autorisation `ALTER ANY CREDENTIAL` est exigée.  
  
-   Pour ajouter des informations d’identification à une connexion, l’autorisation `ALTER ANY LOGIN` est exigée.  
  
-   Pour créer une clé asymétrique, l’autorisation `CREATE ASYMMETRIC KEY` est exigée.  

**Comment modifier mon Active Directory par défaut, de sorte que mon coffre de clés soit créé dans le même abonnement et Active Directory que le principal du service que j’ai créé pour le connecteur [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] ?**

![aad-change-default-directory-helpsteps](../../../relational-databases/security/encryption/media/aad-change-default-directory-helpsteps.png)

1. Accédez au portail Azure Classic : [https://manage.windowsazure.com](https://manage.windowsazure.com)  
2. Dans le menu de gauche, faites défiler vers le bas et sélectionnez **Paramètres**.
3. Sélectionnez l’abonnement Azure que vous utilisez actuellement, puis cliquez sur **Modifier l’annuaire** à partir des commandes en bas de l’écran.
4. Dans la fenêtre contextuelle, utilisez la liste déroulante **Annuaire** pour sélectionner l’annuaire Active Directory que vous souhaitez utiliser. Celui-ci devient l’annuaire par défaut.
5. Assurez-vous que vous êtes l’administrateur global d’Active Directory qui vient d’être sélectionné. Si vous n’êtes pas l’administrateur global, il se peut que vous ayez perdu des autorisations de gestion en raison du basculement d’annuaires.
6. Une fois la fenêtre contextuelle fermée, si aucun de vos abonnements ne s’affiche, vous devez mettre à jour le filtre **Filtrer par annuaire** du filtre **Abonnements** du menu supérieur droit de l’écran pour afficher les abonnements à l’aide de l’annuaire Active Directory qui vient d’être mis à jour.

    > [!NOTE] 
    > Il se peut que vous ne disposiez pas des autorisations permettant de modifier réellement l’annuaire par défaut sur votre abonnement Azure. Dans ce cas, créez le principal du service AAD dans votre annuaire par défaut afin qu’il soit dans le même annuaire que le coffre de clés Azure utilisé ultérieurement.

Pour en savoir plus sur Active Directory, lisez [Association des abonnements Azure avec Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-how-subscriptions-associated-directory/)
  
##  <a name="AppendixC"></a> C. Explications des codes d’erreur du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
 **Codes d’erreur du fournisseur :**  
  
Code d'erreur  |Symbole  |Description    
---------|---------|---------  
0 | scp_err_Success | L'opération a réussi.    
 1 | scp_err_Failure | L’opération a échoué.    
2 | scp_err_InsufficientBuffer | Cette erreur indique au moteur d’allouer davantage de mémoire pour la mémoire tampon.    
3 | scp_err_NotSupported | L'opération n'est pas prise en charge. Par exemple, le type ou l’algorithme de clé spécifié n’est pas pris en charge par le fournisseur EKM.    
4 | scp_err_NotFound | Le type ou l’algorithme de clé spécifié n’a pas pu être trouvé par le fournisseur EKM.    
5 | scp_err_AuthFailure | L’authentification auprès du fournisseur EKM a échoué.    
6 | scp_err_InvalidArgument | L’argument fourni n’est pas valide.    
7 | scp_err_ProviderError | Une erreur non spécifiée interceptée par le moteur SQL s’est produite dans le fournisseur EKM.    
2049 | scp_err_KeyNameDoesNotFitThumbprint | Le nom de la clé est trop long pour tenir dans l’empreinte numérique du moteur SQL. Le nom de la clé ne doit pas dépasser 26 caractères.    
2050 | scp_err_PasswordTooShort | La chaîne secrète, qui est la concaténation de l’ID du client AAD et de la clé secrète, comporte moins de 32 caractères.    
2051 | scp_err_OutOfMemory | Le moteur SQL ne dispose pas de suffisamment de mémoire et n’a pas pu allouer de mémoire pour le fournisseur EKM.    
2052 | scp_err_ConvertKeyNameToThumbprint | Impossible de convertir le nom de la clé en empreinte numérique.    
2053 | scp_err_ConvertThumbprintToKeyName|  Impossible de convertir l’empreinte numérique en nom de clé.    
3000 | ErrorSuccess | L’opération AKV a réussi.    
3001 | ErrorUnknown | L’opération AKV a échoué avec une erreur non spécifiée.    
3002 | ErrorHttpCreateHttpClientOutOfMemory | Impossible de créer une opération HttpClient pour AKV en raison d’une insuffisance de mémoire.    
3003 | ErrorHttpOpenSession | Impossible d’ouvrir une session HTTP en raison d’une erreur réseau.    
3004 | ErrorHttpConnectSession | Impossible de connecter une session HTTP en raison d’une erreur réseau.    
3005 | ErrorHttpAttemptConnect | Tentative de connexion impossible en raison d’une erreur réseau.    
3006 | ErrorHttpOpenRequest | Impossible d’ouvrir une requête en raison d’une erreur réseau.    
3007 | ErrorHttpAddRequestHeader | Impossible d’ajouter un en-tête de requête.    
3008 | ErrorHttpSendRequest | Impossible d’envoyer une requête en raison d’une erreur réseau.    
3009 | ErrorHttpGetResponseCode | Impossible d’obtenir un code de réponse en raison d’une erreur réseau.    
3010 | ErrorHttpResponseCodeUnauthorized | Le serveur a répondu 401 pour la requête.    
3011 | ErrorHttpResponseCodeThrottled | Le serveur a limité la requête.    
3012 | ErrorHttpResponseCodeClientError | La demande envoyée à partir du connecteur n’est pas valide. Cela signifie généralement le nom de clé n’est pas valide ou contient des caractères non valides.
3013 | ErrorHttpResponseCodeServerError | Le serveur a renvoyé un code de réponse compris entre 500 et 600.    
3014 | ErrorHttpQueryHeader | Impossible de rechercher l’en-tête de réponse.    
3015 | ErrorHttpQueryHeaderOutOfMemoryCopyHeader | Impossible de copier l’en-tête de réponse en raison d’une insuffisance de mémoire.    
3016 | ErrorHttpQueryHeaderOutOfMemoryReallocBuffer | Impossible d’interroger l’en-tête de réponse en raison d’une insuffisance de mémoire lors de la réallocation d’une mémoire tampon.    
3017 | ErrorHttpQueryHeaderNotFound | L’en-tête de requête est introuvable dans la réponse.    
3018 | ErrorHttpQueryHeaderUpdateBufferLength | Impossible de mettre à jour la longueur de la mémoire tampon lors de l’interrogation de l’en-tête de réponse.    
3019 | ErrorHttpReadData | Impossible de lire les données de la réponse en raison d’une erreur réseau. 
3076 | ErrorHttpResourceNotFound | Le serveur a répondu par une erreur 404, car le nom de la clé est introuvable. Vérifiez que le nom de la clé existe dans le coffre.
3077 | ErrorHttpOperationForbidden | Le serveur a répondu par une erreur 403, car l’utilisateur n’a pas l’autorisation appropriée pour effectuer l’action. Vérifiez que vous disposez des autorisations pour l’opération spécifiée. Au minimum, le connecteur exige des autorisations « get, list, wrapKey, unwrapKey » pour fonctionner correctement.   
  
Si votre code d’erreur ne figure pas dans ce tableau, voici d’autres raisons pouvant expliquer pourquoi l’erreur se produit :   
  
-   Vous ne disposez peut-être pas d’un accès Internet et vous ne pouvez pas accéder à votre service Azure Key Vault. Vérifiez votre connexion Internet.  
  
-   Le service Azure Key Vault n’est peut-être pas disponible. Réessayer ultérieurement.  
  
-   Vous avez peut-être supprimé la clé asymétrique d’Azure Key Vault ou de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Restaurez la clé.  
  
-   Si vous obtenez l’erreur « Impossible de charger la bibliothèque », vérifiez que la version de Visual Studio C++ Redistribuable dont vous disposez convient à la version de SQL Server que vous exécutez. Le tableau ci-dessous indique la version à installer à partir du Centre de téléchargement Microsoft.   
  
Version de SQL Server  |Lien d’installation du package redistribuable    
---------|--------- 
2008, 2008 R2, 2012, 2014 | [Packages redistribuables Visual C++ pour Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)    
2016 | [Package redistribuable Visual C++ pour Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)    
  
  
## <a name="additional-references"></a>Références supplémentaires  
 Informations supplémentaires sur la gestion de clés extensible :  
  
-   [Gestion de clés extensible &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
 Chiffrements SQL prenant en charge la gestion de clés extensible :  
  
-   [Activer le chiffrement transparent des données à l’aide de la gestion de clés extensible (EKM)](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
  
-   [Chiffrement de sauvegarde](../../../relational-databases/backup-restore/backup-encryption.md)  
  
-   [Créer une sauvegarde chiffrée](../../../relational-databases/backup-restore/create-an-encrypted-backup.md)  
  
 Commandes [!INCLUDE[tsql](../../../includes/tsql-md.md)] associées :  
  
-   [sp_configure &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
-   [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
-   [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)  
  
-   [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
-   [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
-   [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
-   [ALTER LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-login-transact-sql.md)  
  
 Documentation relative à Azure Key Vault  
  
-   [Qu'est-ce qu'Azure Key Vault ?](https://azure.microsoft.com/documentation/articles/key-vault-whatis/)  
  
-   [Prise en main d'Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/)  
  
-   Informations de référence sur les [applets de commande Azure Key Vault](https://msdn.microsoft.com/library/dn868052.aspx) de PowerShell  
  
## <a name="see-also"></a> Voir aussi  
 [Gestion de clés extensible à l’aide d’Azure Key Vault](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  [Utiliser le connecteur SQL Server avec les fonctionnalités de chiffrement SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)   
 [Fournisseur EKM activé (option de configuration de serveur)](../../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md)   
 [Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)  
  
  
