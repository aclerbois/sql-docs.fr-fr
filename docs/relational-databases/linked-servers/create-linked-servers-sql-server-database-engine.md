---
title: Créer des serveurs liés (moteur de base de données SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 11/20/2015
ms.prod: sql
ms.prod_service: database-engine
ms.component: linked-servers
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.linkedserver.properties.general.f1
- sql13.swb.linkedserver.properties.security.f1
- sql13.swb.linkedserver.properties.provider.f1
- sql13.swb.linkedserver.properties.options.f1
helpviewer_keywords:
- linked servers [SQL Server], creating
ms.assetid: 3228065d-de8f-4ece-a9b1-e06d3dca9310
caps.latest.revision: 18
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 8c7dfed0144de73aa7bf84db9999e4b6a5aec6c8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-linked-servers-sql-server-database-engine"></a>Créer des serveurs liés (moteur de base de données SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
 > Pour accéder au contenu relatif aux versions précédentes de SQL Server, consultez [Créer des serveurs liés (moteur de base de données SQL Server)](https://msdn.microsoft.com/en-US/library/ff772782(SQL.120).aspx).

  Cette rubrique indique comment créer un serveur lié et accéder aux données provenant d'un autre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. En créant un serveur lié, vous pouvez utiliser des données provenant de plusieurs sources. Il n'est pas nécessaire que le serveur lié soit une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais il s'agit d'un scénario courant.  
  
##  <a name="Background"></a> Arrière-plan  
 Un serveur lié autorise l'accès à des sources de données OLE DB par l'intermédiaire de requêtes distribuées et hétérogènes. Une fois qu'un serveur lié a été créé, il est possible d'exécuter des requêtes distribuées sur ce serveur, et les requêtes peuvent joindre des tables de plusieurs sources de données. Si le serveur lié est défini comme une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les procédures stockées distantes peuvent être exécutées.  
  
 Les fonctions et les arguments requis du serveur lié peuvent considérablement varier. Cette rubrique fournit des exemples typiques, mais toutes les options ne sont pas décrites. Pour plus d’informations, consultez [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
##  <a name="Security"></a> Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Quand vous utilisez des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] , vous avez besoin de l’autorisation **ALTER ANY LINKED SERVER** sur le serveur ou de l’appartenance au rôle serveur fixe **setupadmin** . Quand vous utilisez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , vous avez besoin de l’autorisation **CONTROL SERVER** ou de l’appartenance au rôle serveur fixe **sysadmin** .  
  
##  <a name="Procedures"></a> Comment créer un serveur lié  
 Vous pouvez utiliser l'une des options suivantes :  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
##### <a name="to-create-a-linked-server-to-another-instance-of-sql-server-using-sql-server-management-studio"></a>Pour créer un serveur lié à une autre instance de Serveur SQL à l'aide de SQL Server Management Studio  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ouvrez l’Explorateur d’objets, développez **Objets serveur**, cliquez avec le bouton droit sur **Serveurs liés**, puis cliquez sur **Nouveau serveur lié**.  
  
2.  Sur la page **Général** , dans la zone **Serveur lié** , tapez le nom de l'instance de **SQL Server** associée au lien.  
  
     **SQL Server**  
     Permet d'identifier le serveur lié comme une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous utilisez cette méthode pour définir un serveur lié [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le nom spécifié dans **Serveur lié** doit correspondre au nom réseau du serveur. En outre, toutes les tables extraites à partir du serveur appartiennent à la base de données par défaut définie pour la connexion d'accès au serveur lié.  
  
     **Autre source de données**  
     Spécifiez un type de serveur OLE DB autre que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cliquer sur cette option active les options situées au-dessous.  
  
     **Fournisseur**  
     Sélectionnez une source de données OLE DB dans la zone de liste. Le fournisseur OLE DB est inscrit avec le PROGID donné dans le Registre.  
  
     **Nom de produit**  
     Tapez le nom de produit de la source de données OLE DB à ajouter en tant que serveur lié.  
  
     **Source de données**  
     Tapez le nom de la source de données tel qu'il est interprété par le fournisseur OLE DB. Si vous vous connectez à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], spécifiez le nom d'instance.  
  
     **Chaîne du fournisseur**  
     Tapez l'identificateur de programme unique (PROGID) du fournisseur OLE DB qui correspond à la source de données. Pour obtenir des exemples de chaînes de fournisseur valides, consultez [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
     **Emplacement**  
     Tapez l'emplacement de la base de données tel qu'il est interprété par le fournisseur OLE DB.  
  
     **Catalogue**  
     Tapez le nom du catalogue à utiliser lors de la connexion au fournisseur OLE DB.  
  
     Pour tester la capacité de connexion à un serveur lié dans l’Explorateur d’objets, cliquez avec le bouton droit sur le serveur lié, puis sélectionnez **Tester la connexion**.  
  
    > [!NOTE]  
    >  Si l'instance de **SQL Server** est l'instance par défaut, entrez le nom de l'ordinateur qui héberge l'instance de **SQL Server**. Si l’instance de **SQL Server** est une instance nommée, entrez le nom de l’ordinateur et le nom de l’instance, par exemple **Accounting\SQLExpress**.  
  
3.  Dans la zone **Type de serveur** , sélectionnez **SQL Server** pour indiquer que le serveur lié est une autre instance de **SQL Server**.  
  
4.  Dans la page **Sécurité** , spécifiez le contexte de sécurité qui sera utilisé lorsque le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'origine se connectera au serveur lié. Dans un environnement de domaine où les utilisateurs se connectent à l'aide de leurs connexions de domaine, sélectionner **Seront effectuées dans le contexte de sécurité de la connexion actuelle** est souvent le meilleur choix. Lorsque les utilisateurs se connectent au **SQL Server** d'origine en utilisant un compte de connexion **SQL Server** , le meilleur choix est souvent de sélectionner **Seront effectuées dans ce contexte de sécurité**, puis de fournir les informations d'identification nécessaires pour l'authentification sur le serveur lié.  
  
     **Connexion locale**  
     Affiche la connexion locale qui peut se connecter au serveur lié. La connexion locale peut être une connexion utilisant l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou une connexion utilisant l'authentification Windows. Utilisez cette liste pour restreindre la connexion à des connexions spécifiques ou pour autoriser certaines connexions à se connecter sous une connexion différente.  
  
     **Impersonate**  
     Permet de transmettre le nom d'utilisateur et le mot de passe de la connexion locale au serveur lié. Pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , une connexion possédant un nom et un mot de passe identiques doit exister sur le serveur distant. Pour les connexions Windows, la connexion doit être une connexion valide sur le serveur lié.  
  
     Pour utiliser l'emprunt d'identité, la configuration doit se conformer aux exigences relatives à la délégation.  
  
     **Utilisateur distant**  
     Utilisez l’utilisateur distant pour mapper les utilisateurs qui ne sont pas définis dans **Connexion locale**. L' **Utilisateur distant** doit être une connexion utilisant l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur distant.  
  
     **Mot de passe distant**  
     Spécifie le mot de passe de l'utilisateur distant.  
  
     **Ajouter**  
     Ajoute une nouvelle connexion locale.  
  
     **Supprimer**  
     Supprime une connexion locale existante.  
  
     **Ne seront pas effectuées**  
     Spécifie qu'une connexion ne sera pas établie pour les connexions non définies dans la liste.  
  
     **Seront effectuées sans contexte de sécurité**  
     Spécifie qu'une connexion sera établie sans utiliser de contexte de sécurité pour les connexions d'accès non définies dans la liste.  
  
     **Seront effectuées dans le contexte de sécurité de la connexion actuelle**  
     Spécifie qu'une connexion sera établie à l'aide du contexte de sécurité en cours de la connexion pour les connexions non définies dans la liste. Si vous vous connectez au serveur local en utilisant l'authentification Windows, vos informations d'identification Windows seront utilisées pour vos connexions au serveur distant. Si vous vous connectez au serveur local en utilisant l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le nom et le mot de passe de connexion seront utilisés pour vos connexions au serveur distant. Dans ce cas, une connexion possédant exactement les mêmes nom et mot de passe doit exister sur le serveur distant.  
  
     **Seront effectuées dans ce contexte de sécurité**  
     Spécifie qu’une connexion sera établie en utilisant la connexion d’accès et le mot de passe définis dans les zones **Ouverture de session à distance** et **Avec le mot de passe** pour les connexions non définies dans la liste. L'ouverture de session à distance doit être une connexion utilisant l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le serveur distant.  
  
5.  Éventuellement, pour afficher ou spécifier les options de serveur, cliquez sur la page **Options du serveur**  .  
  
     **Compatible avec le classement**  
     Concerne l'exécution des requêtes distribuées sur les serveurs liés. Si la valeur de cette option est true, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considère que tous les caractères du serveur lié sont compatibles avec le serveur local, en matière de jeu de caractères et d'ordre de classement (ou ordre de tri). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut alors envoyer au fournisseur des comparaisons sur les colonnes de caractères. Si cette option n'est pas activée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compare toujours les colonnes de caractères en local.  
  
     Cette option ne doit être définie que si la source de données correspondant au serveur lié possède le même jeu de caractères et respecte le même ordre de tri que le serveur local.  
  
     **Accès aux données**  
     Active ou désactive un serveur lié pour l'accès des requêtes distribuées.  
  
     **RPC**  
     Active l'appel de procédure à distance (RPC) à partir du serveur spécifié.  
  
     **Sortie RPC**  
     Active l'appel de procédure à distance (RPC) à destination du serveur spécifié.  
  
     **Utiliser le classement distant**  
     Détermine si le classement d'une colonne distante ou d'un serveur local doit être utilisé.  
  
     Si la valeur est true, le classement des colonnes distantes est utilisé pour les sources de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tandis que le classement spécifié dans collation name (Nom du classement) est utilisé pour les sources de données autres que[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Si la valeur est false, les requêtes distribuées utilisent toujours le classement par défaut du serveur local, tandis que collation name et le classement des colonnes distantes sont ignorés. La valeur par défaut est false.  
  
     **Nom du classement**  
     Spécifie le nom du classement utilisé par la source de données distante si use remote collation (Utiliser le classement distant) a la valeur true et si la source de données n'est pas une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le nom doit être l'un des classements pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Utilisez cette option lors d'un accès à une source de données OLE DB autre que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais dont le classement correspond à l'un des classements de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Le serveur lié doit prendre en charge un classement unique utilisable pour toutes les colonnes du serveur. Ne définissez pas cette option si le serveur lié prend en charge plusieurs classements dans une source de données unique ou si le classement du serveur lié ne correspond peut-être pas à l'un des classements de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     **Délai de connexion**  
     Valeur du délai d'expiration (en secondes) de la connexion à un serveur lié.  
  
     Si la valeur est 0, utilisez la valeur d’option **sp_configure** par défaut [remote login timeout](../../database-engine/configure-windows/configure-the-remote-login-timeout-server-configuration-option.md) .  
  
     **Délai de requête**  
     Valeur du délai d'expiration, en secondes, des requêtes par rapport à un serveur lié.  
  
     Si la valeur est 0, utilisez la valeur d’option **sp_configure** par défaut [remote query timeout](../../database-engine/configure-windows/configure-the-remote-query-timeout-server-configuration-option.md) .  
  
     **Activer la promotion des transactions distribuées**  
     Cette option permet de protéger les actions d'une procédure de serveur à serveur par le biais d'une transaction MS DTC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator). Lorsque cette option a la valeur TRUE, l'appel d'une procédure stockée distante démarre une transaction distribuée et inscrit la transaction avec MS DTC. Pour plus d’informations, consultez [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md).  
  
6.  Cliquez sur **OK**.  
  
##### <a name="to-view-the-provider-options"></a>Pour afficher les options du fournisseur  
  
-   Pour afficher les options que le fournisseur met à disposition, cliquez sur la page **Options de fournisseurs** .  
  
     Tous les fournisseurs n'offrent pas les mêmes options. Par exemple, certains types de données proposent des index et d'autres pas. Utilisez cette boîte de dialogue pour aider [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à comprendre les possibilités du fournisseur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe des fournisseurs de données communes, toutefois lorsque le produit qui fournit les données est modifié, le fournisseur installé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut ne pas prendre en charge toutes les nouvelles fonctionnalités. La meilleure source d'informations à propos des fonctions du produit qui fournit les données est la documentation pour ce produit.  
  
     **Paramètre dynamique**  
     Indique que le fournisseur autorise la syntaxe de marqueur de paramètre '?' pour les requêtes paramétrables. Définissez cette option uniquement si le fournisseur prend en charge l'interface **ICommandWithParameters** et accepte le point d'interrogation en tant que marqueur de paramètre. La configuration de cette option permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'exécuter des requêtes paramétrables sur le fournisseur. Cette possibilité peut améliorer les performances de certaines requêtes.  
  
     **Requêtes imbriquées**  
     Indique que le fournisseur autorise les instructions `SELECT` imbriquées dans la clause FROM. La configuration de cette option permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de déléguer au fournisseur certaines requêtes nécessitant l'imbrication des instructions SELECT dans la clause FROM.  
  
     **Uniquement niveau zéro**  
     Seules les interfaces OLE DB de niveau 0 sont invoquées pour le fournisseur.  
  
     **Autoriser inprocess**  
     [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autorise l’instanciation du fournisseur en tant que serveur in-process. Lorsque cette option n'est pas définie, le comportement par défaut consiste à instancier le fournisseur en dehors du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L'instanciation du fournisseur en dehors du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] protège le processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des erreurs contenues dans le fournisseur. Lorsque le fournisseur est instancié en dehors du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les mises à jour ou insertions faisant référence à des colonnes longues (**text**, **ntext**ou **image**) ne sont pas autorisées.  
  
     **Mises à jour non transactionnelles**  
     [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autorise les mises à jour, même si **ITransactionLocal** n’est pas disponible. Si cette option est activée, les mises à jour du fournisseur ne sont pas récupérables puisque celui-ci ne prend pas en charge les transactions.  
  
     **Indexer en tant que chemin d'accès**  
     [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente d’utiliser les index du fournisseur pour extraire des données. Par défaut, les index sont uniquement utilisés pour les métadonnées et ne sont jamais ouverts.  
  
     **Interdire l'accès ad hoc**  
     [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’autorise pas l’accès ad hoc par le biais des fonctions OPENROWSET et OPENDATASOURCE au fournisseur OLE DB. Lorsque cette option n'est pas définie, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne permet pas non plus l'accès ad hoc.  
  
     **Prend en charge l'opérateur 'Like'**  
     Indique que le fournisseur prend en charge les requêtes qui utilisent le mot clé LIKE.  
  
###  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Pour créer un serveur lié à l’aide de [!INCLUDE[tsql](../../includes/tsql-md.md)], utilisez les instructions [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md), [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md) et [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md).  
  
##### <a name="to-create-a-linked-server-to-another-instance-of-sql-server-using-transact-sql"></a>Pour créer un serveur lié à une autre instance de SQL Server à l'aide de Transact-SQL  
  
1.  Dans l'éditeur de requête, entrez la commande [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante pour créer une liaison avec une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nommée `SRVR002\ACCTG`:  
  
    ```sql  
    USE [master]  
    GO  
    EXEC master.dbo.sp_addlinkedserver   
        @server = N'SRVR002\ACCTG',   
        @srvproduct=N'SQL Server' ;  
    GO  
  
    ```  
  
2.  Exécutez le code suivant pour configurer le serveur lié de manière à utiliser les informations d'identification de domaine de la connexion qui utilise le serveur lié.  
  
    ```sql  
    EXEC master.dbo.sp_addlinkedsrvlogin   
        @rmtsrvname = N'SRVR002\ACCTG',   
        @locallogin = NULL ,   
        @useself = N'True' ;  
    GO  
  
    ```  
  
##  <a name="FollowUp"></a> Suivi : mesures à prendre après avoir créé un serveur lié  
  
#### <a name="to-test-the-linked-server"></a>Pour tester le serveur lié  
  
-   Exécutez le code suivant pour tester la connexion au serveur lié. Cet exemple retourne les noms des bases de données sur le serveur lié.  
  
    ```sql  
    SELECT name FROM [SRVR002\ACCTG].master.sys.databases ;  
    GO  
  
    ```  
  
#### <a name="writing-a-query-that-joins-tables-from-a-linked-server"></a>Écriture d'une requête qui joint les tables d'un serveur lié  
  
-   Utilisez des noms en quatre parties pour faire référence à un objet sur un serveur lié. Exécutez le code suivant pour retourner une liste de toutes les connexions sur le serveur local avec leurs connexions correspondantes sur le serveur lié.  
  
    ```sql  
    SELECT local.name AS LocalLogins, linked.name AS LinkedLogins  
    FROM master.sys.server_principals AS local  
    LEFT JOIN [SRVR002\ACCTG].master.sys.server_principals AS linked  
        ON local.name = linked.name ;  
    GO  
    ```  
  
     Lorsque NULL est retourné pour la connexion au serveur lié, cela indique que la connexion n'existe pas sur le serveur lié. Ces connexions ne seront pas en mesure d'utiliser le serveur lié, à moins que le serveur lié ne soit configuré pour passer un contexte de sécurité différent ou que le serveur lié accepte des connexions anonymes.  
  
## <a name="see-also"></a> Voir aussi  
 [Serveurs liés &#40;moteur de base de données&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)  
  
  
