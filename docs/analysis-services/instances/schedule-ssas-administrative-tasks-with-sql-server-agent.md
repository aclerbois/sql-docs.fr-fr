---
title: Planifier des tâches administratives SSAS avec SQL Server Agent | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f0a8525196bacff6d0bf75b28a17c154a6eb919a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="schedule-ssas-administrative-tasks-with-sql-server-agent"></a>Planifier des tâches administratives SSAS avec SQL Server Agent
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Le service SQL Server Agent vous permet de planifier des tâches administratives [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à exécuter dans l’ordre et au moment que vous précisez. Les tâches planifiées vous aident à automatiser les processus qui s’exécutent selon des cycles réguliers ou prévisibles. Vous pouvez planifier l'exécution de tâches administratives, telles que le traitement de cubes, durant les périodes de faible activité. Vous pouvez également déterminer l'ordre dans lequel les tâches s'exécutent en créant des étapes de travail au sein d'un travail de l'Agent SQL Server. Par exemple, vous pouvez traiter un cube, puis effectuer une sauvegarde de ce cube.  
  
 Les étapes de travail vous permettent de contrôler le flux d'exécution. Si un travail échoue, vous pouvez configurer l'Agent SQL Server de sorte qu'il continue d'exécuter les tâches restantes ou qu'il arrête l'exécution. Vous pouvez également configurer SQL Server Agent de sorte qu'il envoie des notifications concernant le succès ou l'échec de l'exécution d'un travail.  
  
 Cette rubrique est une procédure pas à pas qui décrit deux méthodes d'utilisation de SQL Server Agent pour exécuter le script XMLA. Le premier exemple montre comment planifier le traitement d'une dimension unique. Le deuxième exemple montre comment combiner les tâches de traitement dans un seul script qui s'exécute sur une planification. Pour effectuer cette procédure pas à pas, vous devez satisfaire aux conditions suivantes.  
  
## <a name="prerequisites"></a>Conditions préalables  
 Le service Agent SQL Server doit être installé.  
  
 Par défaut, les travaux s'exécutent sous le compte de service. Le compte par défaut de SQL Server Agent est NT Service\SQLAgent$\<nom_instance >. Pour effectuer une sauvegarde ou une tâche de traitement, ce compte doit être un administrateur système sur l'instance Analysis Services. Pour plus d’informations, consultez [Accorder des droits d’administrateur de serveur à une instance Analysis Services](../../analysis-services/instances/grant-server-admin-rights-to-an-analysis-services-instance.md).  
  
 Vous devez également disposer d'une base de données de test. Vous pouvez déployer l'exemple de base de données multidimensionnelle AdventureWorks ou un projet du didacticiel MDX Analysis Services dans cette procédure pas à pas. Pour plus d’informations, consultez [Installer les exemples de données et de projets pour le didacticiel sur la modélisation multidimensionnelle Analysis Services](../../analysis-services/install-sample-data-and-projects.md).  
  
## <a name="example-1-processing-a-dimension-in-a-scheduled-task"></a>Exemple 1 : traitement d'une dimension dans une tâche planifiée  
 Cet exemple montre comment créer et planifier un travail qui traite une dimension.  
  
 Une tâche planifiée [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est un script XMLA imbriqué dans un travail de SQL Server Agent. Ce travail est planifié pour s'exécuter à une heure et une fréquence souhaitées. SQL Server Agent faisant partie de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous travaillez avec le Moteur de base de données et avec [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour créer et planifier une tâche administrative.  
  
###  <a name="bkmk_CreateScript"></a> Créer un script pour traiter une dimension dans un travail de SQL Server Agent  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Ouvrez un dossier de base de données et recherchez une dimension. Cliquez sur la dimension et sélectionnez **Traiter**.  
  
2.  Dans la boîte de dialogue **Traiter la dimension** , dans la colonne **Options de traitement** sous **Liste d'objets**, vérifiez que l'option pour cette colonne est **Traiter entièrement**. Si ce n’est pas le cas, sous **Options de traitement**, cliquez sur l’option, puis sélectionnez **Traiter entièrement** dans la liste déroulante.  
  
3.  Cliquez sur **Script**.  
  
     Cette étape permet d'ouvrir une fenêtre **Requête Xml** qui contient le script XMLA qui traite la dimension.  
  
4.  Dans la boîte de dialogue **Traiter la dimension** , cliquez sur **Annuler** pour fermer la boîte de dialogue.  
  
5.  Dans la fenêtre **Requête XMLA** , mettez en surbrillance le script XMLA, cliquez avec le bouton droit sur le script en surbrillance et sélectionnez **Copier**.  
  
     Cette étape copie le script XMLA dans le presse-papiers Windows. Vous pouvez laisser le script XMLA dans le presse-papiers ou le coller dans le Bloc-notes ou un éditeur de texte différent. Voici un exemple de script XMLA.  
  
    ```  
    <Batch xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
     <Parallel>  
      <Process xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
        <Object>  
          <DatabaseID>Adventure Works DW Multidimensional</DatabaseID>  
          <DimensionID>Dim Account</DimensionID>  
        </Object>  
        <Type>ProcessFull</Type>  
        <WriteBackTableCreation>UseExisting</WriteBackTableCreation>  
      </Process>  
     </Parallel>  
    </Batch>  
    ```  
  
###  <a name="bkmk_ProcessJob"></a> Créer et planifier le travail de traitement de la dimension  
  
1.  Connectez-vous à une instance du moteur de base de données et ouvrez l'Explorateur d'objets.  
  
2.  Développez **Agent SQL Server**.  
  
3.  Cliquez avec le bouton droit sur **Travaux** et sélectionnez **Nouveau travail**.  
  
4.  Dans la boîte de dialogue **Nouveau travail** , tapez un nom pour le travail dans le champ **Nom**.  
  
5.  Sous **Sélectionner une page**, sélectionnez **Étapes**puis cliquez sur **Nouvelle**.  
  
6.  Dans la boîte de dialogue **Nouvelle étape de travail** , tapez un nom pour l'étape dans **Nom de l'étape**.  
  
7.  Dans **Serveur**, tapez **localhost** pour une instance par défaut de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et **localhost\\**\<*nom_instance*> pour une instance nommée.  
  
     Si vous allez exécuter le travail à partir d'un ordinateur distant, utilisez le nom du serveur et le nom de l'instance où le travail s'exécutera. Utilisez le format \< *nom du serveur*> pour une instance par défaut, et \< *nom du serveur*>\\<*nom de l’instance*> pour une instance nommée.  
  
8.  Dans **Type**, sélectionnez **Commande SQL Server Analysis Services**.  
  
9. Dans **Commande**, cliquez avec le bouton droit et sélectionnez **Coller**. Le script XMLA que vous avez créé à l'étape précédente doit apparaître dans la fenêtre de commande.  
  
10. Cliquez sur **OK**.  
  
11. Sous **Sélectionner une page**, cliquez sur **Planifications**puis cliquez sur **Nouvelle**.  
  
12. Dans la boîte de dialogue **Nouvelle planification du travail** , entrez un nom pour la planification dans **Nom**, puis cliquez sur **OK**.  
  
     Cette étape crée une planification pour dimanche à 0h00 du matin. L'étape suivante montre comment exécuter manuellement le travail. Vous pouvez également spécifier une planification qui exécute le travail pendant que vous le surveillez.  
  
13. Dans la boîte de dialogue **Nouveau travail** , cliquez sur **OK**.  
  
14. Dans **l’Explorateur d’objets**, développez **Travaux**, cliquez avec le bouton droit sur le travail que vous avez créé, puis sélectionnez **Démarrer le travail à l’étape**.  
  
     Étant donné que le travail comporte une seule étape, il s'exécute immédiatement. Si le travail contient plusieurs étapes, vous pouvez sélectionner celle à laquelle il doit démarrer.  
  
15. Lorsque le travail est terminé, cliquez sur **Fermer**.  
  
## <a name="example-2-batch-processing-a-dimension-and-a-partition-in-a-scheduled-task"></a>Exemple 2 : traitement par lots d'une dimension et d'une partition dans une tâche planifiée  
 Les procédures de cet exemple montrent comment créer et planifier un travail qui traite par lots une dimension de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et en même temps traiter une partition de cube qui dépend de la dimension pour l'agrégation. Pour plus d’informations sur le traitement par lots des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consultez [Traitement par lots &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/batch-processing-analysis-services.md).  
  
###  <a name="bkmk_BatchProcess"></a> Créer un script pour le traitement par lots d'une dimension et d'une partition dans un travail de SQL Server Agent  
  
1.  À l’aide de la même base de données, développez **Dimensions**, cliquez avec le bouton droit sur la dimension **Client** et sélectionnez **Traiter**.  
  
2.  Dans la boîte de dialogue **Traiter la dimension** , dans la colonne **Options de traitement** sous **Liste d'objets**, vérifiez que l'option pour cette colonne est **Traiter entièrement**.  
  
3.  Cliquez sur **Script**.  
  
     Cette étape permet d'ouvrir une fenêtre **Requête Xml** qui contient le script XMLA qui traite la dimension.  
  
4.  Dans la boîte de dialogue **Traiter la dimension** , cliquez sur **Annuler** pour fermer la boîte de dialogue.  
  
5.  Développez **Cubes**, **Adventure Works**, **Groupes de mesures**, **Internet Sales**, **Partitions**, cliquez avec le bouton droit sur la dernière partition dans la liste, puis sélectionnez **Traiter**.  
  
6.  Dans la boîte de dialogue **Traiter la partition** , dans la colonne **Options de traitement** sous **Liste d'objets**, vérifiez que l'option pour cette colonne est **Traiter entièrement**.  
  
7.  Cliquez sur **Script**.  
  
     Cette étape permet d'ouvrir une seconde fenêtre **Requête Xml** qui contient le script XMLA qui traite la partition.  
  
8.  Dans la boîte de dialogue **Traiter la partition** , cliquez sur **Annuler** pour fermer l'éditeur.  
  
     À ce stade, vous devez fusionner les deux scripts et vérifier que la dimension est traitée en premier.  
  
    > [!WARNING]  
    >  Si la partition est traitée en premier, le traitement ultérieur de la dimension provoque le non traitement de la partition. La partition nécessiterait alors un second traitement pour atteindre l'état traité.  
  
9. Dans la fenêtre **Requête XMLA** qui contient le script XMLA qui traite la partition, mettez en surbrillance le code à l’intérieur des indicateurs `Batch` et `Parallel` , cliquez avec le bouton droit sur le script en surbrillance, puis sélectionnez **Copier**.  
  
    ```  
    <Process xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
        <Object>  
          <DatabaseID> Adventure Works DW Multidimensional</DatabaseID>  
          <CubeID>Adventure Works</CubeID>  
          <MeasureGroupID>Fact Internet Sales 1</MeasureGroupID>  
          <PartitionID> Internet_Sales_2004</PartitionID>  
        </Object>  
        <Type>ProcessFull</Type>  
        <WriteBackTableCreation>UseExisting</WriteBackTableCreation>  
      </Process>  
    ```  
  
10. Ouvrez la fenêtre **Requête XMLA** qui contient le script XMLA qui traite la dimension. Cliquez avec le bouton droit dans le script à gauche de l’indicateur `</Process>` et sélectionnez **Coller**.  
  
     L'exemple suivant affiche le script XMLA modifié.  
  
    ```  
    <Batch xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
     <Parallel>  
      <Process xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
        <Object>  
          <DatabaseID>Adventure Works DW Multidimensional</DatabaseID>  
          <DimensionID>Dim Customer</DimensionID>  
        </Object>  
        <Type>ProcessFull</Type>  
        <WriteBackTableCreation>UseExisting</WriteBackTableCreation>  
      </Process>  
      <Process xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
        <Object>  
          <DatabaseID>Adventure Works DW Multidimensional</DatabaseID>  
          <CubeID>Adventure Works</CubeID>  
          <MeasureGroupID>Fact Internet Sales 1</MeasureGroupID>  
          <PartitionID>Internet_Sales_2004</PartitionID>  
        </Object>  
        <Type>ProcessFull</Type>  
        <WriteBackTableCreation>UseExisting</WriteBackTableCreation>  
      </Process>  
     </Parallel>  
    </Batch>  
    ```  
  
11. Mettez en surbrillance le script XMLA modifié, cliquez avec le bouton droit sur le script en surbrillance, puis sélectionnez **Copier**.  
  
12. Cette étape copie le script XMLA dans le presse-papiers Windows. Vous pouvez laisser le script XMLA dans le presse-papiers, l'enregistrer dans un fichier ou le coller dans le Bloc-notes ou un éditeur de texte différent.  
  
###  <a name="bkmk_Scheduling"></a> Créer et planifier le travail de traitement par lots  
  
1.  Connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]et ouvrez l'Explorateur d'objets.  
  
2.  Développez **Agent SQL Server**. Démarrez le service, s'il n'est pas déjà en cours d'exécution.  
  
3.  Cliquez avec le bouton droit sur **Travaux** et sélectionnez **Nouveau travail**.  
  
4.  Dans la boîte de dialogue **Nouveau travail** , tapez un nom pour le travail dans le champ **Nom**.  
  
5.  Dans **Étapes**, cliquez sur **Nouveau**.  
  
6.  Dans la boîte de dialogue **Nouvelle étape de travail** , tapez un nom pour l'étape dans **Nom de l'étape**.  
  
7.  Dans **Type**, sélectionnez **Commande SQL Server Analysis Services**.  
  
8.  Dans **Exécuter en tant que**, sélectionnez le **Compte de service SQL Server Agent**. Comme l'indique la section Configuration requise, ce compte doit avoir des autorisations d'administrateur sur Analysis Services.  
  
9. Dans **Serveur**, spécifiez le nom du serveur de l'instance d'Analysis Service.  
  
10. Dans **Commande**, cliquez avec le bouton droit et sélectionnez **Coller**.  
  
11. Cliquez sur **OK**.  
  
12. Dans la page **Planifications** , cliquez sur **Nouvelle**.  
  
13. Dans la boîte de dialogue **Nouvelle planification du travail** , entrez un nom pour la planification dans **Nom**, puis cliquez sur **OK**.  
  
     Cette étape crée une planification pour dimanche à 0h00 du matin. L'étape suivante montre comment exécuter manuellement le travail. Vous pouvez également sélectionner une planification qui exécute le travail pendant que vous le surveillez.  
  
14. Cliquez sur **OK** pour fermer la boîte de dialogue.  
  
15. Dans **l’Explorateur d’objets**, développez **Travaux**, cliquez avec le bouton droit sur le travail que vous avez créé, puis sélectionnez **Démarrer le travail à l’étape**.  
  
     Étant donné que le travail comporte une seule étape, il s'exécute immédiatement. Si le travail contient plusieurs étapes, vous pouvez sélectionner celle à laquelle il doit démarrer.  
  
16. Lorsque le travail est terminé, cliquez sur **Fermer**.  
  
## <a name="see-also"></a>Voir aussi  
 [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md)   
  
  
