---
title: MSSQLSERVER_21879 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 21879 (Database Engine error)
ms.assetid: fcfab735-05ca-423a-89f1-fdee7e2ed8c0
caps.latest.revision: 7
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 086498a5c4300c3c67d5f2b71a0ee1c638b6e31e
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver21879"></a>MSSQLSERVER_21879
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|21879|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SQLErrorNum21879|  
|Texte du message|Impossible d'interroger le serveur redirigé '%s' pour le serveur de publication d'origine '%s' et la base de données du serveur de publication '%s' pour déterminer le nom du serveur distant ; erreur %d, message d'erreur '%s'.|  
  
## <a name="explanation"></a>Explication  
**sp_validate_redirected_publisher** utilise un serveur lié temporaire créé pour se connecter au serveur de publication redirigé afin de découvrir le nom du serveur distant. L'erreur 21879 est retournée lorsque la requête de serveur lié échoue. L'appel pour demander le nom du serveur distant est généralement la première utilisation qui est faite du serveur lié temporaire, donc si des problèmes de connectivité existent, ils sont susceptibles d'apparaître d'abord lors de cet appel. Cet appel distant exécute simplement select **@@servername** sur le serveur distant.  
  
Le serveur lié utilisé pour interroger le serveur de publication redirigé utilise le mode de sécurité, le nom de connexion et le mot de passe fournis quand **sp_adddistpublisher** a été appelé pour le serveur de publication d’origine.  
  
-   Si l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été utilisée (mode de sécurité 0), alors la connexion et le mot de passe spécifiés sont utilisés pour la connexion au serveur distant.  
  
-   Si l'authentification Windows a été utilisée (mode de sécurité 1) une connexion approuvée est utilisée pour la connexion.  
  
    -   Si **sp_validate_redirected_publisher**est appelé explicitement par un utilisateur, la connexion Windows sous laquelle l’utilisateur s’exécute est utilisée pour la connexion.  
  
    -   Si le serveur de publication **sp_validate_redirected_** est appelé par un agent de réplication à partir de **sp_get_redirected_publisher**, la connexion Windows associée à l’agent est utilisée.  
  
L’erreur 21879 peut indiquer que **sp_validate_redirected_publisher** a été appelé à l’aide d’une connexion qui n’est pas connue sur le serveur de publication cible redirigé.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Assurez-vous que la connexion d'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou la connexion via l'authentification Windows, est valide sur tous les réplicas de groupe de disponibilité et possède des autorisations suffisantes pour accéder aux tables de métadonnées d'abonnement (syssubscriptions et sysmergesubscriptions) dans la base de données du serveur de publication.  
  
Il existe des considérations spéciales quand l’erreur 21879 est retournée à partir d’un appel à **sp_get_redirected_publisher** initié par un agent de réplication s’exécutant sur un nœud autre que le serveur de distribution ; par exemple, un agent de fusion s’exécutant sur un abonné. Si l'authentification Windows est utilisée pour la connexion au serveur de publication redirigé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être configuré pour l'authentification Kerberos afin que connexion soit établie avec succès. Lorsque l'authentification Windows est utilisée et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas configuré pour l'authentification Kerberos, l'erreur 18456 indiquant que la connexion 'NT AUTHORITY\ANONYMOUS LOGON' a échoué est reçue par un agent de fusion s'exécutant sur un abonné. Il existe trois moyens possibles de résoudre ce problème :  
  
-   Configurez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'authentification Kerberos. Consultez **Authentification Kerberos et SQL Server** dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Utilisez **sp_changedistpublisher** pour changer le mode de sécurité associé au serveur de publication d’origine dans MSdistpublishers, ainsi que pour spécifier un nom de connexion et un mot de passe à utiliser pour la connexion.  
  
-   Spécifiez le paramètre de ligne de commande *BypassPublisherValidation* sur la ligne de commande de l’agent de fusion pour ignorer la validation quand **sp_get_redirected_publisher** est appelé sur le serveur de distribution.  
  
