---
title: Configurer la synchronisation Web | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- SQL10.REP.CONFIGWEBSYNCWIZARD.SNAPSHARE.F1
- SQL13.REP.CONFIGWEBSYNCWIZARD.SNAPSHARE.F1
- SQL10.REP.CONFIGWEBSYNCWIZARD.VIRDIRINFO.F1
- SQL13.REP.CONFIGWEBSYNCWIZARD.CLIENTAUTH.F1
- SQL10.REP.CONFIGWEBSYNCWIZARD.DIRACCESS.F1
- SQL10.REP.CONFIGWEBSYNCWIZARD.SUBTYPE.F1
- SQL10.REP.CONFIGWEBSYNCWIZARD.ANONACCESS.F1
- SQL10.REP.CONFIGWEBSYNCWIZARD.WEBSERV.F1
- SQL10.REP.CONFIGWEBSYNCWIZARD.CLIENTAUTH.F1
- SQL13.REP.CONFIGWEBSYNCWIZARD.SUBTYPE.F1
- SQL10.REP.CONFIGWEBSYNCWIZARD.COMPLETEWIZ.F1
- SQL13.REP.CONFIGWEBSYNCWIZARD.DIRACCESS.F1
- SQL13.REP.CONFIGWEBSYNCWIZARD.ANONACCESS.F1
- SQL13.REP.CONFIGWEBSYNCWIZARD.AUTHACCESS.F1
- SQL10.REP.CONFIGWEBSYNCWIZARD.AUTHACCESS.F1
- SQL13.REP.CONFIGWEBSYNCWIZARD.COMPLETEWIZ.F1
- SQL13.REP.CONFIGWEBSYNCWIZARD.VIRDIRINFO.F1
- SQL13.REP.CONFIGWEBSYNCWIZARD.WEBSERV.F1
helpviewer_keywords:
- Web synchronization, security best practices
- Web synchronization, configuring
ms.assetid: 21f8e4d4-cd07-4856-98f0-9c9890ebbc82
caps.latest.revision: 74
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e806f794a378672b28aa2334eda1afe076bd2ab9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-web-synchronization"></a>Configurer la synchronisation Web
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  L'option de synchronisation Web de la réplication de fusion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet de répliquer les données à l'aide du protocole HTTPS sur Internet. Pour utiliser la synchronisation Web, vous devez d'abord effectuer les actions de configuration suivantes :  
  
1.  Créez de nouveaux comptes de domaine et mappez les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
2.  Configurez l'ordinateur qui exécute [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Information Services (IIS) pour synchroniser les abonnements.  
  
3.  Configurez une publication de fusion pour autoriser la synchronisation Web.  
  
4.  Configurez un ou plusieurs abonnements de sorte qu'ils utilisent la synchronisation Web.  
  
