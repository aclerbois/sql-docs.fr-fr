---
title: Créer un flux de travail personnalisé (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.component: develop
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
ms.assetid: 8e4403e9-595c-4b6b-9d0c-f6ae1b2bc99d
caps.latest.revision: 8
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 54be51ae5b2cabc58ecfe6d584e5cca450cd68ec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-custom-workflow-master-data-services"></a>Créer un flux de travail personnalisé (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] utilise des règles d'entreprise pour créer des solutions de flux de travail de base, comme la mise à jour et la validation automatique des données et l'envoi de notifications par courrier électronique, en fonction des conditions que vous spécifiez. Lorsque vous avez besoin de traitements plus complexes que ceux fournis par ces actions de flux de travail intégrées, utilisez un flux de travail personnalisé. Un flux de travail personnalisé est un assembly .NET. que vous créez. Lorsque votre assembly de flux de travail est appelé, votre code peut exécuter n'importe quelle action requise par votre situation. Par exemple, si votre flux de travail nécessite le traitement d'événements complexes, tels que des approbations multicouches ou des arbres de décision compliqués, vous pouvez configurer [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] pour démarrer un flux de travail personnalisé qui analyse les données et détermine où les transmettre pour l'approbation.  
  
## <a name="how-custom-workflows-are-processed"></a>Comment s'effectue le traitement des flux de travail personnalisés  
 Il existe trois composants principaux impliqués dans le traitement des flux de travail personnalisés : l'application Web de [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], le service d'intégration de flux de travail MDS SQL Server et l'assembly de gestionnaire de flux de travail. Ces composants fonctionnent sur un flux de travail personnalisé comme suit :  
  
1.  Vous utilisez [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] pour valider une entité qui démarre un flux de travail.  
  
2.  [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] envoie les membres qui répondent aux conditions de la règle d'entreprise à une file d'attente de Service Broker dans la base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)].  
  
3.  À intervalles réguliers, le service d'intégration de flux de travail MDS SQL Server appelle une procédure stockée dans la base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)].  
  
4.  Lorsque cette procédure stockée trouve des enregistrements dans la file d'attente de Service Broker, elle les retourne au service d'intégration de flux de travail MDS de SQL Server.  
  
5.  Les services d'intégration de flux de travail MDS de SQL Server acheminent les données vers votre assembly de gestionnaire de flux de travail.  
  
> [!NOTE]  
>  Remarque : le service d'intégration de flux de travail MDS de SQL Server est destiné à déclencher des processus simples. Si votre code personnalisé requiert un traitement complexe, effectuez votre traitement dans un thread distinct ou à l'extérieur du processus de flux de travail.  
  
## <a name="configure-master-data-services-for-custom-workflows"></a>Configurer des services Master Data pour les flux de travail personnalisés  
 La création d'un flux de travail personnalisé requiert l'écriture d'un code personnalisé et la configuration de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] pour passer les données de flux de travail à votre gestionnaire de flux de travail. Procédez comme suit pour activer un traitement de flux de travail personnalisé :  
  
1.  Créez un assembly .NET qui implémente <xref:Microsoft.MasterDataServices.WorkflowTypeExtender.IWorkflowTypeExtender>.  
  
2.  Configurez le service d'intégration de flux de travail MDS de SQL Server pour qu'il se connecte à votre base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] et pour associer une balise à votre gestionnaire de flux de travail.  
  
3.  Démarrez le service d'intégration de flux de travail MDS de SQL Server.  
  
4.  Créez une règle d'entreprise dans [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] qui démarre un flux de travail référencé avec le nom de votre gestionnaire de flux de travail.  
  
5.  Appliquez la règle d'entreprise à un membre qui déclenche votre flux de travail personnalisé.  
  
### <a name="create-the-workflow-handler-assembly"></a>Créer l'assembly de gestionnaire de flux de travail  
 Un flux de travail personnalisé est un assembly de la bibliothèque de classes .NET qui implémente l'interface <xref:Microsoft.MasterDataServices.WorkflowTypeExtender.IWorkflowTypeExtender>. Le service d'intégration de flux de travail MDS de SQL Server appelle la méthode <xref:Microsoft.MasterDataServices.WorkflowTypeExtender.IWorkflowTypeExtender.StartWorkflow%2A> pour exécuter votre code. Pour découvrir un exemple de code qui implémente <xref:Microsoft.MasterDataServices.WorkflowTypeExtender.IWorkflowTypeExtender.StartWorkflow%2A>, consultez [Exemple de flux de travail personnalisé &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-example.md).  
  
 Suivez ces étapes afin d'utiliser Visual Studio 2010 pour créer un assembly que le service d'intégration de flux de travail MDS de SQL Server peut appeler pour gérer un flux de travail personnalisé :  
  
