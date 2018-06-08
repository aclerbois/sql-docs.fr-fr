---
title: DDL d’exécution de SQL Server Analysis Services, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.asexecuteddltask.f1
- sql13.dts.designer.asexecuteddltask.general.f1
- sql13.dts.designer.asexecuteddltask.ddl.f1
helpviewer_keywords:
- Analysis Services Execute DDL task
- DDL
ms.assetid: 7f25c8c6-b601-41f2-9553-be0a2ee0751a
caps.latest.revision: 48
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f33b2da7660c4e230bb20e38b309d73b2f07bcf6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="analysis-services-execute-ddl-task"></a>Tâche DDL d'exécution de SQL Server Analysis Services
  La tâche DDL d'exécution de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] exécute des instructions DDL (Data Definition Language) qui peuvent créer, supprimer ou modifier des modèles d'exploration de données et des objets multidimensionnels tels que des cubes et des dimensions. Par exemple, une instruction DDL peut créer une partition dans le cube **Adventure Works** ou supprimer une dimension dans [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)], l’exemple de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] inclus dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La tâche DDL d'exécution de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise un gestionnaire de connexions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour se connecter à une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou à un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Pour plus d'informations, consultez [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md).  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] comprend une série de tâches qui effectuent des opérations de Business Intelligence, telles que le traitement des objets analytiques et l’exécution des requêtes de prédiction d’exploration de données.  
  
 Pour plus d'informations sur les tâches Business Intelligence associées, cliquez sur l'une des rubriques suivantes :  
  
-   [Tâche de traitement d’Analysis Services](../../integration-services/control-flow/analysis-services-processing-task.md)  
  
-   [Tâche de requête d'exploration de données](../../integration-services/control-flow/data-mining-query-task.md)  
  
## <a name="ddl-statements"></a>Instructions DDL  
 Les instructions DDL sont représentées en tant qu'instructions en langage ASSL ( [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Scripting Language) et insérées dans une commande XMLA (XML for Analysis).  
  
-   Le langage ASSL permet de définir et de décrire une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], ainsi que les bases de données et les objets de base de données qu’elle contient. Pour plus d’informations, consultez [Référence Analysis Services Scripting Language &#40;ASSL&#41;](../../analysis-services/scripting/analysis-services-scripting-language-assl-for-xmla.md).  
  
-   XMLA est un langage de commande qui permet d'envoyer des commandes d'action, telles que Create, Alter ou Process, à une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour plus d’informations, consultez [Référence XML for Analysis &#40;XMLA&#41;](../../analysis-services/xmla/xml-for-analysis-xmla-reference.md).  
  
 Si le code DDL est stocké dans un fichier distinct, la tâche DDL d’exécution de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise un gestionnaire de connexions de fichiers pour spécifier le chemin du fichier. Pour plus d’informations, consultez [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
 Étant donné que les instructions DDL peuvent contenir des mots de passe et d’autres informations sensibles, un package qui comporte une ou plusieurs tâches DDL d’exécution [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] doit utiliser le niveau de protection de package **EncryptAllWithUserKey** ou **EncryptAllWithPassword**. Pour plus d’informations, consultez [Integration Services &#40;SSIS&#41;, packages](../../integration-services/integration-services-ssis-packages.md).  
  
### <a name="ddl-examples"></a>Exemples d'instructions DDL  
 Les trois instructions DDL suivantes ont été générées en créant des scripts d’objets dans [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)], la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] incluse dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 L’instruction DDL suivante supprime la dimension **Promotion** .  
  
```  
<Delete xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
    <Object>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
        <DimensionID>Dim Promotion</DimensionID>  
    </Object>  
</Delete>  
  
```  
  
 L'instruction DDL suivante traite le cube [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)] .  
  
```  
<Batch xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
  <Parallel>  
    <Process xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
      <Object>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
      </Object>  
      <Type>ProcessFull</Type>  
      <WriteBackTableCreation>UseExisting</WriteBackTableCreation>  
    </Process>  
  </Parallel>  
</Batch>  
  
```  
  
 L’instruction DDL suivante crée le modèle d’exploration de données **Forecasting** .  
  
```  
<Create xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
    <ParentObject>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
        <MiningStructureID>Forecasting</MiningStructureID>  
    </ParentObject>  
    <ObjectDefinition>  
        <MiningModel xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
            <ID>Forecasting</ID>  
            <Name>Forecasting</Name>  
            <Algorithm>Microsoft_Time_Series</Algorithm>  
            <AlgorithmParameters>  
                <AlgorithmParameter>  
                    <Name>PERIODICITY_HINT</Name>  
                    <Value xsi:type="xsd:string">{12}</Value>  
                </AlgorithmParameter>  
            </AlgorithmParameters>  
            <Columns>  
                <Column>  
                    <ID>Amount</ID>  
                    <Name>Amount</Name>  
                    <SourceColumnID>Amount</SourceColumnID>  
                    <Usage>Predict</Usage>  
                </Column>  
                <Column>  
                    <ID>Model Region</ID>  
                    <Name>Model Region</Name>  
                    <SourceColumnID>Model Region</SourceColumnID>  
                    <Usage>Key</Usage>  
                </Column>  
                <Column>  
                    <ID>Quantity</ID>  
                    <Name>Quantity</Name>  
                    <SourceColumnID>Quantity</SourceColumnID>  
                    <Usage>Predict</Usage>  
                </Column>  
                <Column>  
                    <ID>Time Index</ID>  
                    <Name>Time Index</Name>  
                    <SourceColumnID>Time Index</SourceColumnID>  
                    <Usage>Key</Usage>  
                </Column>  
            </Columns>  
            <Collation>Latin1_General_CS_AS_KS</Collation>  
        </MiningModel>  
    </ObjectDefinition>  
</Create>  
  
```  
  
 Les trois instructions DDL suivantes ont été générées en créant des scripts d’objets dans [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)], la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] incluse dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 L’instruction DDL suivante supprime la dimension **Promotion** .  
  
