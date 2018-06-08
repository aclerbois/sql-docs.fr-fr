---
title: Bien démarrer avec le contrôle ReportViewer 2016 | Microsoft Docs
ms.custom: ''
ms.date: 06/12/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: application-integration
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 01a821c4-2920-400c-be03-93d26c749bb1
caps.latest.revision: 12
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 1395b2351ebba9aa1d805b651640593942aaa9cd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integrating-reporting-services-using-reportviewer-controls---get-started"></a>Intégration de Reporting Services à l’aide de contrôles ReportViewer - Bien démarrer

Découvrez comment les développeurs peuvent incorporer des rapports paginés dans des sites web ASP.NET et des applications Windows Forms par le biais du contrôle ReportViewer Reporting Services 2016. Vous pouvez ajouter le contrôle à un nouveau projet, ou mettre à jour un projet existant.

## <a name="adding-the-reportviewer-control-to-a-new-web-project"></a>Ajout du contrôle ReportViewer à un nouveau projet web

1. Créez un **site web ASP.NET vide** ou ouvrez un projet ASP.NET existant.

    ![ssRS-Create-New-ASPNET-Project](../../reporting-services/application-integration/media/ssrs-create-new-aspnet-project.png)

2. Installez le package nuget du contrôle ReportViewer 2016 par le biais de la **console du gestionnaire de package Nuget**.

    ```
    Install-Package Microsoft.ReportingServices.ReportViewerControl.WebForms
    ```
3. Ajoutez une nouvelle page .aspx au projet et inscrivez l’assembly du contrôle ReportViewer en vue de l’utiliser dans la page.

    ```
    <%@ Register assembly="Microsoft.ReportViewer.WebForms, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" namespace="Microsoft.Reporting.WebForms" tagprefix="rsweb" %>
    ```
    
4. Ajoutez un **ScriptManagerControl** à la page.

5. Ajoutez le contrôle ReportViewer à la page. L’extrait de code ci-dessous peut être mis à jour pour référencer un rapport hébergé sur un serveur de rapports à distance.

    ```
    <rsweb:ReportViewer ID="ReportViewer1" runat="server" ProcessingMode="Remote">
      <ServerReport ReportPath="" ReportServerUrl="" />
    </rsweb:ReportViewer>
    ```
    
La page finale devrait ressembler à ce qui suit.

```
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="WebForm1.aspx.cs" Inherits="Sample" %>

<%@ Register assembly="Microsoft.ReportViewer.WebForms, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" namespace="Microsoft.Reporting.WebForms" tagprefix="rsweb" %>

<!DOCTYPE html>

<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <meta http-equiv="X-UA-Compatible" content="IE=edge" /> 
    <title></title>
</head>
<body>
    <form id="form1" runat="server">
    <asp:ScriptManager runat="server"></asp:ScriptManager>        
        <rsweb:ReportViewer ID="ReportViewer1" runat="server" ProcessingMode="Remote">
            <ServerReport ReportServerUrl="http://AContosoDepartment/ReportServer" ReportPath="/LatestSales" />
        </rsweb:ReportViewer>
    </form>
</body>
</html>

```

## <a name="updating-an-existing-project-to-use-the-reportviewer-control"></a>Mise à jour d’un projet existant pour utiliser le contrôle ReportViewer

Pour utiliser le contrôle ReportViewer 2016 dans un projet existant, ajoutez le contrôle par le biais de Nuget et mettez à jour les références d’assembly vers la version *14.0.0.0*. Cette opération entraîne la mise à jour du fichier web.config du projet et de toutes les pages .aspx qui référencent le contrôle ReportViewer.

### <a name="sample-webconfig-changes"></a>Exemple de modifications apportées au fichier web.config

