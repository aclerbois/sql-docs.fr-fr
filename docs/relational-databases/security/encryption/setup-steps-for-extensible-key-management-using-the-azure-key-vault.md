---
title: Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
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
- EKM, with key vault setup
- SQL Server Connector, setup
- SQL Server Connector
ms.assetid: c1f29c27-5168-48cb-b649-7029e4816906
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 1d49310aa2c1d178dfb47f05a72ccac73cd0882f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setup-steps-for-extensible-key-management-using-the-azure-key-vault"></a>Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Les étapes suivantes parcourent l’installation et la configuration du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]pour Azure Key Vault.  
  
## <a name="before-you-start"></a>Avant de commencer  
 Pour utiliser Azure Key Vault avec votre serveur SQL Server, il existe quelques conditions préalables :  
  
-   Vous devez disposer d’un abonnement Azure  
  
-   Installez la dernière version d’ [Azure PowerShell](https://azure.microsoft.com/en-us/documentation/articles/powershell-install-configure/) (version 1.0.1 ou ultérieure).  

-   Créez un répertoire Azure Active Directory  

-   Familiarisez-vous avec les principaux du stockage de la gestion de clés extensible à l’aide d’Azure Key Vault en consultant la rubrique [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  

-   La version de Visual Studio C++ Redistribuable dont vous disposez convient à la version de SQL Server que vous exécutez :
  
Version de SQL Server  |Lien d’installation du package redistribuable    
---------|--------- 
2008, 2008 R2, 2012, 2014 | [Packages redistribuables Visual C++ pour Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)    
2016 | [Package redistribuable Visual C++ pour Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)    
 
  
## <a name="part-i-set-up-an-azure-active-directory-service-principal"></a>Partie I : Configurer un principal du service Azure Active Directory  
 Pour accorder à votre service Azure Key Vault des autorisations d’accès à SQL Server, vous avez besoin d’un compte de principal du Service dans Azure Active Directory (AAD).  
  
1.  Accédez au [portail Azure Classic](https://manage.windowsazure.com), puis connectez-vous.  
  
2.  Inscrivez une application auprès d’Azure Active Directory. Pour obtenir des instructions pas à pas expliquant comment inscrire une application, consultez la section **Get an identity for the application** (Obtenir une identité pour l’application) du [billet de blog Azure Key Vault](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/).  
  
3.  Copiez l’ **ID client** et la **clé secrète client** pour une étape ultérieure, où ils seront utilisés pour accorder à votre coffre de clés l’accès à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 ![ekm-client-id](../../../relational-databases/security/encryption/media/ekm-client-id.png "ekm-client-id")  
  
 ![ekm-key-id](../../../relational-databases/security/encryption/media/ekm-key-id.png "ekm-key-id")  
  
## <a name="part-ii-create-a-key-vault-and-key"></a>Partie II : Créer un coffre de clés et une clé  
 Le coffre de clés et la clé créés ici seront utilisés par le moteur de base de données SQL Server pour la protection des clés de chiffrement.  
  
> [!IMPORTANT]  
>  L’abonnement dans lequel le coffre de clés est créé doit se trouver dans le même annuaire Azure Active Directory par défaut que celui où le principal du service Azure Active Directory a été créé. Si vous souhaitez utiliser un Active Directory que votre Active Directory par défaut pour la création d’un principal du service pour le connecteur SQL Server, vous devez modifier l’Active Directory par défaut dans votre compte Azure avant de créer votre coffre de clés. Pour savoir comment passer d’Active Directory par défaut à celui que vous souhaitez utiliser, reportez-vous au [FAQ](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB)du connecteur SQL Server.  
  
1.  **Ouvrir PowerShell et se connecter**  
  
     Installez et démarrez la [dernière version d’Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) (version 1.0.1 ou ultérieure). Connectez-vous à votre compte Azure à l’aide de la commande suivante :  
  
    ```powershell  
    Login-AzureRmAccount  
    ```  
  
     L’instruction retourne ceci :  
  
    ```  
    Environment           : AzureCloud  
    Account               : <account_name>  
    TenantId              : <tenant_id>  
    SubscriptionId        : <subscription_id>  
    CurrentStorageAccount :  
    ```  
  
    > [!NOTE]  
    >  Si vous avez plusieurs abonnements et que vous voulez en spécifier un en vue de l’utiliser pour le coffre, recourez à `Get-AzureRmSubscription` pour afficher les abonnements et à `Select-AzureRmSubscription` pour choisir l’abonnement approprié. Dans le cas contraire, PowerShell en sélectionne automatiquement un par défaut.  
  
2.  **Créer un groupe de ressources**  
  
     Toutes les ressources Azure créées via Azure Resource Manager doivent être contenues dans des groupes de ressources. Créez un groupe de ressources pour héberger votre coffre de clés. Cet exemple utilise `ContosoDevRG`. Choisissez vos propres noms de groupe de ressources et de coffre de clés **uniques** , car tous les noms de coffre de clés sont globalement uniques.  
  
    ```powershell  
    New-AzureRmResourceGroup -Name ContosoDevRG -Location 'East Asia'  
    ```  
  
     L’instruction retourne ceci :  
  
    ```  
    ResourceGroupName: ContosoDevRG  
    Location         : eastasia  
    ProvisioningState: Succeeded  
    Tags             :   
    ResourceId       : /subscriptions/<subscription_id>/  
                        resourceGroups/ContosoDevRG  
    ```  
  
    > [!NOTE]  
    >  Pour le `-Location parameter`, utilisez la commande `Get-AzureLocation` pour savoir comment spécifier un emplacement autre que celui fourni dans cet exemple. Si vous avez besoin de plus d’informations, tapez : `Get-Help Get-AzureLocation`  
  
3.  **Créer un coffre de clés**  
  
     L’applet de commande `New-AzureRmKeyVault` nécessite un nom de groupe de ressources, un nom de coffre de clés et un emplacement géographique. Par exemple, pour un coffre de clés nommé `ContosoDevKeyVault`, tapez :  
  
    ```powershell  
    New-AzureRmKeyVault -VaultName 'ContosoDevKeyVault' `  
       -ResourceGroupName 'ContosoDevRG' -Location 'East Asia'  
    ```  
  
     Notez le nom de votre coffre de clés.  
  
     L’instruction retourne ceci :  
  
    ```  
    Vault Name                       : ContosoDevKeyVault  
    Resource Group Name              : ContosoDevRG  
    Location                         : East Asia  
    ResourceId                       : /subscriptions/<subscription_id>/  
                                        resourceGroups/ContosoDevRG/providers/  
                                        Microsoft/KeyVault/vaults/ContosoDevKeyVault  
    Vault URI: https://ContosoDevKeyVault.vault.azure.net  
    Tenant ID                        : <tenant_id>  
    SKU                              : Standard  
    Enabled For Deployment?          : False  
    Enabled For Template Deployment? : False  
    Enabled For Disk Encryption?     : False  
    Access Policies                  :  
             Tenant ID              : <tenant_id>  
             Object ID              : <object_id>  
             Application ID         :   
             Display Name           : <display_name>  
             Permissions to Keys    : get, create, delete, list, update, import,   
                                      backup, restore  
             Permissions to Secrets : all  
    Tags                             :  
    ```  
  
4.  **Accorder au principal du service Azure Active Directory des autorisations d’accès au coffre de clés**  
  
     Vous pouvez autoriser d’autres utilisateurs et applications à utiliser votre coffre de clés.   
    Dans le cas présent, utilisons le principal du service Azure Active Directory créé dans la partie I pour autoriser l’instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    > [!IMPORTANT]  
    >  Le principal du service Azure Active Directory doit avoir au moins les autorisations `get`, `list`, `wrapKey`et `unwrapKey` pour le coffre de clés.  
  
     Comme illustré ci-dessous, utilisez l’ **ID client** de la partie I pour le paramètre `ServicePrincipalName` . La commande `Set-AzureRmKeyVaultAccessPolicy` s’exécute en mode silencieux sans aucune sortie si son exécution réussit.  
  
    ```powershell  
    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoDevKeyVault'`  
      -ServicePrincipalName EF5C8E09-4D2A-4A76-9998-D93440D8115D `  
      -PermissionsToKeys get, list, wrapKey, unwrapKey  
    ```  
  
     Appelez l’applet de commande `Get-AzureRmKeyVault` pour vérifier les autorisations. Dans la sortie de l’instruction, sous « Access Policies », vous devez voir le nom de votre application AAD répertorié en tant qu’autre client ayant accès à ce coffre de clés.  
  
       
5.  **Générer une clé asymétrique dans le coffre de clés**  
  
     Il existe deux manières de générer une clé dans Azure Key Vault : 1) importer une clé existante ou 2) créer une clé.  

    ### <a name="best-practice"></a>Recommandation :
    
    Pour garantir la récupération rapide de clé et être en mesure d’accéder à vos données en dehors d’Azure, nous recommandons les meilleures pratiques suivantes :
 
    1. Créer votre clé de chiffrement localement sur un appareil HSM local. (S’assurer qu’il s’agit d’une clé RSA 2048 bits asymétrique, et donc stockable dans Azure Key Vault.)
    2. Importer la clé de chiffrement dans Azure Key Vault. Consultez les étapes ci-dessous pour connaître la marche à suivre.
    3. Avant d’utiliser la clé dans le coffre de clés Azure pour la première fois, sauvegardez une clé Azure Key Vault. En savoir plus sur la commande [Backup-AzureKeyVaultKey](https://msdn.microsoft.com/library/mt126292.aspx) .
    4. Chaque fois que des modifications sont apportées à la clé (par exemple, ajout de listes de contrôle d’accès, ajout de balises, ajout d’attributs clé), veillez à sauvegarder une autre clé Azure Key Vault.

        > [!NOTE]  
        >  La sauvegarde d’une clé est une opération de clé Azure Key Vault qui retourne un fichier qui peut être enregistré n’importe où.

    ### <a name="types-of-keys"></a>Types de clés :
    Il existe deux types de clés que vous pouvez générer dans Azure Key Vault. Les deux sont des clés RSA asymétriques 2 048 bits.  
  
    -   **À protection logicielle :** traitées dans le logiciel et chiffrées au repos. Les opérations sur les clés à protection logicielle ont lieu sur les machines virtuelles Azure. Recommandé pour les clés qui ne sont pas utilisées dans un déploiement de production.  
  
    -   **Protégées par HSM :** créées et protégées par un module de sécurité matériel HSM pour une sécurité renforcée. Coûte environ 1 $ par version de clé.  
  
        > [!IMPORTANT]  
        >  Le connecteur SQL Server exige que le nom de clé utilise uniquement les caractères « a-z », « A-Z », « 0-9 » et « - », avec une limite de 26 caractères.   
        > Des versions de clés différentes sous le même nom de clé dans Azure Key Vault ne fonctionnent pas avec le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour permuter une clé Azure Key Vault utilisée par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], reportez-vous aux étapes relatives à la substitution des clés dans la section [Résolution des problèmes et maintenance du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).  

    ### <a name="import-an-existing-key"></a>Importer une clé existante   
  
    Si vous avez une clé à protection logicielle RSA 2048 bits existante, vous pouvez charger la clé dans Azure Key Vault. Par exemple, si vous disposez d’un fichier .PFX enregistré sur votre lecteur `C:\\` dans un fichier nommé `softkey.pfx` et que vous souhaitez charger cette clé dans Azure Key Vault, tapez la commande suivante pour définir la variable `securepfxpwd` avec un mot de passe `12987553` pour le fichier .PFX :  
  
    ``` powershell  
    $securepfxpwd = ConvertTo-SecureString –String '12987553' `  
      –AsPlainText –Force  
    ```  
  
    Ensuite, vous pouvez taper la commande suivante pour importer la clé à partir du fichier .PFX, afin de protéger la clé par matériel (recommandé) dans le service Key Vault :  
  
    ``` powershell  
        Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' `  
          -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' `  
          -KeyFilePassword $securepfxpwd $securepfxpwd  -Destination 'HSM'  
    ```  
 
    > [!IMPORTANT]  
    > L'importation de la clé asymétrique est fortement recommandée dans les scénarios de production, car elle permet à l'administrateur de déposer la clé dans un système de dépôt de clés (Key escrow). Si la clé asymétrique est créée dans le coffre, elle ne peut pas être déposée, car la clé privée ne peut jamais sortir du coffre. Les clés utilisées pour protéger les données critiques doivent être déposées. La perte d'une clé asymétrique rend une partie des données définitivement irrécupérable.  

    ### <a name="create-a-new-key"></a>Créer une clé

    ##### <a name="example"></a>Exemple :  
    Si vous le souhaitez, vous pouvez créer une nouvelle clé de chiffrement directement dans Azure Key Vault, et la doter d’une protection logicielle ou HSM. Dans cet exemple, nous allons créer une clé à protection logicielle à l’aide de l’ `Add-AzureKeyVaultKey cmdlet`:  

    ``` powershell  
    Add-AzureKeyVaultKey -VaultName 'ContosoDevKeyVault' `  
      -Name 'ContosoRSAKey0' -Destination 'Software'  
    ```  
  
    L’instruction retourne ceci :  
  
    ```  
    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes  
    Key        :  {"kid":"https:contosodevKeyVault.azure.net/keys/  
                   ContosoRSAKey0/<guid>","dty":"RSA:,"key_ops": ...  
    VaultName  : contosodevkeyvault  
    Name       : contosoRSAKey0  
    Version    : <guid>  
    Id         : https://contosodevkeyvault.vault.azure.net:443/  
                 keys/ContosoRSAKey0/<guid>  
    ```  

    > [!IMPORTANT]  
    >  Le coffre de clés prend en charge plusieurs versions de la même clé nommée, mais les clés que le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit utiliser ne doivent pas avoir une version ou être restaurées. Si l'administrateur veut modifier la clé utilisée pour le chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , une nouvelle clé avec un autre nom doit être créée dans le coffre et utilisée pour chiffrer la clé de chiffrement des données.  
   
  
## <a name="part-iii-install-the-includessnoversionincludesssnoversion-mdmd-connector"></a>Partie III : Installer le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
 Téléchargez le connecteur SQL Server à partir du [Centre de téléchargement Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=521700). (Cette opération doit être effectuée par l’administrateur de l’ordinateur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .)  

> [!NOTE]  
>  Les versions 1.0.0.440 et antérieures ont été remplacées et ne sont plus prises en charge dans les environnements de production. Effectuez la mise à niveau vers la version 1.0.1.0 ou ultérieure en accédant au [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=45344) et en utilisant les instructions fournies dans la page [Résolution des problèmes et maintenance du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md) sous « Mise à niveau du connecteur SQL Server ».
  
 ![ekm-connector-install](../../../relational-databases/security/encryption/media/ekm-connector-install.png "ekm-connector-install")  
  
 Par défaut, le connecteur est installé à l’emplacement C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault. Cet emplacement peut être changé lors de l'installation. (S'il est changé, adaptez les scripts ci-dessous).  
  
 Il n’existe aucune interface pour le connecteur, mais s’il est correctement installé, le fichier **Microsoft.AzureKeyVaultService.EKM.dll** est installé sur l’ordinateur. Il s’agit de la DLL du fournisseur EKM de services chiffrement qui doit être inscrite auprès de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l’aide de l’instruction `CREATE CRYPTOGRAPHIC PROVIDER` .  
  
 L’installation du connecteur SQL Server vous permet également de télécharger des exemples de scripts pour le chiffrement SQL Server.  
  
 Pour consulter des explications des codes d’erreur, les paramètres de configuration ou les tâches de maintenance pour le connecteur SQL Server, reportez-vous aux annexes à la fin de cette rubrique :  
  