```  
<Delete xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
    <Object>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
        <DimensionID>Dim Promotion</DimensionID>  
    </Object>  
</Delete>  
  
```  
  
 L'instruction DDL suivante traite le cube [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)] .  
  
```  
<Batch xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
  <Parallel>  
    <Process xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
      <Object>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
      </Object>  
      <Type>ProcessFull</Type>  
      <WriteBackTableCreation>UseExisting</WriteBackTableCreation>  
    </Process>  
  </Parallel>  
</Batch>  
  
```  
  
 L’instruction DDL suivante crée le modèle d’exploration de données **Forecasting** .  
  
```  
<Create xmlns="http://schemas.microsoft.com/analysisservices/2003/engine">  
    <ParentObject>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
        <MiningStructureID>Forecasting</MiningStructureID>  
    </ParentObject>  
    <ObjectDefinition>  
        <MiningModel xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
            <ID>Forecasting</ID>  
            <Name>Forecasting</Name>  
            <Algorithm>Microsoft_Time_Series</Algorithm>  
            <AlgorithmParameters>  
                <AlgorithmParameter>  
                    <Name>PERIODICITY_HINT</Name>  
                    <Value xsi:type="xsd:string">{12}</Value>  
                </AlgorithmParameter>  
            </AlgorithmParameters>  
            <Columns>  
                <Column>  
                    <ID>Amount</ID>  
                    <Name>Amount</Name>  
                    <SourceColumnID>Amount</SourceColumnID>  
                    <Usage>Predict</Usage>  
                </Column>  
                <Column>  
                    <ID>Model Region</ID>  
                    <Name>Model Region</Name>  
                    <SourceColumnID>Model Region</SourceColumnID>  
                    <Usage>Key</Usage>  
                </Column>  
                <Column>  
                    <ID>Quantity</ID>  
                    <Name>Quantity</Name>  
                    <SourceColumnID>Quantity</SourceColumnID>  
                    <Usage>Predict</Usage>  
                </Column>  
                <Column>  
                    <ID>Time Index</ID>  
                    <Name>Time Index</Name>  
                    <SourceColumnID>Time Index</SourceColumnID>  
                    <Usage>Key</Usage>  
                </Column>  
            </Columns>  
            <Collation>Latin1_General_CS_AS_KS</Collation>  
        </MiningModel>  
    </ObjectDefinition>  
</Create>  
  
```  
  
