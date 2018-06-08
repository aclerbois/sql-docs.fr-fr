---
title: Prendre en charge le multi-ciblage dans les composants personnalisés | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-custom-objects
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- SQL Server (starting with 2016)
ms.assetid: ec611374-16bf-4a56-8fd9-45d3ddd7befc
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c94edaab7910c8f7cc86e1eafd51eb189429477b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="support-multi-targeting-in-your-custom-components"></a>Prendre en charge le multi-ciblage dans les composants personnalisés
 Vous pouvez désormais utiliser le concepteur SSIS dans SQL Server Data Tools (SSDT) pour créer, gérer et exécuter des packages qui ciblent SQL Server 2016, SQL Server 2014 ou SQL Server 2012. Pour obtenir SSDT pour Visual Studio 2015, consultez [Télécharger la dernière version de SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md). 

 Dans l’Explorateur de solutions, cliquez avec le bouton droit sur un projet Integration Services, puis sélectionnez **Propriétés** pour ouvrir les pages de propriétés du projet. Sous l’onglet **Général** de **Propriétés de configuration**, sélectionnez la propriété **TargetServerVersion** , puis choisissez SQL Server 2016, SQL Server 2014 ou SQL Server 2012.  
   
 ![Propriété TargetServerVersion dans la boîte de dialogue Propriétés du projet](../../integration-services/media/targetserverversion2.png "Propriété TargetServerVersion dans la boîte de dialogue Propriétés du projet")  
 
 ## <a name="multiple-version-support-and-multi-targeting-for-custom-components"></a>Prise en charge de versions multiples et multiciblage pour les composants personnalisés
 
Les cinq types d’extensions SSIS personnalisées prennent en charge le multiciblage.
-   Gestionnaires de connexions
-   Tâches
-   Énumérateurs
-   Modules fournisseurs d'informations
-   Composants de flux de données

Pour les extensions managées, le concepteur SSIS charge la version de l’extension correspondant à la version cible spécifiée. Exemple :
-   Lorsque la version cible est SQL Server 2012, le concepteur charge la version 2012 de l’extension.
-   Lorsque la version cible est SQL Server 2016, le concepteur charge la version 2016 de l’extension.

Les extensions COM ne prennent pas en charge le multiciblage. Le concepteur SSIS charge toujours l’extension COM pour la version actuelle de SQL Server, quelle que soit la version cible spécifiée.

## <a name="add-basic-support-for-multiple-versions-and-multi-targeting"></a>Ajouter la prise en charge de base des versions multiples et du multiciblage

Pour plus d’informations et des conseils, consultez [Getting your SSIS custom extensions to be supported by the multi-version support of SSDT 2015 for SQL Server 2016](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/). Ce billet de blog décrit les étapes et exigences suivantes :

-   Déployer vos assemblys dans les dossiers appropriés.

-   Créer un fichier de mappage d’extensions pour SQL Server 2014 et ultérieur.

## <a name="add-code-to-switch-versions"></a>Ajouter du code permettant de changer de version

### <a name="switch-versions-in-a-custom-connection-manager-task-enumerator-or-log-provider"></a>Changer de version dans un gestionnaire de connexions, une tâche, un énumérateur ou un module fournisseur d’informations personnalisé

Pour un gestionnaire de connexions, une tâche, un énumérateur ou un module fournisseur d’informations personnalisé, ajoutez une logique de passage à une version antérieure dans la méthode **SaveToXML**.

```csharp
public void SaveToXML(XmlDocument doc, IDTSInfoEvents events)
{
    if (TargetServerVersion == DTSTargetServerVersion.SQLServer2014)
    {
        // Add logic to downgrade from SQL Server 2016 to SQL Server 2014.
    }

    if (TargetServerVersion == DTSTargetServerVersion.SQLServer2012)
    {
         // Add logic to downgrade from SQL Server 2016 to SQL Server 2012.
    }
}
```

### <a name="switch-versions-in-a-custom-data-flow-component"></a>Changer de version dans un composant de flux de données personnalisé

Pour un gestionnaire de connexions, une tâche, un énumérateur ou un module fournisseur d’informations personnalisé, ajoutez une logique de passage à une version antérieure dans la nouvelle méthode **PerformDowngrade**.

```csharp
public override void PerformDowngrade(int pipelineVersion, DTSTargetServerVersion targetServerVersion)
{
    if (targetServerVersion == DTSTargetServerVersion.DTSTSV_SQLSERVER2014)
    {
        // Add logic to downgrade from SQL Server 2016 to SQL Server 2014.
        ComponentMetaData.Version = 8;
    }

    if (targetServerVersion == DTSTargetServerVersion.DTSTSV_SQLSERVER2012)
    {
          // Add logic to downgrade from SQL Server 2016 to SQL Server 2012.
        ComponentMetaData.Version = 6;
    }
}
```

## <a name="common-errors"></a>Erreurs fréquentes

### <a name="invalidcastexception"></a>InvalidCastException

**Message d’erreur.** Impossible d’effectuer un cast d’un objet COM de type ’System.__ComObject’ en type d’interface ’Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100’. Cette opération a échoué car l’appel QueryInterface sur le composant COM pour l’interface avec l’IID ’{BE8C48A3-155B-4810-BA5C-BDF68A659E9E}’ a échoué en raison de l’erreur suivante : interface non prise en charge (Exception de HRESULT : 0 x 80004002 (E_NOINTERFACE)). (Microsoft.SqlServer.DTSPipelineWrap).

**Solution.** Si votre extension personnalisée fait référence à des assemblys d’interopérabilité SSIS comme Microsoft.SqlServer.DTSPipelineWrap ou Microsoft.SqlServer.DTSRuntimeWrap, définissez la valeur de la propriété **Incorporer les types d’interopérabilité** sur **False**.

![Incorporer les types d’interopérabilité](../../integration-services/extending-packages-custom-objects/media/embed-interop-types.png)

### <a name="unable-to-load-some-types-when-target-version-is-sql-server-2012"></a>Impossible de charger certains types lorsque la version cible est SQL Server 2012

Ce problème affecte certains types comme IErrorReportingService ou IUserPromptService.

**Message d’erreur (exemple).** Impossible de charger le type ’Microsoft.DataWarehouse.Design.IErrorReportingService’ à partir de l’assembly ’Microsoft.DataWarehouse, Version = 13.0.0.0, Culture = neutral, PublicKeyToken = 89845dcd8080cc91’.

**Solution de contournement.** Utilisez un objet MessageBox à la place de ces interfaces lorsque la version cible est SQL Server 2012.

