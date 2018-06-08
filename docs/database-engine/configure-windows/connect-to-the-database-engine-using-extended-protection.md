---
title: Se connecter au moteur de base de données à l’aide de la protection étendue | Microsoft Docs
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
- spoofing attacks
- service binding
- luring attacks
- Schannel
- channel binding
- Extended Protection
ms.assetid: ecfd783e-7dbb-4a6c-b5ab-c6c27d5dd57f
caps.latest.revision: 22
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: f0eae05397ebe6ce2c73841e9c27746e9a946dfc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-the-database-engine-using-extended-protection"></a>Se connecter au moteur de base de données à l'aide de la protection étendue
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la **protection étendue** depuis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. La**protection étendue de l'authentification** est une fonctionnalité des composants réseau implémentée par le système d'exploitation. La**protection étendue** est prise en charge dans Windows 7 et Windows Server 2008 R2. La**protection étendue** figure dans les Service Packs pour les systèmes d'exploitation [!INCLUDE[msCoName](../../includes/msconame-md.md)] plus anciens. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est plus sécurisé lorsque les connexions sont établies à l'aide de la **protection étendue**.  
  
> [!IMPORTANT]  
>  Windows n'active pas la **protection étendue** par défaut. Pour plus d'informations sur l'activation de la **protection étendue** dans Windows, consultez [Protection étendue de l'authentification](http://support.microsoft.com/kb/968389)  
  
## <a name="description-of-extended-protection"></a>Description de la protection étendue  
 La**protection étendue** utilise la liaison de canal et la liaison de service pour mieux empêcher une attaque de relais d'authentification. Dans une attaque de relais d'authentification, un client qui peut effectuer l'authentification NTLM (par exemple, l'Explorateur Windows, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Outlook, une application SqlClient .NET, etc.), se connecte à une personne malveillante (par exemple, un serveur de fichiers CIFS nuisible). La personne malveillante utilise les informations d'identification du client pour se faire passer pour le client et s'authentifier auprès d'un service (par exemple, une instance du service [!INCLUDE[ssDE](../../includes/ssde-md.md)] ).  
  
 Cette attaque se présente sous deux formes :  
  
-   Dans une attaque par ruse, le client est attiré dans un piège pour se connecter volontairement à la personne malveillante.  
  
-   Dans une attaque d'usurpation, le client envisage de se connecter à un service valide, mais ignore que le routage DNS et/ou le routage IP sont empoisonnés pour rediriger à la place la connexion vers la personne malveillante.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la liaison de canal et la liaison de service pour réduire ces attaques sur les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="service-binding"></a>liaison de service  
 La liaison de service répond aux attaques par leurre en demandant à un client d'envoyer un nom de principal du service (SPN) signé du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auquel il envisage de se connecter. Dans le cadre de la réponse d'authentification, le service valide que le SPN reçu dans le paquet correspond à son propre SPN. Si un client est leurré pour se connecter à une personne malveillante, le client inclura le SPN signé de la personne malveillante. La personne malveillante ne peut pas relayer le paquet pour s'authentifier auprès du véritable service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comme client, parce qu'il inclurait le SPN de la personne malveillante. La liaison de service implique un coût unique et négligeable, mais ne traite pas les attaques d'usurpation. La liaison de service se produit lorsqu'une application cliente n'utilise pas le chiffrement pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="channel-binding"></a>liaison de canal  
 La liaison de canal établit un canal sécurisé (Schannel) entre un client et une instance du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le service vérifie l'authenticité du client en comparant le jeton de liaison de canal du client spécifique à ce canal à son propre jeton de liaison de canal. La liaison de canal répond à la fois aux attaques par leurre et d'usurpation. Toutefois, elle implique un coût d'exécution plus important, car elle nécessite le chiffrement TLS (Transport Layer Security) de tout le trafic de session. La liaison de canal se produit lorsqu'une application cliente utilise le chiffrement pour se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], que le chiffrement soit appliqué par le client ou par le serveur.  
  
> [!WARNING]  
>  Les fournisseurs de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[msCoName](../../includes/msconame-md.md)] pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prennent en charge TLS 1.0 et SSL 3.0. Si vous appliquez un autre protocole (comme TLS 1.1 ou TLS 1.2) en apportant des modifications dans la couche SChannel du système d’exploitation, vos connexions à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] risquent d’échouer.  
  
### <a name="operating-system-support"></a>Prise en charge du système d'exploitation  
 Les liens suivants fournissent davantage d'informations sur la prise en charge de la **protection étendue**par Windows :  
  
