---
title: 'Procédure : déployer une extension pour le traitement des données sur un serveur de rapports | Microsoft Docs'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: extensions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- data processing extensions [Reporting Services], deploying
- assemblies [Reporting Services], data processing extension deployments
ms.assetid: e00dface-70f8-434b-9763-8ebee18737d2
caps.latest.revision: 45
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 3d34cbbe50726165b06dcf8d16e6ec88383b689d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deploying-a-data-processing-extension-to-a-report-server"></a>Déploiement d’une extension pour le traitement des données sur un serveur de rapports
  Les serveurs de rapports utilisent les extensions de traitement des données pour extraire, puis traiter les données qui figurent dans les rapports rendus. Vous devez déployer l'assembly d'extension utilisé pour le traitement des données sur un serveur de rapports, et ce sous la forme d'un assembly privé. Vous devez également créer une entrée dans le fichier de configuration du serveur de rapports, à savoir dans le fichier dénommé RSReportServer.config.  
  
## <a name="procedures"></a>Procédures  
  
#### <a name="to-deploy-a-data-processing-extension-assembly"></a>Pour déployer un assembly d'extension pour le traitement des données  
  
1.  Copiez l'assembly en question depuis son emplacement vers le répertoire bin du serveur de rapports sur lequel l'extension pour le traitement des données doit être utilisée. L’emplacement par défaut du répertoire bin du serveur de rapports est le suivant : %ProgramFiles%\Microsoft SQL Server\MSRS10_50.\<*nom_instance*>\Reporting Services\ReportServer\bin.  
  
    > [!NOTE]  
    >  Cette étape permet d'éviter la mise à niveau vers une instance de SQL Server plus récente. Pour plus d'informations, consultez [Upgrade and Migrate Reporting Services](../../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).  
  
2.  Une fois le fichier correspondant à l'assembly copié, ouvrez le fichier RSReportServer.config. Ce fichier se trouve dans le répertoire ReportServer. Dans le fichier de configuration, créez une entrée correspondant au fichier d'assembly copié. Vous pouvez ouvrir le fichier de configuration dans Visual Studio ou simplement à l’aide d’un éditeur de texte, tel que le Bloc-notes.  
  
3.  Localisez l’élément **Data** dans le fichier RSReportServer.config. L'entrée correspondant à votre nouvelle extension pour le traitement des données doit être créée à l'emplacement suivant :  
  
    ```  
    <Extensions>  
       <Data>  
          <Your extension configuration information goes here>  
       </Data>  
    </Extensions>  
    ```  
  
4.  Ajoutez une entrée correspondant à votre nouvelle extension pour le traitement des données. Celle-ci doit comporter un élément **Extension** dont les valeurs **Name** et **Type** doivent être définies. Cette entrée peut se présenter comme suit :  
  
    ```  
    <Extension Name="ExtensionName" Type="CompanyName.ExtensionName.MyConnectionClass, MyExtensionAssembly" />  
    ```  
  
     La valeur **Name** doit correspondre au nom unique de l’extension utilisée pour le traitement des données. La valeur **Type** est une liste séparée par des virgules comportant une entrée dans laquelle doit figurer l’espace de noms complet de la classe qui implémente les interfaces <xref:Microsoft.ReportingServices.Interfaces.IExtension> et <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection>, suivi du nom de votre assembly (l’extension de fichier .dll ne doit pas figurer dans cette entrée). Par défaut, les extensions utilisées pour le traitement des données sont visibles par les utilisateurs finaux. Pour les masquer des interfaces utilisateur, comme le Gestionnaires de rapports, ajoutez un attribut **Visible** à l'élément **Extension** , et affectez-lui la valeur **false**.  
  
5.  Vous devez définir un groupe de codes pour votre assembly personnalisé octroyant l’autorisation **FullTrust** à votre extension. Pour ce faire, vous devez ajouter le groupe de codes au fichier rssrvpolicy.config qui se trouve par défaut dans le répertoire suivant : %ProgramFiles%\Microsoft SQL Server\\<MSRS10_50.\<*nom_instance*>\Reporting Services\ReportServer. Ce groupe de codes peut se présenter comme suit :  
  
    ```  
    <CodeGroup class="UnionCodeGroup"  
       version="1"  
       PermissionSetName="FullTrust"  
       Name="MyExtensionCodeGroup"  
       Description="Code group for my data processing extension">  
          <IMembershipCondition class="UrlMembershipCondition"  
             version="1"  
             Url="C:\Program Files\Microsoft SQL Server\MSRS10_50.<Instance Name>\Reporting Services\ReportServer\bin\MyExtensionAssembly.dll"  
           />  
    </CodeGroup>  
    ```  
  
 L'appartenance URL n'est qu'une des nombreuses conditions d'appartenance que vous pouvez sélectionner pour l'extension permettant le traitement des données. Pour plus d’informations sur la sécurité d’accès du code dans [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], consultez [Développement sécurisé &#40;Reporting Services&#41;](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md).  
  
## <a name="verifying-the-deployment"></a>Vérification du déploiement  
 Si vous le souhaitez, vous pouvez vous assurer que votre extension pour le traitement des données a été correctement déployée sur le serveur de rapports sélectionné en utilisant la méthode de service Web <xref:ReportService2010.ReportingService2010.ListExtensions%2A>. À cette même fin, vous pouvez également ouvrir le Gestionnaire de rapports, puis vérifier que votre extension est effectivement répertoriée dans la liste de sources de données qu'il contient. Pour plus d’informations sur le Gestionnaire de rapports et les sources de données, consultez [Créer, modifier, puis supprimer des sources de données partagées &#40;SSRS&#41;](../../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Déploiement d’une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)   
 [Extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implémentation d’une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
