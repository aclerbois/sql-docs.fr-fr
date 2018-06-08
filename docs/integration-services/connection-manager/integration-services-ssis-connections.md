---
title: Connexions Integration Services (SSIS) | Microsoft Docs
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
- sql13.asvs.connectionmanager.f1
- sql13.dts.designer.adddtsconnection.f1
helpviewer_keywords:
- Integration Services packages, connections
- SSIS packages, connections
- sources [Integration Services], connections
- packages [Integration Services], connections
- destinations [Integration Services], connections
- tasks [Integration Services], connections
- connections [Integration Services], about connections
- connections [Integration Services]
- SQL Server Integration Services packages, connections
ms.assetid: 72f5afa3-d636-410b-9e81-2ffa27772a8c
caps.latest.revision: 92
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c6ece524c8ee7565b692902b5f8d1e7dbcbd5db6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-ssis-connections"></a>Connexions Integration Services (SSIS)
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilisent des connexions pour effectuer différentes tâches et pour implémenter des fonctionnalités [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] :  
  
-   Connexion à des banques de données sources et de destination telles que du texte, des données XML, des classeurs Excel et des bases de données relationnelles pour extraire et charger des données.  
  
-   Connexion à des bases de données relationnelles qui contiennent des données de référence pour effectuer des recherches exactes ou floues.  
  
-   Connexion à des bases de données relationnelles pour exécuter des instructions SQL telles que des commandes SELECT, DELETE et INSERT, et également des procédures stockées.  
  
-   Connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour effectuer des tâches de maintenance et de transfert telles que la sauvegarde de bases de données et le transfert de connexions.  
  
-   Écriture d'entrées de journal dans des fichiers texte, dans des fichiers XML et dans des tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , écriture de configurations de package dans des tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour créer des tables de travail temporaires dont certaines transformations ont besoin pour effectuer leur travail.  
  
-   Connexion à des projets et des bases de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour accéder à des modèles d'exploration de données, traiter des cubes et des dimensions, et exécuter du code DDL.  
  
-   Spécification de fichiers et de dossiers existants, ou création de nouveaux fichiers et dossiers à utiliser avec des énumérateurs et des tâches de boucles Foreach.  
  
-   Connexion à des files d’attente de messages, à WMI (Windows Management Instrumentation), à SMO ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Object), au web et à des serveurs de messagerie.  
  
 Pour établir ces connexions, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise des gestionnaires de connexions, comme décrit dans la section suivante.  
  
## <a name="connection-managers"></a>Gestionnaires de connexions  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise le gestionnaire de connexions comme représentation logique d'une connexion. Au moment de la conception, vous définissez les propriétés d'un gestionnaire de connexions pour décrire la connexion physique qu' [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée lors de l'exécution du package. Par exemple, un gestionnaire de connexions inclut la propriété **ConnectionString** que vous définissez lors de la conception ; au moment de l'exécution, une connexion physique est créée à l'aide de la valeur de la propriété de chaîne de connexion.  
  
 Un package peut utiliser plusieurs instances d'un type de gestionnaire de connexions et vous pouvez définir les propriétés sur chaque instance. Au moment de l'exécution, chaque instance d'un type de gestionnaire de connexions crée une connexion qui possède des attributs différents.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fournit différents types de gestionnaires de connexions qui permettent aux packages d'établir une connexion à divers serveurs et à diverses sources de données :  
  
-   Il y a des gestionnaires de connexions intégrés que le programme d'installation installe lorsque vous installez [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
-   Il y a des gestionnaires de connexions qui sont téléchargeables à partir du site Web [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
-   Vous pouvez créer votre propre gestionnaire de connexions personnalisé si les gestionnaires de connexions existants ne répondent pas à vos besoins.  

### <a name="package-level-and-project-level-connection-managers"></a>Gestionnaires de connexions au niveau du package et au niveau du projet
Un gestionnaire de connexions peut être créé au niveau du package ou au niveau du projet. Le gestionnaire de connexions créé au niveau du projet est disponible pour tous les packages du projet. Le gestionnaire de connexions créé au niveau du package n'est, quant à lui, disponible que pour ce seul package.  
  
 Pour partager des connexions aux sources, vous utilisez les gestionnaires de connexions créés au niveau du projet au lieu des sources de données. Pour ajouter un gestionnaire de connexions au niveau du projet, le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] doit utiliser le modèle de déploiement de projet. Quand un projet est configuré pour utiliser ce modèle, le dossier **Gestionnaires de connexions** apparaît dans **l’Explorateur de solutions**et le dossier **Sources de données** est supprimé de **l’Explorateur de solutions**.  
  
> [!NOTE]  
>  Si vous souhaitez utiliser des sources de données dans votre package, vous devez convertir le projet en modèle de déploiement de package.  
>   
>  Pour plus d’informations sur les deux modèles et sur la conversion d’un projet en modèle de déploiement de projet, consultez [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).

