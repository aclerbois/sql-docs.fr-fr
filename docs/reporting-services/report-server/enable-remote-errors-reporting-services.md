---
title: Activer les erreurs distantes (Reporting Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- remote data source [Reporting Services]
- EnableRemoteError server property
ms.assetid: 5f05022b-d557-43e0-b50a-f5e2a1846b83
caps.latest.revision: 12
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 22884688b04a6aa5bc376506fa7f84abb7af466c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="enable-remote-errors-reporting-services"></a>Activer les erreurs distantes (Reporting Services)
  Vous pouvez définir des propriétés de serveur sur un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de façon à retourner des informations supplémentaires concernant les conditions d'erreur qui se produisent sur des serveurs distants. Si un message d'erreur contient le texte « Pour obtenir plus d'informations sur cette erreur, accédez au serveur de rapports sur le serveur local ou activez les erreurs distantes », vous pouvez définir la propriété **EnableRemoteErrors** de façon à accéder à des informations supplémentaires qui peuvent vous aider à résoudre le problème. Pour plus d’informations, consultez [Propriétés système de Report Server](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md) dans la documentation en ligne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Dans cette rubrique :  
  
-   [Activer les erreurs distantes pour le mode SharePoint](#bkmk_sharepoint)  
  
-   [Activer les erreurs distantes au moyen de SQL Server Management Studio (mode natif)](#bkmk_mgtStudio)  
  
-   [Activer les erreurs distantes au moyen d'un script (mode natif)](#bkmk_script)  
  
-   [Modification de la table ConfigurationInfo (mode natif)](#bkmk_ConfigurationInfo)  
  
##  <a name="bkmk_sharepoint"></a> Activer les erreurs distantes pour le mode SharePoint  
 Il existe deux procédures différentes pour activer les erreurs distantes pour le mode SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . La procédure est différente pour les deux architectures différentes de serveur de rapports. La dernière architecture basée sur un service SharePoint qui a été introduite dans la version [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] utilise un paramètre qui peut être configuré pour chaque application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . L'architecture plus ancienne utilise un paramètre de niveau de site unique.  
  
#### <a name="enable-remote-errors-for-a-reporting-services-service-application"></a>Activer les erreurs distantes pour une application de service Reporting Services  
  
1.  Pour un serveur de rapports en mode SharePoint installé avec [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou une version plus récente de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], activez le paramètre d'application de service **Activer les erreurs distantes**. Le paramètre peut être configuré pour chaque application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
2.  Dans l'Administration Centrale de SharePoint, sous le groupe **Gestion des applications** , cliquez sur **Gérer les applications de service** .  
  
3.  Trouvez votre application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] puis cliquez sur le nom de votre application de service.  
  
4.  Cliquez sur **Paramètres système**.  
  
5.  Cliquez sur **Activer les erreurs distantes** dans la section **Sécurité** .  
  
6.  Cliquez sur **OK**.  
  
#### <a name="enable-remote-errors-for-a-sharepoint-site"></a>Activer les erreurs distantes pour un site SharePoint  
  
1.  Pour un serveur de rapports en mode SharePoint installé avec une version de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] avant [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], activez le paramètre de site **Activer les erreurs distantes en mode local**.  
  
2.  Cliquez sur **Paramètres du site** dans **Actions du site** pour le site vous souhaitez modifier.  
  
3.  Dans le groupe **Reporting Services** , cliquez sur **Paramètres du site Reporting Services** .  
  
4.  Cliquez sur **Activer les erreurs distantes en mode local**.  
  
5.  Cliquez sur **OK**  
  
##  <a name="bkmk_mgtStudio"></a> Activer les erreurs distantes au moyen de SQL Server Management Studio (mode natif)  
  
1.  Démarrez Management Studio et connectez-vous à une instance du serveur de rapports. Pour plus d’informations, consultez [Se connecter à un serveur de rapports dans Management Studio](../../reporting-services/tools/connect-to-a-report-server-in-management-studio.md) dans la documentation en ligne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
2.  Cliquez avec le bouton droit sur le nœud du serveur de rapports, puis sélectionnez **Propriétés**.  
  
3.  Cliquez sur **Avancé** pour ouvrir la page de propriétés. Pour plus d’informations, consultez [Propriétés du serveur &#40;page Avancé&#41; - Reporting Services](../../reporting-services/tools/server-properties-advanced-page-reporting-services.md) dans la documentation en ligne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
4.  Dans **EnableRemoteErrors**, sélectionnez **True**.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="bkmk_script"></a> Activer les erreurs distantes au moyen d'un script (mode natif)  
  
1.  Créez un fichier texte et copiez-y le script suivant.  
  
    ```  
    Public Sub Main()  
      Dim P As New [Property]()  
      P.Name = "EnableRemoteErrors"  
      P.Value = True  
      Dim Properties(0) As [Property]  
      Properties(0) = P  
      Try  
        rs.SetSystemProperties(Properties)  
        Console.WriteLine("Remote errors enabled.")  
      Catch SE As SoapException  
        Console.WriteLine(SE.Detail.OuterXml)  
      End Try  
    End Sub  
    ```  
  
2.  Enregistrez le fichier sous le nom EnableRemoteErrors.rss.  
  
3.  Cliquez sur **Démarrer**, pointez sur **Exécuter**, tapez **cmd**et cliquez sur **OK** pour ouvrir une fenêtre d'invite de commandes.  
  
4.  Accédez au répertoire qui contient le fichier .rss que vous venez de créer.  
  
5.  Tapez la ligne de commande suivante, en remplaçant *servername* par le nom de votre serveur.  
  
    ```  
    rs -i EnableRemoteErrors.rss -s http://servername/ReportServer  
    ```  
  
6.  Pour plus d’informations, consultez [Utilitaire RS.exe &#40;SSRS&#41;](../../reporting-services/tools/rs-exe-utility-ssrs.md).  
  
##  <a name="bkmk_ConfigurationInfo"></a> Modification de la table ConfigurationInfo (mode natif)  
  
1.  > [!NOTE]  
    >  Vous pouvez modifier la table **ConfigurationInfo** dans la base de données du serveur de rapports afin d'affecter la valeur **EnableRemoteErrors** à **True**, mais si le serveur de rapports est utilisé de manière active, vous devez utiliser SQL Server Management Studio ou un script pour modifier les paramètres. Si vous modifiez le paramètre dans la base de données, vous devez redémarrer le service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] avant que les modifications entrent en vigueur.  
  
  