## <a name="configuration-of-the-analysis-services-execute-ddl-task"></a>Configuration de la tâche DDL d'exécution d'Analysis Services  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="programmatic-configuration-of-the-analysis-services-execute-ddl-task"></a>Configuration par programmation de la tâche DDL d'exécution d'Analysis Services  
 Pour plus d'informations sur la définition par programme de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.DataTransformationServices.Tasks.DTSProcessingTask.ASExecuteDDLTask>  
  
## <a name="analysis-services-execute-ddl-task-editor-general-page"></a>Éditeur de tâche DDL d'exécution Analysis Services (page Général)
  La page **Général** de la boîte de dialogue **Éditeur de tâche DDL d’exécution Analysis Services** permet de nommer et de décrire la tâche DDL d’exécution [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
### <a name="options"></a>Options  
 **Nom**  
 Fournit un nom unique pour la tâche DDL d’exécution [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Tapez une description de la tâche DDL d'exécution [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
## <a name="analysis-services-execute-ddl-task-editor-ddl-page"></a>Éditeur de tâche DDL d'exécution d'Analysis Services (page DDL)
  Utilisez la page **DDL** de la boîte de dialogue **Éditeur de tâche DDL d’exécution d’Analysis Services** pour spécifier une connexion à un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou à une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et fournir des informations sur la source des instructions de langage de définition de données (DDL).  
  
### <a name="static-options"></a>Options statiques  
 **Connexion**  
 Sélectionnez un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou un gestionnaire de connexions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans la liste, ou cliquez sur <\<**Nouvelle connexion**> et utilisez la boîte de dialogue **Ajout d’un gestionnaire de connexions Analysis Services** pour créer une connexion.  
  
 **Rubriques connexes :** [Référence de l'interface utilisateur de la boîte de dialogue Ajout d’un gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md), [Gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/analysis-services-connection-manager.md)  
  
 **SourceType**  
 Spécifiez le type de source des instructions DDL. Cette propriété dispose des options répertoriées dans le tableau suivant :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Définissez la source de l'instruction DDL enregistrée dans la zone de texte **SourceDirect** . Sélectionnez cette valeur pour afficher l'option dynamique de la section suivante.|  
|**Connexion de fichiers**|Définissez la source par un fichier qui contient l'instruction DDL. Sélectionnez cette valeur pour afficher l'option dynamique de la section suivante.|  
|**Variable**|Définissez la source par une variable. Sélectionnez cette valeur pour afficher l'option dynamique de la section suivante.|  
  
### <a name="dynamic-options"></a>Options dynamiques  
  
#### <a name="sourcetype--direct-input"></a>SourceType = Entrée directe  
 **Source**  
 Tapez les instructions DDL ou cliquez sur le bouton représentant des points de suspension **(…)** , puis tapez les instructions dans la boîte de dialogue **Instructions DDL** .  
  
#### <a name="sourcetype--file-connection"></a>SourceType = Connexion de fichiers  
 **Source**  
 Sélectionnez une connexion de fichiers dans la liste ou cliquez sur <\<**Nouvelle connexion**> et utilisez la boîte de dialogue **Gestionnaire de connexions** de fichiers pour créer une connexion.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md)  
  
#### <a name="sourcetype--variable"></a>SourceType = Variable  
 **Source**  
 Sélectionnez une variable dans la liste ou cliquez sur \<**Nouvelle variable**> et utilisez la boîte de dialogue **Ajouter une variable** pour créer une variable.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md)  
  