### <a name="built-in-connection-managers"></a>Gestionnaires de connexions intégrés  
 Le tableau suivant répertorie les types de gestionnaires de connexions fournis par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
|Type|Description|Rubrique|  
|----------|-----------------|-----------|  
|ADO|Établit une connexion à des objets ADO (ActiveX Data Objects).|[Gestionnaire de connexions ADO](../../integration-services/connection-manager/ado-connection-manager.md)|  
|ADO.NET|Établit une connexion à une source de données au moyen d'un fournisseur .NET.|[Gestionnaire de connexions ADO.NET](../../integration-services/connection-manager/ado-net-connection-manager.md)|  
|CACHE|Lit les données du flux de données ou d'un fichier cache (.caw) et peut enregistrer des données dans le fichier cache.|[Gestionnaire de connexions du cache](../../integration-services/connection-manager/cache-connection-manager.md)|  
|DQS|Établit une connexion à un serveur Data Quality Services et à une base de données Data Quality Services sur le serveur.|[Gestionnaire de connexions de nettoyage DQS](../../integration-services/connection-manager/dqs-cleansing-connection-manager.md)|  
|EXCEL|Établit une connexion à un fichier de classeur Excel.|[Gestionnaire de connexions Excel](../../integration-services/connection-manager/excel-connection-manager.md)|  
|FILE|Établit une connexion à un fichier ou un dossier.|[Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md)|  
|FLATFILE|Établit une connexion à des données dans un fichier plat unique.|[Gestionnaire de connexions de fichiers plats](../../integration-services/connection-manager/flat-file-connection-manager.md)|  
|FTP|Établit une connexion à un serveur FTP.|[Gestionnaires de connexions FTP](../../integration-services/connection-manager/ftp-connection-manager.md)|  
|HTTP|Établit une connexion à un serveur Web.|[Gestionnaire de connexions HTTP](../../integration-services/connection-manager/http-connection-manager.md)|  
|MSMQ|Établit une connexion à une file d'attente de messages.|[Gestionnaire de connexions MSMQ](../../integration-services/connection-manager/msmq-connection-manager.md)|  
|MSOLAP100|Établit une connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|[Gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/analysis-services-connection-manager.md)|  
|MULTIFILE|Établit une connexion à plusieurs fichiers et dossiers.|[Gestionnaire de connexions de fichiers multiples](../../integration-services/connection-manager/multiple-files-connection-manager.md)|  
|MULTIFLATFILE|Établit une connexion à plusieurs fichiers et dossiers de données.|[Gestionnaire de connexions de fichiers plats multiples](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md)|  
|OLEDB|Établit une connexion à une source de données au moyen d'un fournisseur OLE DB.|[Gestionnaire de connexions OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md)|  
|ODBC|Établit une connexion à une source de données au moyen d'ODBC.|[Gestionnaire de connexions ODBC](../../integration-services/connection-manager/odbc-connection-manager.md)|  
|SMOServer|Établit une connexion à un serveur SMO ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Objects).|[Gestionnaire de connexions SMO](../../integration-services/connection-manager/smo-connection-manager.md)|  
|SMTP|Établit une connexion à un serveur de messagerie SMTP.|[Gestionnaire de connexions SMTP](../../integration-services/connection-manager/smtp-connection-manager.md)|  
|SQLMOBILE|Établit une connexion à une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.|[Gestionnaire de connexions de SQL Server Compact Edition](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)|  
|WMI|Établit une connexion à un serveur et spécifie la portée de la gestion WMI (Windows Management Instrumentation) sur le serveur.|[Gestionnaire de connexions WMI](../../integration-services/connection-manager/wmi-connection-manager.md)|  
  
### <a name="connection-managers-available-for-download"></a>Gestionnaires de connexions pouvant être téléchargés  
 Le tableau suivant répertorie d'autres types de gestionnaires de connexions que vous pouvez télécharger à partir du site Web [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
> [!IMPORTANT]  
>  Les gestionnaires de connexions répertoriés dans le tableau suivant fonctionnent uniquement avec [!INCLUDE[ssEnterpriseEd11](../../includes/ssenterpriseed11-md.md)] et [!INCLUDE[ssDeveloperEd11](../../includes/ssdevelopered11-md.md)].  
  
