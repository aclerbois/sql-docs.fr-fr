---
title: Configurer Always Encrypted à l’aide de SQL Server Management Studio | Microsoft Docs
ms.custom: ''
ms.date: 11/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.PAGE.F1
- SQL13.SWB.COLUMNENCRYPTIONKEY.PAGE.F1
- SQL13.SWB.COLUMNMASTERKEY.ROTATION.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
caps.latest.revision: 15
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7ace7cc5ca437a6ad67c5f7bf8cd138e470d0047
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-always-encrypted-using-sql-server-management-studio"></a>Configurer Always Encrypted à l’aide de SQL Server Management Studio
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

Cet article décrit les tâches de configuration d’Always Encrypted et de gestion des bases de données qui utilisent Always Encrypted avec [SSMS (SQL Server Management Studio)](../../../ssms/download-sql-server-management-studio-ssms.md).

Quand vous utilisez SSMS pour configurer Always Encrypted, SSMS gère à la fois les clés Always Encrypted et les données sensibles. Par conséquent, les clés et les données apparaissent en texte clair à l’intérieur du processus SSMS. Ainsi, il est important que vous exécutiez SSMS sur un ordinateur sécurisé. Si votre base de données est hébergée dans SQL Server, vérifiez que SSMS est exécuté sur un ordinateur autre que celui qui héberge votre instance de SQL Server. L’objectif principal d’Always Encrypted étant de garantir la sécurité des données sensibles chiffrées même si le système de base de données est compromis, l’exécution d’un script PowerShell qui traite des clés ou des données sensibles sur l’ordinateur SQL Server peut réduire ou annuler les avantages de la fonctionnalité. Pour obtenir des recommandations supplémentaires, consultez [Considérations en matière de sécurité pour la gestion des clés](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md#SecurityForKeyManagement).

SSMS ne prend pas en charge la séparation des rôles entre ceux qui gèrent la base de données (administrateurs de bases de données) et ceux qui gèrent des secrets de chiffrement et ont accès aux données en texte clair (administrateurs de la sécurité et/ou administrateurs d’applications). Si votre organisation applique la séparation des rôles, vous devez utiliser PowerShell pour configurer Always Encrypted. Pour plus d’informations, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md) et [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).

## <a name="configuring-always-encrypted-using-the-always-encrypted-wizard"></a>Configuration d’Always Encrypted à l’aide de l’Assistant Always Encrypted

[L’Assistant Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-wizard.md) est un outil puissant qui vous permet de définir la configuration de chiffrement souhaitée pour les colonnes de base de données sélectionnées. Selon la configuration actuelle d’Always Encrypted et la configuration cible souhaitée, l’Assistant peut chiffrer une colonne, la déchiffrer (supprimer le chiffrement) ou la rechiffrer (par exemple, à l’aide d’une nouvelle clé de chiffrement de colonne ou d’un type de chiffrement qui est différent du type actuel, configuré pour la colonne). Plusieurs colonnes peuvent être configurées avec une seule exécution de l’Assistant.

Si vous n’avez mis en service aucune clé pour Always Encrypted, l’Assistant les génère automatiquement pour vous. Il vous suffit de sélectionner un magasin de clés pour votre clé principale de colonne : magasin de certificats Windows ou Azure Key Vault. L’Assistant génère automatiquement des noms pour les clés et leurs objets de métadonnées dans la base de données. Si vous avez besoin de davantage de contrôle sur le mode de mise en service des clés (et de plus de possibilités pour un magasin de clés contenant une clé principale de colonne), vous pouvez utiliser les boîtes de dialogue **Nouvelle clé principale de colonne** et **Nouvelle clé de chiffrement de colonne** (décrites ci-dessous) pour mettre en service les clés avant de démarrer l’Assistant. Dans l’Assistant Always Encrypted, vous pouvez ensuite choisir la clé de chiffrement de colonne existante.

Pour plus d’informations sur l’utilisation de l’Assistant, consultez  [Assistant Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-wizard.md).

## <a name="querying-encrypted-columns"></a>Interrogation de colonnes chiffrées

Cette section explique comment :   
-   Récupérer des valeurs de chiffrement stockées dans des colonnes chiffrées.   
-   Récupérer des valeurs de texte brut stockées dans des colonnes chiffrées.   
-   Envoyer des valeurs de texte brut ciblant des colonnes chiffrées (par exemple, dans des instructions `INSERT` ou `UPDATE` et en tant que paramètres de recherche de clauses `WHERE` dans des instructions `SELECT` ).   

### <a name="retrieving-ciphertext-values-stored-in-encrypted-columns"></a>Récupération de valeurs de chiffrement stockées dans des colonnes chiffrées    

