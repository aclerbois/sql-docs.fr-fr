---
title: Gestionnaire de connexions HTTP | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.httpconnection.server.f1
- sql13.dts.designer.httpconnection.proxy.f1
helpviewer_keywords:
- HTTP connection manager
- Web site connections [Integration Services]
- connection managers [Integration Services], HTTP
- Web server connections [Integration Services]
- connections [Integration Services], HTTP
ms.assetid: 26b2b3e1-d02c-46ca-8d31-7aef2bbc3c53
caps.latest.revision: 44
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 3e80ca7ecf409f201a3d29904cb8693828a7c3ad
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="http-connection-manager"></a>Gestionnaire de connexions HTTP
  Une connexion HTTP permet à un package d'accéder à un serveur Web via HTTP pour l'envoi et la réception de fichiers. La tâche de service Web incluse dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise ce gestionnaire de connexions.  
  
 Quand vous ajoutez un gestionnaire de connexions HTTP à un package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un gestionnaire de connexions qui sera converti en connexion HTTP au moment de l’exécution, définit les propriétés du gestionnaire de connexions et ajoute le gestionnaire de connexions à la collection **Connections** du package.  
  
 La propriété **ConnectionManagerType** du gestionnaire de connexions a pour valeur **HTTP**.  
  
 Vous pouvez configurer le gestionnaire de connexions HTTP de plusieurs manières :  
  
-   Utilisez des informations d'identification. Si le gestionnaire de connexions utilise des informations d'identification, ses propriétés incluent le nom d'utilisateur, le mot de passe et le domaine.  
  
    > [!IMPORTANT]  
    >  Le gestionnaire de connexions HTTP prend en charge uniquement l'authentification anonyme et l'authentification de base. Il ne prend pas en charge l'authentification Windows.  
  
-   Utilisez un certificat client. Si le gestionnaire de connexions utilise un certificat client, ses propriétés incluent le nom du certificat.  
  
-   Indiquez un délai de connexion au serveur et une taille de segment pour l'écriture de données.  
  
-   Utilisez un serveur proxy. Le serveur Proxy peut également être configuré pour utiliser les informations d'identification et pour ignorer le serveur proxy et utiliser à la place des adresses locales.  
  
## <a name="configuration-of-the-http-connection-manager"></a>Configuration du gestionnaire de connexions HTTP  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d’informations sur la configuration d’un gestionnaire de connexions par programmation, consultez <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>.  
  
## <a name="http-connection-manager-editor-server-page"></a>Éditeur du gestionnaire de connexions HTTP (page Serveur)
  Utilisez l'onglet **Serveur** de la boîte de dialogue **Éditeur du gestionnaire de connexions HTTP** pour configurer le gestionnaire de connexions HTTP en spécifiant des propriétés telles que l'URL et les informations d'identification de sécurité. Une connexion HTTP permet à un package d'accéder à un serveur Web via HTTP pour l'envoi et la réception de fichiers. Une fois le gestionnaire de connexions HTTP configuré, vous pouvez également tester la connexion.  
  
> [!IMPORTANT]  
>  Le gestionnaire de connexions HTTP prend en charge uniquement l'authentification anonyme et l'authentification de base. Il ne prend pas en charge l'authentification Windows.  
  
 Pour en savoir plus sur le gestionnaire de connexions HTTP, consultez [HTTP Connection Manager](../../integration-services/connection-manager/http-connection-manager.md). Pour en savoir plus sur un scénario d'utilisation courante pour le gestionnaire de connexions HTTP, consultez [Web Service Task](../../integration-services/control-flow/web-service-task.md).  
  