> [!NOTE]  
>  Si vous envisagez de répliquer d'importants volumes de données ou d'utiliser des types de données volumineuses, tels que **varchar(max)**, lisez la section « Réplication d'importants volumes de données » dans cette rubrique.  
  
 Pour configurer correctement la synchronisation Web, vous devez décider comment configurer la sécurité pour répondre à vos besoins et stratégies spécifiques. Il est préférable de prendre ces décisions et de créer les comptes nécessaires avant d'essayer de configurer IIS, la publication et les abonnements.  
  
 Dans les procédures qui suivent, une configuration de la sécurité simplifiée utilisant des comptes locaux est décrite, pour des raisons de concision. Cette configuration simplifiée est appropriée pour les installations où IIS, le serveur de publication et le serveur de distribution [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécutent sur le même ordinateur, bien que l'utilisation d'une topologie à plusieurs serveurs soit beaucoup plus vraisemblable (et recommandée) pour une installation de production. Vous pouvez substituer des comptes de domaine aux comptes locaux dans les procédures.  
  
## <a name="creating-new-accounts-and-mapping-sql-server-logins"></a>Création de nouveaux comptes et mappage des connexions SQL Server  
 L'écouteur de réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (replisapi.dll) se connecte au serveur de publication en empruntant l'identité du compte spécifié pour le pool d'applications associé au site Web de réplication.  
  
 Le compte utilisé pour l'écouteur de réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit avoir les autorisations décrites dans [Merge Agent Security](../../relational-databases/replication/merge-agent-security.md), sous la section « Connexion au serveur de publication ou au serveur de distribution ». En résumé, le compte doit :  
  
-   être membre de la liste d'accès à la publication (PAL) ;  
  
-   être mappé à une connexion associée à un utilisateur enregistré dans la base de données de publication ;  
  
-   être mappé à une connexion associée à un utilisateur enregistré dans la base de données de distribution ;  
  
-   être autorisé à lire sur le partage de fichiers d'instantanés.  
  
 Si vous utilisez la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la première fois, vous devez également créer des comptes et des connexions pour les agents de réplication. Pour plus d'informations, consultez les sections « Configuration de la publication » et « Configuration de l'abonnement » dans cette rubrique.  
  
 Avant de configurer la synchronisation Web, il est recommandé de lire la section « Meilleures pratiques de sécurité pour la synchronisation Web » plus loin dans cette rubrique. Pour plus d'informations sur la sécurité de la synchronisation Web, consultez [Security Architecture for Web Synchronization](../../relational-databases/replication/security/security-architecture-for-web-synchronization.md).  
  
## <a name="configuring-the-computer-that-is-running-iis"></a>Configuration de l'ordinateur qui exécute IIS  
 La synchronisation Web requiert l'installation et la configuration d'IIS. Vous devez connaître l'URL du site web de réplication pour pouvoir configurer une publication en vue d'utiliser la synchronisation web.  
  
 La synchronisation web est prise en charge sur IIS à compter de la version 5.0. L'Assistant Configuration de la synchronisation Web n'est pas pris en charge sur IIS version 7.0. À partir de SQL Server 2012, nous recommandons aux utilisateurs d’installer SQL Server avec la réplication pour utiliser le composant de synchronisation web sur le serveur IIS. Il peut s’agir de l’édition gratuite de SQL Server Express.  
  
 SSL est obligatoire pour la synchronisation web. Vous aurez besoin d'un certificat de sécurité délivré par une autorité de certification. Vous pouvez utiliser un certificat de sécurité auto-émis à des fins de test uniquement.  
   
  
 **Pour configurer IIS pour la synchronisation Web**  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] : [Configurer IIS pour la synchronisation web](../../relational-databases/replication/configure-iis-for-web-synchronization.md)  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] : [Configurer IIS 7 pour la synchronisation web](../../relational-databases/replication/configure-iis-7-for-web-synchronization.md)  
  
## <a name="creating-a-web-garden"></a>Création d'un domaine privé Web  
 L'écouteur de réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge deux opérations de synchronisation simultanées par thread. Si cette limite est dépassée, l'écouteur de réplication risque de cesser de répondre. Le nombre de threads alloués à replisapi.dll est déterminé par la propriété Nombre maximal de processus de travail du pool d'applications. Par défaut, cette propriété a la valeur 1.  
  
 Vous pouvez prendre en charge un nombre supérieur d'opérations de synchronisation simultanées par UC en augmentant la valeur de la propriété Nombre maximal de processus de travail. La montée en puissance parallèle en augmentant le nombre de processus de travail par UC s'appelle la création d'un « domaine privé Web ».  
  
 Le domaine privé Web autorisera plus de deux abonnés à se synchroniser en même temps. Cela augmente également l'utilisation du processeur par replisapi.dll, ce qui peut nuire aux performances générales du serveur. Il est important de prendre ces éléments en compte lorsque vous choisissez une valeur pour Nombre maximal de processus de travail.  
  
#### <a name="to-increase-maximum-worker-processes-in-iis-7"></a>Pour augmenter le nombre maximal de processus de travail dans IIS 7  
  
1.  Dans **Gestionnaire des services Internet (IIS)**, développez le nœud du serveur local, puis cliquez sur le nœud **Pool d'applications** .  
  
2.  Sélectionnez le pool d'applications associé au site de synchronisation Web, puis cliquez sur **Paramètres avancés** dans le volet **Actions** .  
  
3.  Dans la boîte de dialogue Paramètres avancés, sous l'en-tête **Traiter le modèle** , cliquez sur la ligne intitulée **Nombre maximal de processus de travail**. Modifiez la valeur de la propriété, puis cliquez sur **OK**.  
  