1.  Dans Visual Studio 2010, créez un projet de **Bibliothèque de classes** qui utilise le langage de votre choix. Pour créer une bibliothèque de classes C#, sélectionnez les types de projet **Visual C#\Windows**, puis sélectionnez le modèle de **Bibliothèque de classes**. Entrez un nom pour votre projet, tel que **MDSWorkflowTest**, puis cliquez sur **OK**.  
  
2.  Ajoutez une référence à Microsoft.MasterDataServices.WorkflowTypeExtender.dll. Cet assembly se trouve dans \<votre dossier d’installation>\Master Data Services\WebApplication\bin.  
  
3.  Ajoutez « using Microsoft.MasterDataServices.Core.Workflow; » à votre fichier de code C#.  
  
4.  Héritez de <xref:Microsoft.MasterDataServices.WorkflowTypeExtender.IWorkflowTypeExtender> dans votre déclaration de classe. La déclaration de classe doit ressembler à : « public class WorkflowTester : IWorkflowTypeExtender ».  
  
5.  Implémentez l'interface <xref:Microsoft.MasterDataServices.WorkflowTypeExtender.IWorkflowTypeExtender>. La méthode <xref:Microsoft.MasterDataServices.WorkflowTypeExtender.IWorkflowTypeExtender.StartWorkflow%2A> est appelée par le service d'intégration de flux de travail MDS de SQL Server pour démarrer votre flux de travail.  
  
6.  Copiez votre assembly à l’emplacement de l’exécutable du service d’intégration de flux de travail MDS de SQL Server, nommé Microsoft.MasterDataServices.Workflow.exe, dans \<votre dossier d’installation>\Master Data Services\WebApplication\bin.  
  
### <a name="configure-sql-server-mds-workflow-integration-service"></a>Configurer le service d'intégration de flux de travail MDS de SQL Server  
 Modifiez le fichier de configuration de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] pour inclure les informations de connexion de votre base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] et pour associer un indicateur à votre assembly de gestionnaire de flux de travail, en procédant comme suit :  
  
1.  Recherchez Microsoft.MasterDataServices.Workflow.exe.config dans \<votre dossier d’installation>\Master Data Services\WebApplication\bin.  
  
2.  Ajoutez les informations de connexion de la base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] au paramètre « ConnectionString ». Si votre installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un classement respectant la casse, le nom de la base de données doit être écrit selon la même casse que dans la base de données. Par exemple, la balise complète du paramètre peut se présenter comme suit :  
  
    ```xml  
    <setting name="ConnectionString" serializeAs="String">  
        <value>Server=myServer;Database=myDatabase;Integrated Security=True</value>  
    </setting>  
    ```  
  
3.  Sous le paramètre « ConnectionString » ajoutez un paramètre « WorkflowTypeExtenders » pour associer un nom de balise à votre assembly de gestionnaire de flux de travail. Exemple :  
  
    ```xml  
    <setting name="WorkflowTypeExtenders" serializeAs="String">  
        <value>TEST=MDSWorkflowTestLib.WorkflowTester, MDSWorkflowTestLib</value>  
    </setting>  
    ```  
  
     Le texte interne de la balise de \<valeur> se présente sous la forme suivante : \<balise de flux de travail>=\<nom du type de flux de travail qualifié par l’assembly>. La \<balise de flux de travail> est un nom que vous utilisez pour identifier l’assembly de gestionnaire de flux de travail quand vous créez une règle d’entreprise dans [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]. Le \<nom de type de flux de travail qualifié par l’assembly> est l’espace de noms de votre classe de flux de travail, suivi d’une virgule, suivie du nom complet de l’assembly. Si votre assembly est un nom fort, vous devez également inclure les informations de version et son PublicKeyToken. Vous pouvez inclure plusieurs balises de \<paramètre> si vous avez créé plusieurs gestionnaires de flux de travail pour différents types de flux de travail.  
  
