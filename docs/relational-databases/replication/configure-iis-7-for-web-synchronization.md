---
title: Configurer IIS 7 pour la synchronisation web | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2016
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
- IIS 7 server configuration [SQL Server replication]
- Web synchronization, IIS 7 servers
ms.assetid: c201fe2c-0a76-44e5-a233-05e14cd224a6
caps.latest.revision: 11
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: ebf8cefa129638c36fa37b081ffcb345a2c49f54
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-iis-7-for-web-synchronization"></a>Configurer IIS 7 pour la synchronisation web
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Les procédures décrites dans cette rubrique vous guident dans le processus de configuration manuelle des services IIS ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Information Services) version 7 et ultérieures pour une utilisation avec la synchronisation web en vue de la réplication de fusion. 
  
 La configuration d’IIS 7 ou version ultérieure est la première des trois étapes nécessaires pour activer la synchronisation web.  
  
 L’ensemble du processus de configuration est présenté dans [Configurer la synchronisation web](../../relational-databases/replication/configure-web-synchronization.md).  
  
> [!IMPORTANT]  
>  Assurez-vous que votre application utilise uniquement [!INCLUDE[dnprdnlong](../../includes/dnprdnlong-md.md)] ou une version ultérieure et qu'aucune version antérieure de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] n'est installée sur le serveur IIS. Les versions antérieures du [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] risquent de provoquer des erreurs, telles que : « Le format d'un message pendant la synchronisation Web n'était pas valide. Vérifiez que les composants de réplication sont correctement configurés sur le serveur Web ».  
  
 Pour utiliser la synchronisation Web, vous devez configurer IIS en effectuant les étapes ci-dessous. Chaque étape est détaillée dans cette rubrique.  
  
1.  Installez et configurez l'écouteur de réplication [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur l'ordinateur qui exécute IIS.  
  
2.  Configurer SSL (Secure Sockets Layer). SSL est requis pour les communications entre IIS et tous les abonnés.  
  
3.  Configurez l'authentification IIS.  
  
4.  Configurez un compte et définissez des autorisations pour l'écouteur de réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="installing-the-sql-server-replication-listener"></a>Installation de l'écouteur de réplication SQL Server  

La synchronisation web est prise en charge sur IIS à compter de la version 5.0. L’Assistant Configuration de la synchronisation Web d’IIS versions 5 et 6 n’est pas disponible avec IIS version 7.0 et ultérieures. **À partir de SQL Server 2012, pour utiliser le composant de synchronisation web sur le serveur IIS, vous devez installer SQL Server avec la réplication. Il peut s’agir de l’édition gratuite de SQL Server Express.**
  
#### <a name="to-install-and-configure-the-sql-server-replication-listener"></a>Pour installer et configurer l'écouteur de réplication SQL Server  
  
1.  Installez la réplication SQL Server sur l’ordinateur IIS.

2. Créez un nouveau répertoire de fichiers pour replisapi.dll sur l'ordinateur exécutant IIS. Vous pouvez créer le répertoire où vous le souhaitez, mais nous vous recommandons de le créer sous le répertoire \<*lecteur*>:\Inetpub. Par exemple, créez le répertoire \<*lecteur*>:\Inetpub\SQLReplication\\.  
  
3.  Copiez replisapi.dll à partir du répertoire [!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]com\ vers le répertoire de fichiers que vous avez créé à l'étape 1.  
  
4.  Inscrivez replisapi.dll :  
  
    1.  Cliquez sur **Démarrer**, puis sur **Exécuter**. Dans la zone **Ouvrir** , entrez **cmd**, puis cliquez sur **OK**.  
  
    2.  Dans le répertoire créé à l'étape 1, exécutez la commande suivante :  
  
         **regsvr32 replisapi.dll**  
  
5.  Créez un nouveau site Web pour la réplication ou bien utilisez un site Web existant. Les composants de la réplication accèdent à ce site Web au cours de la synchronisation. Les procédures décrites dans cette rubrique utilisent le site Web par défaut. Pour plus d'informations sur la création de sites Web, consultez la documentation d'IIS.  
  
6.  Créez un répertoire virtuel dans IIS. Le répertoire virtuel doit être créé sous le site Web créé à l'étape 4 et doit être mappé au répertoire créé à l'étape 1. Soyez aussi restrictif que possible lors de l'attribution des autorisations sur ce répertoire. Vous devez sélectionner au moins les autorisations **Lire** et **Exécuter** .  
  
    1.  Dans le **Gestionnaire des services Internet (IIS)**, dans le volet **Connexions** , cliquez avec le bouton droit sur **Site Web par défaut**, puis sélectionnez **Ajouter un répertoire virtuel**.  
  
    2.  Dans le champ **Alias**, entrez **SQLReplication**.  
  
    3.  Pour **Chemin d’accès physique**, entrez **\<lecteur>:\Inetpub\SQLReplication\\**, puis cliquez sur **OK**.  
  
