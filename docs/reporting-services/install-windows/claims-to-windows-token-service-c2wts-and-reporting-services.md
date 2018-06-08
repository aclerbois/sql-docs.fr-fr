---
title: Service d’émission de jetons Revendications vers Windows (C2WTS) et Reporting Services | Microsoft Docs
ms.custom: ''
ms.date: 09/15/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.component: install-windows
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 82145ec54fc4a619bc5b399d44b9d34f858b3fa4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="claims-to-windows-token-service-c2wts-and-reporting-services"></a>Service d'émission de jetons Revendications vers Windows (C2WTS) et Reporting Services

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)])

Le service d’émission de jetons Revendications vers Windows (C2WTS) de SharePoint est nécessaire si vous souhaitez afficher les rapports en mode natif dans le [composant WebPart Visionneuse de rapports SQL Server Reporting Services](../report-server-sharepoint/deploy-report-viewer-web-part.md).

Il est également nécessaire avec le mode SharePoint de SQL Server Reporting Services, si vous souhaitez utiliser l’authentification Windows pour les sources de données situées hors de la batterie de serveurs SharePoint. C2WTS est nécessaire même si vos sources de données sont sur le même ordinateur que le service partagé. Toutefois dans ce scénario, la délégation contrainte n'est pas nécessaire.

> [!NOTE]
> L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

## <a name="report-viewer-web-part-configuration"></a>Configuration du composant WebPart Visionneuse de rapports

Le composant WebPart Visionneuse de rapports peut être utilisé pour incorporer des rapports SQL Server Reporting Services en mode natif dans votre site SharePoint. Ce composant WebPart est disponible pour SharePoint 2013 et SharePoint 2016. SharePoint 2013 et SharePoint 2016 utilisent tous deux l’authentification basée sur les revendications. SQL Server Reporting Services (mode natif) utilise par défaut l’authentification Windows. Par conséquent, C2WTS doit être correctement configuré pour que les rapports s’affichent comme il se doit.

## <a name="sharepoint-mode-integaration"></a>Intégration du mode SharePoint

**Cette section s’applique uniquement à SQL Server 2016 Reporting Services et antérieur.**

Le service d’émission de jetons Revendications vers Windows (C2WTS) SharePoint est nécessaire avec le mode SharePoint de SQL Server Reporting Services si vous souhaitez utiliser l’authentification Windows pour les sources de données situées hors de la batterie de serveurs SharePoint. Ceci vaut même si l’utilisateur accède aux sources de données avec l’authentification Windows, car la communication entre le serveur web frontal (WFE) et le service partagé Reporting Services s’effectue toujours via l’authentification basée sur les revendications.

## <a name="steps-needed-to-configure-c2wts"></a>Étapes nécessaires pour configurer C2WTS

Les jetons créés par C2WTS fonctionnent uniquement avec la délégation contrainte (contraint à des services spécifiques) et l'option de configuration « Utiliser tout protocole d'authentification ». Comme indiqué précédemment, si vos sources de données sont sur le même ordinateur que le service partagé, la délégation contrainte n'est pas nécessaire.

Si votre environnement utilise la délégation contrainte Kerberos, le service SharePoint server et les sources de données externes doivent résider dans le même domaine Windows. Tout service qui s’appuie sur le service d’émission de jetons Revendications vers Windows (C2WTS) doit utiliser la délégation **contrainte** Kerberos pour permettre à C2WTS d’utiliser une transition de protocole Kerberos dans la conversion de revendications en informations d’identification Windows. Ces exigences s'appliquent à tous les services partagés SharePoint. Pour plus d’informations, consultez [Planifier l’authentification Kerberos dans SharePoint 2013](http://technet.microsoft.com/library/ee806870.aspx).  

1. Configurez le compte de service C2WTS. Ajoutez le compte de service au groupe Administrateurs local sur chaque serveur d’applications sur lequel C2WTS sera utilisé.

    Pour le **Composant WebPart Visionneuse de rapports**, il s’agit des serveurs web frontaux. Pour le **mode intégré SharePoint**, il s’agit des serveurs d’applications sur lesquels le service Reporting Services s’exécute.

