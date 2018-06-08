---
title: Se connecter à partir d’applications clientes (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d71320fad55b9a0d052ad1bb9c9fd25ab861246c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="connect-from-client-applications-analysis-services"></a>Connexion à partir d'applications clientes (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Si vous débutez avec Analysis Services, utilisez les informations de cette rubrique pour vous connecter à une instance existante de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l'aide d'applications et d'outils courants. Cette rubrique explique également comment se connecter sous différentes identités d'utilisateur à des fins de test.  
  
-   [Établir une connexion à l'aide de SQL Server Management Studio (SSMS)](#bkmk_SSMS)  
  
-   [Établir une connexion à l'aide d'Excel](#bkmk_excel)  
  
-   [Établir une connexion à l'aide de SQL Server Data Tools](#bkmk_SSDT)  
  
-   [Tester les connexions](#bkmk_tshoot)  
  
 La documentation de référence sur les chaînes de connexion est fournie séparément. Pour plus d’informations, consultez [Propriétés des chaînes de connexion &#40;Analysis Services&#41;](../../analysis-services/instances/connection-string-properties-analysis-services.md).  
  
 La réussite des connexions dépend d'une configuration de port valide et d'autorisations utilisateur appropriées. Cliquez sur les liens suivants pour plus d'informations sur chaque exigence.  
  
-   [Configurer le pare-feu Windows pour autoriser l'accès à Analysis Services](../../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md)  
  
-   [Autorisation d’accès aux objets et les opérations de & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/authorizing-access-to-objects-and-operations-analysis-services.md)  
  
##  <a name="bkmk_SSMS"></a> Établir une connexion à l'aide de SQL Server Management Studio (SSMS)  
 Connectez-vous à Analysis Services dans SSMS afin de gérer des instances de serveur et des bases de données de manière interactive. Vous pouvez également exécuter des requêtes MDX ou XMLA afin d'effectuer des tâches administratives ou de récupérer des données. Contrairement à d'autres outils et applications qui chargent les bases de données uniquement lorsqu'une requête est envoyée, SSMS charge toutes les bases de données lorsque vous vous connectez au serveur, en supposant que vous avez l'autorisation d'afficher la base de données. Cela signifie que si vous avez de nombreuses bases de données tabulaires sur le serveur, elles sont toutes chargées dans la mémoire système lorsque vous vous connectez à l'aide de SSMS.  
  
 Vous pouvez tester les autorisations en exécutant SSMS sous une identité d'utilisateur spécifique, puis vous connecter à Analysis Services sous le nom de cet utilisateur.  
  
 Maintenez la touche Maj enfoncée et cliquez avec le bouton droit sur le raccourci **SQL Server Management Studio** pour accéder à l’option **Exécuter en tant qu’autre utilisateur** .  
  
1.  Démarrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Dans la boîte de dialogue **Se connecter au serveur** , sélectionnez le type de serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
2.  Sous l'onglet Connexion, entrez le nom du serveur en tapant le nom de l'ordinateur sur lequel le serveur est en cours d'exécution. Vous pouvez spécifier le serveur à l'aide de son nom réseau ou d'un nom de domaine complet.  
  
     Pour une instance nommée, le nom du serveur doit être spécifié selon le format suivant : nom serveur\nom instance. ADV-SRV062\Finance constitue un exemple de cette convention d'affectation des noms pour un serveur dont le nom réseau est ADV-SRV062, et sur lequel Analysis Services a été installé en tant qu'instance nommée portant le nom Finance.  
  
     Pour les serveurs déployés dans un cluster de basculement, connectez-vous à l'aide du nom réseau du cluster SSAS. Ce nom est spécifié pendant l'installation de SQL Server, comme **Nom réseau SQL Server**. Notez que si vous avez installé SSAS comme instance nommée sur un cluster de basculement Windows Server (WSFC), vous n'ajoutez jamais le nom de l'instance pour la connexion. Cette pratique est propre à SSAS ; en revanche, une instance nommée du moteur de base de données relationnelle cluster inclut le nom de l'instance. Par exemple, si vous avez installé SSAS et le moteur de base de données en tant qu'instance nommée (Contoso-Accounting) avec un nom réseau SQL Server SQL-CLU, vous vous connecteriez à SSAS en utilisant « SQL-CLU », et au moteur de base de données en tant que « SQL-CLU\Contoso-Accounting ». Consultez [Procédure : mettre en cluster SQL Server Analysis Services](http://go.microsoft.com/fwlink/p/?LinkId=396548) pour plus d'informations et des exemples.  
  
     Pour les serveurs déployés dans un cluster d'équilibrage de la charge réseau, connectez-vous à l'aide du nom du serveur virtuel d'équilibrage de la charge réseau.  
  
3.  L'authentification est toujours l'authentification Windows, et l'identité d'utilisateur est toujours l'utilisateur Windows qui se connecte au moyen de Management Studio.  
  
     Pour que la connexion réussisse, vous devez disposer de l'autorisation d'accès au serveur ou à une base de données située sur le serveur. La plupart des tâches à effectuer dans Management Studio nécessitent des autorisations d'administrateur. Vérifiez que le compte auquel vous vous connectez est membre du rôle d'administrateur de serveur. Pour plus d’informations, consultez [Accorder des droits d’administrateur de serveur à une instance Analysis Services](../../analysis-services/instances/grant-server-admin-rights-to-an-analysis-services-instance.md).  
  
4.  Cliquez sur **Propriétés de connexion** pour spécifier une base de données, définir des valeurs de délai d'attente ou des options de chiffrement. Parmi les informations de connexion facultatives, citons les propriétés de connexion qui sont utilisées uniquement pour la connexion actuelle.  
  
5.  Cliquez sur l'onglet **Paramètres de connexion supplémentaires** pour définir les propriétés de connexion qui ne sont pas disponibles dans la boîte de dialogue Se connecter au serveur. Par exemple, vous pouvez taper `Roles=Reader` dans la zone de texte.  
  
     Se connecter via un rôle disposant de moins d'autorisations permet de tester les comportements de la base de données lorsque ce rôle est appliqué.  
  
    ```  
    Provider=MSOLAP; Data Source=SERVERNAME; Initial Catalog=AdventureWorks2012; Roles=READER  
    ```  
  
##  <a name="bkmk_excel"></a> Établir une connexion à l'aide d'Excel  
 Microsoft Excel est souvent utilisé lors de l'analyse de données commerciales. Dans le cadre d'une installation d'Excel, Office installe le fournisseur OLE DB Analysis Services (MSOLAP DLL), ADOMD.NET et d'autres fournisseurs de données afin de vous permettre d'utiliser plus facilement les données se trouvant sur vos serveurs réseau. Si vous utilisez une version plus récente de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] avec une version antérieure d'Excel, vous devrez probablement installer de nouveaux fournisseurs de données sur chaque poste de travail qui se connecte à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d’informations, consultez [Fournisseurs de données utilisés pour les connexions Analysis Services](../../analysis-services/instances/data-providers-used-for-analysis-services-connections.md) .  
  
 Quand vous configurez une connexion à un cube Analysis Services ou à une base de données model tabulaire, Excel enregistre les informations de connexion dans le fichier .odc en vue d'une utilisation ultérieure. La connexion est établie dans le contexte de sécurité de l'utilisateur Windows actuel. Pour que la connexion réussisse, ce compte d'utilisateur doit disposer d'autorisations en lecture sur la base de données.  
  
 Lorsque vous utilisez des données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans un classeur Excel, les connexions sont conservées pendant la durée de la requête. C'est pourquoi vous risquez de voir un grand nombre de connexions pour chaque session, conservées pendant des périodes très courtes, lors de l'analyse d'une charge de travail de requête depuis Excel.  
  
 Vous pouvez tester les autorisations lorsque vous démarrez Excel sous une identité d'utilisateur spécifique.  
  
 Maintenez la touche Maj enfoncée et cliquez avec le bouton droit sur le raccourci **Excel** pour accéder à l’option **Exécuter en tant qu’autre utilisateur** .  
  
1.  Dans l'onglet Données d'Excel, cliquez sur **À partir d'autres sources**, puis sur **À partir d'Analysis Services**. Entrez le nom du serveur, puis sélectionnez un cube ou une perspective pour la requête.  
  
     Pour les serveurs déployés dans un cluster à charge équilibrée, utilisez le nom du serveur virtuel attribué au cluster.  
  
2.  Lors de la configuration d'une connexion dans Excel, dans la dernière page de l'Assistant Connexion de données, vous pouvez spécifier des paramètres d'authentification pour Excel Services. Ces paramètres sont utilisés pour définir des propriétés sur le classeur dans l'éventualité où vous deviez le télécharger sur un serveur SharePoint disposant d'Excel Services. Ils sont utilisés dans les opérations d'actualisation des données. Les options sont **Authentification Windows**, **Service Banque d’informations sécurisé** (SSS) et **Aucun**.  
  
     Évitez d'utiliser **Aucun**. Analysis Services ne vous permet de spécifier un nom d'utilisateur et un mot de passe dans la chaîne de connexion que si vous vous connectez à un serveur pour lequel l'accès HTTP a été configuré. De même, n'utilisez SSS que si vous savez déjà que l'ID de l'application cible SSS est mappé à un ensemble d'informations d'identification d'utilisateur Windows qui ont un accès utilisateur aux bases de données Analysis Services. Dans la plupart des scénarios, l'utilisation de l'option par défaut Authentification Windows constitue le meilleur choix pour une connexion Analysis Services à partir d'Excel.  
  
 Pour plus d'informations, consultez [Se connecter ou importer des données à partir de SQL Server Analysis Services](http://go.microsoft.com/fwlink/?linkID=215150).  
  
##  <a name="bkmk_SSDT"></a> Établir une connexion à l'aide de SQL Server Data Tools  
 SQL Server Data Tools est utilisé pour créer des solutions de décisionnel, notamment des modèles Analysis Services, des rapports Reporting Services et des packages SSIS. Pour générer des rapports ou des packages, vous devrez peut-être spécifier une connexion à Analysis Services.  
  
 Les liens suivants expliquent comment se connecter à Analysis Services à partir d'un projet Report Server ou d'un projet Integration Services :  
  
-   [Type de connexion Analysis Services pour MDX &#40;SSRS&#41;](../../reporting-services/report-data/analysis-services-connection-type-for-mdx-ssrs.md)  
  
-   [Gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/analysis-services-connection-manager.md)  
  
> [!NOTE]  
>  Lorsque vous utilisez SQL Server Data Tools pour travailler avec un projet Analysis Services existant, n'oubliez pas que vous pouvez travailler en mode hors connexion à l'aide d'un projet local ou contrôlé par version, ou vous connecter en mode en ligne pour mettre à jour des objets Analysis Services lorsque la base de données s'exécute. Pour plus d'informations, consultez [Connect in Online Mode to an Analysis Services Database](../../analysis-services/multidimensional-models/connect-in-online-mode-to-an-analysis-services-database.md). En général, les connexions à partir de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] sont en mode projet, dans lequel les modifications sont déployées sur la base de données uniquement lorsque vous déployez explicitement le projet.  
  
##  <a name="bkmk_tshoot"></a> Tester les connexions  
 Vous pouvez utiliser SQL Server Profiler pour surveiller les connexions à Analysis Services. Les événements Audit Login et Audit Logout fournissent la preuve d'une connexion. La colonne d'identité indique le contexte de sécurité dans lequel est établie la connexion.  
  
1.  Démarrez **SQL Server Profiler** sur l'instance d'Analysis Services, puis démarrez une nouvelle trace.  
  
2.  Dans Sélection des événements, vérifiez que les événements **Audit Login** et **Audit Logout** sont activés dans la section Audit de sécurité.  
  
3.  Connectez-vous à Analysis Services via un service d'application (tel que SharePoint ou Reporting Services) à partir d'un ordinateur client distant. L'événement Audit Login affiche l'identité de l'utilisateur qui se connecte à Analysis Services.  
  
 Les erreurs de connexion sont souvent le fait d'une configuration de serveur incomplète ou non valide. Commencez toujours par vérifier la configuration du serveur :  
  
-   Interrogez le serveur à partir d'un ordinateur distant pour vérifier qu'il accepte les connexions distantes.  
  
-   **Les règles de pare-feu sur le serveur permettent les connexions entrantes des clients dans le même domaine**  
  
     À l’exception de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, toutes les connexions à un serveur distant nécessitent que le pare-feu soit configuré pour autoriser l’accès au port écouté par Analysis Services. Si vous obtenez des erreurs de connexion, vérifiez que le port est accessible et que des autorisations utilisateur sont accordées aux bases de données appropriées.  
  
     Pour effectuer un test, utilisez Excel ou SSMS sur un ordinateur distant, en spécifiant l'adresse IP et le port utilisé par l'instance Analysis Services. Si vous pouvez établir une connexion, les règles de pare-feu sont valides pour l'instance et cette dernière accepte les connexions distantes.  
  
     En outre, si vous utilisez TCP/IP comme protocole de connexion, n'oubliez pas qu'Analysis Services requiert que les connexions clientes proviennent du même domaine ou d'un domaine approuvé. Si les connexions se transmettent entre les limites de sécurité, vous devrez probablement configurer l'accès HTTP. Pour plus d’informations, consultez [Configurer l’accès HTTP à Analysis Services sur Internet Information Services &#40;IIS&#41; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md).  
  
-   Pouvez-vous vous connecter à l'aide de certains outils mais pas avec d'autres ? Le problème provient peut-être d'une version incorrecte d'une bibliothèque cliente. Vous pouvez obtenir les bibliothèques clientes à partir de la page de téléchargement de SQL Server Feature Pack.  
  
 Autres ressources qui peuvent vous aider à résoudre les problèmes de connexion :  
  
 [Résolution des problèmes courants de connectivité dans les scénarios de connectivité de SQL Server 2005 Analysis Services](http://technet.microsoft.com/library/cc917670.aspx). Ce document date de quelques années, mais les informations et les méthodologies décrites sont toujours d'actualité.  
  
## <a name="see-also"></a>Voir aussi  
 [Se connecter à Analysis Services](../../analysis-services/instances/connect-to-analysis-services.md)   
 [Méthodologies d’authentification pris en charge par Analysis Services](../../analysis-services/instances/authentication-methodologies-supported-by-analysis-services.md)   
 [Emprunt d’identité](../../analysis-services/tabular-models/impersonation-ssas-tabular.md)   
 [Créer une Source de données & #40 ; SSAS multidimensionnel & #41 ;](../../analysis-services/multidimensional-models/create-a-data-source-ssas-multidimensional.md)  
  
  
