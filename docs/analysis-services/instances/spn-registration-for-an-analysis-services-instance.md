---
title: Inscription SPN pour une instance Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c08495fb3a6486f58462562be120ea1d4cd7f8ad
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="spn-registration-for-an-analysis-services-instance"></a>Inscription du nom SPN pour une instance Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un nom de principal du service (SPN) identifie de manière unique une instance de service dans un domaine Active Directory lorsque Kerberos est utilisé pour authentifier mutuellement les identités de service et de client. Un SPN est associé au compte d'ouverture de session sous lequel s'exécute l'instance du service.  
  
 Pour les applications clientes qui se connectent à Analysis Services via l'authentification Kerberos, les bibliothèques clientes Analysis Services créent un nom SPN à l'aide du nom d'hôte de la chaîne de connexion et d'autres variables connues, telles que la classe de service, qui sont résolues dans n'importe quelle version donnée d'Analysis Services.  
  
 Pour que l'authentification mutuelle ait lieu, les noms SPN construits par le client doivent correspondre à un objet SPN associé sur un contrôleur de domaine Active Directory (DC). Cela signifie que vous pouvez avoir besoin d'inscrire plusieurs SPN pour qu'une seule instance d'Analysis Services traite toutes les méthodes auxquelles un utilisateur peut recourir pour spécifier le nom d'hôte sur une chaîne de connexion. Par exemple, vous avez peut-être besoin de deux noms SPN pour gérer à la fois le nom de domaine complet (FQDN) d'un serveur, ainsi que le nom d'ordinateur abrégé. Enregistrer correctement le nom SPN Analysis Services est une étape essentielle pour la réussite d'une connexion. Si le SPN est inexistant, incorrect ou en double, la connexion échoue.  
  
 L'inscription du SPN est une tâche effectuée manuellement par l'administrateur Analysis Services. Contrairement au moteur de base de données SQL Server, Analysis Services n'inscrit jamais automatiquement son SPN au démarrage du service. L'inscription manuelle est nécessaire lorsque Analysis Services s'exécute sous le compte virtuel par défaut, un compte d'utilisateur de domaine ou un compte intégré, y compris un SID par service.  
  
 L'inscription du SPN n'est pas nécessaire si le service s'exécute sous un compte de service administré prédéfini créé par un administrateur de domaine. Notez que selon le niveau fonctionnel de votre domaine, l'inscription d'un nom SPN peut nécessiter des autorisations d'administrateur de domaine.  
  
