---
title: Déployer le composant WebPart Visionneuse de rapports de SQL Server Reporting Services sur un site SharePoint | Microsoft Docs
ms.custom: ''
ms.date: 10/05/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server-sharepoint
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 155e01d4c19f111b553510a8db648d66b4f4a788
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deploy-the-sql-server-reporting-services-report-viewer-web-part-on-a-sharepoint-site"></a>Déployer le composant WebPart Visionneuse de rapports de SQL Server Reporting Services sur un site SharePoint

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)])

Le composant WebPart Visionneuse de rapports est un composant WebPart personnalisé qui peut être utilisé pour afficher des rapports SQL Server Reporting Services (mode natif) au sein de votre site SharePoint. Vous pouvez l’utiliser pour afficher, parcourir, imprimer et exporter des rapports sur un serveur de rapports. Le composant WebPart Visionneuse de rapports est associé aux fichiers de définition de rapport (.rdl) qui sont traités par un serveur de rapports SQL Server Reporting Services ou un serveur Power BI Report Server. Il ne peut pas être utilisé avec les rapports Power BI hébergés dans Power BI Report Server.

Utilisez les instructions suivantes pour déployer manuellement le package de solution qui ajoute le composant WebPart Visionneuse de rapports à un environnement SharePoint Server 2013 ou SharePoint Server 2016. Le déploiement de la solution est une étape indispensable pour configurer le composant WebPart.

**Le composant WebPart Visionneuse de rapports est un package de solution autonome et n’est pas associé au mode intégré SharePoint pour SQL Server Reporting Services.**

## <a name="requirements"></a>Spécifications

> [!IMPORTANT]
> Vous ne pouvez pas installer ce composant WebPart si vous avez déjà configuré le mode intégré SharePoint de Reporting Services.
>

**Versions SharePoint Server prises en charge :**
* SharePoint Server 2016
* SharePoint Server 2013

**Versions Reporting Services prises en charge :**  
* SQL Server 2008 Reporting Services (mode natif) et versions ultérieures.
* Power BI Report Server

## <a name="download-the-report-viewer-web-part-solution-package"></a>Télécharger le package de solution du composant WebPart Visionneuse de rapports

Le composant WebPart Visionneuse de rapports est disponible sur le Centre de téléchargement Microsoft.