-   [A. Instructions de maintenance du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixA)  
  
-   [C. Explications des codes d’erreur du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixC)  
  
  
## <a name="part-iv-configure-includessnoversionincludesssnoversion-mdmd"></a>Partie IV : Configurer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
 Reportez-vous à [B. Forum Aux Questions (FAQ)](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB) pour lire une note concernant les niveaux d’autorisation minimaux nécessaires pour chaque action de cette section.  
  
1.  **Lancer sqlcmd.exe ou [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Management Studio**  
  
2.  **Configurer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour utiliser la gestion de clés extensible (EKM)**  
  
     Exécutez le script [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivant pour configurer le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] afin d’utiliser un fournisseur EKM.  
  
    ```sql  
    -- Enable advanced options.  
    USE master;  
    GO  
  
    sp_configure 'show advanced options', 1;  
    GO  
    RECONFIGURE;  
    GO  
  
    -- Enable EKM provider  
    sp_configure 'EKM provider enabled', 1;  
    GO  
    RECONFIGURE;  
    ```  
  
3.  **Inscrire (créer) le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en tant que fournisseur EKM avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]**  
  
     -- Créez un fournisseur de services de chiffrement à l’aide du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , qui est un fournisseur EKM pour le coffre Azure Key Vault.    
    L’exemple suivant utilise le nom `AzureKeyVault_EKM_Prov`.  
  
    ```sql  
    CREATE CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov   
    FROM FILE = 'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';  
    GO  
    ```  
  
    > [!NOTE]  
    >  Le chemin d’accès ne peut pas dépasser 256 caractères.  
  
  
