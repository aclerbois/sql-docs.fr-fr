---
title: Sauvegarder et restaurer des applications de service Reporting Services SharePoint | Microsoft Docs
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
ms.openlocfilehash: 09da0857743d49cb90a074c95190dd8809e7efbb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="back-up-and-restore-reporting-services-sharepoint-service-applications"></a>Sauvegarder et restaurer des applications de service Reporting Services SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Cette rubrique explique comment sauvegarder et restaurer des applications de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à l’aide de l’Administration centrale de SharePoint ou de PowerShell.

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

## <a name="before-you-begin"></a>Avant de commencer

### <a name="limitations-and-restrictions"></a>Limitations et restrictions

> [!NOTE]
>  Les applications de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peuvent être partiellement sauvegardées et restaurées à l’aide de la fonctionnalité SharePoint de sauvegarde et de restauration. **Des étapes supplémentaires sont nécessaires** ; celles-ci sont documentées dans cette rubrique. Actuellement, le processus de sauvegarde **ne permet pas** de sauvegarder les clés de chiffrement et les informations d’identification pour les comptes d’exécution sans assistance (UEA) ou l’authentification Windows à la base de données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].

### <a name="recommendations"></a>Recommandations
  
-   Sauvegardez les clés de chiffrement avant de démarrer la sauvegarde SharePoint. Si vous ne sauvegardez pas les clés de chiffrement, vous ne pourrez pas accéder à vos données chiffrées après la restauration de l’application de service. Vous devrez alors les supprimer.  
  
-   Vérifiez si votre application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise un compte UEA ou l'authentification Windows pour l'accès aux bases de données. Si elle utilise l'un ou l'autre, vérifiez quelles sont les informations d'identification appropriées pour que vous puissiez correctement configurer l'application de service après le processus de restauration.  
  
-   Vérifiez que le journal de sauvegarde SharePoint est créé dans le même dossier que le fichier de sauvegarde. Le fichier est généralement nommé **spbackup.log**.  
  
## <a name="back-up-the-service-application"></a>Sauvegardez l’application de service.

 Exécutez les étapes suivantes dans l'ordre :  
  
1.  Sauvegardez les clés de chiffrement.  
  
2.  Sauvegardez l’application de service.  
  
3.  Vérifiez si votre application de service utilise un compte UEA ou l'authentification Windows pour l'accès aux bases de données. Si c'est le cas, notez les informations d'identification afin de pouvoir les utiliser pour configurer l'application de service après sa restauration.  

### <a name="back-up-the-encryption-keys-using-sharepoint-central-administration"></a>Sauvegarder les clés de chiffrement à l’aide de l’Administration centrale de SharePoint

