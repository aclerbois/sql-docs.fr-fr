---
title: "Configuration du matériel (système de plateforme Analytique)"
author: barbkess
ms.author: barbkess
manager: jhubbard
ms.prod: analytics-platform-system
ms.prod_service: mpp-data-warehouse
ms.service: 
ms.component: 
ms.technology: mpp-data-warehouse
ms.custom: 
ms.date: 01/05/2017
ms.reviewer: na
ms.suite: sql
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 064e7485-7026-4acf-8084-f5d30757d177
caps.latest.revision: "43"
ms.openlocfilehash: 05670f1727691b1abc0fd98dd5970c7697725b8e
ms.sourcegitcommit: cc71f1027884462c359effb898390c8d97eaa414
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
# <a name="appliance-configuration"></a>Configuration du matériel
Fournit des listes de contrôle pour les tâches requises pour configurer le système de plateforme d’Analytique pour votre propre environnement. Ces tâches de configuration sont nécessaires avant de pouvoir utiliser l’application.  
  
> [!WARNING]  
> À l’aide du système de plateforme Analytique**Configuration Manager** est le meilleur moyen et le seul moyen, pour effectuer les tâches disponibles dans l’outil pris en charge.  
  
## <a name="BeforeTasks"></a>Avant de commencer  
  
### <a name="prerequisites"></a>Prerequisites  
  
1.  L’application doit être installée dans le centre de données et sous tension.  
  
2.  Vérifiez que vous disposez des informations suivantes, qui sont fournies par votre fabricant de matériel :  
  
    -   Adresse IP externe du nœud de contrôle de PDW (*PDW_region -*CTL01)  
  
    -   Nom de domaine d’application  
  
    -   Nom d’utilisateur et mot de passe de l’administrateur de domaine d’application  
  
3.  Obtenir un certificat approuvé. Vous allez importer ce dans une étape ultérieure afin d’autoriser les clients à se connecter à la **Console d’administration** avec des connexions sécurisées. Enregistrez le certificat sur le nœud de contrôle dans le fichier PFX protégé par mot de passe.  
  
4.  Lancer le **Configuration Manager**, en procédant comme suit :  
  
    1.  Utilisez **Bureau à distance** pour se connecter au nœud de contrôle de PDW (*PDW_region*-CTL01). (Vous devrez peut-être vous connecter avec l’adresse IP externe pour CTL01).  
  
    2.  Lancer le **Configuration Manager** à partir de la **Démarrer** menu du nœud de contrôle de PDW. Le premier écran de Configuration Manager affiche la topologie d’appliances, qui a été créée par le fabricant de votre matériel. Il s’agit d’une liste de nœuds matériel reconnu par votre logiciel de SQL Server PDW dans le cadre de votre application. Vous ne devez pas modifier les paramètres sur l’écran de la topologie d’appliances.  
  
## <a name="CMTasks"></a>Effectuer des tâches de Configuration Manager  
Le serveur SQL Server PDW**Configuration Manager** (PDWCM) est un outil d’administration de matériel que les administrateurs système SQL Server PDW utilisent pour effectuer des opérations au niveau du matériel et pour modifier les paramètres au niveau du matériel. Par exemple, utilisez PDWCM pour réinitialiser les mots de passe, définir le fuseau horaire, modifier les adresses IP, configurer les certificats SSL, activer l’accès à distance via le pare-feu, démarrer ou arrêter l’application et définir l’initialisation instantanée des fichiers.  
  
Utilisez **Configuration Manager** pour effectuer les tâches de configuration suivantes.  
  
