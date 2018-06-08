---
title: Résoudre les problèmes d’abonnements et de remise de Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 05/31/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: troubleshooting
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ae1775f7-9919-48ca-8bd7-cc16df274e2c
caps.latest.revision: 16
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 3065ee61d14d51caf67b9a62bb88621f9f5beaf6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-reporting-services-subscriptions-and-delivery"></a>Résolution des problèmes d’abonnements et de remise de Reporting Services
  
    
Utilisez cette rubrique pour résoudre les problèmes liés aux abonnements, aux planifications et à la remise de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion.md)] .  
## <a name="log-information"></a>Informations des journaux
 
La page Abonnement dans [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] inclut un état d’un abonnement, mais s’il existe un problème avec l’abonnement, les informations détaillées se trouvent dans les journaux [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] . 
![ssrs_tutorial_datadriven_subscription_status_ReportManager](../../reporting-services/media/ssrs-tutorial-datadriven-subscription-status-reportmanager.png)

**Journaux des traces :** Les journaux des traces sont des fichiers texte écrits dans : `\Program Files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\LogFiles`

Voici un exemple d’entrée de journal :

```
   subscription WindowsService_10   4c7c    05/24/2016-01:05:06  e ERROR     Failure writing file \\ServerName\SalesReports\so71949.xls : Microsoft.ReportingServices.FileShareDeliveryProvider.FileShareProvider+NetworkErrorException: An impersonation error occurred using the security context of the current user. ---> System.ArgumentException: Value does not fall within the expected range.  05/24/2016
```
Pour plus d’informations sur les journaux des traces [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , consultez : 
+ [Journal des traces du service Report Server](../../reporting-services/report-server/report-server-service-trace-log.md).
+ [Fichiers journaux et sources de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md).

**Vues du journal des exécutions :**

Les journaux des exécutions sont des vues dans la base de données ReportServer SQL. Pour plus d’informations sur [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , consultez [Journal des exécutions du serveur de rapports et vue ExecutionLog3](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  

----------
## <a name="unable-to-send-reports-using-e-mail-with-windows-server-2003-and-pop3"></a>Impossible d'envoyer des rapports par courrier électronique avec Windows Server 2003 et POP3  
Si vous exécutez une application de messagerie électronique avec le protocole POP3 (Post Office Protocol version 3) surMicrosoft Windows Server 2003, il est possible que vous ne puissiez pas envoyer de rapports en utilisant le serveur POP3 local. Si vous configurez le serveur de rapports pour envoyer des courriers électroniques en utilisant le serveur POP3 local, et que vous créez un abonnement qui envoie un rapport, vous risquez de recevoir le message d'erreur suivant :  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`Failure sending mail: <error message>`  
  
où \<error message> est remplacé par des informations complémentaires sur les messages d’erreur retournées par des objets de données de collaboration (CDO).  
  
### <a name="to-resolve-this-problem"></a>Pour rectifier ce problème :  
* Définissez sur 1 la valeur de l’élément `SendUsing` dans le fichier **Rsreportserver.config** .  
* Effacez la valeur de la propriété `SMTPServer` pour obtenir une propriété vide. Vous devrez également affecter une valeur à la propriété `SMTPServerPickupDirectory` .   
  
Pour plus d’informations sur l’utilisation d’un service SMTP local pour une remise de rapports par messagerie électronique, consultez Configurer un serveur de rapports pour la remise par messagerie (Gestionnaire de configuration de SSRS).  
  
## <a name="failure-sending-mail-the-server-rejected-the-sender-address-the-server-response-was-454-573-client-does-not-have-permission-to-submit-mail-to-this-server"></a>Échec de l'envoi du message électronique : Le serveur a rejeté l'adresse de l'expéditeur. La réponse du serveur était : 454 5.7.3 Le client n'est pas autorisé à soumettre du courrier à ce serveur  
Cette erreur se produit lorsque les paramètres de stratégie de sécurité du serveur SMTP autorisent uniquement des utilisateurs authentifiés à soumettre des messages pour une remise ultérieure. Si le serveur SMTP n'accepte pas les soumissions par messagerie à partir d'utilisateurs anonymes, demandez à l'administrateur système de vous donner l'autorisation d'utiliser le serveur.  
> Cette erreur peut également se produire lorsque vous spécifiez un nom de serveur Exchange comme SMTPServer. Pour utiliser un serveur Exchange pour la remise du courrier électronique, vous devez indiquer le nom de la passerelle SMTP configurée pour votre serveur Exchange. Pour plus d'informations sur le serveur Exchange, contactez l'administrateur du système.  
  
## <a name="subscriptions-are-not-processing"></a>Les abonnements ne fonctionnent pas  
Les abonnements peuvent échouer dans les conditions suivantes.   
* La planification utilisée pour déclencher le rapport a expiré. Pour les abonnements qui sont déclenchés par la mise à jour d'un instantané de rapport, la planification utilisée pour actualiser l'instantané a peut-être expiré.  
  
* Le serveur de rapports, l'Agent SQL Server, ou l'application du serveur de messagerie ne fonctionne pas.  
* Il est impossible de remettre le rapport (il est trop volumineux, par exemple). Pour déterminer si la remise n'aboutit pas car le rapport est trop volumineux, enregistrez le rapport dans un fichier, puis envoyez-le par courrier électronique. Veillez à choisir le format de rendu que vous avez spécifié dans l'abonnement. Si vous obtenez une erreur de remise, utilisez l'extension de remise dans un partage de fichiers au lieu de la remise par courrier électronique Report Server.  
* L'ordinateur utilisé pour la remise du partage de fichiers ne fonctionne pas ou le partage de fichiers est configuré pour un accès en lecture seule.  
* L'extension de remise spécifiée dans l'abonnement a été désinstallée ou désactivée.  
* Les informations d'identification ne sont plus stockées mais intégrées ou demandées par invite.  
* Le nom du paramètre ou le type de données a changé dans la définition du rapport et le rapport a été republié. Si un abonnement comprend un paramètre qui n'est plus valide, l'abonnement devient inactif.  
  
Pour plus d’informations, consultez la rubrique Wiki TechNet [Troubleshoot issues and errors with Reporting Services](http://social.technet.microsoft.com/wiki/contents/articles/1633.ssrs-troubleshoot-issues-and-errors-with-reporting-services.aspx)(Résoudre les problèmes et erreurs avec Reporting Services).  
  
  
    
  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect.md)]

