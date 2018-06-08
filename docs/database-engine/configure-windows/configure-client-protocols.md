---
title: Configurer des protocoles clients | Microsoft Docs
ms.custom: ''
ms.date: 07/27/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- default protocols
- network protocols [SQL Server], client configuration
- TCP/IP [SQL Server], client protocols
- disabling client protocols
- ordering protocols [SQL Server]
- protocols [SQL Server], order for client computers
- configure client protocols
- client protocols [SQL Server]
- protocols [SQL Server], client configuration
- default protocols, client
ms.assetid: 3dfa2702-ba65-43b4-a777-6727846e133a
caps.latest.revision: 35
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 0602c0605ac549184d7eee0fa752d28c0c591367
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-client-protocols"></a>configurer des protocoles clients
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit comment configurer les protocoles clients utilisés par les applications clientes dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la communication cliente par le biais du protocole réseau TCP/IP et du protocole des canaux nommés. Le protocole de mémoire partagée est également disponible si le client se connecte à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur le même ordinateur. Il existe trois méthodes courantes de sélection du protocole.  
  
-   Configurer toutes les applications clientes de manière à ce qu'elles utilisent le même protocole réseau, en définissant l'ordre des protocoles dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Configurer une application cliente spécifique de manière à ce qu'elle utilise un protocole réseau particulier, en créant un alias. Pour plus d’informations, consultez [Créer ou supprimer un alias de serveur devant être utilisé par un client &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md).  
  
-   Certaines applications clientes, telles que sqlcmd.exe, peuvent spécifier le protocole dans la chaîne de connexion. Pour plus d’informations, consultez [Se connecter au moteur de base de données avec sqlcmd](../../relational-databases/scripting/sqlcmd-connect-to-the-database-engine.md).  
  
##  <a name="SSMSProcedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
  
###  <a name="EnableDisable"></a> Pour activer ou désactiver un protocole client  
  
1.  Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], développez **Configuration de SQL Server Native Client**, cliquez avec le bouton droit sur **Protocoles clients**, puis sélectionnez **Propriétés**.  
  
2.  Dans la zone **Protocoles désactivés**, cliquez sur un protocole, puis sélectionnez **Activer** pour l’activer.  
  
3.  Dans la zone **Protocoles activés**, cliquez sur un protocole, puis sélectionnez **Désactiver** pour le désactiver.  
  
###  <a name="ChangeDefault"></a> Pour modifier le protocole par défaut ou l’ordre des protocoles pour les ordinateurs clients  
  
1.  Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], développez **Configuration de SQL Server Native Client**, cliquez avec le bouton droit sur **Protocoles clients**, puis sélectionnez **Propriétés**.  
  
2.  Dans la zone **Protocoles activés**, cliquez sur **Monter** ou sur **Descendre** pour modifier l’ordre dans lequel les protocoles sont essayés lors d’une tentative de connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le protocole figurant en première position dans la zone **Protocoles activés** est le protocole par défaut.  
  
    > [!IMPORTANT]  
    >  Le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée des entrées de Registre pour les configurations d'alias de serveur et la bibliothèque réseau cliente par défaut. Toutefois, l'application n'installe pas les bibliothèques réseau clientes, ni les protocoles réseau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les bibliothèques réseau clientes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont installées au cours de l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les protocoles réseau sont installés lors de l’installation de Microsoft Windows (ou via **Réseaux** dans le **Panneau de configuration**). Un protocole réseau spécifique peut ne pas être disponible lors de l’installation de Windows. Pour plus d'informations concernant l’installation de ces protocoles réseau, consultez la documentation de l'éditeur.  
  
###  <a name="Configure"></a> Pour configurer un client pour l’utilisation de TCP/IP  
  
1.  Dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], développez **Configuration de SQL Server Native Client**, cliquez avec le bouton droit sur **Protocoles clients**, puis sélectionnez **Propriétés**.  
  
2.  Dans la zone **Protocoles activés**, cliquez sur la flèche vers le haut ou vers le bas pour modifier l’ordre dans lequel les protocoles sont essayés lors d’une tentative de connexion à SQL Server. Le protocole figurant en première position dans la zone **Protocoles activés** est le protocole par défaut.  
  
 Vous pouvez activer le protocole de mémoire partagée séparément en cochant la case **Activer le protocole de mémoire partagée**.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer l’option de configuration du serveur remote login timeout](../../database-engine/configure-windows/configure-the-remote-login-timeout-server-configuration-option.md)  
  
  
