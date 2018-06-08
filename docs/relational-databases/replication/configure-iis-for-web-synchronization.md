---
title: Configurer IIS pour la synchronisation web | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- IIS server configuration [SQL Server replication]
- websync.log
- Web synchronization, IIS servers
ms.assetid: d651186e-c9ca-4864-a444-2cd6943b8e35
caps.latest.revision: 88
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 2d4bc5f866ac5cfe9cb29212bd2cf1a30c0b8e3e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-iis-for-web-synchronization"></a>Configurer IIS pour la synchronisation web
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les procédures de cette rubrique constituent la deuxième étape de la configuration de la synchronisation Web pour la réplication de fusion. Cette étape est réalisée après l'activation d'une publication pour la synchronisation Web. Le processus de configuration est présenté dans [Configurer la synchronisation Web](../../relational-databases/replication/configure-web-synchronization.md). Après avoir terminé les procédures de cette rubrique, poursuivez par la troisième étape, qui est la configuration d'un abonnement pour qu'il utilise la synchronisation Web. Cette troisième étape est décrite dans les rubriques suivantes :  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] : [Guide pratique pour configurer un abonnement qui utilise la synchronisation web \(SQL Server Management Studio\)](http://msdn.microsoft.com/library/ms345214.aspx)  
  
-   Programmation [!INCLUDE[tsql](../../includes/tsql-md.md)] de réplication. [Procédure : configurer un abonnement pour utiliser la synchronisation Web (programmation Transact-SQL de la réplication)](http://msdn.microsoft.com/library/ms345206.aspx)  
  
-   Objets RMO. [Procédure : configurer un abonnement pour utiliser la synchronisation Web (programmation RMO)](http://msdn.microsoft.com/library/ms345207.aspx)  
  
 La synchronisation Web utilise un ordinateur exécutant [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Information Services (IIS) pour synchroniser des abonnements par extraction de données (pull) avec des publications de fusion. IIS version 5.0, IIS version 6.0 et [!INCLUDE[iisver](../../includes/iisver-md.md)] sont pris en charge. L'Assistant Configuration de la synchronisation Web n'est pas pris en charge sur [!INCLUDE[iisver](../../includes/iisver-md.md)].  
  
> [!IMPORTANT]  
>  Assurez-vous que votre application utilise uniquement [!INCLUDE[dnprdnlong](../../includes/dnprdnlong-md.md)] ou une version ultérieure et qu'aucune version antérieure de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] n'est installée sur le serveur IIS. Les versions antérieures de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] peuvent provoquer des erreurs. Il peut s'agir de l'erreur suivante : « Le format d'un message pendant la synchronisation Web n'était pas valide. Vérifiez que les composants de réplication sont correctement configurés sur le serveur Web. »  
  
> [!CAUTION]  
>  N'utilisez pas WebSync et d'autres emplacements de dossier d'instantanés à la fois.  
  
 Pour utiliser la synchronisation Web, vous devez configurer IIS en effectuant les étapes ci-dessous. Chaque étape est détaillée dans cette rubrique.  
  
1.  Configurer SSL (Secure Sockets Layer). SSL est requis pour les communications entre IIS et tous les Abonnés.  
  
2.  Installez les composants de connectivité [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l'ordinateur exécutant IIS à l'aide de l'Assistant Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Installez les composants de connectivitéation Wizard. Si vous envisagez d'utiliser l'Assistant Configuration de la synchronisation Web mentionné dans l'étape 3, vous devez également installer [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] sur l'ordinateur exécutant IIS.  
  
3.  Configurez l'ordinateur exécutant IIS pour la synchronisation Web. Vous pouvez configurer l'ordinateur manuellement ou utiliser l'Assistant Configuration de la synchronisation Web. Nous vous recommandons d'utiliser l'Assistant.  
  
    > [!NOTE]  
    >  Si l'ordinateur qui exécute IIS utilise une version 64 bits de Windows, vous devez exécuter la commande suivante pour vous assurer que le serveur est correctement configuré pour exécuter des applications ISAPI (Internet Server API). Pour plus d'informations, consultez la documentation relative à IIS.  
  
    ```  
    cscript %SystemDrive%\inetpub\AdminScripts\adsutil.vbs set w3svc/AppPools/Enable32bitAppOnWin64 1  
    ```  
  
4.  Définissez les autorisations appropriées pour l'écouteur de réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
5.  Exécutez la synchronisation Web en mode de diagnostic pour tester la connexion à l'ordinateur exécutant IIS et pour vérifier que le certificat SSL est correctement installé.  
  
## <a name="configuring-secure-sockets-layer"></a>Configuration de SSL (Secure Sockets Layer)  
 Pour configurer SSL, spécifiez un certificat que l'ordinateur exécutant IIS va utiliser. La synchronisation Web pour la réplication de fusion prend en charge l'utilisation des certificats de serveur, mais pas celle des certificats clients. Pour configurer IIS pour le déploiement, vous devez d'abord obtenir un certificat auprès d'une Autorité de certification. Une Autorité de certification est une entité chargée d'établir et de se porter garante de l'authenticité des clés publiques de chiffrement appartenant à des utilisateurs, à des ordinateurs ou à d'autres Autorités de certification. Pour plus d'informations sur les certificats, consultez la documentation de IIS. Après avoir installé le certificat, vous devez l'associer au site Web utilisé par la synchronisation Web.  
  
#### <a name="to-specify-a-certificate-for-deployment"></a>Pour spécifier un certificat pour un déploiement  
  
1.  Connectez-vous à l'ordinateur exécutant IIS en tant qu'administrateur.  
  
2.  Démarrez le **Gestionnaire des services Internet (IIS)**:  
  
    1.  Cliquez sur **Démarrer**, puis sur **Exécuter**.  
  
    2.  Dans la zone **Ouvrir** , tapez **inetmgr**, puis cliquez sur **OK**.  
  
3.  Exécutez l'Assistant Certificat d'IIS :  
  
    1.  Dans **Gestionnaire des services Internet (IIS)**, développez le nœud **ordinateur local** , puis développez **Sites Web** .  
  
    2.  Cliquez avec le bouton droit sur **Site Web par défaut**, puis cliquez sur **Propriétés**.  
  
    3.  Dans la boîte de dialogue **Propriétés du site Web par défaut** , sur l'onglet **Sécurité de répertoire** , cliquez sur **Certificat de serveur**.  
  
    4.  Terminez l'Assistant Certificat de serveur Web.  
  
4.  Cliquez sur **OK**.  
  
 Si vous ne pouvez pas obtenir de certificat de serveur auprès d'une Autorité de certification, vous pouvez spécifier un certificat de test. Pour configurer IIS 6.0 à des fins de test, installez un certificat à l'aide de l'utilitaire SelfSSL. Cet utilitaire est présent dans le Kit de ressources techniques IIS 6.0. Vous pouvez télécharger les outils à partir du [Centre de téléchargement Microsoft](http://go.microsoft.com/fwlink/?LinkId=30958). Pour IIS 5.0, visitez le site [Aide et Support Microsoft](http://go.microsoft.com/fwlink/?LinkId=46229).  
  
> [!NOTE]  
>  Un certificat doit être associé à un site Web avant que ce dernier puisse utiliser SSL. SelfSSL associe automatiquement le certificat au site Web par défaut. Si vous avez déjà un certificat ou si vous installez plus tard un certificat d'une Autorité de certification, vous devez explicitement associer ce certificat au site Web utilisé par la synchronisation Web. Vérifiez qu'un seul certificat est associé au site Web utilisé pour synchroniser les abonnements. S'il y a plusieurs certificats, l'Abonné utilisera le premier site Web disponible.  
  
#### <a name="to-specify-a-certificate-for-testing-in-iis-60"></a>Pour spécifier un certificat pour des tests dans IIS 6.0  
  
1.  Connectez-vous à l'ordinateur exécutant IIS en tant qu'administrateur.  
  
2.  Téléchargez et installez SelfSSL. Par défaut, l’application est installée à l’emplacement \<*lecteur*>:\Program Files\IIS Resources\SelfSSL. Les raccourcis vers l’application et la documentation sont copiés à l’emplacement \<*lecteur*>:\Documents and Settings\All Users\Start Menu\Programs\IIS Resources\SelfSSL.  
  
3.  Exécutez SelfSSL :  
  
    -   Pour exécuter SelfSSL avec les valeurs par défaut pour tous les paramètres, recherchez le répertoire d'installation de l'application, puis double-cliquez sur SelfSSL.exe.  
  
        > [!NOTE]  
        >  Par défaut, le certificat installé par SelfSSL est valide pour sept jours.  
  
    -   Pour spécifier des valeurs pour un ou plusieurs paramètres : cliquez sur **Démarrer**, puis cliquez sur **Exécuter**. Dans la zone **Ouvrir** , entrez **cmd**, puis cliquez sur **OK**. Recherchez le répertoire d'installation de SelfSSL, tapez `SelfSSL`, puis spécifiez les valeurs d'un ou plusieurs paramètres. Pour obtenir la liste des paramètres, tapez `SelfSSL -?`.  
  
## <a name="installing-connectivity-components-and-sql-server-management-studio"></a>Installation des composants de connectivité et de SQL Server Management Studio  
  
#### <a name="to-install-sql-server-connectivity-components-and-sql-server-management-studio"></a>Pour installer les composants de connectivité de SQL Server et SQL Server Management Studio  
  
1.  Connectez-vous en tant qu'administrateur à l'ordinateur exécutant IIS.  
  
2.  Démarrez l'Assistant Installation de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à partir du disque d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations sur l’utilisation de cet Assistant, consultez [Installer SQL Server 2016 avec l’Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md).  
  
3.  Dans la page **Sélection de composant** , sélectionnez **Connectivité des outils clients**.  
  
4.  Si vous projetez d'utiliser l'Assistant Configuration de la synchronisation Web, sélectionnez **Outils de gestion - De base**.  
  
5.  Terminez l'Assistant puis redémarrez l'ordinateur.  
  
    > [!NOTE]  
    >  Vous pouvez installer d'autres composants, mais seuls les composants de connectivité sont nécessaires pour la synchronisation Web.  
  
## <a name="configuring-the-computer-that-is-running-iis-by-using-the-configure-web-synchronization-wizard"></a>Configuration de l'ordinateur exécutant IIS à l'aide de l'Assistant Configuration de la synchronisation Web  
 Configurez le serveur IIS à l'aide de l'Assistant Configuration de la synchronisation Web ou manuellement. Il est recommandé d'utiliser l'Assistant ; la section suivante présente néanmoins les étapes de configuration manuelle. L'Assistant Synchronisation Web qui est disponible avec [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] peut être utilisé uniquement pour les publications créées sur un serveur de publication qui exécute [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou sur un serveur de publication qui a été mis à niveau vers [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. L'Assistant ne peut pas être utilisé pour les publications dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. L'Assistant peut être utilisé avec les abonnements dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et les versions ultérieures, ainsi que dans [!INCLUDE[ssEWnoversion](../../includes/ssewnoversion-md.md)] 3.0 et les versions ultérieures.  
  
 La configuration présente les caractéristiques suivantes :  
  
-   utilise le site Web par défaut dans IIS. Vous pouvez toutefois utiliser un autre site Web. Pour plus d'informations sur la création de sites Web, consultez la documentation de IIS.  
  
    > [!NOTE]  
    >  Le site Web que vous spécifiez donne accès aux composants utilisés par la synchronisation Web. Il ne donne pas accès à d'autres données ou à d'autres pages Web, sauf si vous configurez le site dans ce sens.  
  
-   crée un répertoire virtuel et ses alias associés. L'alias est utilisé lors de l'accès aux composants de la synchronisation Web. Par exemple, si l’adresse du serveur IIS est `https://server.domain.com` et si vous spécifiez l’alias « websync1 », l’adresse permettant d’accéder au composant replisapi.dll est `https://server.domain.com/websync1/replisapi.dll`.  
  
-   Utilise l'authentification de base. Il est recommandé d'utiliser l'authentification de base car elle vous permet d'exécuter IIS et le serveur de publication/distribution [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur des ordinateurs distincts (c'est la configuration recommandée) sans recourir à la délégation Kerberos. L'utilisation de SSL avec l'authentification de base garantit que les connexions, les mots de passe et toutes les données sont chiffrés lors du transit. (SSL est nécessaire quel que soit le type d'authentification utilisé.) Pour plus d’informations sur les bonnes pratiques pour la synchronisation web, consultez la section « Méthodes préconisées pour la synchronisation Web » de la rubrique [Configurer la synchronisation web](../../relational-databases/replication/configure-web-synchronization.md).  
  
#### <a name="to-configure-the-computer-that-is-running-iis-by-using-the-configure-web-synchronization-wizard"></a>Pour configurer l'ordinateur exécutant IIS à l'aide de l'Assistant Configuration de la synchronisation Web  
  
1.  Sur l'ordinateur exécutant IIS, démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Connectez-vous au serveur de publication, puis développez le nœud du serveur.  
  
3.  Développez le dossier **Publications locales** , cliquez avec le bouton droit sur la publication, puis cliquez sur **Configurer la synchronisation Web**.  
  
4.  Dans l'Assistant Configuration de la synchronisation Web, sur la page **Type d'Abonné** , sélectionnez **SQL Server**.  
  
5.  Sur la page **Serveur Web** :  
  
    1.  Sélectionnez l'instance de IIS qui va synchroniser les abonnements.  
  
    2.  Sélectionnez **Créer un nouveau répertoire virtuel**.  
  
    3.  Dans le volet inférieur de la page, développez l'instance de IIS, développez **Sites Web**, puis cliquez sur **Site Web par défaut**.  
  
6.  Sur la page **Informations sur le répertoire virtuel** :  
  
    1.  Dans la zone **Alias** , entrez un alias pour le répertoire virtuel.  
  
    2.  Dans la zone **Chemin d'accès** , entrez un chemin d'accès au répertoire virtuel. Par exemple, si vous avez entré **websync1** dans la zone **Alias** , entrez **C:\Inetpub\wwwroot\websync1** dans la zone **Chemin d’accès** . Cliquez sur **Suivant**.  
  
    3.  Dans les deux boîtes de dialogue, cliquez sur **Oui**. Ceci indique que vous souhaitez créer un nouveau dossier et copier la DLL [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ISAPI (Internet Server API). .  
  
7.  Sur la page **Accès authentifié** :  
  
    1.  Vérifiez que les options **Authentification Windows intégrée** et **Authentification Digest pour les serveurs de domaine Windows** sont désactivées.  
  
    2.  Sélectionnez **Authentification de base**.  
  
    3.  Dans les zones **Domaine par défaut** et **Domaine** , entrez le domaine de l'ordinateur exécutant IIS.  
  
8.  Sur la page **Accès à l'annuaire** :  
  
    1.  Cliquez sur **Ajouter**puis, dans la boîte de dialogue **Sélectionner des utilisateurs ou des groupes** , ajoutez les comptes sous lesquels les Abonnés établiront des connexions à IIS. Il s’agit des comptes que vous spécifiez dans la page **Informations sur le serveur Web** de l’Assistant Nouvel abonnement ou via le paramètre [de](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)*@internet_login* .  
  
9. Sur la page **Accès au partage de fichiers d'instantanés** , entrez le partage d'instantané. Les autorisations appropriées sont définies sur ce partage pour que les Abonnés puissent accéder aux fichiers d'instantanés. Pour plus d’informations sur les autorisations pour le partage, consultez [Sécuriser le dossier d’instantanés](../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
10. Sur la page **Fin de l'Assistant** , cliquez sur **Terminer**.  
  
     Si une défaillance se produit, telle qu'une erreur réseau lors de la configuration d'un ordinateur distant exécutant IIS, toutes les actions terminées sont restaurées et toutes les actions restantes sont annulées. Si une action  terminée ne peut pas être annulée, l'état indiqué dans la page finale de l'Assistant affiche **Succès** et les actions terminées restent validées.  
  
11. Si l'ordinateur exécutant IIS utilise une version 64 bits de Windows, replisapi.dll doit être copié dans le répertoire approprié :  
  
    1.  Cliquez sur **Démarrer**, puis sur **Exécuter**. Dans la zone **Ouvrir** , entrez **iisreset**, puis cliquez sur **OK**.  
  
    2.  Après l'arrêt et le redémarrage d'IIS, copiez replisapi.dll à partir du dossier [!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]COM\replisapi vers le répertoire spécifié à l'étape 6b.  
  
    3.  Cliquez sur **Démarrer**, puis sur **Exécuter**. Dans la zone **Ouvrir** , entrez **cmd**, puis cliquez sur **OK**.  
  
    4.  Dans le répertoire spécifié à l'étape 6b, exécutez la commande suivante :  
  
         **regsvr32 replisapi.dll**  
  
## <a name="manually-configuring-the-computer-that-is-running-iis"></a>Configuration manuelle de l'ordinateur exécutant IIS  
 Pour configurer manuellement l'ordinateur exécutant IIS, vous devez installer et configurer l'écouteur de réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puis configurer l'autorisation pour les Abonnés qui vont se connecter à IIS.  
  
#### <a name="to-install-and-configure-the-sql-server-replication-listener"></a>Pour installer et configurer l'écouteur de réplication SQL Server  
  
1.  Créez un répertoire de fichiers sur l'ordinateur exécutant IIS pour y placer le fichier replisapi.dll. Vous pouvez créer le répertoire où vous le souhaitez, mais nous vous recommandons de le créer sous le répertoire \<*lecteur*>:\Inetpub. Par exemple, créez le répertoire \<*lecteur*>:\Inetpub\SQLReplication\\.  
  
    > [!IMPORTANT]  
    >  Il est vivement recommandé de créer ce répertoire sur une partition de système de fichiers NTFS plutôt que FAT. Quand vous utilisez le système de fichiers NTFS, vous pouvez vous servir des autorisations du système de fichiers NTFS pour contrôler avec précision les utilisateurs en mesure d'accéder à la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
2.  Copiez replisapi.dll à partir du répertoire [!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]com\ vers le répertoire de fichiers que vous avez créé à l'étape 1.  
  
3.  Inscrivez replisapi.dll :  
  
    1.  Cliquez sur **Démarrer**, puis sur **Exécuter**. Dans la zone **Ouvrir** , entrez **cmd**, puis cliquez sur **OK**.  
  
    2.  Dans le répertoire créé à l'étape 1, exécutez la commande suivante :  
  
         **regsvr32 replisapi.dll**  
  
4.  Créez un nouveau site Web pour la réplication ou bien utilisez un site Web existant. Les composants de la réplication accèdent au site Web au cours de la synchronisation. Pour plus d'informations sur la création de sites Web, consultez la documentation de IIS.  
  
5.  Créez un répertoire virtuel dans IIS. Le répertoire virtuel doit être créé sous le site Web créé à l'étape 4 et doit être mappé au répertoire créé à l'étape 1. Pour plus d'informations sur la création de répertoires virtuels, consultez la documentation IIS. Il est recommandé d'être aussi restrictif que possible lors de l'attribution des autorisations sur ce répertoire. Vous devez sélectionner les autorisations **Lire** et **Exécuter** , mais vous pouvez désactiver les autorisations **Exécuter les scripts**, **Écrire**et **Parcourir** .  
  
6.  Configurez IIS pour permettre à replisapi.dll de s'exécuter. Les autorisations attribuées à l'étape 4 sont suffisantes pour les versions antérieures de IIS, mais IIS 6.0 requiert l'activation des extensions ISAPI (Internet Server API). Pour plus d'informations, consultez « Configuring ISAPI Extensions » et « Enabling and Disabling Dynamic Content » dans la documentation de IIS 6.0.  
  
#### <a name="to-configure-iis-authentication"></a>Pour configurer l'authentification IIS  
  
-   Quand des Abonnés se connectent à IIS, IIS doit les authentifier avant qu'ils puissent accéder aux ressources et aux processus. IIS offre trois types d'authentification : anonyme, de base et intégrée. L'authentification peut être appliquée au site Web tout entier ou au répertoire virtuel que vous avez créé.  
  
     Nous vous recommandons d'utiliser l'authentification de base avec SSL. SSL est nécessaire quel que soit le type d'authentification utilisé. Pour plus d'informations sur la configuration de l'authentification, consultez la documentation de IIS.  
  
## <a name="setting-permissions-for-the-sql-server-replication-listener"></a>Définition des autorisations pour l'écouteur de réplication SQL Server  
 Lorsqu'un Abonné se connecte à l'ordinateur exécutant IIS, il est authentifié à l'aide du type d'authentification spécifié lors de la configuration de IIS. Après avoir authentifié l'Abonné, IIS vérifie que l'Abonné est autorisé à appeler la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous contrôlez les utilisateurs en mesure d'appeler la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en définissant des autorisations pour replisapi.dll. La configuration correcte des autorisations est cruciale pour empêcher les accès non autorisés à la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Pour configurer les autorisations minimales du compte sous lequel est exécuté l'écouteur de réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , suivez la procédure ci-dessous. Les étapes de la procédure s'appliquent à [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] exécutant IIS 6.0.  
  
 En plus de suivre les étapes ci-dessous, vérifiez que les noms de connexion requis se trouvent dans la liste d'accès à la publication (PAL, Publication Access List). Pour plus d’informations sur la liste d’accès à la publication, consultez [Sécuriser le serveur de publication](../../relational-databases/replication/security/secure-the-publisher.md).  
  
#### <a name="to-configure-the-account-and-permissions"></a>Pour configurer le compte et les autorisations  
  
1.  Créez un compte local sur l'ordinateur exécutant IIS :  
  
    1.  Cliquez avec le bouton droit sur **Poste de travail**, puis cliquez sur **Gérer**.  
  
    2.  Dans **Gestion de l'ordinateur**, développez **Utilisateurs et groupes locaux**.  
  
    3.  Cliquez avec le bouton droit sur **Utilisateurs**, puis cliquez sur **Nouvel utilisateur**.  
  
    4.  Entrez un nom d'utilisateur et un mot de passe fort.  
  
    5.  Cliquez sur **Créer**, puis sur **Fermer**.  
  
2.  Ajoutez le compte au groupe IIS_WPG :  
  
    1.  Dans **Gestion de l'ordinateur**, développez **Utilisateurs et groupes locaux**puis cliquez sur **Groupes**.  
  
    2.  Cliquez avec le bouton droit sur **IIS_WPG**, puis cliquez sur **Ajouter au groupe**.  
  
    3.  Dans la boîte de dialogue **Propriétés de IIS_WPG** , cliquez sur **Ajouter**.  
  
    4.  Dans la boîte de dialogue **Choisir des utilisateurs, des ordinateurs ou des groupes** , ajoutez le compte créé à l'étape 1.  
  
    5.  Vérifiez que le nom figurant dans le champ **À partir de cet emplacement** est celui de l'ordinateur local et non pas un nom de domaine. Si le nom n'est pas un ordinateur local, cliquez sur **Emplacements**. Dans la boîte de dialogue **Emplacements** , choisissez l'ordinateur local, puis cliquez sur **OK**.  
  
    6.  Dans les boîtes de dialogue **Choisir des utilisateurs** et **Propriétés de IIS_WPG** , cliquez sur **OK**.  
  
3.  Attribuez au compte les autorisations minimales sur le dossier qui contient replisapi.dll :  
  
    1.  Recherchez le dossier créé pour replisapi.dll, cliquez avec le bouton droit sur le dossier, puis cliquez sur **Partage et sécurité**.  
  
    2.  Sur l'onglet **Sécurité** , cliquez sur **Ajouter**.  
  
    3.  Dans la boîte de dialogue **Choisir des utilisateurs, des ordinateurs ou des groupes** , ajoutez le compte créé à l'étape 1.  
  
    4.  Vérifiez que le nom figurant dans le champ **À partir de cet emplacement** est celui de l'ordinateur local et non pas un nom de domaine. Si le nom n'est pas un ordinateur local, cliquez sur **Emplacements**. Dans la boîte de dialogue **Emplacements** , choisissez l'ordinateur local, puis cliquez sur **OK**.  
  
    5.  Vérifiez que le compte a seulement les autorisations suivantes : **Lecture**, **Lecture et exécution** et **Affichage du contenu du dossier**.  
  
    6.  Sélectionnez tous les utilisateurs ou groupes qui ne requièrent pas l'accès au répertoire, puis cliquez sur **Supprimer**.  
  
    7.  Cliquez sur **OK**.  
  
4.  Créez un pool d'applications dans **Gestionnaire des services Internet (IIS)**:  
  
    1.  Cliquez sur **Démarrer**, puis sur **Exécuter**.  
  
    2.  Dans la zone **Ouvrir** , tapez **inetmgr**, puis cliquez sur **OK**.  
  
    3.  Dans **Gestionnaire des services Internet (IIS)**, développez le nœud **ordinateur local** .  
  
    4.  Cliquez avec le bouton droit sur **Pools d'applications**, pointez sur **Nouveau** puis cliquez sur **Pool d'applications**.  
  
    5.  Entrez le nom du pool dans le champ **ID du pool d'applications** , puis cliquez sur **OK**.  
  
5.  Associez le compte au pool d'applications :  
  
    1.  Dans **Gestionnaire des services Internet (IIS)**, développez le nœud **ordinateur local** , puis développez **Pools d'applications**.  
  
    2.  Cliquez avec le bouton droit sur le pool d'applications que vous avez créé, puis cliquez sur **Propriétés**.  
  
    3.  Dans la boîte de dialogue **Propriétés de \<nom_pool_applications>**, sous l’onglet **Identité**, cliquez sur **Configurable**.  
  
    4.  Dans les champs **Nom d'utilisateur** et **Mot de passe** , entrez le compte et le mot de passe créés à l'étape 1.  
  
    5.  Cliquez sur **OK**.  
  
6.  Associez le pool d'applications au répertoire virtuel utilisé pour la synchronisation Web :  
  
    1.  Dans **Gestionnaire des services Internet (IIS)**, développez le nœud **ordinateur local** , puis développez **Sites Web**.  
  
    2.  Développez le site Web que vous utilisez pour la synchronisation Web, cliquez avec le bouton droit sur le répertoire virtuel que vous avez créé pour la synchronisation Web, puis cliquez sur **Propriétés**.  
  
    3.  Sous l’onglet **Répertoire virtuel** de la boîte de dialogue **Propriétés de \<nom_répertoire_virtuel>**, sélectionnez le pool d’applications créé à l’étape 5 dans la liste déroulante **Pool d’applications**.  
  
    4.  Cliquez sur **OK**.  
  
## <a name="testing-the-connection-to-replisapidll"></a>Test de la connexion à replisapi.dll  
 Exécutez la synchronisation Web en mode de diagnostic pour tester la connexion à l'ordinateur exécutant IIS et pour vérifier que le certificat SSL (Secure Sockets Layer) est correctement installé. Pour exécuter la synchronisation Web en mode de diagnostic, vous devez avoir la qualité d'administrateur sur l'ordinateur qui exécute les services Internet (IIS).  
  
#### <a name="to-test-the-connection-to-replisapidll"></a>Pour tester la connexion à replisapi.dll  
  
1.  Vérifiez que les paramètres du réseau local (LAN) sur l'Abonné sont corrects :  
  
    1.  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Explorer, dans le menu **Outils** , cliquez sur **Options Internet**.  
  
    2.  Sur l'onglet **Connexions** , cliquez sur **Paramètres LAN**.  
  
    3.  S'il n'y a pas de serveur proxy utilisé sur le réseau local, désactivez **Détecter automatiquement les paramètres de connexion** et **Utiliser un serveur proxy pour votre réseau local**.  
  
    4.  Si un serveur proxy est utilisé, activez **Utiliser un serveur proxy pour votre réseau local** et **Ne pas utiliser de serveur proxy pour les adresses locales**.  
  
    5.  Cliquez sur **OK**.  
  
2.  Sur l'Abonné, dans Internet Explorer, connectez-vous au serveur en mode de diagnostic en ajoutant `?diag` à l'adresse pour le fichier replisapi.dll. Par exemple : `https://server.domain.com/directory/replisapi.dll?diag`.  
  
3.  Si le certificat spécifié pour IIS n'est pas reconnu par le système d'exploitation Windows, la boîte de dialogue **Alerte de sécurité** s'affiche. Cette alerte peut se produire si le certificat est un certificat de test ou si le certificat a été émis par une Autorité de certification non reconnue par Windows.  
  
    > [!NOTE]  
    >  Si cette boîte de dialogue ne s'affiche pas, vérifiez que le certificat pour le serveur auquel vous accédez a été ajouté au magasin de certificats sur l'Abonné en tant que certificat approuvé. Pour plus d'informations sur l'exportation de certificats, consultez la documentation d'IIS.  
  
    1.  Dans la boîte de dialogue **Alerte de sécurité** , cliquez sur **Afficher le certificat**.  
  
    2.  Dans la boîte de dialogue **Certificat** , sur l'onglet **Général** , cliquez sur **Installer le certificat**.  
  
    3.  Terminez l'Assistant Importation de certificat, en acceptant les valeurs par défaut.  
  
    4.  Dans la boîte de dialogue **Avertissement de sécurité** , cliquez sur **Oui**.  
  
    5.  Dans la boîte de dialogue de confirmation de l'Assistant Importation de certificat, cliquez sur **OK**.  
  
    6.  Fermez la boîte de dialogue **Certificat** .  
  
    7.  Dans la boîte de dialogue **Alerte de sécurité** , cliquez sur **Oui**.  
  
    > [!NOTE]  
    >  Les certificats sont installés pour les utilisateurs. Ce processus doit être effectué pour chaque utilisateur qui réalisera des synchronisations avec IIS.  
  
4.  Dans la boîte de dialogue **Se connecter à \<NomServeur>**, spécifiez l’ID de connexion et le mot de passe que l’Agent de fusion utilisera pour se connecter à IIS. Ces informations d'identification seront aussi spécifiées dans l'Assistant Nouvel abonnement.  
  
5.  Dans la fenêtre Internet Explorer relative aux **informations de diagnostic SQL Websync**, vérifiez que la valeur de chaque colonne **État** de la page est **RÉUSSITE**.  
  
6.  Vérifiez que le certificat est installé correctement sur l'Abonné :  
  
    1.  Fermez, puis rouvrez Internet Explorer.  
  
    2.  Connectez-vous au serveur en mode de diagnostic. Si le certificat est installé correctement, la boîte de dialogue **Alerte de sécurité** ne s'affiche pas. Si cette boîte de dialogue s'affiche, l'Agent de fusion échoue quand il tente de se connecter à l'ordinateur exécutant IIS. Vous devez vérifier que le certificat pour le serveur auquel vous accédez a été ajouté au magasin de certificats sur l'Abonné en tant que certificat approuvé. Pour plus d'informations sur l'exportation de certificats, consultez la documentation d'IIS.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer la synchronisation Web](../../relational-databases/replication/configure-web-synchronization.md)  
  
  