> [!NOTE]  
>  Selon la configuration de votre serveur, vous pouvez obtenir une erreur « Accès refusé » lorsque vous essayez d'enregistrer le fichier Microsoft.MasterDataServices.Workflow.exe.config. Si cela se produit, désactivez temporairement le contrôle de compte d'utilisateur (UAC) sur le serveur. Pour cela, ouvrez le panneau de configuration et cliquez sur **Système et sécurité**. Sous **Centre de maintenance**, cliquez sur **Modifier les paramètres du contrôle de compte d’utilisateur**. Dans la boîte de dialogue **Paramètres de contrôle de compte d’utilisateur**, faites glisser la barre vers le bas afin de ne pas recevoir de notification. Redémarrez votre ordinateur et répétez les étapes précédentes pour modifier votre fichier de configuration. Après l'enregistrement du fichier, réinitialisez les paramètres de contrôle de compte d'utilisateur au niveau par défaut.  
  
### <a name="start-sql-server-mds-workflow-integration-service"></a>Démarrer le service d'intégration de flux de travail MDS de SQL Server  
 Par défaut, le service d'intégration de flux de travail MDS de SQL Server n'est pas installé. Vous devez installer le service avant de pouvoir l'utiliser. Pour plus de sécurité, créez un utilisateur local pour le service et accordez à cet utilisateur uniquement les autorisations nécessaires pour effectuer les opérations de flux de travail. Pour créer un utilisateur, installer le service et le démarrer, procédez comme suit :  
  
1.  Utilisez les utilisateurs locaux et le gestionnaire de groupes pour créer un utilisateur local nommé, par exemple, mds_workflow_service.  
  
2.  Utilisez SQL Server Management Studio pour accorder l'autorisation utilisateur mds_workflow_service permettant d'exécuter la procédure stockée [mdm].[udpExternalActionsGet]. Pour cela, créez une connexion pour le compte mds_workflow_service, créez un nouvel utilisateur dans la base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)], mappez cet utilisateur à la connexion mds_workflow_service et accordez l'autorisation utilisateur EXECUTE à la procédure stockée [mdm].[udpExternalActionsGet].  
  
3.  Octroyez l'autorisation utilisateur mds_workflow_service pour exécuter l'assembly de gestionnaire de flux de travail. Pour cela, ajoutez l’utilisateur mds_workflow_service à l’onglet **Sécurité** de la boîte de dialogue **Propriétés** de l’assembly du gestionnaire de flux de travail et accordez à l’utilisateur mds_workflow_service les autorisations READ et EXECUTE.  
  
4.  Accordez l'autorisation utilisateur mds_workflow_service pour exécuter l'exécutable du service d'intégration de flux de travail MDS de SQL Server. Pour cela, ajoutez l’utilisateur mds_workflow_service à l’onglet **Sécurité** de la boîte de dialogue **Propriétés** de Microsoft.MasterDataServices.Workflow.exe, dans \<votre dossier d’installation>\Master Data Services\WebApplication\bin et accordez à l’utilisateur mds_workflow_service les autorisations READ et EXECUTE.  
  
5.  Installez le service d'intégration de flux de travail MDS de SQL Server à l'aide de l'utilitaire d'installation .NET, nommé InstallUtil.exe. InstallUtil.exe se trouve dans le dossier d’installation de .NET, par exemple C:\Windows\Microsoft.NET\Framework\v4.0.30319\\. Installez le service d'intégration de flux de travail MDS de SQL Server en entrant la commande suivante à l'invite de commandes avec élévation de privilèges :  
  
    ```  
    C:\Windows\Microsoft.NET\Framework\v4.0.30319\InstallUtil Microsoft.MasterDataServices.Workflow.exe  
    ```  
  
     Spécifiez l'utilisateur mds_workflow_service lorsque vous y êtes invité pendant l'installation.  
  
6.  Démarrez le service d'intégration de flux de travail MDS de SQL Server à l'aide du composant logiciel enfichable Services. Pour cela, recherchez le service d’intégration de flux de travail MDS de SQL Server dans le composant logiciel enfichable Services, sélectionnez-le, puis cliquez sur le lien **Démarrer**.  
  
