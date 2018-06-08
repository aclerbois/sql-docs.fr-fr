---
title: Configurer Reporting Services pour utiliser un autre nom d’objet | Microsoft Docs
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
ms.openlocfilehash: 41ca0f2a2b036af01e4bae88cbd12d94c7bb292f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-reporting-services-to-use-a-subject-alternative-name"></a>Configurer Reporting Services pour utiliser un autre nom d’objet

Cette rubrique explique comment configurer SQL Server Reporting Services (SSRS) pour utiliser un autre nom de l’objet (SAN, Subject Alternative Name) en modifiant le fichier rsreportserver.config et en utilisant l’outil Netsh.exe.

Les instructions s'appliquent à l'URL Reporting Services, ainsi qu'à une URL de service web.

Pour utiliser un nom SAN, le certificat SSL doit être inscrit sur le serveur, être signé et posséder la clé privée. Vous ne pouvez pas utiliser un certificat auto-signé.  
  
 Vous pouvez configurer les URL dans Reporting Services de sorte qu’elles utilisent un certificat SSL. Normalement, un certificat a juste un nom d'objet qui n'autorise qu'une seule URL pour une session SSL (Secure Sockets Layer). Le nom SAN est un champ supplémentaire dans le certificat qui permet à un service SSL d’être à l’écoute de plusieurs URL. Il permet également au service SSL de partager le port SSL avec d’autres applications. Le nom SAN ressemble à `www.s2.com`.  
  
 Pour plus d’informations sur l’activation de SSL pour Reporting Services, consultez [Configurer des connexions SSL sur un serveur de rapports en mode natif](../../reporting-services/security/configure-ssl-connections-on-a-native-mode-report-server.md).  
  
## <a name="configure-ssrs-to-use-a-subject-alternative-name-for-web-service-url"></a>Configurer SSRS pour utiliser un autre nom de l’objet pour l’URL d’un service web
  
1.  Démarrez le Gestionnaire de configuration de Reporting Services.  
  
     Pour plus d’informations, consultez [Gestionnaire de configuration de Reporting Services &#40;mode natif&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2.  Dans la page **URL du service web** , sélectionnez un port SSL et un certificat SSL.  
  
     ![Gestionnaire de configuration de Reporting Services](../../reporting-services/report-server-sharepoint/media/reportingservices-configurationmanager.png "Gestionnaire de configuration de Reporting Services")  
  
     Le gestionnaire de configuration inscrit le certificat SSL pour le port.  
  
3.  Ouvrez le fichier rsreportserver.config.  
  
     Pour SSRS en mode natif, le fichier se trouve par défaut dans le dossier suivant :  
  
    ```  
    \Program Files\Microsoft SQL Server\MSRS11.MSSQLSERVER\Reporting Services\ReportServer  
    ```  
  
4.  Copiez la section URL de l'application du service web Report Server.  
  
     Par exemple, voici la section URL d’origine :  
  
    ```  
        <URL>  
         <UrlString>https://localhost:443</UrlString>  
         <AccountSid>S-1-5-20</AccountSid>  
         <AccountName>NT Authority\NetworkService</AccountName>  
        </URL>  
  
    ```  
  
     Et la section URL modifiée :
  
    ```  
    <URL>  
         <UrlString>https://www.s1.com:443</UrlString>  
         <AccountSid>S-1-5-20</AccountSid>  
         <AccountName>NT Authority\NetworkService</AccountName>  
        </URL>  
        <URL>  
         <UrlString>https://www.s2.com:443</UrlString>  
         <AccountSid>S-1-5-20</AccountSid>  
         <AccountName>NT Authority\NetworkService</AccountName>  
        </URL>  
  
    ```  
  
5.  Enregistrez le fichier rsreportserver.config.  
  
6.  Démarrez une invite de commandes en tant qu'administrateur, puis exécutez l'outil Netsh.exe.  
  
    ```  
    C:\windows\system32\netsh  
    ```  
  
7.  Passez au contexte http en tapant ce qui suit.  
  
    ```  
    Netsh>http  
    ```  
  
8.  Affichez les urlacl existantes en tapant ce qui suit :
  
    ```  
    Netsh http>show urlacl  
    ```  
  
     Une entrée semblable à ce qui suit s'affiche.  
  
    ```  
    Reserved URL            : https:// www.s1.com:443/ReportServer/  
        User: NT SERVICE\ReportServer  
            Listen: Yes  
            Delegate: No  
            SDDL: D:(A;;GX;;;S-1-5-80-1234567890-123456789-123456789-123456789-1234567890)  
    ```  
  
     Une urlacl est une liste de contrôle d'accès discrétionnaire (DACL, Discretionary Access Control List) pour une URL réservée.  
  
9. Créez une entrée pour l'autre nom de l'objet, avec le même utilisateur et la même chaîne SDDL que l'entrée existante, en tapant ce qui suit.  
  
    ```  
    netsh http>add urlacl  url=https://www.s2.com:443/ReportServer    
    user="NT Service\ReportServer" sddl=D:(A;;GX;;;S-1-5-80-1234567980-12346579-123456789-123456789-1234567890)  
  
    ```  
  
10. Dans la page **État de Report Server** du Gestionnaire de configuration de Reporting Services, cliquez sur **Arrêter** , puis sur **Démarrer** pour redémarrer le serveur de rapports.  
  
## <a name="see-also"></a>Voir aussi

 [Fichier de configuration RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
 [Gestionnaire de configuration de Reporting Services](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Modifier un fichier de configuration Reporting Services](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)   
 [Configurer des URL de serveurs de rapports](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