7.  Configurez IIS pour permettre à replisapi.dll de s'exécuter.  
  
    1.  Dans le **Gestionnaire des services Internet (IIS)**, cliquez sur **Site Web par défaut**.  
  
    2.  Dans le volet central, cliquez sur **Mappages de gestionnaire**.  
  
    3.  Dans le volet **Actions** , cliquez sur **Ajouter un mappage de modules**.  
  
    4.  Pour le chemin d’accès **Requête** , entrez **replisapi.dll**.  
  
    5.  Dans la liste déroulante **Module** , sélectionnez **IsapiModule**.  
  
    6.  Pour **Exécutable**, entrez **\<lecteur>:\Inetpub\SQLReplication\replisapi.dll**.  
  
    7.  Dans le champ **nom**, entrez **Replisapi**.  
  
    8.  Cliquez sur le bouton **Restrictions des demandes** , cliquez sur l'onglet **Accès** , puis sur **Exécuter**.  
  
    9. Cliquez sur **OK** pour fermer la boîte de dialogue **Restrictions des demandes** , puis cliquez à nouveau sur **OK** pour fermer la boîte de dialogue **Ajouter un mappage de modules** . Lorsque vous êtes invité à autoriser l'extension ISAPI, cliquez sur **Oui** pour ajouter l'extension.  
  
    10. Vérifiez que Replisapi.dll apparaît sous les mappages de gestionnaire **Activé** . S'il est dans la liste **Désactivé** , cliquez avec le bouton droit sur l'entrée Replisapi, puis cliquez sur **Modifier les autorisations de fonction**. Activez la case à cocher **Exécuter** , puis cliquez sur **OK**.  
  
## <a name="configuring-iis-authentication"></a>Configuration de l'authentification IIS  
 Quand des ordinateurs d'abonnés se connectent à IIS, IIS doit authentifier les abonnés avant qu'ils puissent accéder aux ressources et aux processus. L'authentification peut être appliquée au site Web tout entier ou au répertoire virtuel que vous avez créé.  
  
 Nous vous recommandons d'utiliser l'authentification de base avec SSL. SSL est nécessaire quel que soit le type d'authentification utilisé.  
  
 Nous vous recommandons d'utiliser l'authentification de base avec SSL. SSL est nécessaire quel que soit le type d'authentification utilisé.  
  
#### <a name="to-configure-iis-authentication"></a>Pour configurer l'authentification IIS  
  
1.  Dans le **Gestionnaire des services Internet (IIS)**, cliquez sur **Site Web par défaut**.  
  
2.  Dans le volet central, double-cliquez sur **Authentification**.  
  
3.  Cliquez avec le bouton droit sur Authentification anonyme, puis choisissez Désactiver.  
  
4.  Cliquez avec le bouton droit sur Authentification de base, puis choisissez Activer.  
  
## <a name="configuring-secure-sockets-layer"></a>Configuration de SSL (Secure Sockets Layer)  
 Pour configurer SSL, spécifiez un certificat à utiliser par l'ordinateur exécutant IIS. La synchronisation Web pour la réplication de fusion prend en charge l'utilisation des certificats de serveur, mais pas celle des certificats clients. Pour configurer IIS pour le déploiement, vous devez d'abord obtenir un certificat auprès d'une Autorité de certification. Pour plus d'informations sur les certificats, consultez la documentation de IIS.  
  
 Après avoir installé le certificat, vous devez l'associer au site Web utilisé par la synchronisation Web. Pour le développement et les tests, vous pouvez spécifier un certificat auto-signé. IIS 7 peut créer un certificat automatiquement et l'enregistrer sur votre ordinateur.  
  
 La différence entre un déploiement pour la production et les procédures indiquées ici est que dans le cadre de tests de production et de préproduction, vous utiliseriez un certificat délivré par une autorité de certification au lieu d'un certificat auto-signé.  
  
> [!IMPORTANT]  
>  Un certificat auto-signé n'est pas recommandé pour une installation de production. Les certificats auto-signés ne sont pas sécurisés. Utilisez des certificats auto-signés uniquement à des fins de développement et de test.  
  
 Pour configurer SSL, vous devez effectuer les étapes suivantes :  
  
