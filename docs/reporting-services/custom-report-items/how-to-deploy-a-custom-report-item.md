---
title: 'Procédure : déployer un élément de rapport personnalisé | Microsoft Docs'
ms.custom: ''
ms.date: 03/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: custom-report-items
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- custom report items, deploying
ms.assetid: 80e97b0d-e355-4240-aebd-08cbc84089ed
caps.latest.revision: 26
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 2d4bebf1490bb2da15486fce177932cb1aecf5c7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="how-to-deploy-a-custom-report-item"></a>Procédure : déployer un élément de rapport personnalisé
  Pour déployer un élément de rapport personnalisé dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vous devez d'abord modifier les fichiers de configuration du serveur de rapports, puis copier les assemblys correspondant aux composants de conception et d'exécution dans les dossiers d'application appropriés, et ce à la fois pour le Concepteur de rapports et le serveur de rapports.  
  
### <a name="to-deploy-a-custom-report-item"></a>Pour déployer un élément de rapport personnalisé  
  
1.  Modifiez le fichier Rsreportdesigner.config afin de configurer les composants de conception et d'exécution de l'élément de rapport personnalisé à utiliser dans le concepteur. Notez que l’entrée **ReportItemName** doit correspondre à l’attribut **CustomReportItemAttribute** utilisé dans votre classe **CustomReportItemDesigner**. Exemple :  
  
    ```  
    <ReportItems>  
       <ReportItem Name="Polygons" Type="PolygonsCRI.PolygonsCRI,PolygonsCRI"/>  
    </ReportItems>  
    <ReportItemDesigner>  
       <ReportItem Name="Polygons" Type="PolygonsCRI.PolygonsDesigner, PolygonsDesigner" />  
    </ReportItemDesigner>  
    <ReportItemConverter>  
       <Converter Source="Chart" Target="Polygons" Type="PolygonsCRI.PolygonsConverter, PolygonsDesigner" />  
    </ReportItemConverter>  
    ```  
  
2.  Modifiez le fichier Rsreportserver.config pour inscrire le composant d'exécution de l'élément de rapport personnalisé. Exemple :  
  
    ```  
    <ReportItems>  
       <ReportItem Name="Polygons" Type="PolygonsCRI.PolygonsCRI,PolygonsCRI"/>  
    </ReportItems>  
    ```  
  
3.  Modifiez le fichier Rssrvpolicy.config pour ajouter un **CodeGroup** octroyant les autorisations adéquates à l’élément de rapport personnalisé. Exemple :  
  
    ```  
    <CodeGroup   
       class="UnionCodeGroup"   
       version="1"   
       PermissionSetName="FullTrust"  
       Description="This code group grants MyCustomReportItem.dll FullTrust permission. ">  
       <IMembershipCondition   
          class="UrlMembershipCondition"  
          version="1"  
       Url="C:\Program Files\Microsoft SQL Server\ MSRS10_50.SQLSERVER\Reporting Services\ReportServer\bin\MyCustomReportItem.dll" />  
    </CodeGroup>  
    ```  
  
4.  Copiez le fichier DLL correspondant au composant d'exécution de l'élément de rapport personnalisé dans les répertoires bin du serveur de rapports qui se trouvent à l'emplacement suivant : %ProgramFiles%\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies et \Program Files\Microsoft SQL Server\MSRS10_50.SQLSERVER\Reporting Services\ReportServer\bin.  
  
5.  Copiez le fichier DLL correspondant au composant de conception de l'élément de rapport personnalisé dans le répertoire PrivateAssemblies, lequel se trouve à l'emplacement suivant : %ProgramFiles%\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies.  
  
## <a name="see-also"></a> Voir aussi  
 [Fichiers de configuration de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Bibliothèques de classes d'éléments de rapports personnalisés](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)  
  
  
