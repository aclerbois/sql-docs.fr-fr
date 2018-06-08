---
title: Gestionnaire de connexions FTP | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.ftpconnectionmanager.f1
helpviewer_keywords:
- FTP connection manager
- connections [Integration Services], FTP
- connection managers [Integration Services], FTP
ms.assetid: c4f43455-29ca-44ba-ac7f-ea729b1daf93
caps.latest.revision: 41
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fdef499948869c56f8e08229a71479a0cd8e8d6e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ftp-connection-manager"></a>Gestionnaires de connexion FTP
  Un gestionnaire de connexions FTP permet à un package de se connecter à un serveur FTP (File Transfer Protocol). La tâche FTP incluse dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise ce gestionnaire de connexions.  
  
 Quand vous ajoutez un gestionnaire de connexions FTP à un package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un gestionnaire de connexions qui peut être résolu en tant que connexion FTP au moment de l’exécution, définit les propriétés du gestionnaire de connexions et ajoute le gestionnaire de connexions à la collection **Connexions** sur le package.  
  
 La propriété **ConnectionManagerType** du gestionnaire de connexions a pour valeur **FTP**.  
  
 Vous pouvez configurer le gestionnaire de connexions FTP de plusieurs manières :  
  
-   Spécifiez un nom de serveur et un port de serveur.  
  
-   Spécifiez l'accès anonyme ou fournissez un nom d'utilisateur et un mot de passe pour l'authentification de base.  
  
    > [!IMPORTANT]  
    >  Le gestionnaire de connexions FTP prend en charge uniquement l'authentification anonyme et l'authentification de base. Il ne prend pas en charge l'authentification Windows.  
  
-   Définissez le délai d'attente, le nombre de nouvelles tentatives et la quantité de données à copier en une fois.  
  
-   Indiquez si le gestionnaire de connexions FTP utilise le mode actif ou passif.  
  
 Selon la configuration du site FTP auquel le gestionnaire de connexions FTP se connecte, vous devrez éventuellement changer les valeurs par défaut suivantes du gestionnaire de connexions :  
  
-   Le port du serveur est réglé sur 21. Vous devez spécifier le port que le site FTP écoute.  
  
-   Le nom d'utilisateur prend la valeur « anonymous ». Vous devez fournir les informations d'identification requises par le site FTP.  
  
## <a name="activepassive-modes"></a>Modes actif/passif  
 Un gestionnaire de connexions FTP peut envoyer et recevoir des fichiers en mode actif ou passif. En mode actif, le serveur initie la connexion de données, alors qu'en mode passif il s'agit du client.  
  
## <a name="configuration-of-the-ftp-connection-manager"></a>Configuration du gestionnaire de connexions FTP  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d’informations sur les propriétés que vous pouvez définir dans le Concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Éditeur du gestionnaire de connexions FTP](../../integration-services/connection-manager/ftp-connection-manager-editor.md).  
  
 Pour plus d’informations sur la configuration d’un gestionnaire de connexions par programmation, consultez <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> et [Ajout de connexions par programme](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="ftp-connection-manager-editor"></a>Éditeur du gestionnaire de connexions FTP
  La boîte de dialogue **Éditeur du gestionnaire de connexions FTP** vous permet de spécifier les propriétés de connexion à un serveur FTP.  
  
> [!IMPORTANT]  
>  Le gestionnaire de connexions FTP prend en charge uniquement l'authentification anonyme et l'authentification de base. Il ne prend pas en charge l'authentification Windows.  
  
 Pour en savoir plus sur le gestionnaire de connexions FTP, consultez [Gestionnaire de connexions FTP](../../integration-services/connection-manager/ftp-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Nom du serveur**  
 Indiquez le nom du serveur FTP.  
  
 **Port du serveur**  
 Spécifiez le numéro de port sur le serveur FTP à utiliser pour la connexion. La valeur par défaut de cette propriété est **21**.  
  
 **User name**  
 Indiquez un nom d'utilisateur pour accéder au serveur FTP. La valeur par défaut de cette propriété est **anonyme**.  
  
 **Mot de passe**  
 Indiquez le mot de passe permettant d'accéder au serveur FTP.  
  
 **Délai d'expiration (en secondes)**  
 Spécifiez le nombre de secondes à patienter avant l'expiration de la tâche. Une valeur égale à **0** indique une durée illimitée. La valeur par défaut de cette propriété est **60**.  
  
 **Utiliser le mode passif**  
 Permet de spécifier si le serveur ou le client initie la connexion. Le serveur initie la connexion en mode actif, alors que le client l'initie en mode passif. La valeur par défaut de cette propriété est **mode actif**.  
  
 **Tentatives**  
 Spécifiez le nombre de fois que la tâche doit tenter d'établir une connexion. Une valeur égale à **0** indique un nombre illimité de tentatives.  
  
 **Taille de segment (en Ko)**  
 Spécifiez une taille de segment en kilo-octets pour la transmission des données.  
  
 **Tester la connexion**  
 Après avoir configuré le gestionnaire de connexions FTP, confirmez que la connexion est viable en cliquant sur **Tester la connexion**.  
  
## <a name="see-also"></a> Voir aussi  
 [Tâche FTP](../../integration-services/control-flow/ftp-task.md)   
 [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
  