1.  Configurez le site Web de manière à ce qu'il exige SSL et ignore les certificats client.  
  
2.  Obtenez un certificat auprès d'une autorité de certification ou créez un certificat auto-signé.  
  
3.  Liez le certificat au site Web de réplication.  
  
#### <a name="to-require-ssl-security-for-a-web-site"></a>Pour exiger la sécurité SSL pour un site Web  
  
1.  Dans **Gestionnaire des services Internet (IIS)**, développez le nœud de serveur local, puis cliquez sur le **Site Web par défaut** (ou votre site de synchronisation Web s'il est différent du site Web par défaut).  
  
2.  Dans le volet central, double-cliquez sur **Paramètres SSL**.  
  
3.  Activez l'option **Exiger SSL** . Sous **Certificats clients**, vérifiez que le bouton **Ignorer** est sélectionné.  
  
#### <a name="to-create-a-self-signed-certificate-for-testing"></a>Pour créer un certificat auto-signé à des fins de test  
  
1.  Dans **Gestionnaire des services Internet (IIS)**, cliquez sur le nœud de serveur local, puis dans le volet central, double-cliquez sur **Certificats de serveur**.  
  
2.  Dans le volet **Actions** , cliquez sur **Créer un certificat auto-signé**.  
  
3.  Dans la boîte de dialogue **Créer un certificat auto-signé** , entrez un nom pour le certificat, puis cliquez sur **OK**.  
  
###### <a name="to-bind-a-certificate-to-a-web-site"></a>Pour lier un certificat à un site Web  
  
1.  Dans le volet **Connexions** , cliquez sur **Site Web par défaut** (ou votre site de synchronisation Web, s'il est différent du site Web par défaut).  
  
2.  Dans le volet **Actions** , cliquez sur **Liaisons**, puis sur **Ajouter**. La boîte de dialogue **Ajouter la liaison de site** s'affiche.  
  
3.  Dans la liste déroulante **Type** , sélectionnez **https**. Laissez les paramètres par défaut pour **Adresse IP** et **Port**.  
  
4.  Dans la liste déroulante **Certificat SSL** , sélectionnez le certificat créé dans « Pour créer un certificat auto-signé à des fins de test », cliquez sur **OK**, puis sur **Fermer**.  
  
###### <a name="to-test-the-certificate"></a>Pour tester le certificat  
  
1.  Dans le **Dans leternet Dans leformation Services (IIS) Manager**, cliquez sur **Site Web par défaut.**  
  
2.  Dans le volet **Actions**, cliquez sur **Parcourir \*:443(https)**.  
  
3.  Internet Explorer ouvrira et affichera un message indiquant : « le certificat de sécurité de ce site Web présente un problème ». Cet avertissement indique que le certificat associé n'a pas été émis par une autorité de certification reconnue et n'est peut-être pas digne de confiance. Cet avertissement est attendu, donc cliquez sur **Poursuivre sur ce site Web (non recommandé)**.  
  
4.  Si vous êtes invité à **Se connecter à localhost**, entrez un nom d'utilisateur et un mot de passe pour continuer. La page par défaut du site Web doit s'afficher.  
  
## <a name="setting-permissions-for-the-sql-server-replication-listener"></a>Définition des autorisations pour l'écouteur de réplication SQL Server  
 Lorsque l'ordinateur d'un abonné se connecte à l'ordinateur exécutant IIS, l'abonné est authentifié à l'aide du type d'authentification spécifié lors de la configuration d'IIS. Après avoir authentifié l'abonné, IIS vérifie que l'abonné est autorisé à appeler la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous contrôlez les utilisateurs en mesure d'appeler la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en définissant des autorisations pour replisapi.dll. La configuration correcte des autorisations est cruciale pour empêcher les accès non autorisés à la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Pour configurer les autorisations minimales du compte sous lequel est exécuté l'écouteur de réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , suivez la procédure ci-dessous. Les étapes dans la procédure suivante s'appliquent à [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows Server 2008 qui exécute IIS 7.0.  
  
 En plus de suivre les étapes ci-dessous, vérifiez que les noms de connexion requis se trouvent dans la liste d'accès à la publication (PAL, Publication Access List). Pour plus d’informations sur la liste d’accès à la publication, consultez [Sécuriser le serveur de publication](../../relational-databases/replication/security/secure-the-publisher.md).  
  
 **Important** Le compte créé dans cette section est le compte qui se connecte au serveur de publication et au serveur de distribution lors de la synchronisation. Ce compte doit être ajouté comme compte de connexion SQL sur le serveur de distribution et le serveur de publication.  
  
 Le compte utilisé pour l'écouteur de réplication SQL Server doit avoir les autorisations décrites dans la rubrique Sécurité de l'Agent de fusion, dans la section « Se connecter au serveur de publication ou au serveur de distribution ».  
  
 En résumé, le compte doit :  
  
-   être membre de la liste d'accès à la publication (PAL) ;  
  
-   être mappé à une connexion associée à un utilisateur enregistré dans la base de données de publication ;  
  
-   être mappé à une connexion associée à un utilisateur enregistré dans la base de données de distribution ;  
  
-   être autorisé à lire sur le partage de fichiers d'instantanés.  
  
#### <a name="to-configure-the-account-and-permissions"></a>Pour configurer le compte et les autorisations  
  
1.  Créez un compte local sur l'ordinateur exécutant IIS :  
  
    1.  Ouvrez **Gestionnaire de serveur**. Dans le menu Démarrer, cliquez avec le bouton droit sur **Poste de travail**, puis cliquez sur **Gérer**.  
  
    2.  Dans **Gestionnaire de serveur**, développez **Configuration**, puis développez **Utilisateurs et groupes locaux**.  
  
    3.  Cliquez avec le bouton droit sur **Utilisateurs**, puis cliquez sur **Nouvel utilisateur**.  
  
    4.  Entrez un nom d'utilisateur et un mot de passe fort. Désactivez **L'utilisateur doit changer le mot de passe à la prochaine ouverture de session**.  
  
    5.  Cliquez sur **Créer**, puis sur **Fermer**.  
  
2.  Ajoutez le compte au groupe IIS_IUSRS :  
  
    1.  Dans **Gestionnaire de serveur**, développez **Configuration**, développez **Utilisateurs et groupes locaux**, puis cliquez sur **Groupes**.  
  
    2.  Cliquez avec le bouton droit sur **IIS_IUSRS**, puis cliquez sur **Ajouter au groupe**.  
  
    3.  Dans la boîte de dialogue **Propriétés de IIS_IUSRS** , cliquez sur **Ajouter**.  
  
    4.  Dans la boîte de dialogue **Choisir des utilisateurs, des ordinateurs ou des groupes** , ajoutez le compte créé à l'étape 1.  
  
    5.  Vérifiez que **À partir de cet emplacement** indique le nom de l'ordinateur local (et non un domaine). Si ce champ n'indique pas le nom de l'ordinateur local, cliquez sur **Emplacements**. Dans la boîte de dialogue **Emplacements** , choisissez l'ordinateur local, puis cliquez sur **OK**.  
  
    6.  Dans les boîtes de dialogue **Sélectionnez les utilisateurs** et **Propriétés de IIS_IUSRS** , cliquez sur**OK**.  
  
3.  Attribuez des autorisations de compte minimales sur le dossier qui contient replisapi.dll :  
  
    1.  Dans Windows Explorer, cliquez avec le bouton droit sur le dossier créé pour replisapi.dll, puis cliquez sur **Propriétés**.  
  
    2.  Sur l'onglet **Sécurité** , cliquez sur **Modifier**.  
  
    3.  Dans la boîte de dialogue **Autorisations pour \<nom_dossier>**, cliquez sur **Ajouter** pour ajouter le compte créé à l’étape 1.  
  
    4.  Vérifiez que **À partir de cet emplacement** indique le nom de l'ordinateur local (et non un domaine). Si ce champ n'indique pas le nom de l'ordinateur local, cliquez sur **Emplacements**. Dans la boîte de dialogue **Emplacements** , choisissez l'ordinateur local, puis cliquez sur **OK**.  
  
    5.  Vérifiez que le compte dispose uniquement des autorisations **Lecture**, **Lecture et exécution** et **Affichage du contenu du dossier**.  
  
    6.  Sélectionnez tous les utilisateurs ou groupes qui ne requièrent pas l'accès au répertoire, cliquez sur **Supprimer**, puis sur **OK**.  
  
4.  Créez un pool d'applications dans **Gestionnaire des services Internet (IIS)**:  
  
    1.  Dans **Gestionnaire des services Internet (IIS)**, dans le volet **Connexions** développez le nœud de serveur local.  
  
    2.  Cliquez avec le bouton droit sur **Pools d'applications**, puis cliquez sur **Ajouter un pool d'applications**.  
  
    3.  Entrez un nom pour le pool d'applications, laissez les valeurs par défaut pour les champs restants, puis cliquez sur **OK**.  
  
    > [!NOTE]  
    >  Si vous prévoyez d'avoir plus de deux clients de synchronisation simultanés, vous pouvez créer un domaine privé Web. Pour plus d’informations, consultez « Création d’un domaine privé web » dans [Configurer la synchronisation web](../../relational-databases/replication/configure-web-synchronization.md).  
  
5.  Associez le compte au pool d'applications :  
  
    1.  Dans **Gestionnaire des services Internet (IIS)**, développez le nœud du serveur local, puis cliquez sur le nœud **Pools d'applications**.  
  
    2.  Cliquez avec le bouton droit sur le pool d'applications que vous avez créé, puis cliquez sur **Définir les valeurs par défaut des pools d'applications**.  
  
    3.  Dans la boîte de dialogue **Valeurs par défaut du pool d'applications** , faites défiler jusqu'à la section **Traiter le modèle** , puis cliquez sur le champ **Identité** .  
  
    4.  Cliquez sur le bouton de sélection à droite de la ligne **Identité** .  
  
    5.  Cliquez sur la case d'option **Compte personnalisé** , puis sur **Définir**.  
  
    6.  Dans les champs **Nom d'utilisateur** et **Mot de passe** , entrez le compte et le mot de passe créés à l'étape 1, puis cliquez sur **OK**.  
  
    7.  Cliquez sur **OK** pour fermer la boîte de dialogue **Identité du pool d’applications** , puis cliquez à nouveau sur **OK** pour fermer la boîte de dialogue **Valeurs par défaut du pool d’applications**.  
  
6.  Associez le pool d'applications au site Web de réplication :  
  
    1.  Dans **Gestionnaire des services Internet (IIS)**, développez le nœud de serveur local, puis cliquez sur le **Site Web par défaut** (ou votre site de synchronisation Web s'il est différent du site Web par défaut).  
  
    2.  Dans le volet **Actions** , sous **Gérer le site Web**, cliquez sur **Paramètres avancés**.  
  
    3.  Dans la boîte de dialogue **Paramètres avancés** , cliquez sur le bouton de sélection à droite de **Pool d'applications**.  
  
    4.  Dans la liste déroulante **Pool d'applications** , sélectionnez le pool d'applications créé à l'étape 4, puis cliquez sur **OK**.  
  
    5.  Cliquez à nouveau sur **OK** pour fermer Paramètres avancés.  
  
## <a name="testing-the-connection-to-replisapidll"></a>Test de la connexion à replisapi.dll  
 Exécutez la synchronisation Web en mode de diagnostic pour tester la connexion à l'ordinateur exécutant IIS et pour vérifier que le certificat SSL (Secure Sockets Layer) est correctement installé. Pour exécuter la synchronisation Web en mode de diagnostic, vous devez avoir la qualité d'administrateur sur l'ordinateur qui exécute les services Internet (IIS).  
  
#### <a name="to-test-the-connection-to-replisapidll"></a>Pour tester la connexion à replisapi.dll  
  
1.  Vérifiez que les paramètres du réseau local (LAN) sur l'Abonné sont corrects :  
  
    1.  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Explorer, dans le menu **Outils** , cliquez sur **Options Internet**.  
  
    2.  Sur l'onglet **Connexions** , cliquez sur **Paramètres LAN**.  
  
    3.  S'il n'y a pas de serveur proxy utilisé sur le réseau local, désactivez **Détecter automatiquement les paramètres de connexion** et **Utiliser un serveur proxy pour votre réseau local**.  
  
    4.  Si un serveur proxy est utilisé, cliquez sur **Utiliser un serveur proxy pour votre réseau local** et **Ne pas utiliser de serveur proxy pour les adresses locales**, puis cliquez sur **OK**.  
  
2.  Sur l'Abonné, dans Internet Explorer, connectez-vous au serveur en mode de diagnostic en ajoutant `?diag` à l'adresse pour le fichier replisapi.dll. Par exemple : `https://server.domain.com/directory/replisapi.dll?diag`.  
  
    > [!NOTE]  
    >  Dans l'exemple ci-dessus, **server.domain.com** doit être remplacé par le nom exact de **Délivré à** répertorié sous la section **Certificats de serveur** dans Gestionnaire des services IIS.  
  
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
 [Synchronisation web pour la réplication de fusion](../../relational-databases/replication/web-synchronization-for-merge-replication.md)   
 [Configurer la synchronisation web](../../relational-databases/replication/configure-web-synchronization.md)  
  
  
