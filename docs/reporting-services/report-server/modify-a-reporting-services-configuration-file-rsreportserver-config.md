---
title: Modifier un fichier de configuration Reporting Services (RSreportserver.config) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 958ef51f-2699-4cb2-a92e-3b4322e36a30
caps.latest.revision: 9
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 49575589d605638545dfcc068c240d80faf776ed
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modify-a-reporting-services-configuration-file-rsreportserverconfig"></a>Modify a Reporting Services Configuration File (RSreportserver.config)
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] stocke les paramètres des applications dans un jeu de fichiers de configuration. Le programme d'installation crée les fichiers de configuration de chaque instance du serveur de rapports que vous installez. Dans chaque fichier, les valeurs sont définies soit pendant l'installation, soit lorsque vous utilisez des outils et des applications pour configurer un serveur. Dans certains cas, vous devez modifier directement un fichier pour ajouter ou configurer des paramètres avancés. Les paramètres de configuration sont spécifiés soit comme des éléments, soit comme des attributs XML. Si le langage XML et les fichiers de configuration vous sont familiers, vous pouvez modifier les paramètres définissables par l'utilisateur dans un éditeur de texte ou de code.  
  
 Certains paramètres de configuration peuvent être définis uniquement à l'aide d'un outil. Les paramètres qui contiennent des valeurs chiffrées doivent être modifiés à l'aide de l'outil de configuration de Reporting Services, du programme d'installation ou de l'utilitaire de ligne de commande **rsconfig** . Vous devez être membre du groupe Administrateurs local pour exécuter ces outils.  
  
> [!IMPORTANT]  
>  Soyez prudent lorsque vous modifiez les fichiers de configuration. Si vous modifiez un paramètre réservé à un usage interne, vous risquez de désactiver votre installation. En règle générale, il est déconseillé de modifier les paramètres de configuration, sauf pour essayer de résoudre un problème spécifique. Pour plus d’informations sur les paramètres qui peuvent être modifiés en toute sécurité, consultez [Fichier de configuration RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md) ou [Fichier de configuration RSReportDesigner](../../reporting-services/report-server/rsreportdesigner-configuration-file.md). Pour plus d’informations sur les fichiers de configuration, consultez la documentation du produit [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] .  
  
 Dans cette rubrique :  
  