> [!TIP]  
>  **[!INCLUDE[msCoName](../../includes/msconame-md.md)] pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** est un outil de diagnostic qui permet de dépanner les problèmes de connexion que rencontre Kerberos avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Gestionnaire de configuration de Microsoft Kerberos pour SQL Server](http://www.microsoft.com/download/details.aspx?id=39046).  
  
 Cette rubrique contient les sections suivantes :  
  
 [Circonstances dans lesquelles une inscription du SPN est requise](#bkmk_scnearios)  
  
 [Format SPN pour Analysis Services](#bkmk_SPNSyntax)  
  
 [Inscription du nom SPN pour un compte virtuel](#bkmk_virtual)  
  
 [Inscription du nom SPN pour un compte de domaine](#bkmk_domain)  
  
 [Inscription du nom SPN pour un compte intégré](#bkmk_builtin)  
  
 [Inscription du nom SPN pour une instance nommée](#bkmk_spnNamed)  
  
 [Inscription du nom SPN pour un cluster SSAS](#bkmk_spnCluster)  
  
 [Inscription du nom SPN pour les instances SSAS configurées pour l'accès HTTP](#bkmk_spnHTTP)  
  
 [Inscription du nom SPN pour les instances SSAS à l'écoute sur des ports fixes](#bkmk_spnFixedPorts)  
  
##  <a name="bkmk_scnearios"></a> Circonstances dans lesquelles une inscription du SPN est requise  
 Toute connexion cliente qui spécifie « SSPI=Kerberos » dans la chaîne de connexion entraîne des conditions d'inscription du SPN pour une instance d'Analysis Services.  
  
 L'inscription du SPN est requise dans les cas suivants. Pour des informations plus détaillées, consultez [Configure Analysis Services for Kerberos constrained delegation](../../analysis-services/instances/configure-analysis-services-for-kerberos-constrained-delegation.md).  
  
-   La délégation d'identité est nécessaire pour permettre la circulation de l'identité des utilisateurs à partir de l'application cliente ou du service de niveau intermédiaire vers Analysis Services. La délégation d'identité est généralement utilisée lorsque des filtres ou des autorisations par utilisateur sont définis sur des objets spécifiques.  
  
     Un scénario courant impliquant la délégation d'identité consiste à configurer des services de niveau intermédiaire, comme Excel Services ou Reporting Services, pour une délégation contrainte à des fins d'emprunt de l'identité d'un utilisateur lors de la récupération de données dans Analysis Services. Pour prendre en charge ce comportement, vous devez fournir un SPN Analysis Services en tant que service de destination, lors de la configuration d'Excel Services ou de Reporting Services pour la délégation contrainte.  
  
-   Analysis Services délègue une identité d'utilisateur lors de la récupération de données depuis une base de données relationnelle SQL Server pour les bases de données tabulaires à l'aide du mode DirectQuery. Il s'agit du seul scénario dans lequel Analysis Services délègue l'identité de l'utilisateur à un autre service.  
  
##  <a name="bkmk_SPNSyntax"></a> Format SPN pour Analysis Services  
 Utilisez **setspn** pour inscrire un nom SPN. Sur les systèmes d'exploitation plus récents, **setspn** est installé en tant qu'utilitaire système. Pour plus d'informations, consultez [SetSPN](http://technet.microsoft.com/library/cc731241\(WS.10\).aspx).  
  
 Le tableau suivant décrit chaque partie d'un nom SPN d'Analysis Services.  
  
|Élément|Description|  
|-------------|-----------------|  
|Classe de service|MSOLAPSvc.3 identifie le service en tant qu'instance Analysis Services. Le .3 est une référence à la version du protocole XMLA-over-TCP/IP utilisé dans des transmissions Analysis Services. Il n'a aucun rapport avec la version du produit. Par conséquent, MSOLAPSvc.3 est la classe de service correcte pour SQL Server 2005, 2008, 2008 R2, 2012 ou toute version ultérieure d'Analysis Services jusqu'à ce que le protocole lui-même soit modifié.|  
|Host-name|Identifie l'ordinateur sur lequel s'exécute le service. Ce peut être un nom de domaine complet ou un nom NetBIOS. Vous devez inscrire un SPN pour les deux noms.<br /><br /> Lors de l'inscription d'un SPN pour le nom NetBIOS d'un serveur, veillez à utiliser `SetupSPN –S` pour éviter toute inscription en double. Il n'existe aucune garantie que les noms NetBIOS soient uniques dans une forêt ; en présence d'une inscription SPN en double, la connexion échoue.<br /><br /> Pour les clusters à charge équilibrée Analysis Services, le nom d'hôte doit être le nom virtuel affecté au cluster.<br /><br /> Ne créez jamais de nom SPN à l'aide de l'adresse IP. Kerberos utilise les fonctionnalités de résolution DNS du domaine. La spécification d'une adresse IP ignore cette fonctionnalité.|  
|Port-number|Bien que le numéro de port fasse partie de la syntaxe de SPN, vous ne spécifiez jamais un numéro de port lors de l'inscription d'un nom SPN Analysis Services. Les deux-points (:), généralement utilisés pour fournir un numéro de port dans la syntaxe standard de SPN, permettent à Analysis Services de spécifier le nom de l'instance. Pour une instance d'Analysis Services, le port est supposé être le port par défaut (TCP 2383) ou un port affecté par le service SQL Server Browser (TCP 2382).|  
|Instance-name|Analysis Services est un service réplicable qui peut être installé plusieurs fois sur le même ordinateur. Chaque instance est identifiée par son nom d'instance.<br /><br /> Le nom de l'instance est préfixé par un signe deux-points (:). Si l'on prend l'exemple d'un ordinateur hôte appelé SRV01 et d'une instance nommée SSAS-tabulaire, le SPN doit être SRV01:SSAS-tabulaire.<br /><br /> Notez que la syntaxe pour spécifier une instance nommée d'Analysis Services est différente de celle utilisée par d'autres instances de SQL Server. D'autres services utilisent une barre oblique inverse (\) pour ajouter le nom de l'instance dans un SPN.|  
|Compte de service|Il s'agit du compte de démarrage du service Windows **MSSQLServerOLAPService** . Ce peut être un compte d'utilisateur de domaine Windows, un compte virtuel, un compte de service administré ou un compte intégré comme un SID par service, un NetworkService, ou un LocalSystem. Un compte d’utilisateur de domaine Windows peut être sous la forme domaine\utilisateur ou user@domain.|  
  
##  <a name="bkmk_virtual"></a> Inscription du nom SPN pour un compte virtuel  
 Les comptes virtuels correspondent au type de compte par défaut des services SQL Server. Le compte virtuel est **NT Service\MSOLAPService** pour une instance par défaut et **NT Service\MSOLAP$**\<instance-name > pour une instance nommée.  
  
 Comme son nom l'indique, ce type de compte n'existe pas dans Active Directory. Un compte virtuel n'existe que sur l'ordinateur local. Lors de la connexion à des périphériques, des applications ou des services externes, la connexion est établie à l'aide du compte d'ordinateur local. Pour cette raison, une inscription du nom SPN pour Analysis Services s'exécutant sous un compte virtuel correspond en fait à une inscription SPN pour le compte d'ordinateur.  
  
 **Exemple de syntaxe pour une instance par défaut exécutée en tant NT Service\MSOLAPService**  
  
 Cet exemple illustre la syntaxe **setspn** pour une instance Analysis Services par défaut s'exécutant sous le compte virtuel par défaut. Dans cet exemple, le nom d’hôte de l’ordinateur est **AW-SRV01**. Comme indiqué, l’inscription du nom SPN doit spécifier le *compte d’ordinateur* à la place du compte virtuel, **NT Service\MSOLAPService**.  
  
```  
Setspn -s MSOLAPSvc.3/AW-SRV01.AdventureWorks.com AW-SRV01  
```  
  
> [!NOTE]  
>  Pensez à créer deux inscriptions SPN, l'une pour le nom d'hôte NetBIOS et l'autre pour un nom de domaine complet de l'hôte. Différentes applications clientes utilisent des conventions de nom d'hôte différentes lors de la connexion à Analysis Services. Le fait de disposer de deux inscriptions SPN permet de s'assurer que les deux versions du nom d'hôte sont prises en compte.  
  
 **Exemple de syntaxe pour une instance nommée exécutée en tant que NT Service\MSOLAP$\<-nom de l’instance >**  
  
 Cet exemple illustre la syntaxe **setspn** pour une instance nommée s'exécutant sous le compte virtuel par défaut. Dans cet exemple, le nom d’hôte de l’ordinateur est **AW-SRV02** et le nom de l’instance **AW-FINANCE**. Là encore, il est le compte d’ordinateur qui est spécifié pour le SPN, plutôt que le compte virtuel **NT Service\MSOLAP$**\<-nom de l’instance >.  
  
```  
Setspn -s MSOLAPSvc.3/AW-SRV02.AdventureWorks.com:AW-FINANCE AW-SRV02  
```  
  
##  <a name="bkmk_domain"></a> Inscription du nom SPN pour un compte de domaine  
 L'utilisation d'un compte de domaine pour exécuter une instance Analysis Services est une pratique courante.  
  
 Pour les instances Analysis Services qui s'exécutent dans un cluster à charge réseau ou matériel équilibrée, un compte de domaine est requis, chaque instance du cluster s'exécutant sous le même compte de domaine.  
  
 **Exemple de syntaxe pour une instance par défaut exécutée sous un utilisateur de domaine**  
  
 Cet exemple illustre la syntaxe **setspn** pour une instance Analysis Services par défaut s’exécutant sous un compte d’utilisateur de domaine, **SSAS-Service**, dans le domaine AdventureWorks.  
  
```  
Setspn –s msolapsvc.3\AW-SRV01.Adventureworks.com AdventureWorks\SSAS-Service  
```  
  
> [!TIP]  
>  Vérifiez si le SPN a été créé pour le serveur Analysis Services en exécutant `Setspn -L <domain account>` ou `Setspn -L <machinename>`, selon la manière dont le nom SPN a été inscrit. Vous devriez voir msolapsvc.3 /\<nom d’hôte > dans la liste.  
  
##  <a name="bkmk_builtin"></a> Inscription du nom SPN pour un compte intégré  
 Bien que cette pratique ne soit pas recommandée, les installations Analysis Services plus anciennes sont parfois configurées pour s'exécuter sous des comptes intégrés comme Service réseau, Service local ou Système local.  
  
 **Exemple de syntaxe pour une instance par défaut exécutée sous un compte intégré**  
  
 L'inscription du nom SPN pour un service s'exécutant sous un compte intégré ou un SID par service reprend la même syntaxe SPN que celle utilisée pour le compte virtuel. Mais à la place du nom du compte, utilisez le compte d'ordinateur :  
  
```  
Setspn -s MSOLAPSvc.3/AW-SRV01.AdventureWorks.com AW-SRV01  
```  
  
##  <a name="bkmk_spnNamed"></a> Inscription du nom SPN pour une instance nommée  
 Les instances nommées d'Analysis Services utilisent des affectations de ports dynamiques qui sont détectées par le service SQL Server Browser. Lors de l'utilisation d'une instance nommée, vous devez inscrire un SPN à la fois pour le service SQL Server Browser et pour l'instance nommée Analysis Services. Pour plus d'informations, consultez [Un SPN pour le service SQL Server Browser est requis lorsque vous établissez une connexion à une instance nommée de SQL Server Analysis Services ou de SQL Server](http://support.microsoft.com/kb/950599).  
  
 **Exemple de syntaxe SPN pour le service SQL Browser exécuté en tant que LocalService**  
  
 La classe de service est **MSOLAPDisco.3**. Par défaut, ce service s'exécute comme NT AUTHORITY\LocalService, ce qui signifie que l'inscription du SPN est définie pour le compte d'ordinateur. Dans cet exemple, le compte d’ordinateur est **AW-SRV01**, qui correspond au nom de l’ordinateur.  
  
```  
Setspn -S MSOLAPDisco.3/AW-SRV01.AdventureWorks.com AW-SRV01  
```  
  
##  <a name="bkmk_spnCluster"></a> Inscription du nom SPN pour un cluster SSAS  
 Pour les clusters de basculement Analysis Services, le nom d'hôte doit être le nom virtuel affecté au cluster. C'est le nom du réseau SQL Server, spécifié pendant l'installation de SQL Server lorsque vous avez installé Analysis Services sur un WSFC existant. Ce nom se trouve dans Active Directory. Vous pouvez également le trouver sous l’onglet **Gestionnaire du cluster de basculement** | **Rôle** | **Ressources** . Le nom de serveur indiqué sous l'onglet Ressources est celui qui doit être utilisé comme « nom virtuel » dans la commande SPN.  
  
 **Syntaxe du SPN pour un cluster Analysis Services**  
  
```  
Setspn –s msolapsvc.3/<virtualname.FQDN > <domain user account>  
```  
  
 N'oubliez pas que des nœuds dans un cluster Analysis Services sont requis pour utiliser le port par défaut (TCP 2383) et s'exécutent sous le même compte d'utilisateur de domaine afin que chaque nœud présente le même SID. Pour plus d'informations, consultez [Procédure de mise en cluster de SQL Server Analysis Services](http://msdn.microsoft.com/library/dn736073.aspx) .  
  
##  <a name="bkmk_spnHTTP"></a> Inscription du nom SPN pour les instances SSAS configurées pour l'accès HTTP  
 Selon les spécifications de la solution, vous pouvez avoir configuré Analysis Services pour l'accès HTTP. Si votre solution contient IIS en tant que composant de niveau intermédiaire et que l'authentification Kerberos est une condition de la solution, vous devrez peut-être inscrire manuellement un nom SPN pour IIS. Pour plus d’informations, consultez « Configurer les paramètres sur l’ordinateur qui exécute IIS » dans [Comment configurer SQL Server 2008 Analysis Services et SQL Server 2005 Analysis Services pour utiliser l’authentification Kerberos](http://support.microsoft.com/kb/917409).  
  
 En termes d'inscription du SPN pour l'instance Analysis Services, il n'existe aucune différence entre une instance configurée pour le protocole TCP ou HTTP. La connexion à Analysis Services depuis IIS, à l'aide de l'extension MSMDPUMP ISAPI, est toujours TCP.  
  
 Cela signifie que vous pouvez utiliser les instructions des sections précédentes pour l'instance par défaut ou nommée afin d'inscrire le nom SPN. En spécifiant le nom d'hôte, veillez à utiliser le nom d'hôte spécifié dans le fichier msmdpump.ini au moment de la configuration du service pour l'accès HTTP.  
  
 Pour plus d’informations sur l’accès HTTP, consultez [Configurer l’accès HTTP à Analysis Services sur Internet Information Services &#40;IIS&#41; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md).  
  
##  <a name="bkmk_spnFixedPorts"></a> Inscription du nom SPN pour les instances SSAS à l'écoute sur des ports fixes  
 Vous ne pouvez pas spécifier un numéro de port pour une inscription de SPN d'Analysis Services. Si vous avez installé Analysis Services comme instance par défaut et qu'il est configuré pour être à l'écoute sur un port fixe, vous devez maintenant le configurer pour écouter sur le port par défaut (TCP 2383). Pour les instances nommées, vous devez utiliser le service SQL Server Browser et les affectations de ports dynamiques.  
  
 Une instance Analysis Services ne peut être à l'écoute que sur un port unique. L'utilisation de plusieurs ports n'est pas prise en charge. Pour plus d'informations sur la configuration du port, consultez [Configure the Windows Firewall to Allow Analysis Services Access](../../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Authentification et délégation d'identité Microsoft BI](http://go.microsoft.com/fwlink/?LinkID=286576)   
 [Authentification mutuelle à l'aide de Kerberos](http://go.microsoft.com/fwlink/?LinkId=299283)   
 [Comment configurer SQL Server 2008 Analysis Services et SQL Server 2005 Analysis Services pour utiliser l’authentification Kerberos](http://support.microsoft.com/kb/917409)   
 [Noms de principaux du service (SPN), syntaxe SetSPN (Setspn.exe)](http://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx)   
 [What SPN do I use and how does it get there?](http://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx)   
 [SetSPN](http://technet.microsoft.com/library/cc731241\(WS.10\).aspx)   
 [Guide pas à pas des comptes de service](http://technet.microsoft.com/library/dd548356\(WS.10\).aspx)   
 [Configurer les comptes de Service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)   
 [Comment utiliser des SPN quand vous configurez des applications web hébergées sur les services Internet (IIS)](http://support.microsoft.com/kb/929650)   
 [nouveautés dans les comptes de service](http://technet.microsoft.com/library/dd367859\(WS.10\).aspx)   
 [Configurer l'authentification Kerberos pour les produits SharePoint 2010 (livre blanc)](http://technet.microsoft.com/library/ff829837.aspx)  
  
  
