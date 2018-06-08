---
title: Service et applications de service Reporting Services SharePoint| Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
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
ms.openlocfilehash: d1db1758c979ae3582d562c47690e12af72171cc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reporting-services-sharepoint-service-and-service-applications"></a>Service et applications de service Reporting Services SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  Le mode SharePoint de Reporting Services repose sur l’architecture de service SharePoint et utilise un service SharePoint, ainsi qu’une ou plusieurs applications de service. Lorsque vous créez une application de service, le service devient disponible et la base de données d'application de service est générée. Vous pouvez créer plusieurs applications de service Reporting Services mais une application de service est suffisante pour la plupart des scénarios de déploiement.  

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.
  
## <a name="creating-a-reporting-services-service-application"></a>Création d’une application de service Reporting Services

 Vous pouvez utiliser l’Administration centrale de SharePoint ou les scripts PowerShell pour créer les applications de service Reporting Services. Pour plus d’informations sur l’utilisation de l’Administration centrale de SharePoint, consultez la section « Créez une nouvelle application de service Reporting Services » dans [Installer le mode SharePoint de Reporting Services pour SharePoint 2010](http://msdn.microsoft.com/47efa72e-1735-4387-8485-f8994fb08c8c). Consultez la section PowerShell plus loin dans cette rubrique pour obtenir un exemple de script PowerShell permettant de créer des applications de service.  
  
## <a name="modify-the-associations-of-the-service-application-with-a-proxy-group"></a>Modifier les associations de l’application de service avec un groupe de proxy

 La nouvelle page de création d'une application de service contient la section **Association d'application Web**. Cette section vous permet d'associer l'application de service que vous créez. Utilisez la procédure suivante pour modifier l'association et assigner une configuration personnalisée à l'application de service. Le même processus général peut également être utilisé pour ajouter le proxy au groupe par défaut plutôt que modifier l'association à un groupe personnalisé de l'application de service.  
  
1.  Dans l'Administration Centrale de SharePoint, sous Gestion des applications, cliquez sur **Configurer les associations des applications de service**.  
  
2.  Dans la page des Associations de l'application de service, modifiez la vue en **Applications de service**.  
  
3.  Recherchez et cliquez sur le nom de votre nouvelle application de service Reporting Services. Vous pourriez cliquer également sur la **valeur par défaut** du nom de groupe du proxy de l'application pour ajouter le proxy au groupe par défaut plutôt que compléter les étapes suivantes.  
  
4.  Sélectionnez **Personnalisé** dans la zone de sélection **Modifier le groupe de connexions suivant :**.  
  
5.  Activez la zone pour votre proxy et cliquez sur **OK**.  
  
## <a name="edit-service-application-properties"></a>Modifier les propriétés d’une application de service

 Vous pouvez rouvrir la page de propriétés de l'application de service pour modifier les propriétés.  
  
1.  Dans l'Administration Centrale de SharePoint, sous le groupe Gestion des applications, cliquez sur **Gérer les applications de service**.  
  
2.  Sélectionnez l'application de service en cliquant sur la colonne de type pour sélectionner la ligne entière. Si vous cliquez sur le nom de l'application, la page des options de gestion du service s'affiche au lieu des propriétés de l'application de service.  
  
3.  Dans le ruban Applications de service, cliquez sur **Propriétés**.  
  
## <a name="create-a-reporting-services-service-application-using-powershell"></a>Créer une application de service Reporting Services à l’aide de PowerShell

 Vous pouvez utiliser PowerShell pour créer l'application de service et le proxy. L'exemple suivant suppose que vous connaissez le pool d'applications qui sera utilisé par l'application de service.  
  
1.  Ajoutez l'objet de pool d'applications de votre nom de pool d'applications à une variable qui est passée dans l'action New.  
  
    ```  
    $appPoolName = get-spserviceapplicationpool “<application pool name>”  
    ```  
  
2.  Créez l'application de service en lui attribuant le nom et le nom de pool d'applications que vous fournissez.  
  
    ```  
    New-SPRSServiceApplication –Name ‘MyServiceApplication’ –ApplicationPool $appPoolName –DatabaseName ‘MyServiceApplicationDatabase’ –DatabaseServer ‘<Server Name>’  
    ```  
  
3.  Obtenez le nouvel objet d'application de service et dirigez l'objet dans le canal du nouvel applet de commande de proxy.  
  
    ```  
    Get-SPRSServiceApplication –name MyServiceApplication | New-SPRSServiceApplicationProxy “MyServiceApplicationProxy”  
    ```  
  
## <a name="related-tasks"></a>Tâches associées
  
|Tâche|Lien|  
|----------|----------|  
|Gérez les paramètres de votre application de service.|[Gérer une application de service SharePoint Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)|  
|Sauvegardez et restaurez l'application de service et ses composants connexes tels que les clés de chiffrement et le proxy.|[Applications de service SharePoint de sauvegarde et de restauration Reporting Services](../../reporting-services/report-server-sharepoint/backup-and-restore-reporting-services-sharepoint-service-applications.md)|  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