### <a name="create-a-workflow-business-rule"></a>Créer une règle d'entreprise de flux de travail  
 Utilisez [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] pour créer et publier une règle d'entreprise qui démarre le flux de travail lorsqu'elle est appliquée. Vous devez vous assurer que votre règle d'entreprise contient des actions qui modifient des valeurs d'attribut, afin que la logique prenne la valeur « false » après avoir été appliquée une fois. Par exemple, votre règle d'entreprise peut avoir la valeur « true » si une valeur d'attribut de prix est supérieure à 500 et la valeur d'attribut approuvée est vide. La règle peut ensuite inclure deux actions : une pour définir la valeur d'attribut approuvée en attente et une pour démarrer le flux de travail. Ou bien, vous pouvez créer une règle qui utilise la condition « has changed » et ajouter vos attributs pour modifier les groupes de suivi. Pour plus d’informations sur les règles d’entreprise, consultez [Règles d’entreprise &#40;Master Data Services&#41;](../../master-data-services/business-rules-master-data-services.md).  
  
 Créez une règle d'entreprise qui démarre un flux de travail personnalisé dans [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] en suivant ces étapes :  
  
1.  Dans l’éditeur de règle d’entreprise pour [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], une fois que vous avez spécifié les conditions de votre règle d’entreprise, faites glisser l’action **Démarrer le flux de travail** de la liste **Actions externes** vers l’étiquette **Action** du volet **THEN**.  
  
2.  Dans le volet **Modifier l’action**, dans la zone **Type de flux de travail**, tapez la balise qui identifie l’assembly de gestionnaire de flux de travail. Il s'agit de la balise que vous avez spécifiée dans le fichier de configuration pour votre assembly, par exemple, TEST.  
  
3.  Éventuellement, cochez la case **Inclure les données de membre**. Choisissez cette option pour inclure les noms et les valeurs d'attribut dans le code XML passé au gestionnaire de flux de travail.  
  
4.  Dans la zone **Site du flux de travail**, tapez le nom d’un site web. Pour votre flux de travail personnalisé, cette information peut ne pas s'appliquer, mais elle peut être utilisée pour ajouter du contexte.  
  
5.  Dans la zone **Nom du flux de travail**, tapez le nom de votre flux de travail dans Visual Studio. Pour votre flux de travail personnalisé, cette information peut ne pas s'appliquer, mais elle peut être utilisée pour ajouter du contexte.  
  
6.  Enregistrez et publiez la règle d'entreprise.  
  
### <a name="apply-business-rules-to-start-a-workflow"></a>Appliquer des règles d'entreprise pour démarrer un flux de travail  
 Appliquez la règle d'entreprise à vos données pour démarrer le flux de travail. Pour cela, utilisez [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] pour modifier l'entité qui contient les membres que vous souhaitez valider. Cliquez sur **Appliquer les règles d’entreprise**. En réponse à la règle d'entreprise, [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] remplit la file d'attente Service Broker de la base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]. Lorsque le service d'intégration de flux de travail MDS de SQL Server vérifie la file d'attente, il envoie les données à l'assembly du gestionnaire de flux de travail spécifié et efface la file d'attente. L'assembly du gestionnaire de flux de travail s'exécute quelles que soient les actions codées qu'il contient.  
  
## <a name="troubleshoot-custom-workflows"></a>Dépanner des flux de travail personnalisés  
 Si votre assembly de gestionnaire de flux de travail ne reçoit pas de données, vous pouvez essayer de déboguer le service d'intégration de flux de travail MDS de SQL Server ou bien afficher la file d'attente Service Broker.  
  
### <a name="debug-sql-server-mds-workflow-integration-service"></a>Déboguer le service d'intégration de flux de travail MDS de SQL Server  
 Pour déboguer le service d'intégration de flux de travail de SQL Server, effectuez les étapes suivantes :  
  
1.  Utilisez le composant logiciel enfichable Services pour arrêter le service.  
  
2.  Ouvrez une invite de commandes, naviguez jusqu'à l'emplacement du service, puis exécutez le service en mode console en entrant : Microsoft.MasterDataServices.Workflow.exe - console.  
  
3.  Dans [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)], mettez à jour votre membre et appliquez à nouveau les règles d'entreprise. Les journaux détaillés sont affichés dans la fenêtre de la console.  
  
### <a name="view-the-service-broker-queue"></a>Afficher la file d'attente de Service Broker  
 La file d'attente Service Broker qui contient les données de référence passées dans le cadre du flux de travail est : mdm.microsoft/mdm/queue/externalaction. Les files d’attente se trouvent dans l’**Explorateur d’objets** de SQL Management Studio sous le nœud Service Broker de la base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]. Notez que, si le service a effacé la file d'attente correctement, cette file d'attente est vide.  
  
## <a name="see-also"></a> Voir aussi  
 [Exemple de flux de travail personnalisé &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-example.md)   
 [Description du code XML d’un flux de travail personnalisé &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-xml-description.md)  
  
  
