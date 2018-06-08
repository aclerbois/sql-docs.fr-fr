---
title: Configuration des protocoles réseau par défaut de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 07/11/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- protocols [SQL Server], default settings
- default protocols, after install
ms.assetid: 635ea361-a797-4971-bd05-e3415862bc5c
caps.latest.revision: 4
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 93a6669821de69a5dff3a7ab58ff35cf3f24fee8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="default-sql-server-network-protocol-configuration"></a>Configuration des protocoles réseau par défaut de SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Pour renforcer la sécurité, [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] désactive la connectivité réseau de certaines nouvelles installations. La connectivité réseau faisant appel à TCP/IP n’est pas désactivée si vous utilisez l’édition Enterprise, Standard, Evaluation ou Workgroup ou qu’une installation précédente de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] est présente. Pour toutes les installations, le protocole de mémoire partagée est activé de manière à autoriser les connexions locales au serveur. Le service [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] peut être arrêté si les conditions et options d'installation l'exigent.

Utilisez le nœud Configuration du réseau [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] du Gestionnaire de configuration [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] pour configurer les protocoles réseau après l’installation. Utilisez le nœud Services [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] du Gestionnaire de configuration [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] pour configurer le service [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] Browser de sorte qu’il démarre automatiquement. Pour plus d’informations, consultez [Activer ou désactiver un protocole réseau de serveur](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md).


## <a name="default-configuration"></a>Configuration par défaut

Le tableau suivant décrit la configuration après l'installation.

Édition | Nouvelle installation ou installation précédente présente | Mémoire partagée | TCP/IP    | Canaux nommés
| -------- | -- | -- | -- | --  |  
Enterprise  | Nouvelle installation  | Activé   | Activé   | Désactivés pour les connexions réseau.
Standard    | Nouvelle installation  | Activé   | Activé   | Désactivés pour les connexions réseau.
Web | Nouvelle installation  | Activé   | Activé   | Désactivés pour les connexions réseau.
Développeur   | Nouvelle installation  | Activé   | Désactivé  | Désactivés pour les connexions réseau.
Evaluation  | Nouvelle installation  | Activé   | Activé   | Désactivés pour les connexions réseau.
SQL Server Express  | Nouvelle installation  | Activé   | Désactivé  | Désactivés pour les connexions réseau.
Toutes les éditions    | L'installation précédente est présente mais n'est pas mise à niveau.   | Même configuration que pour une nouvelle installation  | Même configuration que pour une nouvelle installation  | Même configuration que pour une nouvelle installation
Toutes les éditions    | UPGRADE   | Activé   | Les paramètres de l'installation précédente sont conservés.    | Les paramètres de l'installation précédente sont conservés.


>[!NOTE]
> Si l’instance est en cours d’exécution sur un cluster de basculement [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], elle écoute sur les ports de chaque adresse IP sélectionnée pour [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] pendant l’installation de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
 
>[!NOTE]
> Lorsque vous installez [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] avec des arguments d'invite de commandes, vous pouvez utiliser les paramètres `TCPENABLED` et `NPENABLED` pour spécifier les protocoles à activer. Pour plus d’informations, consultez [Installer SQL Server à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md).

## <a name="creating-a-connection-string"></a>Création d’une chaîne de connexion

Pour obtenir des exemples de chaînes de connexion, consultez les rubriques suivantes :
* [Création d'une chaîne de connexion valide à l'aide du protocole de mémoire partagée](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)
* [Création d’une chaîne de connexion valide à l’aide du protocole TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)



## <a name="includessnoversionmdincludesssnoversion-mdmd-browser-settings"></a>[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] Paramètres de Browser

Le service [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] Browser peut être configuré pour démarrer automatiquement au cours de l'installation. Le paramètre par défaut est de démarrer automatiquement dans les conditions suivantes :

* lors de la mise à niveau d'une installation ;
* pendant une installation côte à côte avec une autre instance [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ;
* lors d'une installation sur un cluster ;
* lors de l’installation d’une instance nommée du moteur de base de données, notamment toutes les instances de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ;
* lors de l’installation d’une instance nommée d’Analysis Services.

## <a name="see-also"></a> Voir aussi

[Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)

[Configuration de la surface d'exposition](../../relational-databases/security/surface-area-configuration.md)  