-   [Integrated Windows Authentication with Extended Protection (en anglais)](http://msdn.microsoft.com/library/dd639324.aspx)  
  
-   [Microsoft Security Advisory (973811), Extended Protection for Authentication (en anglais)](http://www.microsoft.com/technet/security/advisory/973811.mspx)  
  
## <a name="settings"></a>Paramètres  
 Trois paramètres de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affectent la liaison de service et la liaison de canal. Les paramètres peuvent être configurés à l'aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ou en utilisant WMI, et peuvent être affichés à l'aide de la facette **Paramètres de protocole serveur** de la gestion basée sur des stratégies.  
  
-   **Forcer le chiffrement**  
  
     Les valeurs possibles sont **On** (activé) et **Off**(désactivé). Pour utiliser la liaison de canal, **Forcer le chiffrement** doit avoir la valeur **On**et le chiffrement sera forcé pour tous les clients. Si la valeur est **Off**, seule la liaison de service est garantie. **Forcer le chiffrement** figure dans **Propriétés de Protocoles pour MSSQLSERVER (onglet Indicateurs)** dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   **Protection étendue**  
  
     Les valeurs possibles sont **Off**, **Autorisée**et **Requis**. La variable **Protection étendue** permet aux utilisateurs de configurer le niveau de **protection étendue** pour chaque instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . **Protection étendue** figure dans **Propriétés de Protocoles pour MSSQLSERVER (onglet Avancé)** dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Lorsque la valeur est **Off**, la **protection étendue** est désactivée. L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] acceptera les connexions de tous les clients, qu'ils soient protégés ou non. La valeur**Off** est compatible avec les anciens systèmes d'exploitation non corrigés, mais elle est moins sécurisée. Utilisez ce paramètre lorsque vous savez que les systèmes d'exploitation clients ne prennent pas en charge la protection étendue.  
  
    -   Lorsque la valeur est **Autorisée**, la **protection étendue** est requise pour les connexions à partir des systèmes d'exploitation qui prennent en charge la **protection étendue**. La**protection étendue** est ignorée pour les connexions des systèmes d'exploitation qui ne prennent pas en charge la **protection étendue**. Les connexions provenant d'applications clientes non protégées qui s'exécutent sur des systèmes d'exploitation clients protégés sont rejetées. Ce paramètre offre une meilleure protection que la valeur **Off**, mais ce n'est pas la configuration la plus sécurisée. Utilisez ce paramètre dans des environnements mixtes, dans lesquels certains systèmes d'exploitation prennent en charge la **Protection étendue** et d'autres non.  
  
    -   Lorsque la valeur est **Requis**, seules les connexions provenant d'applications protégées sur des systèmes d'exploitation protégés sont acceptées. Ce paramètre est le plus sécurisé, mais les connexions provenant de systèmes d'exploitation ou d'applications qui ne prennent pas en charge la **protection étendue** ne pourront pas se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   **SPN NTLM acceptés**  
  
     La variable **SPN NTLM acceptés** est requise lorsqu'un serveur est connu de plusieurs SPN. Lorsqu'un client essaie de se connecter au serveur à l'aide d'un SPN valide que le serveur ne connaît pas, la liaison de service échoue. Pour éviter ce problème, les utilisateurs peuvent spécifier plusieurs SPN qui représentent le serveur à l'aide de **SPN NTLM acceptés**. **SPN NTLM acceptés** est une série de SPN séparés par des points-virgules. Par exemple, pour autoriser les SPN **MSSQLSvc/ NomHôte1.Contoso.com** et **MSSQLSvc/ NomHôte2.Contoso.com**, tapez **MSSQLSvc/NomHôte1.Contoso.com;MSSQLSvc/NomHôte2.Contoso.com** dans la zone **SPN NTLM acceptés** . La variable a une longueur maximale de 2 048 caractères. **SPN NTLM acceptés** figure dans **Propriétés de Protocoles pour MSSQLSERVER (onglet Avancé)** dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="enabling-extended-protection-for-the-database-engine"></a>Activation de la protection étendue pour le moteur de base de données  
 Pour utiliser la **protection étendue**, le serveur et le client doivent tous deux avoir un système d'exploitation qui prend en charge la **protection étendue**, et la **protection étendue** doit être activée sur le système d'exploitation. Pour plus d’informations sur l’activation de la **protection étendue** pour le système d’exploitation, consultez [Protection étendue de l’authentification](http://support.microsoft.com/kb/968389).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge la **protection étendue** depuis [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. La**protection étendue** de quelques versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sera disponible dans de futures mises à jour. Après avoir activé la **protection étendue** sur le serveur, procédez comme suit pour activer la **protection étendue**:  
  
1.  Dans le menu **Démarrer** , cliquez sur **Tous les programmes**, pointez sur **Microsoft SQL Server** , puis cliquez sur **Gestionnaire de configuration SQL Server**.  
  
2.  Développez **Configuration du réseau SQL Server**, cliquez avec le bouton droit sur **Protocoles pour** *\<* nom_instance*>*, puis cliquez sur **Propriétés**.  
  
3.  Pour la liaison de service et la liaison de canal, sous l'onglet **Avancé** , affectez le paramètre approprié à **Protection étendue** .  
  
4.  Éventuellement, lorsqu'un serveur est connu de plusieurs SPN, sous l'onglet **Avancé** , configurez le champ **SPN NTLM acceptés** comme décrit dans la section « Paramètres ».  
  
5.  Pour la liaison de canal, sous l'onglet **Indicateurs** , affectez à **Forcer le chiffrement** la valeur **On**.  
  
6.  Redémarrez le service [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
## <a name="configuring-other-sql-server-components"></a>Configuration d'autres composants SQL Server  
 Pour plus d’informations sur la configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Protection étendue de l’authentification avec Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md).  
  
 Lors de l'utilisation d'IIS pour accéder aux données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l'aide d'une connexion HTTP ou HTTPs, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peut tirer parti de la protection étendue fournie par IIS. Pour plus d'informations sur la configuration d'IIS pour utiliser la protection étendue, consultez [Configure Extended Protection in IIS 7.5](http://go.microsoft.com/fwlink/?LinkId=181105)(en anglais).  
  
## <a name="see-also"></a> Voir aussi  
 [Configuration réseau du serveur](../../database-engine/configure-windows/server-network-configuration.md)   
 [Configuration du réseau client](../../database-engine/configure-windows/client-network-configuration.md)   
 [Extended Protection for Authentication Overview (en anglais)](http://go.microsoft.com/fwlink/?LinkID=177943)   
 [Authentification Windows intégrée avec protection étendue](http://go.microsoft.com/fwlink/?LinkId=179922) (Integrated Windows Authentication with Extended Protection)  
  
  