|Type|Description|Rubrique|  
|----------|-----------------|-----------|  
|ORACLE|Établit une connexion à un serveur Oracle \<informations de version\>.|Le gestionnaire de connexions Oracle est le composant de gestionnaire de connexions du Connecteur [!INCLUDE[msCoName](../../includes/msconame-md.md)] pour Oracle par Attunity. Le Connecteur [!INCLUDE[msCoName](../../includes/msconame-md.md)] pour Oracle par Attunity inclut également une source et une destination. Pour plus d'informations, consultez la page de téléchargement [Microsoft Connectors for Oracle and Teradata by Attunity](http://go.microsoft.com/fwlink/?LinkId=251526)(en anglais).|  
|SAPBI|Établit une connexion à un système SAP NetWeaver BI version 7.|Le gestionnaire de connexions SAP BI est le composant de gestionnaire de connexions du Connecteur [!INCLUDE[msCoName](../../includes/msconame-md.md)] for SAP BI. Le Connecteur [!INCLUDE[msCoName](../../includes/msconame-md.md)] for SAP BI inclut également une source et une destination. Pour plus d'informations, consultez la page de téléchargement [Microsoft SQL Server 2008 Feature Pack](http://go.microsoft.com/fwlink/?LinkId=262016).|  
|TERADATA|Établit une connexion à un serveur Teradata \<informations de version\>.|Le gestionnaire de connexions Teradata est le composant de gestionnaire de connexions du Connecteur [!INCLUDE[msCoName](../../includes/msconame-md.md)] pour Teradata par Attunity. Le Connecteur [!INCLUDE[msCoName](../../includes/msconame-md.md)] pour Teradata par Attunity inclut également une source et une destination. Pour plus d'informations, consultez la page de téléchargement [Microsoft Connectors for Oracle and Teradata by Attunity](http://go.microsoft.com/fwlink/?LinkId=251526)(en anglais).|  
  
### <a name="custom-connection-managers"></a>Gestionnaires de connexions personnalisés  
 Vous pouvez également écrire des gestionnaires de connexions personnalisés. Pour plus d'informations, consultez [Developing a Custom Connection Manager](../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-custom-connection-manager.md).  
  
## <a name="create-connection-managers"></a>Créer des gestionnaires de connexions
  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] propose différents gestionnaires de connexions pour répondre aux besoins de tâches qui se connectent à différents types de serveurs et de sources de données. Les gestionnaires de connexions sont utilisés par les composants de flux de données qui extraient et chargent des données dans différents types de banques de données et par les modules fournisseurs d'informations qui enregistrent des journaux sur un serveur, dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans un fichier. Par exemple, un package contenant une tâche Envoyer un message utilise un gestionnaire de connexions SMTP pour se connecter à un serveur SMTP (Simple Mail Transfer Protocol). Un package contenant une tâche d'exécution SQL peut utiliser un gestionnaire de connexions OLE DB pour se connecter à une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md).  
  
 Pour créer et configurer automatiquement des gestionnaires de connexions quand vous créez un package, vous pouvez utiliser l’Assistant Importation et exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cet Assistant vous aide aussi à créer et configurer les sources et destinations qui utilisent les gestionnaires de connexions. Pour plus d’informations, consultez [Create Packages in SQL Server Data Tools](../../integration-services/create-packages-in-sql-server-data-tools.md).  
  
 Pour créer manuellement un nouveau gestionnaire de connexions et l'ajouter à un package existant, utilisez la zone **Gestionnaires de connexions** sous les onglets **Flux de contrôle**, **Flux de données**et **Gestionnaires d'événements** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Dans la zone **Gestionnaire de connexions** , vous devez choisir le type de gestionnaire de connexions à créer, puis définir ses propriétés dans la boîte de dialogue fournie à cet effet dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Pour plus d'informations, consultez la section « Utilisation de la zone Gestionnaire de connexions », plus loin dans cette rubrique.  
  
 Une fois le gestionnaire de connexions ajouté à un package, vous pouvez l'utiliser dans des tâches, des conteneurs de boucles Foreach, des sources, des transformations et des destinations. Pour plus d’informations, consultez [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md), [Conteneur de boucles Foreach](../../integration-services/control-flow/foreach-loop-container.md) et [Flux de données](../../integration-services/data-flow/data-flow.md).  
  
### <a name="using-the-connection-managers-area"></a>Utilisation de la zone Gestionnaires de connexions  
 Vous pouvez créer des gestionnaires de connexions lorsque l'onglet **Flux de contrôle**, **Flux de données**ou **Gestionnaires d'événements** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] est actif.  
  
 Le diagramme qui suit montre la zone **Gestionnaires de connexions** de l'onglet **Flux de contrôle** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 ![Capture d’écran du concepteur de flux de contrôle avec le package](../../integration-services/connection-manager/media/samplecontrolflow.gif "Capture d’écran du concepteur de flux de contrôle avec le package")    
  