|Tâche de configuration|Description|  
|----------------------|---------------|  
|Vous familiariser avec les noms des composants physiques|[PDW et les composants physiques de l’infrastructure de matériel &#40; Système de plateforme Analytique &#41;](pdw-and-appliance-fabric-physical-components.md)|  
|Lancez le Gestionnaire de Configuration SQL Server PDW|[Lancez le Gestionnaire de Configuration &#40; Système de plateforme Analytique &#41;](launch-the-configuration-manager.md)|  
|Modification du mot de passe administrateur|La solution possède un privée Windows Active Directory Domain Services qui est utilisé pour authentifier les nœuds au sein de l’application.<br /><br />Fabricant de votre matériel, l’application avec un mot de passe administrateur de domaine par défaut. Il doit être remplacé par un mot de passe sécurisé.<br /><br />Le **Configuration Manager** est le seul pris en charge permettant de modifier le mot de passe administrateur.<br /><br />Pour plus d’informations, consultez [réinitialisation de mot de passe &#40; Système de plateforme Analytique &#41; ](password-reset.md).|  
|Modifier le mot de passe pour le **sa** ouverture de session|SQL Server PDW a une ouverture de session administrateur système nommé **sa**. Le **sa** ouverture de session possède tous les privilèges. Il peut accorder, refuser ou révoquer une autorisation. Il peut également voir toutes les vues système.<br /><br />Pour plus d’informations, consultez [réinitialisation de mot de passe &#40; Système de plateforme Analytique &#41; ](password-reset.md).|  
|Définir le fuseau horaire de matériel|Définir le temps (local ou autres souhaité) pour tous les nœuds de l’appliance.<br /><br />Pour plus d’informations, consultez [Configuration du fuseau horaire matériel &#40; Système de plateforme Analytique &#41; ](appliance-time-zone-configuration.md).|  
|Spécifiez les paramètres de réseau externe pour l’application SQL Server PDW|[Configuration réseau de le &#40; Système de plateforme Analytique &#41;](appliance-network-configuration.md)|  
|Importer un certificat de sécurité de la Console d’administration|Un certificat peut fournir des connexions de Secure Sockets Layer (SSL) sur HTTPS à le [contrôler le matériel à l’aide de la Console d’administration &#40; Système de plateforme Analytique &#41; ](monitor-the-appliance-by-using-the-admin-console.md). Par défaut, le **Console d’administration** inclut un certificat auto-signé qui fournit la confidentialité, mais pas l’authentification du serveur. Ce certificat renvoie également une erreur dans Internet Explorer indiquant : « Il existe un problème avec le certificat de sécurité de ce site Web » lorsque l’utilisateur se connecte. Bien que cette connexion chiffre les données en vol entre le client et le serveur, la connexion est toujours exposé à des personnes malveillantes.<br /><br />Les administrateurs de SQL Server PDW doivent immédiatement acquérir un certificat lié à une autorité de certification de confiance reconnue par les clients, afin de disposer d’une connexion sécurisée et de supprimer l’erreur qui signale d’Internet Explorer. Cela nécessite un nom de domaine complet qui mappe l’adresse IP virtuelle du nœud de contrôle (recommandé) ou un nom de certificat qui correspond à la valeur par les utilisateurs dans leurs adresses du navigateur barres pour accéder à la Console d’administration.<br /><br />Utilisez le **Configuration Manager** pour ajouter ou supprimer des certificats de confiance. Directement à l’aide de l’outil de Configuration de certificat Microsoft Windows HTTP Services (`winHttpCertCfg.exe`) pour gérer les certificats non pris en charge.<br /><br />Pour plus d’informations, consultez [fourniture des certificats PDW &#40; Système de plateforme Analytique &#41; ](pdw-certificate-provisioning.md).|  
|Activer ou désactiver des règles de pare-feu Windows qui autorisent ou empêchent l’accès à des ports spécifiques sur le dispositif de SQL Server PDW.|Votre fabricant de matériel sera configurer et activer les règles de pare-feu qui sont nécessaires pour l’application fonctionne correctement. Dans la plupart des cas ne pas activer ou désactiver des règles de pare-feu.<br /><br />Pour plus d’informations, consultez [Configuration du pare-feu PDW &#40; Système de plateforme Analytique &#41; ](pdw-firewall-configuration.md).|  
|Démarrer et arrêter l’application de SQL Server PDW|Arrête ou démarre l’application SQL Server PDW. Pour plus d’informations, consultez [PDW l’état des Services &#40; Système de plateforme Analytique &#41; ](pdw-services-status.md).|  
|Examiner les options d’initialisation instantanée des fichiers à l’aide de la **des privilèges** boîte de dialogue|Initialisation instantanée des fichiers est une fonctionnalité de SQL Server qui permet les opérations de fichier de données pour s’exécuter plus rapidement. Il est activé sur SQL Server PDW uniquement si le compte Service réseau dispose du privilège SE_MANAGE_VOLUME_NAME. Il est désactivé par défaut.<br /><br />Pour plus d’informations, consultez [Configuration de l’initialisation instantanée fichier &#40; Système de plateforme Analytique &#41; ](instant-file-initialization-configuration.md).|  
|Restaurer la base de données master à partir d’une sauvegarde|Supprime l’actuel **master** de base de données et la remplace par une sauvegarde. Pour plus d’informations, consultez [restaurer la base de données Master &#40; Système de plateforme Analytique &#41; ](restore-the-master-database.md).|  
  
## <a name="AddTasks"></a>Effectuer des tâches de Configuration supplémentaires  
Après avoir effectué la **Configuration Manager** effectuer des tâches, la liste suivante des tâches de configuration supplémentaires. Certaines de ces tâches sont facultatives.  
  
|Tâche de configuration|Description|  
|----------------------|---------------|  
|Un logiciel antivirus tiers permettre être installé et configuré sur l’appliance SQL Server PDW faisant face à l’extérieur des nœuds.<br /><br />(Facultatif)|Pour plus d’informations, consultez [logiciel Antivirus &#40; Système de plateforme Analytique &#41; ](antivirus-software.md).|  
|Le mot de passe DSRM peut être modifié.<br /><br />(Facultatif)|Pour plus d’informations, consultez [définir un mot de passe administrateur pour ouvrir une session sur les nœuds AD dans le Mode de restauration des Services d’annuaire &#40; mode DSRM &#41; &#40; Système de plateforme Analytique &#41; ](set-admin-password-for-logging-on-to-ad-nodes-in-directory-services-restore-mode.md).|  
|Configurer l’application pour recevoir des mises à jour logicielles<br /><br />(Recommandé)|L’application doit être configuré pour recevoir des mises à jour au logiciel sous-jacent et SQL Server PDW.<br /><br />Pour plus d’informations, consultez [configurer Windows Server Update Services &#40; WSUS &#41; &#40; Système de plateforme Analytique &#41; ](configure-windows-server-update-services-wsus.md). Pour plus d’informations sur WSUS, consultez [logiciel maintenance &#40; Système de plateforme Analytique &#41; ](software-servicing.md).|  
|Configurez la connectivité à des données externes telles que Hadoop ou Azure blob storage.<br /><br />(Facultatif)|Pour plus d’informations, consultez [configurer la connectivité PolyBase pour données externes &#40; Système de plateforme Analytique &#41; ](configure-polybase-connectivity-to-external-data.md).|  
|Configurez le logiciel Antivirus<br /><br />(Facultatif)|Les solutions antivirus tiers peut être utilisé pour protéger les nœuds externe mais n’est pas obligatoire. Suivez les instructions dans.|  
|Configurer les cartes réseau InfiniBand sur la sauvegarde et de chargement des serveurs<br /><br />(Facultatif)|Pour configurer la sauvegarde et chargement des serveurs de se connecter à SQL Server PDW en utilisant le réseau InfiniBand, vous devez configurer les cartes réseau pour autoriser l’appliance DNS pour résoudre la connexion InfiniBand au réseau InfiniBand actuellement actif.|  
|Configurer pour envoyer des données de télémétrie à Microsoft<br /><br />(Facultatif)|Pour configurer le système de plateforme d’Analytique pour envoyer des données de télémétrie à Microsoft, vous devez exécuter un script PowerShell sur le nœud de contrôle. Pour obtenir des instructions, consultez [envoyer des commentaires de télémétrie à Microsoft &#40; SQL Server PDW &#41; ](send-telemetry-feedback-to-microsoft-sql-server-pdw.md).|  
  
## <a name="see-also"></a>Voir aussi  
[Le logiciel antivirus &#40; Système de plateforme Analytique &#41;](antivirus-software.md)  
[Configurer les cartes réseau InfiniBand &#40; SQL Server PDW &#41;](configure-infiniband-network-adapters.md)  
  