Pour plus d’informations sur la sauvegarde des clés de chiffrement [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , consultez la section « Clés de chiffrement » dans [Gérer une application de service SharePoint Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).  

### <a name="back-up-the-service-application-using-sharepoint-central-administration"></a>Sauvegarder l’application de service à l’aide de l’Administration centrale de SharePoint

Pour sauvegarder l'application de service, procédez comme suit :  
  
1.  Dans l’Administration centrale de SharePoint, sélectionnez **Effectuer une sauvegarde** dans le groupe **Sauvegarde et restauration**.  
  
2.  Sous le nœud **Services partagés** , développez **Applications de services partagés** et sélectionnez votre application de service. Elle sera du type **Application de service SQL Server Reporting Services**.  
  
3.  Sélectionnez **Suivant**.  
  
4.  Tapez un chemin dans **Emplacement de sauvegarde** et sélectionnez **Démarrer la sauvegarde**.  
  
5.  Répétez le processus ci-dessus mais au lieu de sélectionner l'application de service, développez le nœud **Proxys de services partagés** , puis sélectionnez le proxy de l'application de service. Elle sera du type **Proxy d'application de service SQL Server Reporting Services**.  
  
 Pour plus d'informations, consultez les rubriques suivantes dans la documentation de SharePoint :  
  
 [Enregistrer une application de service (SharePoint Foundation 2010) dans la documentation SharePoint](http://msdn.microsoft.com/library/ee748601.aspx).  
  
 [Enregistrer une application de service (SharePoint Server 2010)](http://technet.microsoft.com/library/ee428318.aspx)  
  
### <a name="verify-execution-account-and-database-authentication"></a>Vérifier le compte d’exécution et l’authentification de la base de données

 **Compte d'exécution :** pour vérifier si votre application de service utilise un compte d'exécution :  
  
1.  Dans l’Administration centrale de SharePoint, dans le groupe **Gestion des applications**, sélectionnez **Gérer les applications de service**.  
  
2.  Sélectionnez le nom de votre application de service, puis sélectionnez **Gérer** dans le ruban SharePoint.  
  
3.  Sélectionnez **Compte d’exécution**.  
  
4.  Si un compte d'exécution est configuré, vous devez connaître les informations d'identification lorsqu'il est temps de restaurer la sauvegarde de l'application de service. N'effectuez pas la procédure de sauvegarde et restauration avant de connaître les informations d'identification correctes.  
  
 **Authentification de base de données :** pour vérifier si votre application de service utilise l'authentification Windows pour l'authentification de base de données :  
  
1.  Dans l’Administration centrale de SharePoint, dans le groupe **Gestion des applications**, sélectionnez **Gérer les applications de service**.  
  
2.  Sélectionnez le nom de votre application de service, puis sélectionnez **Propriétés** dans le ruban SharePoint.  
  
3.  Consultez la section **Base de données de service SQL Server Reporting Services (SSRS)** .  
  
4.  Si l'authentification Windows est configurée, vous devez connaître les informations d'identification pour que vous puissiez configurer l'application de service après l'avoir restaurée. N'effectuez pas la procédure de sauvegarde et restauration avant de connaître les informations d'identification correctes.  
  
## <a name="restore-the-service-application"></a>Restaurer l’application de service

 Exécutez les étapes suivantes dans l'ordre :  
  
1.  Restaurez l'application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
2.  Restaurer les clés de chiffrement  
  
3.  Si votre application de service utilisait un compte d'exécution ou une authentification Windows pour l'accès aux bases de données, configurez les informations d'identification.  
  
### <a name="restore-the-service-application-using-sharepoint-central-administration"></a>Restaurer l’application de service à l’aide de l’Administration centrale de SharePoint
  
1.  Dans l’Administration centrale de SharePoint, sélectionnez **Restauration à partir d’une sauvegarde** dans le groupe **Sauvegarde et restauration**.  
  
2.  Tapez le chemin de votre fichier de sauvegarde dans la zone **Emplacement de l’historique de sauvegarde**, puis sélectionnez **Actualiser**.  
  
3.  Sélectionnez la sauvegarde de votre application de service dans la liste **Composant de niveau supérieur**, puis sélectionnez **Suivant**.  
  
4.  Sélectionnez votre application [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], puis sélectionnez **Suivant**.  
  
5.  Dans la section **Noms de connexion et mots de passe** , entrez le mot de passe pour le nom de connexion. La zone de nom de connexion doit être renseignée avec le nom de connexion utilisé par l’application de service avant la sauvegarde.  
  
6.  Sélectionnez **Démarrer la restauration**.  
  
7.  Répétez le processus ci-dessus mais au lieu de restaurer l'application de service, développez le nœud **Services partagés** , puis développez le nœud **Applications de services partagés** .  
  
 Pour plus d'informations, consultez les rubriques suivantes dans la documentation de SharePoint :  
  
 [Restaurer une application de service (SharePoint Foundation 2010)](http://msdn.microsoft.com/library/ee748615.aspx).  
  
 [Restaurer une application de service (SharePoint Server 2010)](https://technet.microsoft.com/library/ee428305.aspx).  

### <a name="restore-the-encryption-keys-using-sharepoint-central-administration"></a>Restaurer les clés de chiffrement à l’aide de l’Administration centrale de SharePoint

 Pour plus d’informations sur la restauration des clés de chiffrement [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , consultez la section « Clés de chiffrement » dans [Gérer une application de service SharePoint Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).  

### <a name="configure-the-execution-account-and-database-authentication"></a>Configurer le compte d’exécution et l’authentification de base de données

 **Compte d'exécution :** si votre application de service utilisait un compte d'exécution, procédez comme suit pour le configurer :  
  
1.  Dans l’Administration centrale de SharePoint, dans le groupe **Gestion des applications**, sélectionnez **Gérer les applications de service**.  
  
2.  Sélectionnez le nom de votre application de service, puis sélectionnez **Gérer** dans le ruban SharePoint.  
  
3.  Sélectionnez **Compte d’exécution**.  
  
4.  Tapez le compte et le mot de passe, puis sélectionnez la zone **Spécifier un compte d'exécution** .  
  
5.  Sélectionnez **OK**.  
  
 **Authentification de base de données :** si votre application de service utilisait l'authentification Windows pour l'authentification de base de données procédez comme suit :  
  
1.  Dans l’Administration centrale de SharePoint, dans le groupe **Gestion des applications**, sélectionnez **Gérer les applications de service**.  
  
2.  Sélectionnez le nom de votre application de service, puis sélectionnez **Propriétés** dans le ruban SharePoint.  
  
3.  Consultez la section **Base de données de service SQL Server Reporting Services (SSRS)** .  
  
4.  Sélectionnez **Authentification Windows**.  
  
5.  Tapez le compte et le mot de passe. Sélectionnez **Utiliser comme informations d'identification Windows** si nécessaire.  
  
6.  Sélectionnez **OK**.

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