### <a name="32-bit-and-64-bit-providers-for-connection-managers"></a>Fournisseurs 32 bits et 64 bits pour gestionnaires de connexions  
 De nombreux fournisseurs utilisés par des gestionnaires de connexions sont disponibles en versions 32 bits et 64 bits. L'environnement de conception [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est un environnement 32 bits et vous ne pouvez voir que des fournisseurs 32 bits pendant la conception d'un package. Par conséquent, vous pouvez configurer un gestionnaire de connexions pour utiliser un fournisseur 64 bits spécifique uniquement si la version 32 bits du même fournisseur est également installée.  
  
 Au moment de l'exécution, la version appropriée est employée même si vous avez spécifié la version 32 bits du fournisseur lors de la conception. La version 64 bits du fournisseur peut être exécutée même si le package est utilisé dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
  Les deux versions du fournisseur ont le même ID. Pour spécifier si l’exécution de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise une version 64 bits disponible du fournisseur, vous devez définir la propriété Run64BitRuntime du projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Si la propriété Run64BitRuntime a la valeur **true**, l’exécution trouve le fournisseur 64 bits et l’utilise ; si Run64BitRuntime a la valeur **false**, l’exécution trouve et utilise le fournisseur 32 bits. Pour plus d’informations sur les propriétés que vous pouvez définir sur les projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], consultez [Integration Services (SSIS) et environnements de Studio](https://msdn.microsoft.com/library/ms140028.aspx).   

## <a name="add-a-connection-manager"></a>Ajouter un gestionnaire de connexions
###  <a name="wizard"></a> Ajouter un gestionnaire de connexions lors de la création d’un package  
  
-   Utiliser l'Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
     En plus de créer et configurer un gestionnaire de connexions, cet Assistant vous aide également à créer et configurer les sources et destinations qui utilisent le gestionnaire de connexions. Pour plus d'informations, consultez [Create Packages in SQL Server Data Tools](../../integration-services/create-packages-in-sql-server-data-tools.md).  
  
###  <a name="package"></a> Ajouter un gestionnaire de connexions à un package existant  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l’Explorateur de solutions, double-cliquez sur le package pour l’ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur l’onglet **Flux de contrôle** , **Flux de données** ou **Gestionnaire d’événements** pour rendre la zone **Gestionnaires de connexions** disponible.  
  
4.  Cliquez avec le bouton droit n’importe où dans la zone **Gestionnaires de connexions** , puis effectuez l’une des opérations suivantes :  
  
    -   Cliquez sur le type du gestionnaire de connexions à ajouter au package.  
  
         —ou—  
  
    -   Si le type que vous voulez ajouter ne figure pas dans la liste, cliquez sur **Nouvelle connexion** pour ouvrir la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS** , sélectionnez un type de gestionnaire de connexions, puis cliquez sur **OK**.  
  
     La boîte de dialogue personnalisée pour le type de gestionnaire de connexions sélectionné s'ouvre. Pour plus d'informations sur les types de gestionnaire de connexions et les options disponibles, reportez-vous au tableau d'options suivant.  
  
    |Gestionnaire de connexions|Options|  
    |------------------------|-------------|  
    |[Gestionnaire de connexions ADO](../../integration-services/connection-manager/ado-connection-manager.md)|[Configurer le gestionnaire de connexions OLE DB](../../integration-services/connection-manager/configure-ole-db-connection-manager.md)|  
    |[Gestionnaire de connexions ADO.NET](../../integration-services/connection-manager/ado-net-connection-manager.md)|[Configurer le gestionnaire de connexions ADO.NET](../../integration-services/connection-manager/configure-ado-net-connection-manager.md)|  
    |[Gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/analysis-services-connection-manager.md)|[Référence de l’interface utilisateur de la boîte de dialogue Ajout d’un gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md)|  
    |[Gestionnaire de connexions Excel](../../integration-services/connection-manager/excel-connection-manager.md)|[Éditeur du gestionnaire de connexions Excel](../../integration-services/connection-manager/excel-connection-manager-editor.md)|  
    |[Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md)|[Éditeur du gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager-editor.md)|  
    |[Gestionnaire de connexions de fichiers multiples](../../integration-services/connection-manager/multiple-files-connection-manager.md)|[Référence de l’interface utilisateur de la boîte de dialogue Ajouter un gestionnaire de connexions de fichiers](../../integration-services/connection-manager/add-file-connection-manager-dialog-box-ui-reference.md)|  
    |[Gestionnaire de connexions de fichiers plats](../../integration-services/connection-manager/flat-file-connection-manager.md)|[Éditeur du gestionnaire de connexions de fichiers plats &#40;page Général&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-general-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats &#40;page Colonnes&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-columns-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats &#40;page Avancé&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-advanced-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats &#40;page Aperçu&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-preview-page.md)|  
    |[Gestionnaire de connexions de fichiers plats multiples](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md)|[Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Général&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-general-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Colonnes&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-columns-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Avancé&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-advanced-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Aperçu&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-preview-page.md)|  
    |[Gestionnaires de connexions FTP](../../integration-services/connection-manager/ftp-connection-manager.md)|[Éditeur du gestionnaire de connexions FTP](../../integration-services/connection-manager/ftp-connection-manager-editor.md)|  
    |[Gestionnaire de connexions HTTP](../../integration-services/connection-manager/http-connection-manager.md)|[Éditeur du gestionnaire de connexions HTTP &#40;page Serveur&#41;](../../integration-services/connection-manager/http-connection-manager-editor-server-page.md)<br /><br /> [Éditeur du gestionnaire de connexions HTTP &#40;page Proxy&#41;](../../integration-services/connection-manager/http-connection-manager-editor-proxy-page.md)|  
    |[Gestionnaire de connexions MSMQ](../../integration-services/connection-manager/msmq-connection-manager.md)|[Éditeur du gestionnaire de connexions MSMQ](../../integration-services/connection-manager/msmq-connection-manager-editor.md)|  
    |[Gestionnaire de connexions ODBC](../../integration-services/connection-manager/odbc-connection-manager.md)|[Référence de l'interface utilisateur du gestionnaire de connexions ODBC](../../integration-services/connection-manager/odbc-connection-manager-ui-reference.md)|  
    |[Gestionnaire de connexions OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md)|[Configurer le gestionnaire de connexions OLE DB](../../integration-services/connection-manager/configure-ole-db-connection-manager.md)|  
    |[Gestionnaire de connexions SMO](../../integration-services/connection-manager/smo-connection-manager.md)|[Éditeur du gestionnaire de connexions SMO](../../integration-services/connection-manager/smo-connection-manager-editor.md)|  
    |[Gestionnaire de connexions SMTP](../../integration-services/connection-manager/smtp-connection-manager.md)|[Éditeur du gestionnaire de connexions SMTP](../../integration-services/connection-manager/smtp-connection-manager-editor.md)|  
    |[Gestionnaire de connexions de SQL Server Compact Edition](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)|[Éditeur du gestionnaire de connexions SQL Server Compact Edition &#40;page Connexion&#41;](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager-editor-connection-page.md)<br /><br /> [Éditeur du gestionnaire de connexions SQL Server Compact Edition &#40;page Tout&#41;](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager-editor-all-page.md)|  
    |[Gestionnaire de connexions WMI](../../integration-services/connection-manager/wmi-connection-manager.md)|[Éditeur du gestionnaire de connexions WMI](../../integration-services/connection-manager/wmi-connection-manager-editor.md)|  
  
     La zone **Gestionnaires de connexions** répertorie le gestionnaire de connexions ajouté.  
  
5.  Vous pouvez aussi cliquer avec le bouton droit sur le gestionnaire de connexions, cliquer sur **Renommer**, puis modifier le nom par défaut du gestionnaire de connexions.  
  
6.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer l’élément sélectionné** dans le menu **Fichier** .  
  
###  <a name="project"></a> Ajouter un gestionnaire de connexions au niveau du projet  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur **Gestionnaires de connexions**, puis cliquez sur **Nouveau gestionnaire de connexions**.  
  
3.  Dans la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS** , sélectionnez le type de gestionnaire de connexions, puis cliquez sur **Ajouter**.  
  
     La boîte de dialogue personnalisée pour le type de gestionnaire de connexions sélectionné s'ouvre. Pour plus d'informations sur les types de gestionnaire de connexions et les options disponibles, reportez-vous au tableau d'options suivant.  
  
    |Gestionnaire de connexions|Options|  
    |------------------------|-------------|  
    |[Gestionnaire de connexions ADO](../../integration-services/connection-manager/ado-connection-manager.md)|[Configurer le gestionnaire de connexions OLE DB](../../integration-services/connection-manager/configure-ole-db-connection-manager.md)|  
    |[Gestionnaire de connexions ADO.NET](../../integration-services/connection-manager/ado-net-connection-manager.md)|[Configurer le gestionnaire de connexions ADO.NET](../../integration-services/connection-manager/configure-ado-net-connection-manager.md)|  
    |[Gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/analysis-services-connection-manager.md)|[Référence de l’interface utilisateur de la boîte de dialogue Ajout d’un gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md)|  
    |[Gestionnaire de connexions Excel](../../integration-services/connection-manager/excel-connection-manager.md)|[Éditeur du gestionnaire de connexions Excel](../../integration-services/connection-manager/excel-connection-manager-editor.md)|  
    |[Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md)|[Éditeur du gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager-editor.md)|  
    |[Gestionnaire de connexions de fichiers multiples](../../integration-services/connection-manager/multiple-files-connection-manager.md)|[Référence de l’interface utilisateur de la boîte de dialogue Ajouter un gestionnaire de connexions de fichiers](../../integration-services/connection-manager/add-file-connection-manager-dialog-box-ui-reference.md)|  
    |[Gestionnaire de connexions de fichiers plats](../../integration-services/connection-manager/flat-file-connection-manager.md)|[Éditeur du gestionnaire de connexions de fichiers plats &#40;page Général&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-general-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats &#40;page Colonnes&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-columns-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats &#40;page Avancé&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-advanced-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats &#40;page Aperçu&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-preview-page.md)|  
    |[Gestionnaire de connexions de fichiers plats multiples](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md)|[Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Général&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-general-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Colonnes&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-columns-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Avancé&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-advanced-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Aperçu&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-preview-page.md)|  
    |[Gestionnaires de connexions FTP](../../integration-services/connection-manager/ftp-connection-manager.md)|[Éditeur du gestionnaire de connexions FTP](../../integration-services/connection-manager/ftp-connection-manager-editor.md)|  
    |[Gestionnaire de connexions HTTP](../../integration-services/connection-manager/http-connection-manager.md)|[Éditeur du gestionnaire de connexions HTTP &#40;page Serveur&#41;](../../integration-services/connection-manager/http-connection-manager-editor-server-page.md)<br /><br /> [Éditeur du gestionnaire de connexions HTTP &#40;page Proxy&#41;](../../integration-services/connection-manager/http-connection-manager-editor-proxy-page.md)|  
    |[Gestionnaire de connexions MSMQ](../../integration-services/connection-manager/msmq-connection-manager.md)|[Éditeur du gestionnaire de connexions MSMQ](../../integration-services/connection-manager/msmq-connection-manager-editor.md)|  
    |[Gestionnaire de connexions ODBC](../../integration-services/connection-manager/odbc-connection-manager.md)|[Référence de l'interface utilisateur du gestionnaire de connexions ODBC](../../integration-services/connection-manager/odbc-connection-manager-ui-reference.md)|  
    |[Gestionnaire de connexions OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md)|[Configurer le gestionnaire de connexions OLE DB](../../integration-services/connection-manager/configure-ole-db-connection-manager.md)|  
    |[Gestionnaire de connexions SMO](../../integration-services/connection-manager/smo-connection-manager.md)|[Éditeur du gestionnaire de connexions SMO](../../integration-services/connection-manager/smo-connection-manager-editor.md)|  
    |[Gestionnaire de connexions SMTP](../../integration-services/connection-manager/smtp-connection-manager.md)|[Éditeur du gestionnaire de connexions SMTP](../../integration-services/connection-manager/smtp-connection-manager-editor.md)|  
    |[Gestionnaire de connexions de SQL Server Compact Edition](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)|[Éditeur du gestionnaire de connexions SQL Server Compact Edition &#40;page Connexion&#41;](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager-editor-connection-page.md)<br /><br /> [Éditeur du gestionnaire de connexions SQL Server Compact Edition &#40;page Tout&#41;](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager-editor-all-page.md)|  
    |[Gestionnaire de connexions WMI](../../integration-services/connection-manager/wmi-connection-manager.md)|[Éditeur du gestionnaire de connexions WMI](../../integration-services/connection-manager/wmi-connection-manager-editor.md)|  
  
     Le gestionnaire de connexions que vous avez ajouté apparaîtra sous le nœud **Gestionnaires de connexions** dans **l’Explorateur de solutions**. Il figurera aussi sous l’onglet **Gestionnaires de connexions** de la fenêtre **Concepteur SSIS** pour tous les packages du projet. Le nom du gestionnaire de connexions de cet onglet aura un préfixe **(project)** afin de distinguer ce gestionnaire de connexions de niveau projet des gestionnaires de connexions de niveau package.  
  
4.  Le cas échéant, cliquez avec le bouton droit sur le gestionnaire de connexions dans la fenêtre **Explorateur de solutions** sous le nœud **Gestionnaires de connexions** (ou) sous l’onglet **Gestionnaires de connexions** de la fenêtre **Concepteur SSIS** , cliquez **Renommer**, puis modifiez le nom par défaut du gestionnaire de connexions.  
  
    > [!NOTE]  
    >  Sous l’onglet **Gestionnaires de connexions** de la fenêtre **Concepteur SSIS** , vous ne pouvez pas remplacer le préfixe **(project)** dans le nom du gestionnaire de connexions. C'est la procédure normale.  

### <a name="add-ssis-connection-manager-dialog-box"></a>Boîte de dialogue Ajout d’un gestionnaire de connexions SSIS
La boîte de dialogue **Ajout d'un gestionnaire de connexions SSIS** vous permet de sélectionner le type de connexion à ajouter à un package.  
  
 Pour en savoir plus sur les gestionnaires de connexions, consultez [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md).  
  
#### <a name="options"></a>Options  
 **Type du gestionnaire de connexions**  
 Sélectionnez un type de connexion et cliquez sur **Ajouter**ou double-cliquez sur un type de connexion, pour spécifier les propriétés de connexion à l’aide de l’éditeur pour chaque type de connexion.  
  
 **Ajouter**  
 Spécifiez les propriétés de connexion à l'aide de l'éditeur pour chaque type de connexion.  
   
##  <a name="parameter"></a> Créer un paramètre pour une propriété du gestionnaire de connexions  
  
1.  Dans la zone **Gestionnaires de connexions** , cliquez avec le bouton droit sur le gestionnaire de connexions pour lequel vous souhaitez créer un paramètre, puis cliquez sur **Paramétrer**.  
  
2.  Configurez les paramètres dans la boîte de dialogue **Paramétrer** . Pour plus d’informations, consultez [Paramétrer (boîte de dialogue)](http://msdn.microsoft.com/library/fac02b6d-d247-447a-8940-e8700c7ac350).  

## <a name="delete-a-connection-manager"></a>Supprimer un gestionnaire de connexions 
###  <a name="DeletePackageLevel"></a> Supprimer un gestionnaire de connexions d’un package  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur l’onglet **Flux de contrôle** , **Flux de données** ou **Gestionnaire d’événements** pour rendre la zone **Gestionnaires de connexions** disponible.  
  
4.  Cliquez avec le bouton droit sur le gestionnaire de connexions à supprimer, puis cliquez sur **Supprimer**.  
  
     Si vous supprimez un gestionnaire de connexions utilisé par un élément de package tel qu'une tâche d'exécution SQL ou une source OLE DB, vous obtiendrez les résultats suivants :  
  
    -   Une icône d'erreur apparaît sur l'élément de package qui a utilisé le gestionnaire de connexions supprimé.  
  
    -   Échec de validation du package.  
  
    -   Impossible d’exécuter le package.  
  
5.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
###  <a name="DeleteProjectLevel"></a> Supprimer un gestionnaire de connexions partagé (gestionnaire de connexions au niveau du projet)  
  
1.  Pour supprimer un gestionnaire de connexions de niveau projet, cliquez avec le bouton droit sur le gestionnaire de connexions sous le nœud **Gestionnaires de connexions** dans la fenêtre **Explorateur de solutions** , puis cliquez **Supprimer**. [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] affiche le message d’avertissement suivant :  
  
    > [!WARNING]  
    >  Lorsque vous supprimez un gestionnaire de connexions au niveau projet, les packages qui utilisent le gestionnaire de connexions peuvent ne pas s'exécuter. Vous ne pouvez pas annuler cette action. Voulez -vous supprimer le gestionnaire de connexions ?  
  
2.  Cliquez sur OK pour supprimer le gestionnaire de connexions ou sur Annuler pour le conserver.  
  
    > [!NOTE]  
    >  Vous pouvez aussi supprimer un gestionnaire de connexions de niveau projet sous l’onglet **Gestionnaire de connexions** de la fenêtre **Concepteur SSIS** ouverte pour un package du projet. Pour ce faire, cliquez avec le bouton droit sur le nom du gestionnaire de connexions figurant sous l’onglet, puis cliquez sur **Supprimer**. 
    
## <a name="set-the-properties-of-a-connection-manager"></a>Définir les propriétés d’un gestionnaire de connexions
Tous les gestionnaires de connexions peuvent être configurés à l'aide de la fenêtre **Propriétés** .  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] fournit également des boîtes de dialogue personnalisées permettant de modifier les différents types de gestionnaires de connexions dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. La boîte de dialogue offre un ensemble d'options différent pour chaque type de gestionnaire de connexions.  
  
### <a name="modify-a-connection-manager-using-the-properties-window"></a>Modifier un gestionnaire de connexions à l’aide de la fenêtre Propriétés  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur SSIS, cliquez sur l’onglet **Flux de contrôle** , l’onglet **Flux de données** ou l’onglet **Gestionnaire d’événements** pour rendre la zone **Gestionnaires de connexions** disponible.  
  
4.  Cliquez avec le bouton droit sur le gestionnaire de connexions, puis sur **Propriétés**.  
  
5.  Dans la fenêtre **Propriétés** , modifiez les valeurs de propriété. La fenêtre **Propriétés** offre un accès à des propriétés non configurables dans l’éditeur standard d’un gestionnaire de connexions.  
  
6.  Cliquez sur **OK**.  
  
7.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
### <a name="modify-a-connection-manager-using-a-connection-manager-dialog-box"></a>Modifier un gestionnaire de connexions à l’aide d’une boîte de dialogue de gestionnaire de connexions  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur l’onglet **Flux de contrôle** , **Flux de données** ou **Gestionnaire d’événements** pour rendre la zone **Gestionnaires de connexions** disponible.  
  
4.  Dans la zone **Gestionnaires de connexions** , double-cliquez sur le gestionnaire de connexions pour ouvrir la boîte de dialogue **Gestionnaire de connexions** . Pour plus d'informations sur les types spécifiques de gestionnaires de connexions et les options disponibles pour chaque type, consultez le tableau suivant.  
  
    |Gestionnaire de connexions|Options|  
    |------------------------|-------------|  
    |[Gestionnaire de connexions ADO](../../integration-services/connection-manager/ado-connection-manager.md)|[Configurer le gestionnaire de connexions OLE DB](../../integration-services/connection-manager/configure-ole-db-connection-manager.md)|  
    |[Gestionnaire de connexions ADO.NET](../../integration-services/connection-manager/ado-net-connection-manager.md)|[Configurer le gestionnaire de connexions ADO.NET](../../integration-services/connection-manager/configure-ado-net-connection-manager.md)|  
    |[Gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/analysis-services-connection-manager.md)|[Référence de l’interface utilisateur de la boîte de dialogue Ajout d’un gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md)|  
    |[Gestionnaire de connexions Excel](../../integration-services/connection-manager/excel-connection-manager.md)|[Éditeur du gestionnaire de connexions Excel](../../integration-services/connection-manager/excel-connection-manager-editor.md)|  
    |[Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md)|[Éditeur du gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager-editor.md)|  
    |[Gestionnaire de connexions de fichiers multiples](../../integration-services/connection-manager/multiple-files-connection-manager.md)|[Référence de l’interface utilisateur de la boîte de dialogue Ajouter un gestionnaire de connexions de fichiers](../../integration-services/connection-manager/add-file-connection-manager-dialog-box-ui-reference.md)|  
    |[Gestionnaire de connexions de fichiers plats](../../integration-services/connection-manager/flat-file-connection-manager.md)|[Éditeur du gestionnaire de connexions de fichiers plats &#40;page Général&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-general-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats &#40;page Colonnes&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-columns-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats &#40;page Avancé&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-advanced-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats &#40;page Aperçu&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-preview-page.md)|  
    |[Gestionnaire de connexions de fichiers plats multiples](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md)|[Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Général&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-general-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Colonnes&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-columns-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Avancé&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-advanced-page.md)<br /><br /> [Éditeur du gestionnaire de connexions de fichiers plats multiples &#40;page Aperçu&#41;](../../integration-services/connection-manager/multiple-flat-files-connection-manager-editor-preview-page.md)|  
    |[Gestionnaires de connexions FTP](../../integration-services/connection-manager/ftp-connection-manager.md)|[Éditeur du gestionnaire de connexions FTP](../../integration-services/connection-manager/ftp-connection-manager-editor.md)|  
    |[Gestionnaire de connexions HTTP](../../integration-services/connection-manager/http-connection-manager.md)|[Éditeur du gestionnaire de connexions HTTP &#40;page Serveur&#41;](../../integration-services/connection-manager/http-connection-manager-editor-server-page.md)<br /><br /> [Éditeur du gestionnaire de connexions HTTP &#40;page Proxy&#41;](../../integration-services/connection-manager/http-connection-manager-editor-proxy-page.md)|  
    |[Gestionnaire de connexions MSMQ](../../integration-services/connection-manager/msmq-connection-manager.md)|[Éditeur du gestionnaire de connexions MSMQ](../../integration-services/connection-manager/msmq-connection-manager-editor.md)|  
    |[Gestionnaire de connexions ODBC](../../integration-services/connection-manager/odbc-connection-manager.md)|[Référence de l'interface utilisateur du gestionnaire de connexions ODBC](../../integration-services/connection-manager/odbc-connection-manager-ui-reference.md)|  
    |[Gestionnaire de connexions OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md)|[Configurer le gestionnaire de connexions OLE DB](../../integration-services/connection-manager/configure-ole-db-connection-manager.md)|  
    |[Gestionnaire de connexions SMO](../../integration-services/connection-manager/smo-connection-manager.md)|[Éditeur du gestionnaire de connexions SMO](../../integration-services/connection-manager/smo-connection-manager-editor.md)|  
    |[Gestionnaire de connexions SMTP](../../integration-services/connection-manager/smtp-connection-manager.md)|[Éditeur du gestionnaire de connexions SMTP](../../integration-services/connection-manager/smtp-connection-manager-editor.md)|  
    |[Gestionnaire de connexions de SQL Server Compact Edition](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)|[Éditeur du gestionnaire de connexions SQL Server Compact Edition &#40;page Connexion&#41;](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager-editor-connection-page.md)<br /><br /> [Éditeur du gestionnaire de connexions SQL Server Compact Edition &#40;page Tout&#41;](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager-editor-all-page.md)|  
    |[Gestionnaire de connexions WMI](../../integration-services/connection-manager/wmi-connection-manager.md)|[Éditeur du gestionnaire de connexions WMI](../../integration-services/connection-manager/wmi-connection-manager-editor.md)|  
  
5.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  

## <a name="related-content"></a>Contenu associé  
  
-   Vidéo, [Utilisez Microsoft Attunity Connector for Oracle pour améliorer les performances des packages](http://technet.microsoft.com/sqlserver/gg598963.aspx), sur technet.microsoft.com  
  
-   Articles Wiki, [Connectivité SSIS](http://social.technet.microsoft.com/wiki/contents/articles/sql-server-integration-services-ssis.aspx#Connectivity), sur social.technet.microsoft.com  
  
-   Entrée de blog, [Se connecter à MySQL SSIS](http://go.microsoft.com/fwlink/?LinkId=217669), sur blogs.msdn.com.  
  
-   Article technique [Extraction et chargement des données SharePoint dans SQL Server Integration Services](http://go.microsoft.com/fwlink/?LinkId=247826), sur msdn.microsoft.com.  
  
-   Article technique, [You get "DTS_E_CANNOTACQUIRECONNECTIONFROMCONNECTIONMANAGER" error message when using Oracle connection manager in SSIS](http://go.microsoft.com/fwlink/?LinkId=233696)(Le message d’erreur « DTS_E_CANNOTACQUIRECONNECTIONFROMCONNECTIONMANAGER’» s’affiche lors de l’utilisation du gestionnaire de connexions Oracle dans SSIS), sur le site support.microsoft.com.  
  
  
