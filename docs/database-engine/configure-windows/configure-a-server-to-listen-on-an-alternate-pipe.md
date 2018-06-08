---
title: Configurer un serveur pour l’écoute d’un canal de remplacement | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Named Pipes [SQL Server], configuring
- listening [SQL Server], pipes
- pipes [SQL Server], alternate
- alternate pipes [SQL Server]
ms.assetid: 914f7491-e2be-4b0d-b3aa-fe5409cdbafa
caps.latest.revision: 29
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: f4f656d7598a44ce667a980916b5fc920a45fb81
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-a-server-to-listen-on-an-alternate-pipe"></a>Configurer un serveur pour l’écoute d’un canal de remplacement
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit comment configurer un serveur pour l'écoute sur un autre canal dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide du Gestionnaire de configuration SQL Server. Par défaut, l’instance par défaut du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] écoute le canal nommé \\\\.\pipe\sql\query. Les instances nommées de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et de [!INCLUDE[ssEW](../../includes/ssew-md.md)] écoutent d'autres canaux.  
  
 Il y a trois manières de se connecter à un canal nommé spécifique avec une application cliente :  
  
-   Exécution du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser sur le serveur.  
  
-   Création d'un alias sur le client, spécification du canal nommé.  
  
-   Programmer le client pour une connexion à l'aide d'une chaîne de connexion personnalisée.  
  
##  <a name="SSMSProcedure"></a> Utilisation du Gestionnaire de configuration SQL Server  
  
#### <a name="to-configure-the-named-pipe-used-by-the-sql-server-database-engine"></a>Pour configurer le canal nommé utilisé par le moteur de base de données SQL Server  
  
1.  Dans le Gestionnaire de configuration SQL Server, dans le volet de la console, développez **Configuration du réseau SQL Server**, puis cliquez pour développer **Protocoles pour** *\<nom_instance>*.  
  
2.  Dans le volet d’informations, cliquez avec le bouton droit sur **Canaux nommés**, puis cliquez sur **Propriétés**.  
  
3.  Sous l'onglet **Protocole** , dans la zone **Nom du canal** , tapez le canal que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit écouter, puis cliquez sur **OK**.  
  
4.  Dans le volet de la console, cliquez sur **Services SQL Server**.  
  
5.  Dans le volet Détails, cliquez avec le bouton droit sur **SQL Server (**\<nom_instance>**)** puis cliquez sur **Redémarrer** pour arrêter et redémarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] écoute un autre canal, il y a trois manières de se connecter à un canal nommé spécifique avec une application cliente :  
  
-   Exécution du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser sur le serveur.  
  
-   Création d'un alias sur le client, spécification du canal nommé.  
  
-   Programmer le client pour une connexion à l'aide d'une chaîne de connexion personnalisée.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer ou modifier un alias de serveur devant être utilisé par un client &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md)   
 [Configuration réseau du serveur](../../database-engine/configure-windows/server-network-configuration.md)  
  
  