Pour récupérer des valeurs à partir d’une colonne chiffrée en tant que texte chiffré (sans déchiffrer les valeurs) :
1.  Veillez à ce que la fonctionnalité Always Encrypted soit désactivée pour la connexion de base de données de la fenêtre de l’éditeur de requête à partir de laquelle vous exécutez votre requête `SELECT` . Consultez la section ci-dessous [Activation et désactivation d’Always Encrypted pour une connexion de base de données](#en-dis) .      
2.  Exécutez une requête `SELECT` . Les données récupérées à partir des colonnes chiffrées seront retournées comme valeurs binaires (chiffrées).   

*Exemple*   
En supposant que `SSN` est une colonne chiffrée dans la table `Patients` , la requête ci-dessous récupère les valeurs de chiffrement binaires, si Always Encrypted est désactivé pour la connexion de base de données.   

![always-encrypted-ciphertext](../../../relational-databases/security/encryption/media/always-encrypted-ciphertext.png)
 
### <a name="retrieving-plaintext-values-stored-in-encrypted-columns"></a>Récupération de valeurs de texte brut stockées dans des colonnes chiffrées    

Pour récupérer des valeurs à partir d’une colonne chiffrée en tant que texte brut (pour déchiffrer les valeurs) :   
1.  Veillez à ce que la fonctionnalité Always Encrypted soit activée pour la connexion de base de données de la fenêtre de l’éditeur de requête à partir de laquelle vous exécutez votre requête `SELECT` . Cela indiquera au fournisseur de données .NET Framework pour SQL Server (utilisé par SSMS) de déchiffrer les données récupérées à partir des colonnes chiffrées. Consultez la section ci-dessous [Activation et désactivation d’Always Encrypted pour une base de données](#en-dis) .
2.  Assurez-vous que vous pouvez accéder à toutes les clés principales de colonne configurées pour les colonnes chiffrées. Par exemple, si votre clé principale de colonne est un certificat, vous devez vous assurer que le certificat est déployé sur l’ordinateur sur lequel SSMS est en cours d’exécution. Ou, si votre clé principale de colonne est une clé stockée dans Azure Key Vault, vous devez vous assurer que vous êtes autorisé à accéder à la clé (vous serez peut-être également invité à vous connecter à Azure.)
3.  Exécutez une requête `SELECT` . Les données récupérées à partir des colonnes chiffrées seront retournées comme texte brut en tant que valeurs des types de données d’origine.   

*Exemple*   
En supposant que le SSN est une colonne `char(11)` chiffrée dans la table `Patients` , la requête, illustrée ci-dessous, renvoie les valeurs en texte brut, si Always Encrypted est activé pour la connexion de base de données et si vous avez accès à la clé principale de colonne configurée pour la colonne `SSN` .   

![always-encrypted-plaintext](../../../relational-databases/security/encryption/media/always-encrypted-plaintext.png)
 
### <a name="sending-plaintext-values-targeting-encrypted-columns"></a>Envoi de valeurs en texte brut ciblant des colonnes chiffrées       

Pour exécuter une requête qui envoie une valeur ciblant une colonne chiffrée, par exemple une requête qui insère, met à jour ou filtre une valeur stockée dans une colonne chiffrée :   
1.  Veillez à ce que la fonctionnalité Always Encrypted soit activée pour la connexion de base de données de la fenêtre de l’éditeur de requête à partir de laquelle vous exécutez votre requête `SELECT` . Cela indiquera au fournisseur de données .NET Framework pour SQL Server (utilisé par SSMS) de chiffrer des variables Transact-SQL paramétrées (voir ci-dessous) ciblant des colonnes chiffrées. Consultez la section ci-dessous [Activation et désactivation d’Always Encrypted pour une base de données](#en-dis) .   
2.  Assurez-vous que vous pouvez accéder à toutes les clés principales de colonne configurées pour les colonnes chiffrées. Par exemple, si votre clé principale de colonne est un certificat, vous devez vous assurer que le certificat est déployé sur l’ordinateur sur lequel SSMS est en cours d’exécution. Ou, si votre clé principale de colonne est une clé stockée dans Azure Key Vault, vous devez vous assurer que vous êtes autorisé à accéder à la clé (vous serez peut-être également invité à vous connecter à Azure.)   
3.  Vérifiez que le paramétrage pour Always Encrypted est activé pour la fenêtre de l’éditeur de requête. (Nécessite au moins SSMS version 17.0.) Déclarez une variable Transact-SQL et initialisez-la avec une valeur à envoyer (insert, update ou filter by) à la base de données. Voir la section [Paramétrage pour Always Encrypted](#param) ci-dessous pour plus d’informations.   
    >   [!NOTE]
    >   Étant donné que la prise en charge d’Always Encrypted est un sous-ensemble limité de conversions de type, dans de nombreux cas, il est nécessaire que ce type de variable Transact-SQL soit identique au type de colonne de base de données ciblée.   
4.  Exécutez votre requête d’envoi de la valeur de la variable Transact-SQL à la base de données. SSMS convertira la variable à un paramètre de requête et chiffrera sa valeur avant de l’envoyer à la base de données.   

*Exemple*   
En supposant que `SSN` est une colonne `char(11)` chiffrée dans la table `Patients` , le script ci-dessous tente de trouver une ligne contenant `'795-73-9838'` dans la colonne SSN pour retourner la valeur de la colonne `LastName` , à condition qu’Always Encrypted soit activé pour la connexion de base de données, que le paramétrage pour Always Encrypted soit activé pour la fenêtre de l’éditeur de requête et que vous ayez accès à la clé principale de colonne configurée pour la colonne `SSN` .   

![always-encrypted-patients](../../../relational-databases/security/encryption/media/always-encrypted-patients.png)
 
### <a name="en-dis"></a> Activation et désactivation d’Always Encrypted pour une connexion de base de données   

L’activation d’Always Encrypted pour une connexion de base de données indique au fournisseur de données .NET Framework pour SQL Server, utilisé par SQL Server Management Studio, de tenter d’effectuer ces actions de manière transparente :   
-   Déchiffrer les valeurs qui sont extraites des colonnes chiffrées et retournées dans les résultats de la requête.   
-   Chiffrer les valeurs des variables paramétrées Transact-SQL qui ciblent des colonnes de base de données chiffrées.   
Pour activer Always Encrypted pour une connexion de base de données, spécifiez `Column Encryption Setting=Enabled` dans l’onglet **Propriétés supplémentaires** de la boîte de dialogue **Se connecter au serveur** .    
Pour désactiver Always Encrypted pour une connexion de base de données, spécifiez `Column Encryption Setting=Disabled` ou supprimez simplement le paramètre **Paramètre de chiffrement de colonne** dans l’onglet **Propriétés supplémentaires** de la boîte de dialogue **Se connecter au serveur** (sa valeur par défaut est **Désactivé**).   

>  [!TIP] 
>  Pour activer/désactiver Always Encrypted pour une fenêtre de l’éditeur de requête existante :   
>  1.   Cliquez avec le bouton droit n’importe où dans la fenêtre de l’éditeur de requête.
>  2.   Sélectionnez **Connexion** > **Modifier la connexion...**, 
>  3.   Cliquez sur **Options** >>,
>  4.   Sélectionnez l’onglet **Propriétés supplémentaires** et saisissez `Column Encryption Setting=Enabled` (pour activer le comportement Always Encrypted) ou supprimez le paramètre (pour désactiver le comportement Always Encrypted).   
>  5.   Cliquez sur **Se connecter**.   
   
### <a name="param"></a>Paramétrage pour Always Encrypted   
 
Le paramétrage d’Always Encrypted est une fonctionnalité de SQL Server Management Studio qui convertit automatiquement les variables Transact-SQL en paramètres de requête (instances de [SqlParameter Class](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx)). (Nécessite au moins SSMS version 17.0.) Cela permet au fournisseur de données .NET Framework pour SQL Server sous-jacent de détecter les données ciblant les colonnes chiffrées et de chiffrer ces données avant de les envoyer à la base de données. 
  
Sans le paramétrage, le fournisseur de données .NET Framework transmet chaque instruction créée dans l’éditeur de requête en tant que requête non paramétrée. Si la requête contient des littéraux ou des variables Transact-SQL qui ciblent des colonnes chiffrées, le fournisseur de données .NET Framework pour SQL Server ne peut pas les détecter et les chiffrer avant d’envoyer la requête à la base de données. Par conséquent, la requête échoue en raison d’une incompatibilité de type (entre la variable littérale en texte brut Transact-SQL et la colonne chiffrée). Par exemple, sans paramétrage, la requête suivante échoue, supposant que la colonne `SSN` est chiffrée.   

```sql
DECLARE @SSN NCHAR(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN
```

#### <a name="enablingdisabling-parameterization-for-always-encrypted"></a>Activation/désactivation du paramétrage d’Always Encrypted   


Le paramétrage d’Always Encrypted est désactivé par défaut.    

Pour activer/désactiver le paramétrage d’Always Encrypted pour la fenêtre active de l’éditeur de requête :   
1.  Sélectionnez **Requête** dans le menu principal.   
2.  Sélectionnez **Options de requête…**.   
3.  Accédez à **Exécution** > **Avancé**.   
4.  Sélectionnez ou désélectionnez **Activer le paramétrage d’Always Encrypted**.   
5.  Cliquez sur **OK**.   

Pour activer/désactiver le paramétrage d’Always Encrypted pour de prochaines fenêtres d’éditeur de requête :   
1.  Sélectionnez **Outils** dans le menu principal.   
2.  Sélectionnez **Options...**.   
3.  Accédez à **Exécution de la requête** > **SQL Server** > **Avancé**.   
4.  Sélectionnez ou désélectionnez **Activer le paramétrage d’Always Encrypted**.   
5.  Cliquez sur **OK**.   

Si vous exécutez une requête dans une fenêtre de l’éditeur de requête qui utilise une connexion de base de données avec Always Enabled activé, mais que le paramétrage n’est pas activé pour la fenêtre de l’éditeur de requête, vous serez invité à l’activer.   
>   [!NOTE]   
>   Le paramétrage d’Always Encrypted fonctionne uniquement dans les fenêtres de l’éditeur de requête qui utilisent des connexions de base de données avec Always Encrypted activé (voir [Activation et désactivation d’Always Encrypted pour une base de données](#en-dis)). Aucune variable Transact-SQL n’est paramétrée si la fenêtre d’éditeur de requête utilise une connexion de base de données sans Always Encrypted activé.   

#### <a name="how-parameterization-for-always-encrypted-works"></a>Fonctionnement du paramétrage d’Always Encrypted   

Si le paramétrage d’Always Encrypted et le comportement Always Encrypted dans la connexion de base de données sont tous deux activés pour une fenêtre de l’éditeur de requête, SQL Server Management Studio tentera de paramétrer des variables Transact-SQL répondant aux conditions préalables suivantes :    
- Variables déclarées et initialisées dans la même instruction (initialisation inline). Les variables déclarées avec des instructions `SET` séparées ne seront pas paramétrées.   
- Variables initialisées avec un littéral unique. Les variables initialisées avec des expressions contenant des opérateurs ou fonctions ne seront pas paramétrées.      

Vous trouverez ci-après des exemples de variables paramétrées par SQL Server Management Studio.   
```sql
DECLARE @SSN char(11) = '795-73-9838';
   
DECLARE @BirthDate date = '19990104';
DECLARE @Salary money = $30000;
```

Et voici quelques exemples de variables que SQL Server Management Studio ne tentera pas de paramétrer :   
```sql
DECLARE @Name nvarchar(50); --Initialization seperate from declaration
SET @Name = 'Abel';
   
DECLARE @StartDate date = GETDATE(); -- a function used instead of a literal
   
DECLARE @NewSalary money = @Salary * 1.1; -- an expression used instead of a literal
```
 
Pour qu’une tentative de paramétrage aboutisse :   
- Le type de littéral utilisé pour l’initialisation de la variable à paramétrer doit correspondre au type de la déclaration de la variable.   
- Si le type déclaré de la variable est un type de date ou d’heure, la variable doit être initialisée à l’aide d’une chaîne à l’aide d’un des formats suivants conformes à la norme ISO 8601.   

Voici des exemples de déclarations de variables Transact-SQL générant des erreurs de paramétrage :   
```sql
DECLARE @BirthDate date = '01/04/1999' -- unsupported date format   
   
DECLARE @Number int = 1.1 -- the type of the literal does not match the type of the variable   
```
SQL Server Management Studio utilise la technologie Intellisense pour vous indiquer quelles variables peuvent être paramétrées avec succès et quelles tentatives de paramétrage échouent (et pourquoi).   

Une déclaration de variable pouvant être paramétrée avec succès est marquée avec un trait de soulignement d’avertissement dans l’éditeur de requête. Si vous placez le curseur sur une instruction de déclaration marquée avec un trait de soulignement d’avertissement, vous verrez les résultats du processus de paramétrage, y compris les valeurs des propriétés clés de l’objet [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) résultant (sur lequel la variable est mappée) : [SqlDbType](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.sqldbtype.aspx), [Size](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.size.aspx), [Precision](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.precision.aspx), [Scale](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.scale.aspx)et [SqlValue](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.sqlvalue.aspx). Vous pouvez également voir la liste complète de toutes les variables correctement paramétrées dans l’onglet **Avertissement** de l’affichage **Liste d’erreurs** . Pour ouvrir l’affichage **Liste d’erreurs** , sélectionnez **Affichage** à partir du menu principal, puis sélectionnez **Liste d’erreurs**.    

Si SQL Server Management Studio a tenté de paramétrer une variable, mais que le paramétrage a échoué, la déclaration de la variable sera marquée avec un soulignement d’erreur. Si vous placez le curseur sur l’instruction de déclaration marquée avec un soulignement d’erreur, vous obtiendrez des informations sur l’erreur. Vous pouvez également voir la liste complète des erreurs de paramétrage pour toutes les variables dans l’onglet **Erreur** de l’affichage **Liste d’erreurs** . Pour ouvrir l’affichage **Liste d’erreurs** , sélectionnez **Affichage** à partir du menu principal, puis sélectionnez **Liste d’erreurs**.   

La capture d’écran ci-dessous montre un exemple de six déclarations de variable. SQL Server Management Studio a paramétré avec succès les trois premières variables. Les trois dernières variables n’ont pas respecté les conditions préalables pour le paramétrage, et par conséquent, SQL Server Management Studio n’a pas tenté de les paramétrer (les déclarations ne sont pas marquées d’une quelconque façon).   

![always-encrypted-parameter-warnings](../../../relational-databases/security/encryption/media/always-encrypted-parameter-warnings.png)
 
Un autre exemple ci-dessous montre deux variables qui répondent aux conditions préalables pour le paramétrage, mais la tentative de paramétrage a échoué, car les variables sont initialisées de manière incorrecte.    
 
![always-encrypted-error](../../../relational-databases/security/encryption/media/always-encrypted-error.png)
 
>   [!NOTE]
>   Étant donné que Always Encrypted prend en charge un sous-ensemble limité de conversions de type, dans de nombreux cas, il est nécessaire que le type de données d’une variable Transact-SQL soit identique au type de colonne de base de données ciblée. Par exemple, en supposant que le type de la colonne `SSN` dans la `Patients` table est `char(11)`, la requête ci-après échoue, dans la mesure où le type de la variable `@SSN` ( `nchar(11)`), ne correspond pas au type de la colonne.   

```sql
DECLARE @SSN nchar(11) = '795-73-9838'
SELECT * FROM [dbo].[Patients]
WHERE [SSN] = @SSN;
```

    Msg 402, Level 16, State 2, Line 5   
    The data types char(11) encrypted with (encryption_type = 'DETERMINISTIC', 
    encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
    column_encryption_key_database_name = 'Clinic') collation_name = 'Latin1_General_BIN2' 
    and nchar(11) encrypted with (encryption_type = 'DETERMINISTIC', 
    encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', 
    column_encryption_key_database_name = 'Clinic') are incompatible in the equal to operator.

>   [!NOTE]
>   Sans paramétrage, la requête entière, y compris les conversions de type, est traitée à l’intérieur de SQL Server/Azure SQL Database. Avec un paramétrage activé, certaines conversions de type sont effectuées par .NET Framework au sein de SQL Server Management Studio. En raison des différences entre le système .NET Framework et le système SQL Server (par exemple, précision différente de certains types, comme float), une requête exécutée avec paramétrage activé peut produire des résultats différents de ceux d’une requête exécutée sans paramétrage activé. 

#### <a name="permissions"></a>Autorisations      

Pour exécuter des requêtes sur des colonnes chiffrées, y compris des requêtes qui extraient des données en texte chiffré, vous avez besoin des autorisations `VIEW ANY COLUMN MASTER KEY DEFINITION` et `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` dans la base de données.   
Outre les autorisations ci-dessus, pour déchiffrer des résultats de requête ou pour chiffrer les paramètres de requête (générés par le paramétrage de variables Transact-SQL), vous devez également accéder à la clé principale de colonne protégeant les colonnes cibles :   
- **Magasin de certificats – Ordinateur local** Vous devez avoir un accès `Read` au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.   
- **Azure Key Vault** Vous avez besoin des autorisations `get`, `unwrapKey`et verify sur le coffre contenant la clé principale de colonne.   
- **Fournisseur du magasin de clés (CNG)** Vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.   
- **Fournisseur de services de chiffrement (CAPI)** Vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.   

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).


<a name="provisioncmk"></a>
## <a name="provisioning-column-master-keys-new-column-master-key"></a>Mise en service des clés principales de colonne (Nouvelle clé principale de colonne)

La boîte de dialogue **Nouvelle clé principale de colonne** vous permet de générer une clé principale de colonne ou de choisir une clé existante dans un magasin de clés, puis de créer des métadonnées de clé principale de colonne pour la clé créée ou sélectionnée dans la base de données.

1.  À l’aide de **l’Explorateur d’objets**, accédez au dossier **Sécurité>Clés Always Encrypted** sous votre base de données.
2.  Cliquez avec le bouton droit sur le dossier **Clés principales de colonne** et sélectionnez **Nouvelle clé principale de colonne...**. 
3.  Dans la boîte de dialogue **Nouvelle clé principale de colonne** , entrez le nom de l’objet de métadonnées de clé principale de colonne.
4.  Sélectionnez un magasin de clés :
    - **Magasin de certificats – Utilisateur actuel** : indique l’emplacement du magasin de certificats de l’utilisateur actuel dans le magasin de certificats Windows, qui est votre magasin personnel. 
    - **Magasin de certificats – Ordinateur local** : indique l’emplacement du magasin de certificats de l’ordinateur local dans le magasin de certificats Windows. 
    - **Azure Key Vault** : vous devez vous connecter à Azure (cliquez sur **Connexion**). Une fois que vous êtes connecté, vous pouvez choisir l’un de vos abonnements Azure et un coffre de clés.
    - **Fournisseur du magasin de clés (CNG)** : indique un magasin de clés qui est accessible via un fournisseur KSP qui implémente l’API CNG (Cryptography Next Generation). En règle générale, ce type de magasin est un module de sécurité matériel. Après avoir sélectionné cette option, vous devez choisir un fournisseur KSP. Le**fournisseur de stockage de clés des logiciels Microsoft** est sélectionné par défaut. Si vous souhaitez utiliser une clé principale de colonne stockée dans un module de sécurité matériel, sélectionnez un fournisseur KSP pour votre appareil (il doit être installé et configuré sur l’ordinateur avant d’ouvrir la boîte de dialogue).
    -   **Fournisseur de services de chiffrement (CAPI)** : magasin de clés qui est accessible via un fournisseur de services de chiffrement (CSP) qui implémente l’API de chiffrement (CAPI). En règle générale, un tel magasin est un module de sécurité matériel. Après avoir sélectionné cette option, vous devez choisir un fournisseur CSP.  Si vous souhaitez utiliser une clé principale de colonne stockée dans un module de sécurité matériel, sélectionnez un fournisseur CSP pour votre appareil (il doit être installé et configuré sur l’ordinateur avant d’ouvrir la boîte de dialogue).
    
    >   [!NOTE]
    >   CAPI étant une API déconseillée, l’option Fournisseur de services de chiffrement (CAPI) est désactivée par défaut. Vous pouvez l’activer en créant la valeur DWORD Fournisseur CAPI activé sous la clé **[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\sql13\Tools\Client\Always Encrypted]** dans le Registre Windows et en lui affectant la valeur 1. Vous devez utiliser CNG au lieu de CAPI, sauf si votre magasin de clés ne prend pas en charge CNG.
   
    Pour plus d’informations sur les magasins de clés ci-dessus, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

5.  Choisissez une clé existante dans votre magasin de clés, ou cliquez sur le bouton **Générer une clé** ou **Générer un certificat** pour créer une clé dans le magasin de clés. 
6.  Cliquez sur **OK** et la nouvelle clé apparaît dans la liste. 

SQL Server Management Studio crée des métadonnées pour votre clé principale de colonne dans la base de données. La boîte de dialogue effectue cette opération en générant et en émettant une instruction [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) .


<a name="provisioncek"></a> 
## <a name="provisioning-column-encryption-keys-new-column-encryption-key"></a>Mise en service des clés de chiffrement de colonne (Nouvelle clé de chiffrement de colonne)

La boîte de dialogue **Nouvelle clé de chiffrement de colonne** vous permet de générer une clé de chiffrement de colonne, de la chiffrer avec une clé principale de colonne et de créer des métadonnées de clé de chiffrement de colonne dans la base de données.

1.  À l’aide de **l’Explorateur d’objets**, accédez au dossier **Sécurité/Clés Always Encrypted** sous votre base de données.
2.  Cliquez avec le bouton droit sur le dossier **Clés de chiffrement de colonne** et sélectionnez **Nouvelle clé de chiffrement de colonne…**. 
3.  Dans la boîte de dialogue **Nouvelle clé de chiffrement de colonne** , entrez le nom de l’objet de métadonnées de clé de chiffrement de colonne.
4.  Sélectionnez un objet de métadonnées qui représente votre clé principale de colonne dans la base de données.
5.  Cliquez sur **OK**. 


SQL Server Management Studio génère une clé de chiffrement de colonne, puis récupère les métadonnées pour la clé principale de colonne sélectionnée à partir de la base de données. SQL Server Management Studio utilise ensuite les métadonnées de clé principale de colonne pour contacter le magasin de clés qui contient votre clé principale de colonne et chiffrer la clé de chiffrement de colonne. Enfin, les métadonnées de la nouvelle clé de chiffrement de colonne sont créées dans la base de données. La boîte de dialogue effectue cette opération en générant et en émettant une instruction [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) .

### <a name="permissions"></a>Autorisations

Vous avez besoin des autorisations relatives à la base de données *ALTER ANY ENCRYPTION MASTER KEY* et *VIEW ANY COLUMN MASTER KEY DEFINITION* dans la base de données pour que la boîte de dialogue crée les métadonnées de clé de chiffrement de colonne et accède aux métadonnées de clé principale de colonne.
Pour accéder à un magasin de clés et utiliser la clé principale de colonne, vous pouvez avoir besoin d’autorisations sur le magasin de clés et/ou la clé :
- **Magasin de certificats – Ordinateur local** : vous devez avoir un accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : vous avez besoin des autorisations *get*, *unwrapKey*, *wrapKey*, *sign*et *verify*  sur le coffre contenant la clé principale de colonne.
- **Fournisseur du magasin de clés (CNG)** : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)** : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

<a name="rotatecmk"></a>
## <a name="rotating-column-master-keys"></a>Permutation des clés principales de colonne

La permutation d’une clé principale de colonne est le processus de remplacement d’une clé principale de colonne existante par une nouvelle clé principale de colonne. Vous devrez peut-être permuter une clé si elle a été compromise, ou pour vous conformer aux stratégies ou aux réglementations de conformité de votre organisation qui régissent la permutation périodique des clés de chiffrement. Une permutation des clés principales de colonne implique le déchiffrement de clés de chiffrement de colonne qui sont protégées avec la clé principale de colonne active, leur rechiffrement à l’aide de la nouvelle clé principale de colonne et la mise à jour des métadonnées de clé. Pour plus d’informations, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

**Étape 1: Configurer une nouvelle clé principale de colonne**

Mettez en service une nouvelle clé principale de colonne, en suivant les étapes décrites dans la section Mise en service des clés principales de colonne ci-dessus.

**Étape 2 : Chiffrer les clés de chiffrement de colonne avec la nouvelle clé principale de colonne**

Une clé principale de colonne protège généralement une ou plusieurs clés de chiffrement de colonne. Chaque clé de chiffrement de colonne a une valeur chiffrée, stockée dans la base de données, qui est le produit du chiffrement de la clé de chiffrement de colonne avec la clé principale de colonne.
Dans cette étape, chiffrez chacune des clés de chiffrement de colonne qui sont protégées par la clé principale de colonne que vous permutez avec la nouvelle clé principale de colonne, puis stockez la nouvelle valeur chiffrée dans la base de données. Par conséquent, chaque clé de chiffrement de colonne affectée par la permutation comporte deux valeurs chiffrées : une valeur chiffrée avec la clé principale de colonne existante et une nouvelle valeur chiffrée avec la nouvelle clé principale de colonne.

1.  Dans **l’Explorateur d’objets**, accédez au dossier **Sécurité>Clés Always Encrypted>Clés principales de colonne**, puis recherchez la clé principale de colonne que vous permutez.
2.  Cliquez avec le bouton droit sur la clé principale de colonne, puis sélectionnez **Permuter**.
3.  Dans la boîte de dialogue **Permutation de la clé principale de colonne** , sélectionnez le nom de la nouvelle clé principale de colonne que vous avez créée à l’étape 1, dans le champ **Cible** .
4.  Consultez la liste des clés de chiffrement de colonne protégées par les clés principales de colonne existantes. Ces clés sont affectées par la permutation.
5.  Cliquez sur **OK**.

SQL Server Management Studio obtient les métadonnées des clés de chiffrement de colonne qui sont protégées avec l’ancienne clé principale de colonne ainsi que les métadonnées de l’ancienne et de la nouvelle clés principales de colonne. SSMS utilise ensuite les métadonnées de clé principale de colonne pour accéder au magasin de clés contenant l’ancienne clé principale de colonne et déchiffre les clés de chiffrement de colonne. Par la suite, SSMS accède au magasin de clés contenant la nouvelle clé principale de colonne pour produire un nouvel ensemble de valeurs chiffrées des clés de chiffrement de colonne, puis ajoute les nouvelles valeurs aux métadonnées (en générant et en émettant des instructions [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) ).

> [!NOTE]
> Vérifiez qu’aucune des clés de chiffrement de colonne, chiffrées avec l’ancienne clé principale de colonne, n’est chiffrée avec toute autre clé principale de colonne. En d’autres termes, chaque clé de chiffrement de colonne affectée par la permutation doit avoir précisément une valeur chiffrée dans la base de données. Si une clé de chiffrement de colonne concernée a plusieurs valeurs chiffrées, vous devez supprimer les valeurs superflues avant de passer à la permutation (consultez *l’étape 4* sur la suppression d’une valeur chiffrée d’une clé de chiffrement de colonne).

**Étape 3: Configurer vos applications avec la nouvelle clé principale de colonne**

Dans cette étape, vous devez vérifier que toutes vos applications clientes qui interrogent des colonnes de base de données protégées par la clé principale de colonne en permutation (c’est-à-dire les colonnes de base de données chiffrées avec une clé de chiffrement de colonne elle-même chiffrée avec la clé principale de colonne en permutation) peuvent accéder à la nouvelle clé principale de colonne. Cette étape dépend du type de magasin de clés dans lequel est stockée votre nouvelle clé principale de colonne. Exemple :
- Si la nouvelle clé principale de colonne est un certificat stocké dans le magasin de certificats Windows, vous devez déployer le certificat au même emplacement de magasin de certificats (*Utilisateur actuel* ou *Ordinateur local*) en tant qu’emplacement spécifié dans le chemin d’accès à la clé de votre clé principale de colonne dans la base de données. L’application doit être en mesure d’accéder au certificat :
    - Si le certificat est stocké à l’emplacement du magasin de certificats *Utilisateur actuel* , il doit être importé dans le magasin de l’utilisateur actuel de l’identité Windows de l’application (utilisateur).
    - Si le certificat est stocké à l’emplacement du magasin de certificats *Ordinateur local* , l’identité Windows de l’application doit disposer d’une autorisation d’accès au certificat.
- Si la nouvelle clé principale de colonne est stockée dans Microsoft Azure Key Vault, l’application doit être implémentée afin de pouvoir s’authentifier auprès d’Azure et a l’autorisation d’accéder à la clé.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

> [!NOTE]
> À ce stade de la permutation, l’ancienne clé principale de colonne et la nouvelle clé principale de colonne sont valides et peuvent être utilisées pour accéder aux données.

**Étape 4 : Nettoyer les valeurs clés de chiffrement de colonne chiffrées avec l’ancienne clé principale de colonne**

Après avoir configuré toutes vos applications pour utiliser la nouvelle clé principale de colonne, supprimez de la base de données les valeurs des clés de chiffrement de colonne chiffrées avec *l’ancienne* clé principale de colonne. La suppression des anciennes valeurs garantit que vous êtes prêt pour la permutation suivante (rappelez-vous que chaque clé de chiffrement de colonne, protégée par une clé principale de colonne à permuter, doit avoir précisément une valeur chiffrée).

Une autre raison de nettoyer l’ancienne valeur avant l’archivage ou la suppression de l’ancienne clé principale de colonne a trait aux performances ; quand vous interrogez une colonne chiffrée, un pilote de client prenant en charge Always Encrypted risque de devoir tenter de déchiffrer deux valeurs : l’ancienne et la nouvelle. Le pilote ignore laquelle des deux clés principales de colonne est valide dans l’environnement de l’application et il récupère donc les deux valeurs chiffrées à partir du serveur. En cas d’échec du déchiffrement de l’une des valeurs, parce qu’elle est protégée par la clé principale de colonne non disponible (par exemple, si l’ancienne clé principale de colonne a été supprimée de magasin ), le pilote tente de déchiffrer une autre valeur à l’aide de la nouvelle clé principale de colonne.

> [!WARNING]
> Si vous supprimez la valeur d’une clé de chiffrement de colonne avant que sa clé principale de colonne correspondante ne soit devenue disponible pour une application, l’application ne peut plus déchiffrer la colonne de base de données.

1.  Dans **l’Explorateur d’objets**, accédez au dossier **Sécurité>Clés Always Encrypted**, puis recherchez la clé principale de colonne existante à remplacer.
2.  Cliquez avec le bouton droit sur la clé principale de colonne existante, puis sélectionnez **Nettoyage**.
3.  Passez en revue la liste des valeurs clés de chiffrement de colonne à supprimer.
4.  Cliquez sur **OK**.

SQL Server Management Studio émet des instructions [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) pour supprimer les valeurs chiffrées des clés de chiffrement de colonne chiffrées avec l’ancienne clé principale de colonne.

**Étape 5 : Supprimer les métadonnées de votre ancienne clé principale de colonne**

Si vous choisissez de supprimer la définition de l’ancienne clé principale de colonne de la base de données, procédez comme suit. 
1.  Dans **l’Explorateur d’objets**, accédez au dossier **Sécurité>Clés Always Encrypted>Clés principales de colonne**, puis recherchez l’ancienne clé principale de colonne à supprimer de la base de données.
2.  Cliquez avec le bouton droit sur l’ancienne clé principale de colonne, puis sélectionnez **Supprimer**. (Cela génère et émet une instruction [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md) pour supprimer les métadonnées de clé principale de colonne.)
3.  Cliquez sur **OK**.

> [!NOTE]
> Nous vous recommandons vivement de ne pas supprimer définitivement l’ancienne clé principale de colonne après la permutation. Au lieu de cela, laissez l’ancienne clé principale de colonne dans son magasin de clés actuel ou archivez-la dans un autre emplacement sécurisé. Si vous restaurez votre base de données à partir d’un fichier de sauvegarde à un point dans le temps avant la configuration de la nouvelle clé principale de colonne, vous aurez besoin de l’ancienne clé pour accéder aux données.

### <a name="permissions"></a>Autorisations

La permutation d’une clé principale de colonne nécessite les autorisations relatives à la base de données suivantes :

- **ALTER ANY COLUMN MASTER KEY** : obligatoire pour créer des métadonnées pour la nouvelle clé principale de colonne et supprimer les métadonnées pour l’ancienne clé principale de colonne.
- **ALTER ANY COLUMN ENCRYPTION KEY** : obligatoire pour modifier les métadonnées de clé de chiffrement de colonne (ajouter les nouvelles valeurs chiffrées).
- **VIEW ANY COLUMN MASTER KEY DEFINITION** : obligatoire pour accéder aux métadonnées des clés principales de colonne et les lire.
- **VIEW ANY COLUMN ENCRYPTION KEY DEFINITION** : obligatoire pour accéder aux métadonnées des clés de chiffrement de colonne et les lire. 

Vous devez également être en mesure d’accéder à l’ancienne clé principale de colonne et à la nouvelle clé principale de colonne dans leurs magasins de clés. Pour accéder à un magasin de clés et utiliser une clé principale de colonne, vous pouvez avoir besoin d’autorisations sur le magasin de clés et/ou la clé :
- **Magasin de certificats – Ordinateur local** : vous devez avoir un accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : vous avez besoin des autorisations *create*, *get*, *unwrapKey*, *wrapKey*, *sign*et *verify* sur le coffre contenant les clés principales de colonne.
- **Fournisseur du magasin de clés (CNG)** : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)** : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

<a name="rotatecek"></a> 
## <a name="rotating-column-encryption-keys"></a>Permutation des clés de chiffrement de colonne

La permutation d’une clé de chiffrement de colonne implique le déchiffrement des données dans toutes les colonnes chiffrées avec la clé à permuter et le rechiffrement des données à l’aide de la nouvelle clé de chiffrement de colonne.

>[!NOTE]
> La permutation d’une clé de chiffrement de colonne peut prendre beaucoup de temps si les tables qui contiennent les colonnes chiffrées avec la clé soumise à la permutation sont volumineuses. Pendant le rechiffrement des données, vos applications ne peuvent pas écrire dans les tables concernées. Votre organisation doit donc apporter un soin particulier à la planification de la permutation des clés de chiffrement de colonne.
Pour permuter une clé de chiffrement de colonne, utilisez l’Assistant Always Encrypted.

1.  Ouvrez l’Assistant pour votre base de données : cliquez avec le bouton droit sur votre base de données, pointez sur **Tâches**, puis cliquez sur **Chiffrer les colonnes**.
2.  Examinez la page **Introduction** , puis cliquez sur **Suivant**.
3.  Dans la page **Sélection de la colonne** , développez les tables et recherchez toutes les colonnes à remplacer qui sont actuellement chiffrés avec l’ancienne clé de chiffrement de colonne.
4.  Pour chaque colonne chiffrée avec l’ancienne clé de chiffrement de colonne, affectez à **Clé de chiffrement** une nouvelle clé générée automatiquement. **Remarque :** Vous pouvez également créer une clé de chiffrement de colonne avant d’exécuter l’Assistant ; consultez la section *Mise en service des clés de chiffrement de colonne* ci-dessus.
5.  Dans la page **Configuration de la clé principale** , sélectionnez un emplacement pour stocker la nouvelle clé et une source de clé principale, puis cliquez sur **Suivant**. **Remarque :** Si vous utilisez une clé de chiffrement de colonne existante (et non générée automatiquement), aucune action ne doit être effectuée dans cette page.
6.  Dans la page **Validation**, indiquez si vous souhaitez exécuter le script immédiatement ou créer un script PowerShell, puis cliquez sur **Suivant**.
7.  Dans la page **Résumé** , passez en revue les options que vous avez sélectionnées, puis cliquez sur **Terminer** et fermez l’Assistant à la fin.
8.  Dans **l’Explorateur d’objets**, accédez au dossier **Sécurité/Clés Always Encrypted/Clés de chiffrement de colonne** , puis recherchez l’ancienne clé de chiffrement de colonne à supprimer de la base de données. Cliquez avec le bouton droit sur la clé et sélectionnez **Supprimer**.

### <a name="permissions"></a>Autorisations

La permutation d’une clé de chiffrement de colonne nécessite les autorisations relatives à la base de données suivantes : **ALTER ANY COLUMN MASTER KEY** : obligatoire si vous utilisez une nouvelle clé de chiffrement de colonne générée automatiquement (une clé principale de colonne et ses métadonnées sont également générées).
**ALTER ANY COLUMN ENCRYPTION KEY** : obligatoire pour ajouter des métadonnées pour la nouvelle clé de chiffrement de colonne.
**VIEW ANY COLUMN MASTER KEY DEFINITION** : obligatoire pour accéder aux métadonnées des clés principales de colonne et les lire.
**VIEW ANY COLUMN ENCRYPTION KEY DEFINITION** : obligatoire pour accéder aux métadonnées des clés de chiffrement de colonne et les lire.

Vous devez également être en mesure d’accéder aux clés principales de colonne pour l’ancienne et la nouvelle clés de chiffrement de colonne. Pour accéder à un magasin de clés et utiliser une clé principale de colonne, vous pouvez avoir besoin d’autorisations sur le magasin de clés et/ou la clé :
- **Magasin de certificats – Ordinateur local** : vous devez avoir l’accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : vous avez besoin des autorisations get, unwrapKey et verify sur le coffre contenant la clé principale de colonne.
- **Fournisseur du magasin de clés (CNG)** : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)** : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="performing-dac-upgrade-operations-when-database-or-dacpac-uses-always-encrypted"></a>Exécution d’opérations de mise à niveau de la DAC quand la base de données ou le fichier DACPAC utilise Always Encrypted

Les[opérations DAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_3) sont prises en charge sur les fichiers DACPAC et les bases de données avec des schémas contenant des colonnes chiffrées. Des considérations particulières s’appliquent à l’opération de mise à niveau de la DAC. Consultez [Mettre à niveau une application de la couche Données](../../../relational-databases/data-tier-applications/upgrade-a-data-tier-application.md) pour savoir comment effectuer une opération de mise à niveau de la DAC dans différents outils, notamment SSMS. 

Quand vous mettez à niveau une base de données à l’aide d’un fichier DACPAC et que le fichier DACPAC ou la base de données cible a des colonnes chiffrées, l’opération de mise à niveau déclenche une opération de chiffrement de données si toutes les conditions suivantes sont remplies :
- La base de données contient une colonne avec des données.
- La même colonne existe dans le fichier DACPAC.
- La configuration de chiffrement de la colonne dans la base de données est différente de la configuration de la colonne correspondante dans le fichier DACPAC. Consultez le tableau ci-dessous pour plus d’informations.

| Condition|Action|
|:---|:---|
|La colonne est chiffrée dans le fichier DACPAC et n’est pas chiffrée dans la base de données.| Les données de la colonne sont chiffrées.|
|La colonne n’est pas chiffrée dans le fichier DACPAC et est chiffrée dans la base de données.| Les données de la colonne sont déchiffrées (le chiffrement est supprimé de la colonne).|
| La colonne est chiffrée à la fois dans le fichier DACPAC et la base de données, mais la colonne dans le fichier DACPAC utilise un type de chiffrement et/ou une clé de chiffrement de colonne autre que la colonne correspondante dans la base de données.|Les données de la colonne sont déchiffrées, puis rechiffrées pour correspondre à la configuration de chiffrement dans le fichier DACPAC.|

> [!NOTE]
> Si la clé principale de colonne configurée pour la colonne dans la base de données ou le fichier DACPAC est stockée dans Azure Key Vault, vous êtes invité à vous connecter à Azure (si ce n’est pas déjà fait).

### <a name="permissions"></a>Autorisations

Pour effectuer une opération de mise à niveau de la DAC si Always Encrypted est configuré dans le fichier DACPAC ou dans la base de données cible, vous pouvez avoir besoin de certaines ou de l’ensemble des autorisations ci-dessous, en fonction des différences entre le schéma dans le fichier DACPAC et le schéma de la base de données cible.

*ALTER ANY COLUMN MASTER KEY*, *ALTER ANY COLUMN ENCRYPTION KEY*, *VIEW ANY COLUMN MASTER KEY DEFINITION*, *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION*

Si l’opération de mise à niveau déclenche une opération de chiffrement de données, vous devez également être en mesure d’accéder aux clés principales de colonne configurées pour les colonnes concernées :
- **Magasin de certificats – Ordinateur local** : vous devez avoir un accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : vous avez besoin des autorisations *create*, *get*, *unwrapKey*, *wrapKey*, *sign*et *verify* sur le coffre contenant la clé principale de colonne.
- **Fournisseur du magasin de clés (CNG)** : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)** : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="migrating-databases-with-encrypted-columns-using-bacpac"></a>Migration de bases de données avec des colonnes chiffrées à l’aide d’un fichier BACPAC

Quand vous exportez une base de données, toutes les données stockées dans les colonnes chiffrées sont récupérées et placées dans le fichier [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) obtenu (sous forme chiffrée). Le fichier BACPAC obtenu contient également les métadonnées pour les clés Always Encrypted.

Quand vous importez le fichier BACPAC dans une base de données, les données chiffrées dans le fichier BACPAC sont chargées dans la base de données et les métadonnées de clé Always Encrypted sont recréées.

Si vous avez une application qui est configurée pour modifier ou récupérer les données chiffrées stockées dans la base de données source (la base de données exportée), vous n’avez rien à faire de spécial pour que l’application puisse interroger les données chiffrées dans la base de données cible, comme les clés dans les deux bases de données sont identiques.


### <a name="permissions"></a>Autorisations

Vous avez besoin des autorisations *ALTER ANY COLUMN MASTER KEY* et *ALTER ANY COLUMN ENCRYPTION KEY* sur la base de données source. Vous avez besoin des autorisations *ALTER ANY COLUMN MASTER KEY*, *ALTER ANY COLUMN ENCRYPTION KEY*, *VIEW ANY COLUMN MASTER KEY DEFINITION*et *VIEW ANY COLUMN ENCRYPTION* sur la base de données cible.

## <a name="migrating-databases-with-encrypted-columns-using-sql-server-import-and-export-wizard"></a>Migration de bases de données avec des colonnes chiffrées à l’aide de l’Assistant Importation et Exportation SQL Server

Par rapport à l’utilisation des fichiers BACPAC, [l’Assistant Importation et Exportation SQL Server](~/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md) vous donne davantage de contrôle sur la gestion des données stockées dans les colonnes chiffrées lors de la migration de données.

- Si votre source de données est une base de données qui utilise Always Encrypted, vous pouvez configurer votre connexion de source de données afin que les données stockées dans les colonnes chiffrées soient déchiffrées durant l’opération d’exportation, ou restent chiffrées.
- Si votre cible de données est une base de données qui utilise Always Encrypted, vous pouvez configurer votre connexion de cible de données afin que les données ciblant les colonnes chiffrées soient chiffrées.

Pour permettre le déchiffrement (pour la source de données) ou le chiffrement (pour la cible de données), vous devez configurer votre connexion de source/cible de données pour utiliser le [fournisseur de données .NET Framework pour SQL Server](https://msdn.microsoft.com/library/system.data.sqlclient.aspx) et vous devez affecter aux mots clés de chaîne de connexion Paramètre de chiffrement de colonne la valeur *Activé*.

Le tableau ci-dessous répertorie les scénarios de migration possibles et comment ils se rapportent à Always Encrypted, ainsi que la configuration de source de données et de cible de données pour chaque connexion.

| Scénario|Configuration de la connexion source| Configuration de la connexion cible
|:---|:---|:---
|Chiffrer les données lors de la migration (les données sont stockées en texte clair dans la source de données et sont migrées vers les colonnes chiffrées dans la cible de données).| Fournisseur de données/pilote : *tout*<br><br>Paramètre de chiffrement de colonne = Désactivé<br><br>(si le fournisseur de données .NET Framework pour SQL Server et .NET Framework 4.6 ou version ultérieure sont utilisés.) | Fournisseur de données/pilote : *Fournisseur de données .NET Framework pour SQL Server* (.NET Framework 4.6 ou version ultérieure nécessaire)<br><br>Paramètre de chiffrement de colonne = Activé
| Déchiffrer les données lors de la migration (les données sont stockées dans des colonnes chiffrées dans la source de données et migrées en texte clair vers la cible de données ; si la cible de données est une base de données, les colonnes cibles ne sont pas chiffrées).<br><br>**Remarque :** Les tables cibles avec les colonnes chiffrées doivent exister avant la migration.|Fournisseur de données/pilote : *Fournisseur de données .NET Framework pour SQL Server* (.NET Framework 4.6 ou version ultérieure nécessaire)<br><br>Paramètre de chiffrement de colonne = Activé|Fournisseur de données/pilote : *tout*<br><br>Paramètre de chiffrement de colonne = Désactivé<br><br>(si le fournisseur de données .NET Framework pour SQL Server et .NET Framework 4.6 ou version ultérieure sont utilisés.)
|Rechiffrer les données lors de la migration (les données sont stockées dans des colonnes chiffrées dans la source de données et migrées en texte clair vers la cible de données des colonnes qui utilisent d’autres types de chiffrement de clés de chiffrement de colonne).<br><br>**Remarque :** Les tables cibles avec les colonnes chiffrées doivent exister avant la migration.| Fournisseur de données/pilote : *Fournisseur de données .NET Framework pour SQL Server* (.NET Framework 4.6 ou version ultérieure nécessaire)<br><br>Paramètre de chiffrement de colonne = Activé|Fournisseur de données/pilote : *Fournisseur de données .NET Framework pour SQL Server* (.NET Framework 4.6 ou version ultérieure nécessaire)<br><br>Paramètre de chiffrement de colonne = Activé
|Déplacer les données chiffrées sans les déchiffrer.<br><br>**Remarque :** Les tables cibles avec les colonnes chiffrées doivent exister avant la migration.| Fournisseur de données/pilote : *tout*<br>Paramètre de chiffrement de colonne = Désactivé<br><br>(si le fournisseur de données .NET Framework pour SQL Server et .NET Framework 4.6 ou version ultérieure sont utilisés.)| Fournisseur de données/pilote : *tout*<br>Paramètre de chiffrement de colonne = Désactivé<br><br>(si le fournisseur de données .NET Framework pour SQL Server et .NET Framework 4.6 ou version ultérieure sont utilisés.)<br><br>L’option ALLOW_ENCRYPTED_VALUE_MODIFICATIONS doit être activée pour l’utilisateur.<br><br>Pour plus d’informations, consultez [Migrer des données sensibles protégées par Always Encrypted](../../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).


### <a name="permissions"></a>Autorisations

Pour **chiffrer** ou **déchiffrer** des données stockées dans la source de données, vous avez besoin des autorisations *VIEW ANY COLUMN MASTER KEY DEFINITION* et *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* dans la base de données source.

Vous devez également pouvoir accéder aux clés principales de colonne, configurées pour les colonnes, qui stockent les données que vous chiffrez ou déchiffrez :
- **Magasin de certificats – Ordinateur local** : vous devez avoir l’accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : vous avez besoin des autorisations get, unwrapKey, wrapKey, sign et verify sur le coffre contenant la clé principale de colonne.
- **Fournisseur du magasin de clés (CNG)** : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)** : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.
Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="see-also"></a> Voir aussi
- [Always Encrypted (moteur de base de données)](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Assistant Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-wizard.md)
- [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)
- [Always Encrypted (développement client)](../../../relational-databases/security/encryption/always-encrypted-client-development.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
- [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)