2. Configurez la délégation pour le compte de service C2WTS.

    Le compte a besoin de la délégation contrainte avec transition de protocole; ainsi que d’autorisations de déléguer aux services avec lesquelles il doit communiquer (par exemple, moteur de base de données SQL Server, SQL Server Analysis Services). Pour configurer la délégation, vous pouvez utiliser le composant Utilisateurs et ordinateurs Active Directory et devez être administrateur du domaine.

    > [!IMPORTANT]
    > Dans tous les cas, les paramètres que vous configurez pour le compte de service C2WTS, sous l’onglet Délégation, doivent correspondre au compte de service principal utilisé. Pour le **composant WebPart Visionneuse de rapports**, il s’agit du compte de service de l’application web SharePoint. Pour le **mode intégré SharePoint**, il s’agit du compte de service Reporting Services.
    >
    > Par exemple, si vous autorisez le compte de service C2WTS à déléguer à un service SQL, vous devez faire de même sur le compte de service Reporting Services pour le mode intégré SharePoint.

    * Cliquez avec le bouton droit sur chaque compte de service et ouvrez la boîte de dialogue des propriétés. Dans la boîte de dialogue, cliquez sur l'onglet **Délégation** .

        L’onglet Délégation est visible uniquement si un SPN (Service Principal Name) a été affecté à l’objet. C2WTS ne nécessite pas de SPN sur le compte C2WTS, mais sans SPN, l’onglet **Délégation** ne sera pas visible. Une autre façon de configurer la délégation contrainte consiste à utiliser un utilitaire tel que **ADSIEdit**.

    * Les options de configuration principales dans l'onglet Délégation sont les suivantes :

        * Sélectionnez **N’approuver cet utilisateur que pour la délégation aux services spécifiés**
        * Sélectionnez **Utiliser tout protocole d’authentification**.

    * Sélectionnez **Ajouter** pour ajouter un service auquel déléguer.

    * Sélectionnez **Utilisateurs ou ordinateurs** * et entrez le compte qui héberge le service. Par exemple, si un serveur SQL Server s’exécute sous un compte nommé *sqlservice*, entrez `sqlservice`. 

    * Sélectionnez la liste des services. Les SPN disponibles sur ce compte s’affichent. Si vous ne voyez pas le service sur ce compte, il est peut-être manquant ou placé sur un autre compte. Vous pouvez utiliser l’utilitaire SetSPN pour ajuster les SPN.

    * Cliquez sur OK pour fermer les boîtes de dialogue.

3. Configurer les *appelants autorisés* dans C2WTS

    C2WTS requiert que les identités des appelants soient explicitement répertoriées dans le fichier de configuration, **c2WTShost.exe.config**. C2WTS n'accepte pas de demandes de tous les utilisateurs authentifiés dans le système, sauf s'il est configuré pour cela. Dans ce cas, l’appelant est le groupe Windows WSS_WPG. Le fichier C2WTShost.exe.confi est enregistré à l’emplacement suivant :

    La modification du compte de service dans l’Administration centrale de SharePoint, pour le service C2WTS, ajoute ce compte au groupe WSS_WPG.

    **\Program Files\Windows Identity Foundation\v3.5\c2WTShost.exe.config**

    L'exemple suivant montre le fichier de configuration :

    ```
    <configuration>
      <windowsTokenService>
        <!--  
            By default no callers are allowed to use the Windows Identity Foundation Claims To NT Token Service.  
            Add the identities you wish to allow below.  
          -->
        <allowedCallers>
          <clear/>
          <add value="WSS_WPG" />
        </allowedCallers>
      </windowsTokenService>
    </configuration>
    ```

4. Démarrez les revendications SharePoint vers Windows Token Service via l’Administration centrale de SharePoint sur la page **Gérer les services sur le serveur**. Le service doit être démarré sur le serveur qui effectuera l'action. Par exemple si vous avez un serveur web frontal et un serveur d’applications exécutant le service partagé SQL Server Reporting Services, il vous suffit de démarrer C2WTS sur le serveur d’applications. C2WTS est obligatoire sur un serveur web frontal uniquement si vous utilisez le composant WebPart Visionneuse de rapports.

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