### <a name="options"></a>Options  
 **URL du serveur**  
 Tapez l'URL du serveur.  
  
 Si vous projetez d'utiliser le bouton **Télécharger WSDL** de la page **Général** de l' **Éditeur de tâche de service Web** pour télécharger un fichier WSDL, tapez l'URL de ce fichier WSDL. Cette URL se termine par « ?wsdl ».  
  
 **Utiliser les informations d'identification**  
 Spécifiez si vous voulez que le gestionnaire de connexions HTTP utilise les informations d'identification de sécurité de l'utilisateur pour l'authentification.  
  
 **User name**  
 Si le gestionnaire de connexions HTTP utilise des informations d'identification, vous devez spécifier un nom d'utilisateur, un mot de passe et un domaine.  
  
 **Mot de passe**  
 Si le gestionnaire de connexions HTTP utilise des informations d'identification, vous devez spécifier un nom d'utilisateur, un mot de passe et un domaine.  
  
 **Domaine**  
 Si le gestionnaire de connexions HTTP utilise des informations d'identification, vous devez spécifier un nom d'utilisateur, un mot de passe et un domaine.  
  
 **Utiliser le certificat client**  
 Spécifiez si vous voulez que le gestionnaire de connexions HTTP utilise un certificat client pour l'authentification.  
  
 **Certificat**  
 Sélectionnez un certificat dans la liste via la boîte de dialogue **Sélectionner un certificat** . La zone de texte affiche le nom associé au certificat.  
  
 **Délai d'expiration (en secondes)**  
 Spécifiez un délai d'expiration pour la connexion au serveur Web. La valeur par défaut de cette propriété est 30 secondes.  
  
 **Taille de segment (en Ko)**  
 Spécifiez une taille de segment pour l'écriture des données.  
  
 **Tester la connexion**  
 Après avoir configuré le gestionnaire de connexions HTTP, vérifiez que la connexion est viable en cliquant sur **Tester la connexion**.  
  
## <a name="http-connection-manager-editor-proxy-page"></a>Éditeur du gestionnaire de connexions HTTP (page Proxy)
  Utilisez l'onglet **Proxy** de la boîte de dialogue **Éditeur du gestionnaire de connexions HTTP** pour configurer le gestionnaire de connexions HTTP afin d'utiliser un serveur proxy. Une connexion HTTP permet à un package d'accéder à un serveur Web via HTTP pour l'envoi et la réception de fichiers.  
  
 Pour en savoir plus sur le gestionnaire de connexions HTTP, consultez [HTTP Connection Manager](../../integration-services/connection-manager/http-connection-manager.md). Pour en savoir plus sur un scénario d'utilisation courante pour le gestionnaire de connexions HTTP, consultez [Web Service Task](../../integration-services/control-flow/web-service-task.md).  
  
### <a name="options"></a>Options  
 **Utiliser le proxy**  
 Indiquez si vous voulez que le gestionnaire de connexions HTTP se connecte via un serveur proxy.  
  
 **URL du proxy**  
 Tapez l'URL du serveur proxy.  
  
 **Ne pas utiliser de proxy en local**  
 Indiquez si vous voulez que le gestionnaire de connexions HTTP contourne le serveur proxy pour les adresses locales.  
  
 **Utiliser les informations d'identification**  
 Indiquez si vous voulez que le gestionnaire de connexions HTTP utilise les informations d'identification de sécurité pour le serveur proxy.  
  
 **User name**  
 Si le gestionnaire de connexions HTTP utilise des informations d'identification, vous devez spécifier un nom d'utilisateur, un mot de passe et un domaine.  
  
 **Mot de passe**  
 Si le gestionnaire de connexions HTTP utilise des informations d'identification, vous devez spécifier un nom d'utilisateur, un mot de passe et un domaine.  
  
 **Domaine**  
 Si le gestionnaire de connexions HTTP utilise des informations d'identification, vous devez spécifier un nom d'utilisateur, un mot de passe et un domaine.  
  
 **Liste de contournement proxy**  
 Liste d'adresses pour lesquelles vous ne souhaitez pas utiliser le serveur proxy.  
  
 **Ajouter**  
 Tapez une adresse pour laquelle vous souhaitez que le gestionnaire de connexions HTTP n'utilise pas le serveur proxy.  
  
 **Supprimer**  
 Sélectionnez une adresse, puis supprimez-la en cliquant sur **Supprimer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Tâche de service Web](../../integration-services/control-flow/web-service-task.md)   
 [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
  