```
<?xml version="1.0"?>
<!--
  For more information on how to configure your ASP.NET application, please visit
  http://go.microsoft.com/fwlink/?LinkId=169433
  -->
<configuration>
  <system.web>
    <compilation debug="true" targetFramework="4.5.2">
      <assemblies>
        <!-- All assemblies updated to version 14.0.0.0. -->
        <add assembly="Microsoft.ReportViewer.Common, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.DataVisualization, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.Design, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.ProcessingObjectModel, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.WebDesign, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.WebForms, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
        <add assembly="Microsoft.ReportViewer.WinForms, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
      </assemblies>
      <buildProviders>
        <!-- Version updated to 14.0.0.0. -->
        <add extension=".rdlc"
          type="Microsoft.Reporting.RdlBuildProvider, Microsoft.ReportViewer.WebForms, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
      </buildProviders>
    </compilation>
    <httpRuntime targetFramework="4.5.2"/>
    <httpHandlers>
      <!-- Version updated to 14.0.0.0 -->
      <add path="Reserved.ReportViewerWebControl.axd" verb="*"
        type="Microsoft.Reporting.WebForms.HttpHandler, Microsoft.ReportViewer.WebForms, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"
        validate="false"/>
    </httpHandlers>
  </system.web>
  <system.webServer>
    <validation validateIntegratedModeConfiguration="false"/>
    <modules runAllManagedModulesForAllRequests="true"/>
    <handlers>
      <!-- Version updated to 14.0.0.0 -->
      <add name="ReportViewerWebControlHandler" verb="*" path="Reserved.ReportViewerWebControl.axd" preCondition="integratedMode"
        type="Microsoft.Reporting.WebForms.HttpHandler, Microsoft.ReportViewer.WebForms, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845DCD8080CC91"/>
    </handlers>
  </system.webServer>
</configuration>
```

### <a name="sample-aspx"></a>Exemple de fichier .aspx

```
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="WebForm1.aspx.cs" Inherits="SampleAspx" %>

<!-- Update version to 14.0.0.0 -->
<%@ Register assembly="Microsoft.ReportViewer.WebForms, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" namespace="Microsoft.Reporting.WebForms" tagprefix="rsweb" %>

<!DOCTYPE html>
```

## <a name="adding-the-reportviewer-control-to-a-new-windows-forms-project"></a>Ajout du contrôle ReportViewer à un nouveau projet Windows Forms

1. Créez une **application Windows Forms** ou ouvrez un projet existant.

    ![ssRS-Create-New-winforms-Project](../../reporting-services/application-integration/media/ssrs-create-new-winforms-project.png)

2. Installez le package nuget du contrôle ReportViewer 2016 par le biais de la **console du gestionnaire de package Nuget**.

    ```
    Install-Package Microsoft.ReportingServices.ReportViewerControl.WinForms
    ```
