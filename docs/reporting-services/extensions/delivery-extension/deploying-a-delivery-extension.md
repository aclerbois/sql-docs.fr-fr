---
title: Déploiement d’une extension de remise | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
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
- delivery extensions [Reporting Services], deploying
- Extension element
- deploying [Reporting Services], extensions
ms.assetid: 4436ce48-397d-42c7-9b5d-2a267e2a1b2c
caps.latest.revision: 45
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 99ada408d4f2a783d2a545d00f780763b7a27796
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deploying-a-delivery-extension"></a>Déploiement d'une extension de remise
  Les extensions de remise fournissent leurs informations de configuration sous la forme d'un fichier de configuration XML. Le fichier XML est conforme au schéma XML défini pour les extensions de remise. Les extensions de remise fournissent l'infrastructure nécessaire pour définir et modifier le fichier de configuration.  
  
 Si une extension de remise est remplacée ou mise à niveau, tous les abonnements qui référencent cette extension restent valides.  
  
 Après avoir écrit et compilé votre extension de remise [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] dans une bibliothèque [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], vous devez la copier dans le répertoire approprié et ajouter une entrée au fichier de configuration [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] approprié afin que le serveur de rapports puisse la trouver.  
  
## <a name="configuration-file-extension-element"></a>Élément Extension du fichier de configuration  
 Les extensions de remise que vous déployez sur le serveur de rapports doivent être entrées sous la forme d’éléments **Extension** dans le fichier de configuration. Le fichier de configuration du serveur de rapports est RSReportServer.config.  
  
 Le tableau suivant décrit les attributs de l’élément **Extension** pour les extensions de remise.  
  
|Attribute|Description|  
|---------------|-----------------|  
|**Nom**|Nom unique de l'extension (par exemple, « Messagerie électronique du serveur de rapports » pour l'extension de remise par messagerie ou « Partage de fichiers du serveur de rapports » pour l'extension de remise par partage de fichiers). La longueur maximale de l'attribut **Name** est de 255 caractères. Le nom doit être unique au sein de toutes les entrées de l’élément **Extension** d’un fichier de configuration. Si un nom existe en double, le serveur de rapports retourne une erreur.|  
|**Type**|Liste séparée par des virgules qui inclut l'espace de noms complet, ainsi que le nom de l'assembly.|  
|**Visible**|La valeur **false** indique que l’extension de remise ne doit pas être visible dans les interfaces utilisateur. Si cet attribut n'est pas défini, la valeur par défaut est **true**.|  
  
 Pour plus d’informations sur le fichier RSReportServer.config, consultez [Fichiers de configuration de Reporting Services](../../../reporting-services/report-server/reporting-services-configuration-files.md).  
  
## <a name="deploying-the-extension-to-the-report-server"></a>Déploiement de l'extension sur le serveur de rapports  
 Le serveur de rapports utilise des extensions de remise pour traiter et remettre des notifications ou des rapports. Vous devez déployer l'assembly d'extension de remise sur le serveur de rapports sous la forme d'un assembly privé. Vous devez également créer une entrée dans le fichier de configuration du serveur de rapports, à savoir dans le fichier dénommé RSReportServer.config.  
  
#### <a name="to-deploy-a-deliver-extension-assembly-to-a-report-server"></a>Pour déployer un assembly d'extension de remise sur un serveur de rapports  
  
1.  Copiez l'assembly depuis son emplacement vers le répertoire bin du serveur de rapports sur lequel l'extension de remise doit être utilisée. L’emplacement par défaut du répertoire Bin du serveur de rapports est le suivant : %ProgramFiles%\Microsoft SQL Server\MSRS13.\<nom_instance>\Reporting Services\ReportServer\bin.  
  
    > [!IMPORTANT]  
    >  Si vous essayez de remplacer un assembly d'extension de remise existant, vous devez commencer par arrêter le service Report Server avant de copier l'assembly mis à jour. Redémarrez le service une fois l'assembly copié.  
  
2.  Une fois le fichier correspondant à l'assembly copié, ouvrez le fichier RSReportServer.config. Le fichier RSReportServer.config est situé dans le répertoire %ProgramFiles%\Microsoft SQL Server\MSRS13.\<nom_instance>\Reporting Services\ReportServer. Vous devez créer une entrée pour le fichier d'assembly d'extension de remise dans le fichier de configuration. Vous pouvez ouvrir le fichier de configuration à l’aide de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] ou à l’aide d’un simple éditeur de texte, tel que le Bloc-notes.  
  
3.  Localisez l’élément **Delivery** dans le fichier RSReportServer.config. Une entrée correspondant à votre nouvelle extension de remise doit être créée à l'emplacement suivant :  
  
    ```  
    <Extensions>  
       <Delivery>  
          <Your extension configuration information goes here>  
       </Delivery>  
    </Extensions>  
    ```  
  
4.  Ajoutez une entrée pour votre extension de remise. Celle-ci doit comporter un élément **Extension** dont les valeurs **Name** et **Type** doivent être définies. Cette entrée peut se présenter comme suit :  
  
    ```  
    <Extension Name="My Delivery Extension Name" Type="CompanyName.ExtensionName.MyDeliveryExtensionClass, AssemblyName" />  
    ```  
  
     La valeur définie pour **Name** correspond au nom unique de l’extension de remise. La valeur définie pour **Type** est une liste séparée par des virgules comportant une entrée pour l’espace de noms complet de la classe qui implémente l’interface <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension>, suivie du nom de votre assembly (sans l’extension de fichier .dll). Par défaut, les extensions de remise sont visibles. Pour les masquer des interfaces utilisateur, comme le portail web, ajoutez un attribut **Visible** à l’élément **Extension**, et affectez-lui la valeur **false**.  
  
5.  Enfin, vous devez définir un groupe de codes pour votre assembly personnalisé octroyant l’autorisation **FullTrust** à votre extension de remise. Pour ce faire, vous devez ajouter le groupe de codes au fichier rssrvpolicy.config qui se trouve par défaut dans le répertoire suivant : %ProgramFiles%\Microsoft SQL Server\MSRS13.\<nom_instance>\Reporting Services\ReportServer. Ce groupe de codes peut se présenter comme suit :  
  
    ```  
    <CodeGroup class="UnionCodeGroup"  
       version="1"  
       PermissionSetName="FullTrust"  
       Name="MyExtensionCodeGroup"  
       Description="Code group for my delivery extension">  
          <IMembershipCondition class="UrlMembershipCondition"  
             version="1"  
             Url="C:\Program Files\Microsoft SQL Server\MSRS13.<InstanceName>\Reporting Services\ReportServer\bin\MyExtensionAssembly.dll"  
           />  
    </CodeGroup>  
    ```  
  
     L'appartenance URL n'est qu'une des nombreuses conditions d'appartenance que vous pouvez sélectionner pour l'extension de remise. Pour plus d’informations sur la sécurité d’accès du code dans [!INCLUDE[ssRS](../../../includes/ssrs-md.md)], consultez [Développement sécurisé &#40;Reporting Services&#41;](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md).  
   
## <a name="verifying-the-deployment"></a>Vérification du déploiement  
 Vous pouvez vérifier que votre extension de remise a été correctement déployée sur le serveur de rapports en utilisant la méthode <xref:ReportService2010.ReportingService2010.ListExtensions%2A> du service Web. Vous pouvez également ouvrir le portail web et vérifier que votre extension est effectivement répertoriée dans la liste des extensions de remise disponibles pour un abonnement. Pour plus d’informations sur le portail web et les abonnements, consultez [Abonnements et remise &#40;Reporting Services&#41;](../../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Implémentation d’une extension de remise](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
