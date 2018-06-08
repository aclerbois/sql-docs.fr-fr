---
title: Configurer les comptes de Service (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: dff21ebf96bf957a7f390b8dea0010fa0396ff7d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-service-accounts-analysis-services"></a>Configurer les comptes de service (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L’approvisionnement de comptes à l’échelle du produit est traité dans la rubrique [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md). Vous y trouverez des informations complètes sur les comptes de service pour tous les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], y compris [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Reportez-vous à cette rubrique pour en savoir plus sur les types de comptes valides, les privilèges Windows assignés par le programme d'installation, les autorisations du système de fichiers, les autorisations de Registre et bien plus encore.  
  
 Cette rubrique fournit des informations complémentaires pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], notamment sur les autorisations supplémentaires nécessaires pour les installations tabulaires et en cluster. Elle traite également des autorisations nécessaires pour prendre en charge les opérations de serveur. Par exemple, vous pouvez configurer des opérations de traitement et de requête pour qu'elles s'exécutent sous le compte de service, auquel cas vous devrez accordez des autorisations supplémentaires.  
  
-   [Privilèges Windows assignés à Analysis Services](#bkmk_winpriv)  
  
-   [Autorisations de système de fichiers assignées à Analysis Services](#bkmk_FilePermissions)  
  
-   [Octroi d'autorisations supplémentaires pour des opérations de serveur spécifiques](#bkmk_tasks)  
  
 Une autre étape de configuration, non documentée ici, consiste à inscrire un nom de principal du service (SPN) pour le compte de service et l'instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Cette étape autorise l'authentification directe d'applications clientes auprès de sources de données principales dans les scénarios à deux tronçons. Cette étape s'applique uniquement aux services configurés pour la délégation Kerberos contrainte. Pour obtenir des instructions supplémentaires, consultez [Configure Analysis Services for Kerberos constrained delegation](../../analysis-services/instances/configure-analysis-services-for-kerberos-constrained-delegation.md) .  
  
## <a name="logon-account-recommendations"></a>Recommandations relatives aux comptes d'ouverture de session  
 Dans un cluster de basculement, toutes les instances d'Analysis Services doivent être configurées pour utiliser un compte d'utilisateur de domaine Windows. Veuillez assigner le même compte à toutes les instances. Pour plus d'informations, consultez [Procédure de mise en cluster d'Analysis Services](http://msdn.microsoft.com/library/dn736073.aspx) .  
  
 Instances autonomes doivent utiliser le compte virtuel par défaut, **NT Service\MSSQLServerOLAPService** pour l’instance par défaut, ou **NT Service\MSOLAP$ ***-nom de l’instance* pour une instance nommée. Cette recommandation s'applique aux instances d'Analysis Services dans tous les modes de serveur, en partant du principe que la version du système d'exploitation est Windows Server 2008 R2 et versions ultérieures, et que celle d'Analysis Services est SQL Server 2012 et versions ultérieures.  
  
## <a name="granting-permissions-to-analysis-services"></a>Octroi d'autorisations à Analysis Services  
 Cette section présente les autorisations requises par Analysis Services pour les opérations locales et internes telles que le démarrage d'un exécutable, la lecture d'un fichier de configuration et le chargement de bases de données à partir du répertoire des données. Si vous recherchez plutôt des conseils sur la configuration des autorisations pour l'accès aux données externes et l'interopérabilité avec d'autres services et applications, consultez [Octroi d'autorisations supplémentaires pour des opérations de serveur spécifiques](#bkmk_tasks) plus loin dans cette rubrique.  
  
 Pour les opérations internes, le détendeur d'autorisations dans Analysis Services n'est pas le compte d'ouverture de session, mais un groupe de sécurité Windows local créé par le programme d'installation qui contient le SID par service. L'attribution des autorisations au groupe de sécurité est cohérente avec les versions antérieures d'Analysis Services. De même, les comptes d'ouverture de session peuvent varier avec le temps, mais le SID par service et le groupe de sécurité local ne changent pas à partir de leur installation sur le serveur. Ainsi, pour Analysis Services, le groupe de sécurité constitue un choix plus approprié pour contenir les autorisations. Si vous octroyez les droits à l'instance de service manuellement, qu'il s'agisse d'autorisations de système de fichiers ou de privilèges Windows, assurez-vous d'octroyer les autorisations au groupe de sécurité local créé pour l'instance de serveur.  
  
 Le nom du groupe de sécurité doit respecter un modèle précis. Son préfixe est toujours **SQLServerMSASUser$**, suivi par le nom de l'ordinateur, et se terminant par le nom de l'instance. L'instance par défaut est **MSSQLSERVER**. Une instance nommée correspond au nom donné durant la configuration.  
  
 Vous pouvez consulter ce groupe de sécurité dans les paramètres de sécurité locale :  
  
-   Exécutez compmgmt.msc | **Utilisateurs et groupes locaux** | **groupes** | **SQLServerMSASUser$**\<nom-serveur >**$MSSQLSERVER** (pour une instance par défaut).  
  
-   Double-cliquez sur le groupe de sécurité pour afficher ses membres.  
  
 Le seul membre du groupe est le SID par service. Juste à côté se trouve le compte d'ouverture de session. Le nom de compte d'ouverture de session est symbolique, il permet de fournir du contexte au SID par service. Si vous modifiez ultérieurement le compte d'ouverture de session et que vous revenez ensuite à cette page, vous remarquerez que le groupe de sécurité et le SID par service ne changent pas, mais que l'étiquette du compte d'ouverture de session est différente.  
  
##  <a name="bkmk_winpriv"></a> Privilèges Windows affectés au compte de service Analysis Services  
 Analysis Services a besoin d'autorisations du système d'exploitation pour le démarrage du service et pour demander des ressources système. Les exigences varient en fonction du mode serveur et selon qu'il s'agit ou non d'une instance en cluster. Si vous souhaitez vous familiariser avec les privilèges Windows, consultez [Privileges](http://msdn.microsoft.com/library/windows/desktop/aa379306\(v=vs.85\).aspx) (Privilèges) et [Privilege Constants (Windows)](http://msdn.microsoft.com/library/windows/desktop/bb530716\(v=vs.85\).aspx) [Constantes de privilèges (Windows)].  
  
 Toutes les instances d’Analysis Services nécessitent le privilège **Ouvrir une session en tant que service** (SeServiceLogonRight). Le programme d'installation de SQL Server assigne automatiquement ce privilège sur le compte de service spécifié durant l'installation. Pour les serveurs qui s'exécutent en mode multidimensionnel et exploration de données, il s'agit du seul privilège Windows requis par le compte de service Analysis Services pour les installations serveur autonome et du seul privilège que le programme d'installation configure pour Analysis Services. Pour les instances tabulaires et en cluster, des privilèges Windows supplémentaires doivent être ajoutés manuellement.  
  
 Les instances de cluster de basculement, en mode tabulaire ou multidimensionnel, nécessitent le privilège **Augmenter la priorité de planification** (SeIncreaseBasePriorityPrivilege).  
  
 Les instances tabulaires utilisent les trois privilèges supplémentaires suivants, qui doivent être accordés manuellement une fois l'instance installée.  
  
|||  
|-|-|  
|**Augmenter une plage de travail de processus** (SeIncreaseWorkingSetPrivilege)|Ce privilège est accessible par défaut à tous les utilisateurs par l'intermédiaire du groupe de sécurité **Utilisateurs** . Si vous verrouillez un serveur en supprimant des privilèges pour ce groupe, le démarrage d'Analysis Services risque d'échouer avec l'erreur suivante : « Le client ne dispose pas d'un privilège qui est obligatoire. » Lorsque cette erreur se produit, restaurez le privilège à Analysis Services en l'accordant au groupe de sécurité Analysis Services approprié.|  
|**Ajuster les quotas de mémoire pour un processus** (SeIncreaseQuotaSizePrivilege)|Ce privilège sert à demander davantage de mémoire si un processus dispose de ressources insuffisantes pour terminer son exécution, avec comme limitation les seuils de mémoire établis pour l'instance.|  
|**Verrouiller les pages en mémoire** (SeLockMemoryPrivilege)|Ce privilège est nécessaire uniquement lorsque la pagination est complètement désactivée. Par défaut, une instance serveur tabulaire utilise le fichier de pagination Windows, mais vous pouvez l'empêcher d'utiliser la pagination Windows en affectant la valeur 0 au paramètre **VertiPaqPagingPolicy** .<br /><br /> Si**VertiPaqPagingPolicy** a la valeur 1 (valeur par défaut), l’instance serveur tabulaire utilise le fichier de pagination Windows. Les allocations ne sont pas verrouillées, ce qui permet à Windows de paginer selon les besoins. La pagination étant utilisée, nul besoin de verrouiller les pages en mémoire. Ainsi, pour la configuration par défaut (où **VertiPaqPagingPolicy** = 1), vous n’avez pas besoin d’accorder le privilège **Verrouiller les pages en mémoire** à une instance tabulaire.<br /><br /> **VertiPaqPagingPolicy** égal à 0. Si vous désactivez la pagination pour Analysis Services, les allocations sont verrouillées, ce qui implique que le privilège **Verrouiller les pages en mémoire** est accordé à l'instance tabulaire. Étant donné ce paramètre et le privilège **Verrouiller les pages en mémoire** , Windows ne peut pas paginer les allocations effectuées à Analysis Services lorsque le système est soumis à une sollicitation de la mémoire. Analysis Services compte sur l’autorisation **Verrouiller les pages en mémoire** pour mettre en œuvre **VertiPaqPagingPolicy** = 0. La désactivation de la pagination Windows n'est pas recommandée. Cela augmente le taux d'erreurs liées à l'insuffisance de mémoire pour les opérations qui réussiraient si la pagination était autorisée. Pour plus d'informations sur [VertiPaqPagingPolicy](../../analysis-services/server-properties/memory-properties.md) , consultez **Memory Properties**.|  
  
#### <a name="to-view-or-add-windows-privileges-on-the-service-account"></a>Pour afficher ou ajouter des privilèges Windows sur le compte de service  
  
1.  Exécutez GPEDIT.msc | Stratégie de l'ordinateur local | Configuration ordinateur | Paramètres Windows | Paramètres de sécurité | Stratégies locales | Attribution des droits utilisateur.  
  
2.  Examinez les stratégies existantes qui contiennent **SQLServerMSASUser$**. Il s'agit d'un groupe de sécurité local présent sur les ordinateurs ayant une installation d'Analysis Services. Les privilèges Windows et les autorisations sur les dossiers de fichiers sont accordés à ce groupe de sécurité. Doublez-cliquez sur **Ouvrir une session en tant que service** pour voir de quelle façon le groupe de sécurité est spécifié sur votre système. Le nom complet du groupe de sécurité varie selon que vous avez installé Analysis Services comme instance nommée ou non. Utilisez ce groupe de sécurité plutôt que le compte de service lors de l'ajout des privilèges de compte.  
  
3.  Pour ajouter des privilèges de compte dans GPEDIT, cliquez avec le bouton droit sur **Augmenter une plage de travail de processus** et sélectionnez **Propriétés**.  
  
4.  Cliquez sur **Ajouter un groupe ou un utilisateur**.  
  
5.  Entrez le groupe d'utilisateurs de l'instance d'Analysis Services. N'oubliez pas que le compte de service est membre d'un groupe de sécurité local, ce qui nécessite que vous ajoutiez le nom de l'ordinateur local comme domaine du compte.  
  
     La liste suivante montre deux exemples pour une instance par défaut et une instance nommée « Tabular » sur un ordinateur « SQL01-WIN12 », où le nom de l'ordinateur est le domaine local.  
  
    -   SQL01-WIN12\SQL01-WIN12$SQLServerMSASUser$MSSQLSERVER  
  
    -   SQL01-WIN12\SQL01-WIN12$SQLServerMSASUser$TABULAR  
  
6.  Répétez l'opération pour **Ajuster les quotas de mémoire pour un processus**et, éventuellement, pour **Verrouiller les pages en mémoire** ou **Augmenter la priorité de planification**.  
  
> [!NOTE]  
>  Les versions antérieures du programme d'installation ajoutaient par inadvertance le compte de service Analysis Services au groupe **Utilisateurs du journal des performances** . Bien que ce problème ait été résolu, il est possible que des installations existantes présentent cette appartenance de groupe superflue. Comme le compte de service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne nécessite aucune appartenance au groupe **Utilisateurs du journal des performances** , vous pouvez le supprimer du groupe.  
  
##  <a name="bkmk_FilePermissions"></a> Autorisations de système de fichiers affectées au compte de service Analysis Services  
  
> [!NOTE]  
>  Consultez [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md) pour obtenir la liste des autorisations associées à chaque dossier de programme.  
>   
>  Consultez [Configurer l’accès HTTP à Analysis Services sur Internet Information Services &#40;IIS&#41; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md) pour obtenir des informations sur les autorisations de fichier associées à la configuration IIS et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 Toutes les autorisations de système de fichiers requises pour les opérations serveur, y compris celles nécessaires pour charger ou décharger des bases de données à partir d'un dossier de données désigné, sont assignées par le programme d'installation de SQL Server durant l'installation.  
  
 Le conteneur des autorisations sur les fichiers de données, les exécutables des fichiers programme, les fichiers de configuration, les fichiers journaux et les fichiers temporaires est un groupe de sécurité local créé par le programme d'installation de SQL Server.  
  
 Un groupe de sécurité est créé pour chaque instance que vous installez. Le groupe de sécurité est nommé d’après l’instance : soit **SQLServerMSASUser$ MSSQLSERVER** pour l’instance par défaut, ou **SQLServerMSASUser$**\<nom_serveur >$\<nom_instance > pour une instance nommée. Le programme d'installation configure ce groupe de sécurité avec les autorisations de fichiers requises pour effectuer les opérations serveur. Si vous vérifiez les autorisations de sécurité dans le répertoire \MSAS13.MSSQLSERVER\OLAP\BIN, vous verrez que le groupe de sécurité (et non le compte de service ou son SID par service) est le détenteur des autorisations sur ce répertoire.  
  
 Le groupe de sécurité contient un membre uniquement : l'identificateur de sécurité (SID) par service du compte de démarrage de l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Le programme d'installation ajoute le SID par service au groupe de sécurité local. L'utilisation d'un groupe de sécurité local, avec son appartenance au SID, est la différence entre la configuration d'Analysis Services par le programme d'installation de SQL Server et le moteur de base de données.  
  
 Si vous pensez que les autorisations de fichiers sont corrompues, suivez les étapes ci-après pour vérifier que le service est correctement configuré :  
  
1.  Utilisez l'outil en ligne de commande Contrôle du service (sc.exe) pour obtenir le SID d'une instance de service par défaut.  
  
     `SC showsid MSSqlServerOlapService`  
  
     Pour une instance nommée (où le nom de l'instance est Tabular), utilisez la syntaxe suivante :  
  
     `SC showsid MSOlap$Tabular`  
  
2.  Utilisez **Computer Manager** | **utilisateurs et groupes locaux** | **groupes** pour inspecter l’appartenance de SQLServerMSASUser$\<nom_serveur >$\<instancename > groupe de sécurité.  
  
     Le SID de membre doit correspondre au SID par service obtenu à l'étape 1.  
  
3.  Utilisez **Explorateur Windows** | **Program Files** | **Microsoft SQL Server** | MSASxx.MSSQLServer | **OLAP** | **bin** pour vérifier que les propriétés de sécurité des dossiers sont accordées au groupe de sécurité de l’étape 2.  
  
> [!NOTE]  
>  Ne supprimez ni ne modifiez jamais un SID. Pour restaurer un SID par service qui a été supprimé par inadvertance, consultez [ http://support.microsoft.com/kb/2620201 ](http://support.microsoft.com/kb/2620201).  
  
 **En savoir plus sur les SID par service**  
  
 Chaque compte Windows a un [SID](http://en.wikipedia.org/wiki/Security_Identifier)associé, mais les services peuvent également avoir un SID appelé « SID par service ». Un SID par service est créé lors de l'installation de l'instance du service, en tant que caractéristique fixe et unique du service. Le SID par service est un SID local au niveau de l'ordinateur, généré à partir du nom du service. Sur une instance par défaut, son nom convivial est NT SERVICE\MSSQLServerOLAPService.  
  
 Le SID par service offre l'avantage de permettre la modification arbitraire du compte d'ouverture de session (plus visible) sans affecter les autorisations de fichiers. Supposons, par exemple, que vous ayez installé deux instances d'Analysis Services, une instance par défaut et une instance nommée, les deux s'exécutant sous le même compte d'utilisateur Windows. Bien que le compte d'ouverture de session soit partagé, chaque instance de service a un SID par service unique. Ce SID est distinct du SID du compte d'ouverture de session. Le SID par service est utilisé pour les autorisations de fichiers et les privilèges Windows. Le SID de compte d'ouverture de session sert quant à lui aux scénarios d'authentification et d'autorisation. Il s'agit donc de SID différents utilisés à des fins différentes.  
  
 Le SID étant non modifiable, les listes de contrôle d'accès du système de fichiers créées durant l'installation du service peuvent être utilisées indéfiniment, quelle que soit la fréquence de modification du compte de service. Par mesure de sécurité supplémentaire, les listes de contrôle d'accès (ACL) qui spécifient des autorisations via un SID garantissent que les exécutables de programme et les dossiers de données ne sont accessibles que par une seule instance d'un service, même si d'autres services s'exécutent sous le même compte.  
  
##  <a name="bkmk_tasks"></a> Octroi d'autorisations Analysis Services supplémentaires pour des opérations spécifiques  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]exécute certaines tâches dans le contexte de sécurité du compte de service (ou du compte d’ouverture de session) qui est utilisé pour démarrer [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]et exécute d’autres tâches dans le contexte de sécurité de l’utilisateur qui demande la tâche.  
  
 Le tableau suivant décrit les autorisations supplémentaires nécessaires pour prendre en charge les tâches s'exécutant en tant que compte de service.  
  
|Opération du serveur|Élément de travail|Justification|  
|----------------------|---------------|-------------------|  
|Accès à distance aux sources de données relationnelles externes|Créer un nom d'accès à la base de données pour le compte de service|Le traitement fait référence à la récupération de données depuis une source de données externe (généralement une base de données relationnelle), qui est ensuite chargée dans une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . L'une des options d'informations d'identification pour la récupération de données externes consiste à utiliser le compte de service. Cette option d'informations d'identification fonctionne uniquement si vous créez une connexion à une base de données pour le compte de service et octroyez des autorisations de lecture sur la base de données source. Pour plus d’informations sur l’utilisation de l’option de compte de service pour cette tâche, consultez [Définir les options d’emprunt d’identité &#40;SSAS - Multidimensionnel&#41;](../../analysis-services/multidimensional-models/set-impersonation-options-ssas-multidimensional.md). De la même manière, si ROLAP est utilisé comme mode de stockage, les mêmes options d'emprunt d'identité sont disponibles. Dans ce cas, le compte doit également disposer d'un accès en écriture aux données sources pour pouvoir traiter les partitions ROLAP (c'est-à-dire pour stocker les agrégations).|  
|DirectQuery|Créer un nom d'accès à la base de données pour le compte de service|DirectQuery est une fonctionnalité tabulaire utilisée pour interroger des datasets externes qui sont soit trop volumineux pour s'ajuster à l'intérieur du modèle tabulaire ou qui présentent d'autres caractéristiques qui font de DirectQuery un meilleur choix que l'option de stockage en mémoire par défaut. L'une des options de connexion disponibles en mode DirectQuery consiste à utiliser le compte de service. Là encore, cette option fonctionne uniquement lorsque le compte de service dispose d'une connexion à une base de données, ainsi que d'autorisations de lecture sur la source de données cible. Pour plus d’informations sur l’utilisation de l’option de compte de service pour cette tâche, consultez [Définir les options d’emprunt d’identité &#40;SSAS - Multidimensionnel&#41;](../../analysis-services/multidimensional-models/set-impersonation-options-ssas-multidimensional.md). Les informations d'identification de l'utilisateur actuel peuvent également être utilisées pour récupérer des données. Dans la majorité des cas, cette option implique une connexion à deux tronçons. Par conséquent, veillez à configurer le compte de service pour la délégation Kerberos contrainte afin que ce compte de service puisse déléguer des identités à un serveur en aval. Pour plus d'informations, consultez [Configure Analysis Services for Kerberos constrained delegation](../../analysis-services/instances/configure-analysis-services-for-kerberos-constrained-delegation.md).|  
|Accès à distance à d'autres instances SSAS|Ajouter le compte de service aux rôles de base de données Analysis Services définis sur le serveur distant|Les partitions distantes et les objets liés de référencement sur d'autres instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] distantes constituent dans les deux cas des fonctions de système nécessitant des autorisations sur un périphérique ou un ordinateur distant. Lorsqu'une personne crée et alimente des partitions distantes ou configure un objet lié, cette opération s'exécute dans le contexte de sécurité de l'utilisateur actuel. Si, par la suite, vous automatisez ces opérations, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] accédera aux instances distantes dans le contexte de sécurité de son compte de service. Pour pouvoir accéder aux objets liés d'une instance distante [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], le compte d'ouverture de session doit être autorisé à lire les objets appropriés sur l'instance distante (accès en lecture sur certaines dimensions, par exemple). De même, l'utilisation de partitions distantes implique obligatoirement que le compte de service dispose de droits d'administration sur l'instance distante. De telles autorisations sont octroyées sur l'instance Analysis Services distante, au moyen de rôles qui associent les opérations autorisées à un objet spécifique. Pour obtenir des instructions sur l’octroi d’autorisations Contrôle total pour les opérations de traitement et de requête, consultez [Octroyer des autorisations de base de données &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-database-permissions-analysis-services.md). Pour plus d’informations sur les partitions distantes, consultez [Créer et gérer une partition distante &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/create-and-manage-a-remote-partition-analysis-services.md).|  
|Écriture différée|Ajouter le compte de service aux rôles de base de données Analysis Services définis sur le serveur distant|Lorsqu'elle est activée dans les applications clientes, l'écriture différée est une fonctionnalité des modèles multidimensionnels qui permet la création de nouvelles valeurs de données au cours de l'analyse des données. Si l'écriture différée est activée dans une dimension ou un cube, le compte de service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] doit disposer d'autorisations d'écriture dans la table d'écriture différée de la base de données relationnelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] source. Si cette table n'existe pas et doit être créée, le compte de service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] doit également disposer des autorisations lui permettant de créer la table dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définie.|  
|Écrire dans une table du journal des requêtes d'une base de données relationnelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Créer une connexion de base de données pour le compte de service et assigner des autorisations d'écriture sur la table du journal des requêtes|Activez la journalisation des requêtes afin de recueillir des données d'utilisation dans une table de base de données, en vue d'une analyse ultérieure. Le compte de service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] doit disposer d'autorisations d'écriture dans la table du journal des requêtes dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définie. Si la table n'existe pas et doit être créée, le compte d'ouverture de session [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] doit également disposer des autorisations lui permettant de créer la table dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définie. Pour plus d’informations, consultez le blog [Improve SQL Server Analysis Services Performance with the Usage Based Optimization Wizard](http://www.mssqltips.com/sqlservertip/2876/improve-sql-server-analysis-services-performance-with-the-usage-based-optimization-wizard/) (Améliorer les performances de SQL Server Analysis Services à l’aide de l’Assistant Optimisation basée sur l’utilisation) et le blog [Query Logging in Analysis Services](http://weblogs.asp.net/miked/archive/2013/07/31/query-logging-in-analysis-services.aspx)(Journalisation des requêtes dans Analysis Services).|  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer les comptes de Service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)   
 [SQL Server Service Account and Per-Service SID (blog)](http://www.travisgan.com/2013/06/sql-server-service-account-and-per.html)   
 [SQL Server utilise un SID de service pour assurer l'isolement du service (article de la Base de connaissances)](http://support.microsoft.com/kb/2620201)   
 [Jeton d'accès (MSDN)](http://msdn.microsoft.com/library/windows/desktop/aa374909\(v=vs.85\).aspx)   
 [Identificateurs de sécurité (MSDN)](http://msdn.microsoft.com/library/windows/desktop/aa379571\(v=vs.85\).aspx)   
 [Jeton d'accès (Wikipedia)](http://en.wikipedia.org/wiki/Access_token)   
 [Listes de contrôle d'accès (Wikipedia)](http://en.wikipedia.org/wiki/Access_control_list)  
  
  