3. Ajoutez un nouveau contrôle à partir du code ou [ajoutez le contrôle à la boîte à outils](##adding-control-to-visual-studio-toolbar).

    ```
    private Microsoft.Reporting.WinForms.ReportViewer reportViewer1;
    
    private void InitializeComponent()
    {
        this.reportViewer1 = new Microsoft.Reporting.WinForms.ReportViewer();
        this.SuspendLayout();
        // 
        // reportViewer1
        // 
        this.reportViewer1.Location = new System.Drawing.Point(168, 132);
        this.reportViewer1.Name = "reportViewer1";
        this.reportViewer1.ServerReport.BearerToken = null;
        this.reportViewer1.Size = new System.Drawing.Size(396, 246);
        this.reportViewer1.TabIndex = 0;
        // 
        // Form1
        // 
        this.Controls.Add(this.reportViewer1);
    }
    ```

## <a name="how-to-set-100-height-on-the-report-viewer-2016-control"></a>Comment définir la propriété height sur 100% sur le contrôle ReportViewer 2016

Le nouveau contrôle ReportViewer 2016 est optimisé pour les pages en mode HTML5 standard et fonctionne sur tous les navigateurs modernes. Avant, avec l’ancien contrôle RVC, la définition de la propriété height sur 100% était prise en compte même si cette propriété n’était spécifiée pour aucun des ancêtres. Ce comportement a changé dans HTML5. Quand vous définissez cette propriété sur le nouveau contrôle RVC, l’opération ne fonctionne correctement que si l’élément parent a une hauteur définie, c’est-à-dire une valeur différente d’auto, ou que tous les ancêtres de RVC ont une propriété height définie sur 100%.

Les deux exemples ci-après montrent comment opérer.

### <a name="by-setting-the-height-of-all-the-parent-elements-to-100"></a>En définissant la propriété height de tous les éléments parents sur 100%

```
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <style>
        html,body,form,#div1 {
            height: 100%; 
        }
    </style>
   </head>
<body>
    <form id="form1" runat="server">
    <div id="div1" >
            <asp:ScriptManager runat="server"></asp:ScriptManager>
        <rsweb:ReportViewer ID="ReportViewer1" runat="server" ProcessingMode="Remote" Height="100%" Width="100%">
            <ServerReport ReportServerUrl="http://test/ReportServer" ReportPath="/testreport" />
        </rsweb:ReportViewer>
    </div>
    </form>
</body>
</html>

```

### <a name="by-setting-the-style-height-attribute-on-the-parent-of-the-reportviewer-control"></a>En définissant l’attribut de style height sur le parent du contrôle ReportViewer

Pour plus d’informations sur les longueurs en pourcentage de la fenêtre d’affichage, consultez [Viewport-percentage lengths](https://www.w3.org/TR/css3-values/#viewport-relative-lengths) (Longueurs en pourcentage de la fenêtre d’affichage).

```
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
</head>
<body>
    <form id="form1" runat="server">
    <div style="height:100vh;">
            <asp:ScriptManager runat="server"></asp:ScriptManager>
        <rsweb:ReportViewer ID="ReportViewer1" runat="server" ProcessingMode="Remote" Height="100%" Width="100%">
            <ServerReport ReportServerUrl="http://test/ReportServer" ReportPath="/testreport" />
        </rsweb:ReportViewer>
    </div>
    </form>
</body>
</html>

```

## <a name="adding-control-to-visual-studio-toolbar"></a>Ajout du contrôle à la barre d’outils de Visual Studio

Le contrôle ReportViewer est désormais fourni sous la forme d’un package NuGet. Il n’apparaît donc pas par défaut dans la boîte à outils de Visual Studio. Vous pouvez l’ajouter à la boîte à outils en effectuant la procédure suivante.

1. Installez le package NuGet pour WinForms ou WebForms comme indiqué ci-dessus.

2. Supprimez le contrôle ReportViewer qui est répertorié dans la boîte à outils. Il s’agit du contrôle ayant la version 12.x.

    ![ssRS-remove-old-rvcontrol-toolbox](../../reporting-services/application-integration/media/ssrs-remove-old-rvcontrol-toolbox.png)

3. Cliquez avec le bouton droit n’importe où dans la boîte à outils, puis sélectionnez **Choisir les éléments...**.

    ![ssRS-toolbox-choose-item](../../reporting-services/application-integration/media/ssrs-toolbox-choose-item.png)
    
4. Dans **Composants .NET Framework**, sélectionnez **Parcourir**.

    ![ssRS-toolbox-browse](../../reporting-services/application-integration/media/ssrs-toolbox-browse.png)

5. Sélectionnez **Microsoft.ReportViewer.WinForms.dll** ou **Microsoft.ReportViewer.WebForms.dll** à partir du package NuGet que vous avez installé.

    > [!NOTE] 
    > Le package NuGet est installé dans le répertoire de solution de votre projet. Le chemin de la dll ressemble au suivant : `{Solution Directory}\packages\Microsoft.ReportingServices.ReportViewerControl.Winforms.{version}\lib\net40` ou `{Solution Directory}\packages\Microsoft.ReportingServices.ReportViewerControl.WebForms.{version}\lib\net40`.

6. Le nouveau contrôle doit s’afficher dans la boîte à outils. Vous pouvez ensuite le déplacer vers un autre onglet de la boîte à outils si vous le souhaitez.

    ![ssRS-toolbox-rvcontrol](../../reporting-services/application-integration/media/ssrs-toolbox-rvcontrol.png)

### <a name="things-to-be-aware-of"></a>Éléments à prendre en considération

- Cette opération ajoute une référence au package NuGet installé dans votre projet actuel. L’élément dans la boîte à outils est conservé pour les autres projets. Quand vous installez le package NuGet dans une nouvelle solution/projet, l’élément de la boîte à outils peut référencer une version antérieure. 

- Le contrôle reste dans la boîte à outils même si l’assembly n’est plus disponible. Si ce projet a été supprimé et que vous essayez d’ajouter le contrôle à partir de la boîte à outils, Visual Studio génère une erreur. Pour corriger cette erreur, supprimez le contrôle de la boîte à outils et rajoutez-le en suivant la procédure ci-dessus.


## <a name="common-issues"></a>Problèmes courants
    
- Le contrôle ReportViewer 2016 est conçu pour être utilisé avec les navigateurs modernes. Le contrôle peut ne pas fonctionner si les navigateurs restituent la page web dans un mode de compatibilité Internet Explorer. Les sites intranet peuvent nécessiter une balise meta pour remplacer le paramètre qui incite le rendu des pages intranet en mode de compatibilité.

    ```
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    ```
      
## <a name="providing-feedback"></a>Envoi de commentaires

Informez l’équipe des problèmes que vous rencontrez avec le contrôle sur les [forums MSDN Reporting Services](https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=sqlreportingservices) ou par e-mail à l’adresse [RVCFeedback@microsoft.com](mailto:RVCFeedback@microsoft.com).

## <a name="see-also"></a>Voir aussi

[Collecte de données dans le contrôle ReportingViewer version 2016](../../reporting-services/application-integration/integrating-reporting-services-using-reportviewer-controls-data-collection.md)  
D’autres questions ? [Essayez le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)