## <a name="configuring-the-publication"></a>Configuration de la publication  
 Pour utiliser la synchronisation Web, vous devez créer une publication de la même manière que pour une topologie de fusion standard. Pour plus d’informations, consultez [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
 Une fois la publication créée, activez l'option permettant d'autoriser la synchronisation Web à l'aide d'une des méthodes suivantes : [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]ou les objets RMO (Replication Management Objects). Pour activer la synchronisation Web, vous devez fournir l'adresse du serveur Web pour les connexions des abonnés.  
  
 Si vous utilisez un serveur de publication pour la première fois, vous devez également configurer un serveur de distribution et un partage de fichiers d'instantanés. L'Agent de fusion de chaque Abonné doit disposer d'autorisations de lecture sur le partage d'instantanés. Pour plus d’informations, consultez [Configurer la distribution](../../relational-databases/replication/configure-distribution.md) et [Sécuriser le dossier d’instantanés](../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
 **gen** est un mot réservé dans les fichiers XML de websync. Ne pas tentez de publier des tables contenant des colonnes nommées **gen**.  
  
## <a name="configuring-the-subscription"></a>Configuration de l'abonnement  
 Après avoir activé une publication et configuré IIS, créez un abonnement par extraction et spécifiez que cet abonnement doit être synchronisé à l'aide d'IIS. (La synchronisation Web est prise en charge uniquement pour les abonnements par extraction de données.)  
  
## <a name="upgrading-from-an-earlier-version-of-sql-server"></a>Mise à niveau à partir d'une version antérieure de SQL Server  
 Si vous disposez d'une topologie de la synchronisation Web existante configurée et si vous mettez à niveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez vous assurer que la version la plus récente de Replisapi.dll est copiée dans le répertoire virtuel utilisé par la synchronisation Web. Par défaut, la version la plus récente de Replisapi.dll se trouve dans C:\Program Files\Microsoft SQL Server \\<nnn\>\COM.  
  
## <a name="replicating-large-volumes-of-data"></a>Réplication d'importants volumes de données  
 Pour éviter les problèmes de mémoire au niveau des ordinateurs des abonnés, la synchronisation Web utilise une taille maximale par défaut de 100 Mo pour le fichier XML utilisé pour le transfert des modifications. La limite peut être augmentée en définissant la clé de Registre suivante :  
  
 **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\100\Replication**  
  
 **WebSyncMaxXmlSize DWORD 2000000**  
  
 La plage des valeurs acceptables pour cette clé est de 100 Mo à 4 Go. La valeur est spécifiée en Ko. Définir ce paramètre avec une valeur élevée ne garantit pas que vous puissiez synchroniser ce volume de données. La limite effective est contrainte par la mémoire contiguë qui est disponible sur l'ordinateur de l'Abonné. Si vous avez besoin d'une valeur supérieure à 100 Mo, nous vous recommandons d'augmenter la valeur de façon incrémentielle et de tester la consommation de mémoire avec une charge de travail standard au niveau de l'Abonné.  
  
 La taille maximale du fichier XML est de 4 Go, mais la réplication synchronise les modifications de ce fichier dans des lots. La taille maximale des lots de données et de métadonnées est de 25 Mo. Vous devez vous assurer que le volume de données de chaque lot ne dépasse pas approximativement 20 Mo, ce qui permet de prendre en charge les métadonnées et toute surcharge additionnelle. Cette limite a les conséquences suivantes :  
  
-   Vous ne pouvez pas répliquer une colonne qui générerait un volume de données et de métadonnées supérieur à 25 Mo. Cela peut être un problème lors de la réplication de lignes contenant des types de données volumineuses, tels que **varchar(max)**.  
  
-   Si vous répliquez d'importants volumes de données, vous pouvez être amené à ajuster la taille de lot de l'Agent de fusion.  
  
 La taille de lot pour la réplication de fusion est mesurée en *générations*, lesquelles sont des collections de modifications par article. Le nombre de générations dans un lot est spécifié à l’aide des paramètres –**DownloadGenerationsPerBatch** et –**UploadGenerationsPerBatch** de l’Agent de fusion. Pour plus d’informations, voir [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md).  
  
 Pour d'importants volumes de données, spécifiez un petit nombre pour chaque paramètre de traitement par lot. Nous vous recommandons de commencer avec une valeur de 10, puis d'ajuster cette valeur selon les besoins et les performances des applications. En général, ces paramètres sont spécifiés dans un profil d'agent. Pour plus d'informations sur ces profils, consultez [Replication Agent Profiles](../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
## <a name="security-best-practices-for-web-synchronization"></a>Meilleures pratiques de sécurité pour la synchronisation Web  
 La synchronisation Web propose un vaste choix de paramètres associés à la sécurité. Nous vous recommandons l'approche suivante :  
  
-   Le serveur de distribution et le serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent se trouver sur le même ordinateur (configuration courante pour la réplication de fusion). IIS doit cependant être installé sur un ordinateur distinct.  
  
-   Utilisez SSL (Secure Sockets Layer) pour chiffrer la connexion entre l'Abonné et l'ordinateur exécutant IIS. Ceci est obligatoire pour la synchronisation Web.  
  
-   Utilisez l'authentification de base pour les connexions de l'Abonné vers IIS. En utilisant l'authentification de base, IIS peut établir des connexions au serveur de publication/distribution au nom de l'Abonné sans recourir à la délégation. La délégation est nécessaire en cas d'utilisation de l'authentification intégrée.  
  
    > [!NOTE]  
    >  L'authentification de base est la méthode servant à transmettre les informations d'identification à IIS. L'authentification de base n'empêche pas de spécifier des comptes de domaine Windows pour les connexions qui sont établies vers IIS.  
  
-   Spécifiez que l'Agent d'instantané doit s'exécuter sous un compte de domaine Windows et qu'il doit se connecter sous ce compte. (Il s'agit de la configuration par défaut.) Spécifiez que chaque Agent de fusion doit s'exécuter sous le compte de domaine de l'utilisateur utilisant l'ordinateur de l'Abonné et qu'il doit se connecter sous ce compte.  
  
     Pour plus d'informations sur les autorisations requises par les agents, consultez [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
-   Spécifiez le même compte de domaine que celui utilisé par l'Agent de fusion lorsque vous indiquez un compte et un mot de passe dans la page **Informations sur le serveur Web** de l'Assistant Nouvel abonnement ou lorsque vous indiquez des valeurs pour les paramètres **@internet_url** et **@internet_login** de [sp_addpullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Ce compte doit disposer d'autorisations en lecture pour le partage des instantanés.  
  
-   Chaque publication doit utiliser un répertoire virtuel distinct pour IIS.  
  
-   Le compte sous lequel s'exécute l'écouteur de réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Replisapi.dll) est également le compte qui se connecte au serveur de publication et au serveur de distribution lors de la synchronisation. Ce compte doit être mappé à un compte de connexion SQL sur le serveur de publication et le serveur de distribution. Pour plus d’informations, consultez la section « Définition des autorisations pour l’écouteur de réplication SQL Server » dans la rubrique [Configurer IIS pour la synchronisation Web](../../relational-databases/replication/configure-iis-for-web-synchronization.md).  
  
-   Vous pouvez utiliser FTP pour remettre l'instantané du serveur de publication à l'ordinateur exécutant IIS. L'instantané est toujours remis de l'ordinateur exécutant IIS à l'Abonné au moyen du protocole HTTPS. Pour plus d’informations, consultez [Transférer des instantanés via FTP](../../relational-databases/replication/transfer-snapshots-through-ftp.md).  
  
-   Si les serveurs de la topologie de réplication se trouvent derrière un pare-feu, vous devrez peut-être ouvrir les ports dans le pare-feu afin d'activer la synchronisation Web.  
  
    -   L'ordinateur de l'abonné se connecte à l'ordinateur qui exécute IIS sur HTTPS à l'aide du protocole SSL, qui est généralement configuré pour utiliser le port 443. Les abonnés[!INCLUDE[ssEW](../../includes/ssew-md.md)] peuvent également se connecter via le protocole HTTP, qui utilise en règle générale le port 80.  
  
    -   L'ordinateur chargé d'exécuter IIS se connecte généralement au serveur de publication ou au serveur de distribution via le port 1433 (instance par défaut). Lorsque le serveur de publication ou le serveur de distribution correspond à une instance nommée sur un serveur avec une autre instance par défaut, le système utilise habituellement le port 1500 pour se connecter à l'instance nommée.  
  
    -   Si l'ordinateur exécutant IIS est séparé du serveur de distribution par un pare-feu et si un partage FTP est employé pour la remise des instantanés, vous devez ouvrir les ports utilisés pour le partage FTP. Pour plus d’informations, consultez [Transférer des instantanés via FTP](../../relational-databases/replication/transfer-snapshots-through-ftp.md).  
  
> [!IMPORTANT]  
>  L'ouverture de ports dans votre pare-feu peut exposer votre serveur à des attaques malveillantes. Assurez-vous de comprendre le fonctionnement des systèmes de pare-feu avant d'ouvrir des ports. Pour plus d'informations, consultez [Security Considerations for a SQL Server Installation](../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Synchronisation web pour la réplication de fusion](../../relational-databases/replication/web-synchronization-for-merge-replication.md)  
  
  