-   [Lecture et utilisation des valeurs de configuration](#bkmk_read_values)  
  
-   [À propos des valeurs par défaut](#bkmk_default_values)  
  
-   [Suppression de paramètres de configuration](#bkmk_delete_config_settings)  
  
-   [Pour modifier un fichier de configuration Reporting Services](#bkmk_edit_configuation_file)  
  
##  <a name="bkmk_read_values"></a> Lecture et utilisation des valeurs de configuration  
 Un serveur de rapports lit les fichiers de configuration lorsque le service démarre et chaque fois que le fichier de configuration est enregistré. Les valeurs nouvelles et modifiées prennent effet dans un nouveau domaine d'application après l'expiration du domaine d'application actuel. Chaque fois que possible, les requêtes en cours de traitement dans le domaine d'application actuel sont autorisées à se terminer. Toutefois, quelques paramètres requièrent une opération immédiate de recyclage du domaine d'application. Dans ce cas, toutes les requêtes en cours de traitement sont redémarrées dans un nouveau domaine d'application.  
  
 Si le serveur de rapports détecte une valeur non valide, il enregistre une erreur dans le journal des applications Windows et, selon la nature de l'erreur, ne démarre pas ou démarre en utilisant une valeur par défaut.  
  
-   Si l'erreur est due à un code XML incorrect, le serveur de rapports ne démarre pas. Si ce serveur de rapports s'exécute tandis que vous déclenchez une erreur, il ignore le fichier de configuration incorrect tant qu'il n'est pas redémarré ou que le domaine d'application n'est pas recyclé. Dès que l'erreur est détectée, le serveur de rapports ne démarre plus.  
  
-   Si l'erreur est liée à une valeur de configuration non valide, le serveur utilise les valeurs internes par défaut et enregistre une erreur dans les fichiers journaux des traces. Dans les rares cas où les valeurs internes par défaut ne sont pas disponibles, le serveur retourne l'erreur rsServerConfigurationError si le paramètre de configuration non valide est indispensable au fonctionnement du serveur. Les erreurs relatives à des paramètres critiques manquants ou non valides sont retournées à l'application cliente dans une page d'erreurs HTML et sont enregistrées dans le journal des événements.  
  
 Toutes les modifications des fichiers de configuration, qu'elles soient réussies ou non, sont enregistrées dans le fichier journal des traces du serveur de rapports. Seules les erreurs sont enregistrées dans le journal d'événements d'applications.  
  
##  <a name="bkmk_default_values"></a> À propos des valeurs par défaut  
 La plupart des paramètres de configuration ont des valeurs par défaut qui sont spécifiées de manière interne sur le serveur de rapports. Le serveur de rapports utilise ces valeurs si une valeur définie par l'utilisateur n'est pas valide ou si elle n'est pas spécifiée. Si une valeur par défaut doit être utilisée en raison d'un paramètre de configuration non valide, une erreur est écrite dans le fichier journal des traces.  
  
##  <a name="bkmk_delete_config_settings"></a> Suppression de paramètres de configuration  
 Pour les paramètres de configuration qui ont des valeurs par défaut, la suppression du paramètre du fichier de configuration n'a aucun effet. La plupart des paramètres de configuration sont en fait définis et configurés en interne. Si vous supprimez un élément du fichier de configuration, la copie interne reste disponible et utilise la valeur par défaut définie pour cet élément.  
  
##  <a name="bkmk_edit_configuation_file"></a> Pour modifier un fichier de configuration Reporting Services  
  
1.  Recherchez le fichier de configuration à modifier :  
  
    -   **RSReportServer.config** se trouve dans le répertoire suivant :  
  
        ```  
        C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer  
        ```  
        
        ||  
        |-|  
        |**[!INCLUDE[applies](../../includes/applies-md.md)]** Version d’évaluation technique de janvier 2017 des rapports Power BI de SQL Server Reporting Services|
        
        ```  
        C:\Program Files\Microsoft SQL Server Reporting Services\RSServer\ReportServer
        ```
  
    -   **RSReportServerServices.exe.config** se trouve dans le répertoire suivant :  
    
        > [!NOTE] 
        > Ce fichier de configuration n’est pas disponible avec la version d’évaluation technique de janvier 2017 des rapports Power BI de SQL Server Reporting Services.
  
        ```  
        C:\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer\bin  
        ```  
  
    -   **RSReportDesigner.config** se trouve dans le répertoire suivant :  
  
        ```  
        C:\Program Files\Microsoft Visual Studio 10.0\Common7\IDE\PrivateAssemblies  
        ```  
  
2.  Enregistrez une copie du fichier pour pouvoir restaurer vos modifications le cas échéant.  
  
3.  Ouvrez le fichier d'origine dans le Bloc-notes ou un éditeur de code. N'utilisez pas Textpad ; il définit la longueur du fichier avant que ce dernier ne soit enregistré, ce qui entraîne l'écriture d'une erreur de type caractère non valide dans le fichier journal des traces.  
  
4.  Tapez l'élément ou la valeur à ajouter ou à utiliser. Les éléments respectent la casse. Si vous ajoutez un élément, veillez à utiliser les majuscules et minuscules appropriées. Des instructions spécifiques relatives à la modification des fichiers de configuration sont disponibles si vous personnalisez des extensions de rendu, des extensions d'authentification ou des extensions pour le traitement des données :  
  
    -   [Authentification avec le serveur de rapports](../../reporting-services/security/authentication-with-the-report-server.md)  
  
    -   [Configurer le portail Web pour passer des cookies d’authentification personnalisée](../../reporting-services/security/configure-the-web-portal-to-pass-custom-authentication-cookies.md)
  
    -   [Personnaliser les paramètres d'extension de rendu dans RSReportServer.Config](../../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)  
  
5.  Enregistrez le fichier.  
  
6.  Consultez les fichiers journaux des traces pour vous assurer qu'aucune erreur ne s'est produite. Si vous trouvez une erreur, cela signifie qu'un paramètre ou sa valeur ont été spécifiés de manière incorrecte. Consultez l’article [Fichier de configuration RSReportServer](../../reporting-services/report-server/rsreportserver-config-configuration-file.md) pour connaître les valeurs valides du paramètre à l’origine de l’erreur. Pour plus d’informations sur l’affichage du journal des traces, consultez [Report Server Service Trace Log](../../reporting-services/report-server/report-server-service-trace-log.md)(Journal des traces du service Report Server).  
  
## <a name="see-also"></a> Voir aussi  
 [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Fichier de configuration ReportingServicesService](../../reporting-services/report-server/reportingservicesservice-configuration-file.md)   
 [Fichier de configuration RSReportDesigner](../../reporting-services/report-server/rsreportdesigner-configuration-file.md)   
 [Déploiement d’une extension pour le traitement des données](../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)   
 [Déploiement d’une extension de remise](../../reporting-services/extensions/delivery-extension/deploying-a-delivery-extension.md)   
 [Déploiement d’une extension de rendu](../../reporting-services/extensions/rendering-extension/deploying-a-rendering-extension.md)   
 [Fichiers de configuration de Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)  
  
  
