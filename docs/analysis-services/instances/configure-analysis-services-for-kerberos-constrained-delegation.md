---
title: Configurer Analysis Services pour la délégation contrainte Kerberos | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6c906ef040504e2fec094ae0935a374e27cbf984
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-analysis-services-for-kerberos-constrained-delegation"></a>Configurer Analysis Services pour la délégation contrainte Kerberos
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Quand vous configurez Analysis Services pour l'authentification Kerberos, vous cherchez probablement à obtenir l'un des deux résultats suivants : laisser le soin à Analysis Services d'emprunter l'identité d'un utilisateur durant l'interrogation de données, ou demander à Analysis Services de déléguer l'identité d'utilisateur à un service de bas niveau. Chaque scénario implique des spécifications de configuration légèrement différentes. De plus, dans les deux cas, une vérification doit avoir lieu pour s'assurer que la configuration a été effectuée correctement.  
  
> [!TIP]  
>  **[!INCLUDE[msCoName](../../includes/msconame-md.md)] pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** est un outil de diagnostic qui permet de dépanner les problèmes de connexion que rencontre Kerberos avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Gestionnaire de configuration de Microsoft Kerberos pour SQL Server](http://www.microsoft.com/download/details.aspx?id=39046).  
  
 Cette rubrique contient les sections suivantes :  
  
-   [Autoriser Analysis Services à emprunter l'identité d'un utilisateur](#bkmk_impersonate)  
  
-   [Configurer Analysis Services pour une délégation approuvée](#bkmk_delegate)  
  
-   [Tester une identité empruntée ou déléguée](#bkmk_test)  
  
> [!NOTE]  
>  La délégation n'est pas nécessaire si la connexion à Analysis Services est un saut unique, ou si votre solution utilise les informations d'identification stockées fournies par le Service Banque d'informations sécurisé ou Reporting Services de SharePoint. Si toutes les connexions sont des connexions directes à partir d'Excel vers une base de données Analysis Services, ou si elles sont basées sur des informations d'identification stockées, vous pouvez utiliser Kerberos (ou NTLM) sans avoir à configurer la délégation contrainte.  
>   
>  La délégation Kerberos contrainte est requise si l'identité de l'utilisateur doit servir à plusieurs connexions d'ordinateur (connexions « à deux tronçons »). Lorsque l'accès aux données Analysis Services dépend de l'identité de l'utilisateur et que la demande de connexion provient d'un service de délégation, utilisez la liste de vérification de la section suivante pour vous assurer qu'Analysis Services peut emprunter l'identité de l'appelant d'origine. Pour plus d'informations sur les flux d'authentification Analysis Services, consultez [Authentification et délégation d'identité Microsoft BI](http://go.microsoft.com/fwlink/?LinkID=286576).  
>   
>  En guise de meilleure pratique de sécurité, Microsoft recommande de toujours utiliser la délégation contrainte plutôt que la délégation non contrainte. La délégation non contrainte présente un risque majeur pour la sécurité car elle permet à l’identité de service d’emprunter l’identité d’un autre utilisateur sur *n’importe quel* service, application ou ordinateur en aval (plutôt qu’uniquement les services définis de manière explicite via la délégation contrainte).  
  
##  <a name="bkmk_impersonate"></a> Autoriser Analysis Services à emprunter l'identité d'un utilisateur  
 Pour permettre aux services de haut niveau tels que Reporting Services, IIS ou SharePoint d'emprunter l'identité d'un utilisateur dans Analysis Services, vous devez configurer la délégation contrainte Kerberos pour ces services. Dans ce scénario, Analysis Services emprunte l'identité de l'utilisateur actuel à l'aide de l'identité fournie par le service de délégation et retourne des résultats selon l'appartenance au rôle de l'identité de cet utilisateur.  
  
|Tâche| Description|  
|----------|-----------------|  
|Étape 1 : Vérifier que les comptes conviennent pour la délégation|Vérifiez que les comptes utilisés pour exécuter les services présentent les propriétés appropriées dans Active Directory. Les comptes de service dans Active Directory ne doivent pas être marqués comme comptes sensibles, ni être spécifiquement exclus des scénarios de délégation. Pour plus d'informations, consultez [Présentation des comptes d'utilisateurs](http://go.microsoft.com/fwlink/?LinkId=235818).<br /><br /> Remarque : généralement, tous les comptes et les serveurs impliqués dans le traitement doivent appartenir au même domaine Active Directory ou à des domaines approuvés dans la même forêt. Toutefois, étant donné que Windows Server 2012 prend en charge la délégation entre les limites de domaine, vous pouvez configurer la délégation contrainte Kerberos si le niveau fonctionnel du domaine est Windows Server 2012. Sinon, vous pouvez configurer Analysis Services pour l'accès HTTP et utiliser les méthodes d'authentification IIS sur la connexion cliente. Pour plus d’informations, consultez [Configurer l’accès HTTP à Analysis Services sur Internet Information Services &#40;IIS&#41; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md).|  
|Étape 2 : Inscrire le SPN|Avant de configurer une délégation contrainte, vous devez enregistrer un Nom de principal de service (SPN) pour l'instance du service Analysis Services. Vous aurez besoin du SPN Analysis Services lors de la configuration de la délégation contrainte Kerberos pour les services de niveau intermédiaire. Pour obtenir des instructions, consultez [SPN registration for an Analysis Services instance](../../analysis-services/instances/spn-registration-for-an-analysis-services-instance.md) .<br /><br /> Un nom de principal du service (SPN) spécifie l'identité unique d'un service dans un domaine configuré pour l'authentification Kerberos. Les connexions clientes utilisant la sécurité intégrée demandent souvent un SPN dans le cadre de l'authentification SSPI. La demande est transmise à un contrôleur de domaine (DC) Active Directory, le centre de distribution de clés KDC accordant un ticket si le nom SPN présenté par le client comporte une inscription du nom SPN correspondante dans Active Directory.|  
|Étape 3 : Configurer une délégation contrainte|Après avoir validé les comptes que vous souhaitez utiliser et inscrit les noms SPN de ces derniers, la prochaine étape consiste à configurer les services de haut niveau, tels que les services Web IIS, Reporting Services ou SharePoint pour la délégation contrainte, en spécifiant l'instance Analysis Services comme service spécifique pour lequel la délégation est autorisée.<br /><br /> Les services qui s'exécutent dans SharePoint, tels que Excel Services ou Reporting Services en mode SharePoint, hébergent fréquemment des classeurs et des rapports qui utilisent des données tabulaires ou multidimensionnelles d'Analysis Services. Configurer une délégation contrainte pour ces services est une tâche de configuration commune, nécessaire pour la prise en charge de l'actualisation des données à partir d'Excel Services. Les liens suivants fournissent des instructions pour les services SharePoint, ainsi que d'autres services susceptibles de présenter une demande de connexion de données en aval pour les données Analysis Services :<br /><br /> [Délégation d’identité pour Excel Services (SharePoint Server 2010)](http://go.microsoft.com/fwlink/?LinkId=299826) ou [Comment configurer Excel Services dans SharePoint Server 2010 pour l’authentification Kerberos](http://support.microsoft.com/kb/2466519)<br /><br /> [Délégation d'identité pour les services PerformancePoint (SharePoint Server 2010)](http://go.microsoft.com/fwlink/?LinkId=299827)<br /><br /> [Délégation d'identité pour SQL Server Reporting Services (SharePoint Server 2010)](http://go.microsoft.com/fwlink/?LinkId=299828)<br /><br /> Pour IIS 7.0, consultez [Configurer l’authentification Windows (IIS 7.0)](http://technet.microsoft.com/library/cc754628\(v=ws.10\).aspx) ou [Comment configurer SQL Server 2008 Analysis Services et SQL Server 2005 Analysis Services pour utiliser l’authentification Kerberos](http://support.microsoft.com/kb/917409).|  
|Étape 4 : Tester les connexions|Lors du test, connectez-vous à partir d'ordinateurs distants, sous des identités différentes, et interrogez Analysis Services en utilisant les mêmes applications en tant qu'utilisateurs professionnels. Vous pouvez utiliser SQL Server Profiler pour surveiller la connexion. Vous devez voir l'identité de l'utilisateur dans la demande. Pour plus d'informations, consultez [Tester une identité empruntée ou déléguée](#bkmk_test) dans cette section.|  
  
##  <a name="bkmk_delegate"></a> Configurer Analysis Services pour une délégation approuvée  
 Configurer Analysis Services pour la délégation contrainte Kerberos permet au service d'emprunter l'identité d'un client sur un service de bas niveau, comme le moteur de base de données relationnelle, afin que les données puissent être interrogées comme si le client avait été connecté directement.  
  
 Les scénarios de délégation pour Analysis Services sont limités aux modèles tabulaires configurés pour le mode **DirectQuery** . Il s'agit du seul scénario dans lequel Analysis Services peut transmettre des informations d'identification déléguées à un autre service. Dans tous les autres scénarios, tels que les scénarios SharePoint mentionnés dans la section précédente, Analysis Services est à l'extrémité réceptrice de la chaîne de délégation. Pour plus d’informations sur DirectQuery, consultez [DirectQuery Mode](../../analysis-services/tabular-models/directquery-mode-ssas-tabular.md).  
  
> [!NOTE]  
>  On pense souvent à tort que le stockage ROLAP, les opérations de traitement ou l'accès aux partitions distantes exigent la délégation contrainte. Ce n'est pas le cas. Toutes ces opérations sont exécutées directement par le compte de service (également appelé compte de traitement), pour lui-même. La délégation n'est pas nécessaire pour ces opérations dans Analysis Services, étant donné que les autorisations pour ces opérations sont accordées directement au compte de service (par exemple, l'accord d'autorisations db_datareader sur la base de données relationnelle afin que le service puisse traiter des données). Pour plus d’informations sur les autorisations et les opérations serveur, consultez [Configurer les comptes de service &#40;Analysis Services&#41;](../../analysis-services/instances/configure-service-accounts-analysis-services.md).  
  
 Cette section explique comment configurer Analysis Services en vue de la délégation approuvée. Une fois cette tâche effectuée, Analysis Services peut transmettre des informations d'identification déléguées à SQL Server, pour la prise en charge du mode DirectQuery utilisé dans les solutions tabulaires.  
  
 Avant de commencer :  
  
-   Vérifiez qu'Analysis Services est démarré.  
  
-   Vérifiez que le SPN inscrit pour Analysis Services est valide. Pour obtenir des instructions, consultez [SPN registration for an Analysis Services instance](../../analysis-services/instances/spn-registration-for-an-analysis-services-instance.md)  
  
 Lorsque les deux conditions préalables requises sont satisfaites, passez aux étapes suivantes. Notez que vous devez être administrateur de domaine pour configurer la délégation contrainte.  
  
1.  Dans Utilisateurs et ordinateurs Active Directory, recherchez le compte de service sous lequel s'exécute Analysis Services. Cliquez avec le bouton droit sur le compte de service et sélectionnez **Propriétés**.  
  
     À titre d'illustration, les captures d'écran suivantes utilisent OlapSvc et SQLSvc pour représenter respectivement Analysis Services et SQL Server.  
  
     OlapSvc est le compte qui est configuré en vue d'une délégation contrainte à SQLSvc. Lorsque vous effectuez cette tâche, OlapSvc est autorisé à passer les informations d'identification déléguées sur un ticket de service à SQLSvc, en empruntant l'identité de l'appelant d'origine lors de la demande de données.  
  
2.  Sous l'onglet Délégation, sélectionnez **N'approuver cet utilisateur que pour la délégation aux services spécifiés**, suivi de **Utiliser uniquement Kerberos**. Cliquez sur **Ajouter** pour spécifier le service Analysis Services qui est autorisé à déléguer les informations d'identification.  
  
     L'onglet Délégation n'est disponible que lorsque le compte d'utilisateur (OlapSvc) est affecté à un service (Analysis Services) et que le service dispose d'un nom SPN inscrit pour ce compte. L'inscription du nom SPN requiert que le service s'exécute.  
  
     ![SSAS_Kerberos_1_AccountProperties](../../analysis-services/instances/media/ssas-kerberos-1-accountproperties.gif "SSAS_Kerberos_1_AccountProperties")  
  
3.  Sur la page Ajouter des services, cliquez sur **Utilisateurs ou ordinateurs**.  
  
     ![SSAS_Kerberos_2_](../../analysis-services/instances/media/ssas-kerberos-2.gif "SSAS_Kerberos_2_")  
  
4.  Sur la page de sélection d'utilisateurs ou de l'ordinateur, entrez le compte utilisé pour exécuter l'instance SQL Server qui fournit des données aux bases de données model tabulaires Analysis Services. Cliquez sur **OK** pour accepter le compte de service.  
  
     Si vous ne pouvez pas sélectionner le compte souhaité, vérifiez que SQL Server s'exécute et qu'un SPN est inscrit pour ce compte. Pour plus d'informations sur les noms SPN pour le moteur de base de données, consultez [Inscrire un nom de principal du service pour les connexions Kerberos](../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).  
  
     ![SSAS_Kerberos_3_SelectUsers](../../analysis-services/instances/media/ssas-kerberos-3-selectusers.gif "SSAS_Kerberos_3_SelectUsers")  
  
5.  L'instance de SQL Server doit maintenant apparaître dans Ajouter des services. Tout service qui utilise également ce compte apparaît aussi dans la liste. Choisissez l'instance de SQL Server que vous souhaitez utiliser. Cliquez sur **OK** pour accepter l'instance.  
  
     ![SSAS_Kerberos_4_](../../analysis-services/instances/media/ssas-kerberos-4.gif "SSAS_Kerberos_4_")  
  
6.  La page de propriétés du compte de service Analysis Services doit désormais ressembler à la capture d'écran suivante. Cliquez sur **OK** pour enregistrer vos modifications.  
  
     ![SSAS_Kerberos_5_Finished](../../analysis-services/instances/media/ssas-kerberos-5-finished.gif "SSAS_Kerberos_5_Finished")  
  
7.  Effectuez un test pour vérifier la réussite de la délégation en établissant une connexion à partir d'un ordinateur client distant, sous une identité différente, et interrogez le modèle tabulaire. Vous devez voir l'identité de l'utilisateur dans la demande dans SQL Server Profiler.  
  
##  <a name="bkmk_test"></a> Tester une identité empruntée ou déléguée  
 Utilisez SQL Server profiler pour contrôler l'identité de l'utilisateur qui interroge des données.  
  
1.  Démarrez **SQL Server Profiler** sur l'instance d'Analysis Services, puis démarrez une nouvelle trace.  
  
2.  Dans Sélection des événements, vérifiez que les événements **Audit Login** et **Audit Logout** sont activés dans la section Audit de sécurité.  
  
3.  Connectez-vous à Analysis Services via un service d'application (tel que SharePoint ou Reporting Services) à partir d'un ordinateur client distant. L'événement Audit Login affiche l'identité de l'utilisateur qui se connecte à Analysis Services.  
  
 Un test approfondi requiert l'utilisation d'outils de surveillance du réseau qui peuvent capturer les demandes et les réponses Kerberos sur le réseau. L'utilitaire de surveillance du réseau (netmon.exe), filtré pour Kerberos, peut être utilisé pour cette tâche. Pour plus d’informations sur l’utilisation de Netmon 3.4 et d’autres outils pour tester l’authentification Kerberos, consultez [Configuration de l’authentification Kerberos : configuration de base (SharePoint Server 2010)](http://technet.microsoft.com/library/gg502602\(v=office.14\).aspx).  
  
 En outre, consultez la page consacrée à la [boîte de dialogue la plus déroutante dans Active Directory](http://windowsitpro.com/windows/most-confusing-dialog-box-active-directory) pour obtenir une description détaillée de chaque option sous l'onglet Délégation de la boîte de dialogue des propriétés de l'objet Active Directory. Cet article explique également comment utiliser LDP pour effectuer des tests et interpréter leurs résultats.  
  
## <a name="see-also"></a>Voir aussi  
 [Authentification et délégation d'identité Microsoft BI](http://go.microsoft.com/fwlink/?LinkID=286576)   
 [Authentification mutuelle à l'aide de Kerberos](http://go.microsoft.com/fwlink/?LinkId=299283)   
 [Se connecter à Analysis Services](../../analysis-services/instances/connect-to-analysis-services.md)   
 [Inscription SPN pour une instance Analysis Services](../../analysis-services/instances/spn-registration-for-an-analysis-services-instance.md)   
 [Propriétés de chaîne de connexion & #40 ; Analysis Services & #41 ;](../../analysis-services/instances/connection-string-properties-analysis-services.md)  
  
  