[Télécharger le package de solution du composant WebPart Visionneuse de rapports](https://www.microsoft.com/download/details.aspx?id=55949)

## <a name="deploy-the-farm-solution"></a>Déployer la solution de batterie de serveurs

Cette section montre comment déployer le package de solution sur votre batterie de serveurs SharePoint. Cette tâche ne doit être effectuée qu'une seule fois.

1. Sur un serveur SharePoint, ouvrez un environnement de ligne de commande SharePoint en utilisant l’option **Exécuter en tant qu’administrateur**.

2. Exécutez [Add-SPSolution](https://technet.microsoft.com/library/ff607552(v=office.16).aspx) pour ajouter la solution de batterie de serveurs.

    ```
    Add-SPSolution –LiteralPath "{path to file}\ReportViewerWebPart.wsp"
    ```

    L'applet de commande retourne le nom de la solution, son ID, et Deployed=False. À l'étape suivante, vous déploierez la solution.

3. Exécutez l’applet de commande [Install-SPSolution](https://technet.microsoft.com/library/ff607534(v=office.16).aspx) pour déployer la solution de batterie de serveurs.

    **SharePoint 2013**

    ```
    Install-SPSolution –Identity ReportViewerWebPart.wsp -CompatibilityLevel "14,15" -GACDeployment -WebApplication {URL to web application}
    ```

    **SharePoint 2016**

    ```
    Install-SPSolution –Identity ReportViewerWebPart.wsp -GACDeployment -WebApplication {URL to web application}
    ```

## <a name="activate-feature"></a>Activer la fonctionnalité

1. Dans votre site SharePoint, cliquez sur l’icône **engrenage** en haut à gauche et sélectionnez **Paramètres du site**.

    ![Paramètres du site à partir de l’icône d’engrenage.](media/sharepoint-site-settings.png)

    Par défaut, les applications Web SharePoint sont accessibles via le port 80. Cela signifie que vous pouvez souvent accéder à un site SharePoint en entrant *http://<computer name>* pour ouvrir la collection de sites racine.

3. Dans **Administration de la collection de sites**, cliquez sur **Fonctionnalités de la collection de sites**.

4. Faites défiler la page vers le bas jusqu’à ce que vous trouviez la fonctionnalité **Composant WebPart Visionneuse de rapports**.

5. Sélectionnez **Activer**.

    ![Activer la fonctionnalité Composant WebPart Visionneuse de rapports](media/web-part-activiate-feature.png)

6. Répétez ces étapes pour les collections de sites supplémentaires en ouvrant chaque site et en cliquant sur Actions du site.

Si vous le souhaitez, vous pouvez également utiliser PowerShell pour activer cette fonctionnalité sur tous les sites à l’aide de l’applet de commande [Enable-SPFeature](https://technet.microsoft.com/library/ff607803.aspx).

```
Get-SPWebApplication "<web application url>" | Get-SPSite -Limit ALL | 
        ForEach-Object {
            Write-Host "Enabling feature for $($_.URL)"
            Enable-SPFeature -identity "ReportViewerWebPart" -URL $_.URL -ErrorAction Continue
        }
```

## <a name="remove-the-solution"></a>Supprimer la solution

Bien que l’Administration centrale de SharePoint permette de retirer une solution, vous n’avez pas besoin de retirer le fichier **powerpivotwebapp.wsp**, sauf si vous dépannez une installation de manière systématique ou si vous corrigez un problème de déploiement.

1. Dans l’Administration centrale de SharePoint, sous **Paramètres système**, sélectionnez **Gérer les solutions de la batterie**.

2. Sélectionnez **ReportViewerWebPart.wsp**.

3. Sélectionnez Retirer la solution.

### <a name="remove-the-web-part-from-site-settings"></a>Supprimer le composant WebPart des paramètres du site

Le retrait de la solution ne supprime pas le composant WebPart Visionneuse de rapports de la liste des composants WebPart de votre site SharePoint. Pour supprimer le composant WebPart Visionneuse de rapports, effectuez les étapes suivantes.

1. Dans votre site SharePoint, cliquez sur l’icône **engrenage** en haut à gauche et sélectionnez **Paramètres du site**.

    ![Paramètres du site à partir de l’icône d’engrenage.](media/sharepoint-site-settings.png)

    Par défaut, les applications Web SharePoint sont accessibles via le port 80. Cela signifie que vous pouvez souvent accéder à un site SharePoint en entrant *http://<computer name>* pour ouvrir la collection de sites racine.

2. Sous **Galeries du concepteur web**, sélectionnez **Composants WebPart**.

3. Cliquez sur l’**icône de modification** située en regard de **ReportViewerNativeMode.dwp**. Il est possible que ce composant ne figure pas sur la première page de résultats.

4. Sélectionnez **Supprimer l’élément**.

    ![Modifier et supprimer le composant WebPart Visionneuse de rapports en mode natif](media/report-viewer-native-mode-edit-delete.png)

Vous pouvez essayer de supprimer le composant WebPart à l’aide de PowerShell, mais il n’existe aucune commande directe permettant de le faire. Pour obtenir un exemple de script, consultez [How to delete web parts from the web part Gallery](https://gallery.technet.microsoft.com/office/How-to-delete-Web-Parts-1132701f).

## <a name="supported-languages"></a>Langues prises en charge

Les langues suivantes sont prises en charge dans le composant WebPart :

* Anglais (en)
* Allemand (de)
* Espagnol (es)
* Français (fr)
* Italien (it)
* Japonais (ja)
* Coréen (ko)
* Portugais (pt)
* Russe (ru)
* Chinois (simplifié - zh-HANS et zh-CHS)
* Chinois (traditionnel - zh-HANT et zh-CHT)

## <a name="troubleshoot"></a>Dépanner

* Erreur lors de la désinstallation de SSRS si le mode intégré SharePoint est configuré :

    Install-SPRSService : [A] Impossible de caster Microsoft.ReportingServices.SharePoint.SharedService.Service.ReportingWebService en [B]Microsoft.ReportingServices.SharePoint.SharedService.Service.ReportingWebService. Le type A provient de 'Microsoft.ReportingServices.SharePoint.SharedService,Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' dans le contexte 'Default' à l’emplacement 'C:\Windows\assembly\GAC_MSIL\Microsoft.Reporting Services.SharePoint.SharedService.dll'. Le type B provient de 'Microsoft.ReportingServices.SharePoint.SharedService,Version=12.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' dans le contexte 'Default' à l’emplacement 'C:\Windows\assembly\GAC_MSIL\Microsoft.Reporting Services.SharePoint.SharedService.dll'.
    
    Solution :
    1. Supprimez le composant WebPart de la Visionneuse de rapports.
    2. Désinstallez SSRS.
    3. Réinstallez le composant WebPart de la Visionneuse de rapports.

* Erreur lors de la tentative de mise à niveau de SharePoint si le mode intégré SharePoint est configuré :

    Impossible de charger le fichier ou l’assembly 'Microsoft.ReportingServices.Alerting.ServiceContract, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' ou une de ses dépendances. Le système ne trouve pas le fichier spécifié. 00000000-0000-0000-0000-000000000000
    
    Solution :
    1. Supprimez le composant WebPart de la Visionneuse de rapports.
    2. Désinstallez SSRS.
    3. Réinstallez le composant WebPart de la Visionneuse de rapports.

## <a name="next-steps"></a>Étapes suivantes

Une fois le composant WebPart Visionneuse de rapports déployé et activé, vous pouvez l’ajouter à une page SharePoint. Pour plus d’informations, consultez [Ajouter le composant WebPart Visionneuse de rapports de SQL Server Reporting Services à une page SharePoint](add-report-viewer-web-part-to-page.md).

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