4.  **Configurer des informations d’identification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour un compte de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] afin d’utiliser le coffre de clés**  
  
     Des informations d’identification doivent être ajoutées à chaque connexion appelée à effectuer un chiffrement à l’aide d’une clé à partir du coffre de clés. Cela peut inclure :  
  
    -   Une connexion administrateur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui utilisera le coffre de clés afin de configurer et de gérer des scénarios de chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    -   D’autres connexions [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui peuvent activer le chiffrement TDE (Transparent Data Encryption), ou d’autres fonctionnalités de chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
     Il existe un mappage un-à-un entre les informations d’identification et les connexions. Autrement dit, chaque connexion doit disposer de ses propres informations d’identification.  
  
     Modifiez le script [!INCLUDE[tsql](../../../includes/tsql-md.md)] ci-dessous comme suit :  
  
    -   Modifiez l’argument `IDENTITY` (`ContosoDevKeyVault`) de sorte qu’il pointe vers votre coffre Azure Key Vault.
        - Si vous utilisez **public Azure**, remplacez l’argument `IDENTITY` par le nom de votre coffre Azure Key Vault de la Partie II.
        - Si vous utilisez un **cloud privé Azure** (par exemple, Azure Government, Azure China ou Azure Germany), remplacez l’argument `IDENTITY` par l’URI du coffre renvoyée à l’étape 3 de la Partie II. N’incluez pas « https:// » dans l’URI du coffre.   
    -   Remplacez la première partie de l’argument `SECRET` par l’ **ID client** Azure Active Directory mentionné dans la Partie I. Dans cet exemple, l’ **ID client** est `EF5C8E094D2A4A769998D93440D8115D`.  
  
        > [!IMPORTANT]  
        >  Vous devez supprimer les tirets de l’ **ID client**.  
  
    -   Complétez la deuxième partie de l’argument `SECRET` avec la **clé secrète client** mentionnée dans la Partie I. Dans cet exemple, la **clé secrète client** de la Partie I est `Replace-With-AAD-Client-Secret`. La chaîne finale pour l’argument `SECRET` est une longue séquence de lettres et de chiffres, *sans tirets*.  
  
    ```sql  
    USE master;  
    CREATE CREDENTIAL sysadmin_ekm_cred   
        WITH IDENTITY = 'ContosoDevKeyVault', -- for public Azure
        -- WITH IDENTITY = 'ContosoDevKeyVault.vault.usgovcloudapi.net', -- for Azure Government
        -- WITH IDENTITY = 'ContosoDevKeyVault.vault.azure.cn', -- for Azure China
        -- WITH IDENTITY = 'ContosoDevKeyVault.vault.microsoftazure.de', -- for Azure Germany   
        SECRET = 'EF5C8E094D2A4A769998D93440D8115DReplace-With-AAD-Client-Secret'   
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;  
  
    -- Add the credential to the SQL Server administrator's domain login   
    ALTER LOGIN [<domain>\<login>]  
    ADD CREDENTIAL sysadmin_ekm_cred;  
    ```  
  
     Pour obtenir un exemple d’utilisation de variables pour les arguments **CREATE CREDENTIAL** et la suppression par programmation des tirets de l’ID client, consultez [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md).  
  
5.  **Ouvrir votre clé Azure Key Vault dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]**  
  
     Si vous avez importé une clé asymétrique, comme décrit dans la Partie II, ouvrez la clé en fournissant le nom de votre clé dans le script [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivant.  
  
    -   Remplacez `CONTOSO_KEY` par le nom souhaité pour la clé dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    -   Remplacez `ContosoRSAKey0` par le nom de votre clé dans Azure Key Vault.  
  
    ```sql  
    CREATE ASYMMETRIC KEY CONTOSO_KEY   
    FROM PROVIDER [AzureKeyVault_EKM_Prov]  
    WITH PROVIDER_KEY_NAME = 'ContosoRSAKey0',  
    CREATION_DISPOSITION = OPEN_EXISTING;  
    ```  
## <a name="next-step"></a>Étape suivante  
  
Maintenant que vous avez terminé la configuration de base, découvrez comment [Utiliser le connecteur SQL Server avec les fonctionnalités de chiffrement SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).   
  
## <a name="see-also"></a> Voir aussi  
 [Gestion de clés extensible à l'aide d'Azure Key Vault](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)   
[Résolution des problèmes et maintenance du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)  
  
  
