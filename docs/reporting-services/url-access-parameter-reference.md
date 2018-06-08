---
title: Référence de paramètre d’accès URL | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2015
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- reports [Reporting Services], display options
- URL access [Reporting Services], report display parameters
ms.assetid: 1c3e680a-83ea-4979-8e79-fa2337ae12a3
caps.latest.revision: 48
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 76f7e0be913313c56b8f05eeb24e43534407ceec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="url-access-parameter-reference"></a>Référence de paramètre d’accès URL
  Vous pouvez utiliser les paramètres suivants dans une URL afin de configurer l’apparence de vos rapports [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)]. Les paramètres les plus courants sont répertoriés dans cette section. Les paramètres ne sont pas sensibles à la casse et commencent par le préfixe de paramètre *rs:* s’ils sont dirigés vers le serveur de rapports ou par *rc:* s’ils sont dirigés vers une visionneuse HTML. Vous pouvez également spécifier des paramètres spécifiques aux périphériques ou des extensions de rendu. Pour plus d’informations sur les paramètres spécifiques au périphérique, consultez [Spécifier les paramètres d’informations de périphérique dans une URL](../reporting-services/specify-device-information-settings-in-a-url.md).  
  
> [!IMPORTANT]  
>  Dans le cas d’un serveur de rapports en mode SharePoint, il est important que l’URL inclue la syntaxe de proxy `_vti_bin` pour acheminer la requête via SharePoint et le proxy HTTP [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . Le proxy ajoute à la requête HTTP le contexte nécessaire pour garantir une exécution correcte du rapport pour les serveurs de rapports en mode SharePoint. Pour obtenir des exemples, consultez [Access Report Server Items Using URL Access](../reporting-services/access-report-server-items-using-url-access.md).  
>   
>  Pour plus d'informations sur l'inclusion de paramètres de rapport dans une URL et pour obtenir des exemples, consultez [Pass a Report Parameter Within a URL](../reporting-services/pass-a-report-parameter-within-a-url.md).  
  
##  <a name="bkmk_top"></a> Dans cette rubrique  
  
-   [Commandes de visionneuse HTML (rc:)](#bkmk_htmlviewer)  
  
-   [Commandes du serveur de rapports (rs:)](#bkmk_reportserver)  
  
-   [Commandes de composant WebPart Visionneuse de rapports (rv:)](#bkmk_webpart)  
  
##  <a name="bkmk_htmlviewer"></a> Commandes de visionneuse HTML (rc:)  
 Les commandes de visionneuse HTML sont utilisées pour cibler la visionneuse HTML (par exemple, à partir du Gestionnaire de rapports). Elles sont précédées de *rc:*.  
  
-   *Toolbar* :  
                  Affiche ou masque la barre d'outils. Si la valeur de ce paramètre est **false**, toutes les options restantes sont ignorées. Si vous omettez ce paramètre, la barre d'outils s'affiche automatiquement pour les formats de rendu assurant sa prise en charge. La valeur par défaut de ce paramètre est **true**.  
  
    > [!IMPORTANT]  
    >  *rc:Toolbar*=**false** ne fonctionne pas pour les chaînes d’accès URL qui utilisent une adresse IP au lieu d’un nom de domaine pour cibler un rapport hébergé sur un site SharePoint.  
  
-   *Parameters* : affiche ou masque la zone de paramètres de la barre d’outils. Si vous affectez à ce paramètre la valeur **true**, la zone de paramètres de la barre d'outils s'affiche. Si vous lui affectez la valeur **false**, la zone de paramètres ne s'affiche pas et ne peut pas être affichée par l'utilisateur. Si vous affectez à ce paramètre une valeur **Collapsed**, la zone de paramètres ne s'affiche pas, mais l'utilisateur final peut la faire apparaître. La valeur par défaut de ce paramètre est **true**.  
  
     Pour obtenir un exemple en mode **Native** :  
  
    ```  
    http://myrshost/reportserver?/Sales&rc:Parameters=Collapsed  
    ```  
  
     Pour obtenir un exemple en mode **SharePoint** :  
  
    ```  
    http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales&rc:Parameters=Collapsed  
    ```  
  
-   *Zoom* : définit la valeur de zoom du rapport sous la forme d’un pourcentage entier ou d’une constante de chaîne. Les valeurs de chaîne standard incluent les valeurs **Page Width** et **Whole Page**. Ce paramètre est ignoré par les versions d’Internet Explorer antérieures à la version 5.0 et par tous les navigateurs non[!INCLUDE[msCoName](../includes/msconame-md.md)] . La valeur par défaut de ce paramètre est **100**.  
  
     Par exemple, en mode **Native** :  
  
    ```  
    http://myrshost/reportserver?/Sales&rc:Zoom=Page Width  
    ```  
  
     Par exemple, en mode **SharePoint** .  
  
    ```  
    http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales&rc:Zoom=Page Width  
    ```  
  
-   *Section* : définit la page du rapport à afficher. Toute valeur supérieure au nombre de pages du rapport affiche la dernière page. Toute valeur inférieure à **0** affiche la page 1 du rapport. La valeur par défaut de ce paramètre est **1**.  
  
     Par exemple, en mode **Native** , pour afficher la page 2 du rapport :  
  
    ```  
    http://myrshost/reportserver?/Sales&rc:Section=2  
    ```  
  
     Par exemple, en mode **SharePoint** , pour afficher la page 2 du rapport :  
  
    ```  
    http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales&rc:Section=2  
    ```  
  
-   *FindString*: recherche un texte spécifique dans un rapport.  
  
     Pour obtenir un exemple en mode **Native** .  
  
    ```  
    http://myrshost/reportserver?/Sales&rc:FindString=Mountain-400  
    ```  
  
     Pour obtenir un exemple en mode **SharePoint** .  
  
    ```  
    http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales&rc:FindString=Mountain-400  
    ```  
  
-   *StartFind* : spécifie la dernière section à explorer. La valeur par défaut de ce paramètre est la dernière page du rapport.  
  
     Pour obtenir un exemple en mode **Native** qui recherche la première occurrence du texte «Mountain-400 » entre les pages un et cinq de l’exemple de rapport intitulé Product Catalog.  
  
    ```  
    http://server/Reportserver?/SampleReports/Product Catalog&rs:Command=Render&rc:StartFind=1&rc:EndFind=5&rc:FindString=Mountain-400  
    ```  
  
-   *EndFind* : définit le numéro de la dernière page à utiliser dans la recherche. Par exemple, une valeur de **5** indique que la dernière page à explorer est la page 5 du rapport. La valeur par défaut est le numéro de la page active. Utilisez ce paramètre conjointement avec le paramètre *StartFind* . Consultez l'exemple ci-dessus.  
  
-   *FallbackPage* : définit le numéro de la page à afficher en cas d’échec de la recherche ou de la sélection de l’explorateur de documents. La valeur par défaut est le numéro de la page active.  
  
-   *GetImage* : récupère une icône particulière pour l’interface utilisateur de la visionneuse HTML.  
  
-   *Icon* : récupère l’icône d’une extension de rendu particulière.  
  
-   *Stylesheet*: spécifie une feuille de style à appliquer à la visionneuse HTML.  
  
-   Paramètre d’informations de périphérique : spécifie un paramètre d’informations de périphérique sous la forme `rc:tag=value`, où *tag* est le nom d’un ensemble de paramètres d’informations de périphérique spécifique à l’extension de rendu actuellement utilisée (consultez la description du paramètre *Format* ). Par exemple, vous pouvez utiliser le paramètre d’informations de périphérique *OutputFormat* pour que l’extension de rendu IMAGE restitue le rapport sous la forme d’une image JPEG à l’aide des paramètres suivants dans la chaîne d’accès URL : `…&rs:Format=IMAGE&rc:OutputFormat=JPEG`. Pour plus d’informations sur tous les paramètres d’informations de périphérique spécifiques aux extensions, consultez [Paramètres d’informations de périphérique pour les extensions de rendu &#40;Reporting Services&#41;](../reporting-services/device-information-settings-for-rendering-extensions-reporting-services.md).  
  
##  <a name="bkmk_reportserver"></a> Commandes du serveur de rapports (rs:)  
 Les commandes de serveur de rapports sont précédées de *rs:* et sont utilisées pour cibler le serveur de rapports :  
  
-   *Command*:  
                  Exécute une action sur un élément de catalogue, selon son type d'élément. La valeur par défaut est déterminée par le type de l'élément de catalogue référencé dans la chaîne d'accès URL. Les valeurs valides sont :  
  
    -   **ListChildren** et **GetChildren** affichent le contenu d'un dossier. Les éléments du dossier sont affichés dans une page générique de navigation des éléments.  
  
         Par exemple, en mode **Native** .  
  
        ```  
        http://myrshost/reportserver?/Sales&rs:Command=GetChildren  
        ```  
  
         Par exemple, une instance nommée en mode **Native** .  
  
        ```  
        http://myssrshost/Reportserver_THESQLINSTANCE?/reportfolder&rs:Command=listChildren  
        ```  
  
         Par exemple, en mode **SharePoint** .  
  
        ```  
        http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales&rs:Command=GetChildren  
        ```  
  
    -   **Render** Le rapport est restitué dans le navigateur pour vous permettre de l’afficher.  
  
         Par exemple, en mode **Native** :  
  
        ```  
        http://myrshost/reportserver?/Sales/YearlySalesByCategory&rs:Command=Render  
        ```  
  
         Par exemple, en mode **SharePoint** .  
  
        ```  
        http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales/YearlySalesByCategory&rs:Command=Render  
        ```  
  
    -   **GetSharedDatasetDefinition** affiche la définition XML associée à un dataset partagé. Les propriétés de dataset partagé, notamment la requête, les paramètres de dataset, les valeurs par défaut, les filtres de dataset et les options de données telles que le classement et le respect de la casse, sont enregistrées dans la définition. Vous devez disposer de l'autorisation **Lire la définition de rapport** sur un dataset partagé pour utiliser cette valeur.  
  
         Par exemple, en mode **Native** .  
  
        ```  
        http://localhost/reportserver/?/DataSet1&rs:command=GetShareddatasetDefinition  
        ```  
  
    -   **GetDataSourceContents** affiche les propriétés d'une source de données partagée donnée au format XML. Si votre navigateur prend en charge XML et que vous êtes un utilisateur authentifié avec l'autorisation **Read Contents** sur la source des données, la définition de la source de données est affichée.  
  
         Par exemple, en mode **Native** .  
  
        ```  
        http://myrshost/reportserver?/Sales/AdventureWorks2012&rs:Command=GetDataSourceContents  
        ```  
  
         Par exemple, en mode **SharePoint** .  
  
        ```  
        http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales/AdventureWorks2012&rs:Command=GetDataSourceContents  
        ```  
  
    -   **GetResourceContents** restitue une ressource et l'affiche dans une page HTML si la ressource est compatible avec le navigateur. Sinon, vous êtes invité à ouvrir ou enregistrer le fichier ou la ressource sur le disque.  
  
         Par exemple, en mode **Native** .  
  
        ```  
        http://myrshost/reportserver?/Sales/StorePicture&rs:Command=GetResourceContents  
        ```  
  
         Par exemple, en mode **SharePoint** .  
  
        ```  
        http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales/StorePicture.jpg&rs:Command=GetResourceContents  
        ```  
  
    -   **GetComponentDefinition** affiche la définition XML associée à un élément de rapport publié. Vous devez disposer d'autorisations **Lire le contenu** d'un élément de rapport publié pour utiliser cette valeur.  
  
-   *Format* :  
                  Spécifie le format de rendu et d’affichage d’un rapport. Exemples de valeurs courantes :  
  
    -   **HTML5**  
  
    -   **PPTX**  
  
    -   **ATOM**  
  
    -   **HTML4.0**  
  
    -   **MHTML**  
  
    -   **IMAGE**  
  
    -   **EXCEL**  
  
    -   **WORD**  
  
    -   **CSV**  
  
    -   **PDF**  
  
    -   **XML**  
  
     La valeur par défaut est **HTML5**. Pour plus d’informations, consultez [Export a Report Using URL Access](../reporting-services/export-a-report-using-url-access.md).  
  
     Pour obtenir une liste complète, consultez la section relative à l’extension **\<Render>** du fichier rsreportserver.config du serveur de rapports.  Pour plus d’informations sur l’emplacement du fichier, consultez [RsReportServer.config Configuration File](../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
     Par exemple, pour obtenir directement une copie PDF d’un rapport à partir d’un serveur de rapports en mode **Native** :  
  
    ```  
    http://myrshost/ReportServer?/myreport&rs:Format=PDF  
    ```  
  
     Par exemple, pour obtenir directement une copie PDF d’un rapport à partir d’un serveur de rapports en mode **SharePoint** :  
  
    ```  
    http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/myrereport.rdl&rs:Format=PDF  
    ```  
  
-   *ParameterLanguage*:  
                  Fournit une langue indépendante de la langue du navigateur pour les paramètres passés dans une URL. La valeur par défaut est la langue du navigateur. La valeur peut être une valeur de culture, telle que **en-us** ou **de-de**.  
  
     Par exemple, en mode **Native** , pour remplacer la langue du navigateur et spécifier la valeur de culture fr-FR :  
  
    ```  
    http://myrshost/Reportserver?/SampleReports/Product+Line+Sales&rs:Command=Render&StartDate=4/10/2008&EndDate=11/10/2008&rs:ParameterLanguage=de-DE  
    ```  
  
-   *Snapshot* : génère un rapport à partir d’un instantané d’historique de rapport. Pour plus d’informations, consultez [Rendre un instantané d’historique de rapport à l’aide de l’accès URL](../reporting-services/render-a-report-history-snapshot-using-url-access.md).  
  
     Par exemple, en mode **Native** , récupérez un instantané d’historique de rapport en date du 04/07/2003 avec un horodateur de 13:40:02 :  
  
    ```  
    http://myrshost/reportserver?/SampleReports/Company Sales&rs:Snapshot=2003-04-07T13:40:02  
    ```  
  
-   *PersistStreams*:  
                  Effectue le rendu d'un flux de données persistant distinct. Ce paramètre est repris par le convertisseur d'image pour transmettre le rapport segment par segment. Après avoir utilisé ce paramètre dans une chaîne d'accès à l'URL, utilisez cette même chaîne avec le paramètre *GetNextStream* au lieu du paramètre *PersistStreams* pour obtenir le segment suivant dans le flux de données persistant. Cette URL permet d'obtenir un flux de 0 octets. Il désigne la fin du flux persistent. La valeur par défaut est **false**.  
  
-   *GetNextStream*:  
                  Permet d’obtenir le bloc de données suivant dans un flux persistant auquel vous accédez à l’aide du paramètre *PersistStreams* . Pour plus d'informations, consultez la description de *PersistStreams*. La valeur par défaut est **false**.  
  
-   *SessionID*:  
                  Spécifie une session de rapport active établie entre l'application cliente et le serveur de rapports. La valeur de ce paramètre est l'identificateur de session.  
  
     Vous pouvez spécifier l'ID de session en tant que cookie ou dans le cadre de l'URL. Lorsque le serveur de rapports a été configuré de manière à ne pas utiliser de cookies de session, la première demande sans un ID de session spécifié provoque une redirection avec un ID de session. Pour plus d'informations sur les sessions de serveur de rapports, consultez [Identifying Execution State](../reporting-services/report-server-web-service-net-framework-soap-headers/identifying-execution-state.md).  
  
-   *ClearSession*:  
                  La valeur **true** indique au serveur de rapports de supprimer un rapport d’une session de rapport. Toutes les instances de rapport associées à un utilisateur authentifié sont supprimées de la session de rapport. (Par définition, une instance de rapport est un rapport identique exécuté plusieurs fois avec des valeurs de paramètre de rapport différentes.) La valeur par défaut est **false**.  
  
-   *ResetSession*:  
                  La valeur **true** indique au serveur de rapports de réinitialiser la session de rapport en supprimant l’association de la session de rapport à tous les instantanés de rapport. La valeur par défaut est **false**.  
  
-   *ShowHideToggle*:  
                  Bascule de l'état afficher à masquer d'une section du rapport. Spécifiez un entier positif pour représenter la section à basculer.  
  
##  <a name="bkmk_webpart"></a> Commandes de composant WebPart Visionneuse de rapports (rv:)  
 Les noms des paramètres de rapport réservés [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sont utilisés pour cibler le composant WebPart Visionneuse de rapports intégré à SharePoint. Ces noms de paramètre sont préfixés par *rv:*. Le composant WebPart Visionneuse de rapports prend également en charge le paramètre *rs:ParameterLanguage* .  
  
-   *Toolbar*: contrôle l’affichage de la barre d’outils pour le composant WebPart Visionneuse de rapports. La valeur par défaut est **Full**. Les valeurs peuvent être les suivantes :  
  
    -   **Full**: affiche la barre d'outils complète.  
  
    -   **Navigation**: affiche uniquement la pagination dans la barre d'outils.  
  
    -   **None**: n'affiche pas la barre d'outils.  
  
     Par exemple, en mode **SharePoint** , pour afficher uniquement la pagination dans la barre d'outils.  
  
    ```  
    http://myspsite/_vti_bin/reportserver?http://myspsite002%fShared+Documents%2fmyreport.rdl&rv:DocMapMode=Displayed&rv:Toolbar=Navigation  
    ```  
  
-   *HeaderArea*: contrôle l’affichage de l’en-tête pour le composant WebPart Visionneuse de rapports. La valeur par défaut est **Full**. Les valeurs peuvent être les suivantes :  
  
    -   **Full**: affiche l'en-tête complet.  
  
    -   **BreadCrumbsOnly**: affiche uniquement l’exploration à l’aide de la barre de navigation dans l’en-tête pour indiquer à l’utilisateur où il se trouve dans l’application.  
  
    -   **None**: n'affiche pas l'en-tête.  
  
     Par exemple, en mode **SharePoint** , pour afficher uniquement l’exploration à l’aide de la barre de navigation dans l’en-tête.  
  
    ```  
    http://myspsite/_vti_bin/reportserver?http://myspsite002%fShared+Documents%2fmyreport.rdl&rv:DocMapMode=Displayed&rv:HeaderArea=BreadCrumbsOnly  
    ```  
  
-   *DocMapAreaWidth*: contrôle la largeur d’affichage, en pixels, de la zone de paramètres dans le composant WebPart Visionneuse de rapports. La valeur par défaut est identique à la valeur par défaut du composant WebPart Visionneuse de rapports. La valeur doit être un entier non négatif.  
  
-   *AsyncRender*: spécifie si un rapport doit être ou non généré de façon asynchrone. La valeur par défaut est **true**; cette valeur indique un rendu de rapport asynchrone. La valeur doit être une valeur booléenne **true** ou **false**.  
  
-   *ParamMode*: contrôle la manière dont est affichée la zone de message de paramètre du composant WebPart Visionneuse de rapports en mode pleine page. La valeur par défaut est **Full**. Les valeurs valides sont :  
  
    -   **Full**: afficher la zone de message du paramètre.  
  
    -   **Collapsed**: réduire la zone de message du paramètre.  
  
    -   **Hidden**: masquer la zone de message du paramètre.  
  
     Par exemple, en mode **SharePoint** , pour réduire la zone de message du paramètre.  
  
    ```  
    http://myspsite/_vti_bin/reportserver?http://myspsite002%fShared+Documents%2fmyreport.rdl&rv:DocMapMode=Displayed&rv:ParamMode=Collapsed  
    ```  
  
-   *DocMapMode*: contrôle la manière dont est affichée la zone de l’explorateur de documents du composant WebPart Visionneuse de rapports en mode pleine page. La valeur par défaut est **Full**. Les valeurs valides sont :  
  
    -   **Full**: afficher la zone d'explorateur de documents.  
  
    -   **Collapsed**: réduire la zone d'explorateur de documents.  
  
    -   **Hidden**: masquer la zone d'explorateur de documents.  
  
-   *DockToolBar*: détermine si la barre d’outils du composant WebPart Visionneuse de rapports est ancrée en haut ou en bas. Les valeurs possibles sont **Top** et **Bottom**. La valeur par défaut est **Top**.  
  
     Par exemple, en mode **SharePoint** , pour ancrer la barre d'outils dans la partie inférieure.  
  
    ```  
    http://myspsite/_vti_bin/reportserver?http://myspsite002%fShared+Documents%2fmyreport.rdl&rv:DocMapMode=Displayed&rv:DockToolBar=Bottom  
    ```  
  
-   *ToolBarItemsDisplayMode*: détermine les éléments de la barre d’outils à afficher. Il s'agit d'une valeur d'énumération de bits. Pour inclure un élément de la barre d'outils, ajoutez la valeur de l'élément à la valeur totale. Par exemple : pour aucun menu Actions, utilisez rv : ToolBarItemsDisplayMode=63 (ou 0x3F), qui est 1+2+4+8+16+32 ; pour les éléments du menu Actions uniquement, utilisez rv : ToolBarItemsDisplayMode=960 (ou 0x3C0). La valeur par défaut, qui inclut tous les éléments de la barre d’outils, est **-1**. Les valeurs valides sont :  
  
    -   1 (0x1) : le bouton **Retour**  
  
    -   2 (0x2) : les contrôles de recherche de texte  
  
    -   4 (0x4) : les contrôles de navigation entre les pages  
  
    -   8 (0x8) : le bouton **Actualiser**  
  
    -   16 (0x10) : la zone de liste **Zoom**  
  
    -   32 (0x20) : le bouton **Flux Atom**  
  
    -   64 (0x40) : l’option du menu **Imprimer** dans **Actions**  
  
    -   128 (0x80) : le sous-menu **Exporter** dans **Actions**  
  
    -   256 (0x100) : l’option du menu **Ouvrir avec le Générateur de rapports** dans **Actions**  
  
    -   512 (0x200) : l’option du menu **S’abonner** dans **Actions**  
  
    -   1024 (0x400) : l’option du menu **Nouvelle alerte de données** dans **Actions**  
  
     Par exemple, en mode **SharePoint** , pour afficher uniquement le bouton **Précédent** , les contrôles de recherche de texte, les contrôles de navigation entre les pages et le bouton **Actualiser** .  
  
    ```  
    http://myspsite/_vti_bin/reportserver?http://myspsite002%fShared+Documents%2fmyreport.rdl&rv:DocMapMode=Displayed&rv:ToolBarItemsDisplayMode=15  
    ```  
  
## <a name="see-also"></a> Voir aussi  
 [Accès URL &#40;SSRS&#41;](../reporting-services/url-access-ssrs.md)   
 [Exporter un rapport à l’aide de l’accès URL](../reporting-services/export-a-report-using-url-access.md)  
  
  
