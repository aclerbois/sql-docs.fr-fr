---
title: Guide pratique pour installer des extensions de sécurité personnalisées | Microsoft Docs
ms.custom: ''
ms.date: 07/10/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: extensions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: bfa0a35b-ccfb-4279-bae6-106c227c5f16
caps.latest.revision: 3
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 7b344fb4320af3ca9c4778740c6a5055481d6fce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="how-to-install-custom-security-extensions"></a>Guide pratique pour installer des extensions de sécurité personnalisées

[!INCLUDE[ssrs-appliesto](../../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../../includes/ssrs-appliesto-pbirs.md)])

Reporting Services 2016 introduit un nouveau portail web pour héberger de nouvelles API Odata, ainsi que de nouvelles charges de travail de rapport telles que les rapports mobiles et les indicateurs de performance clés. Ce nouveau portail s’appuie sur des technologies plus récentes et s’exécute dans un processus séparé, car il est isolé de l’utilitaire ReportingServicesService classique. Ce processus n’est pas une application hébergée ASP.NET et par conséquent rompt avec les extensions de sécurité personnalisées existantes. De plus, les interfaces actuelles des extensions de sécurité personnalisées ne permettent pas de transmettre un contexte externe, laissant aux implémenteurs l’unique option d’inspecter des objets ASP.NET globaux connus ; il était donc nécessaire d’apporter des modifications à l’interface.

## <a name="what-changed"></a>Ce qui a été changé

Une nouvelle interface qui peut être implémentée a été introduite. Elle fournit un IRSRequestContext qui met à disposition les propriétés plus communes qu’utilisent les extensions pour prendre des décisions relatives à l’authentification.

Dans les versions précédentes, le Gestionnaire de rapports faisait office d’interface et pouvait être configuré avec sa propre page de connexion personnalisée. Dans Reporting Services 2016, une seule page hébergée par le serveur de rapports est prise en charge et doit s’authentifier auprès des deux applications.

## <a name="implementation"></a>Implémentation

Dans les versions précédentes, les extensions pouvaient escompter une mise à disposition facile des objets ASP.NET. Étant donné que le nouveau portail ne fonctionne pas dans ASP.NET, l’extension pourrait connaître des problèmes avec les objets de valeur null.

L’exemple le plus typique est l’accès à HttpContext.Current pour lire des informations de demande telles que des en-têtes et des cookies. Pour autoriser les extensions à prendre les mêmes décisions, nous avons introduit une nouvelle méthode dans l’extension qui fournit des informations de demande et est appelée au moment de l’authentification à partir du portail. 

Les extensions doivent implémenter l’interface <xref:Microsoft.ReportingServices.Interfaces.IAuthenticationExtension2> afin de tirer parti de cette méthode. Les extensions doivent implémenter les deux versions de la méthode <xref:Microsoft.ReportingServices.Interfaces.IAuthenticationExtension.GetUserInfo%2A>, l’une étant appelée par le contexte du serveur de rapports, l’autre étant utilisée dans le processus de l’hôte web. L’exemple ci-dessous montre une des implémentations simples pour le portail, dans laquelle l’identité résolue par le serveur de rapports est celle utilisée.

``` 
public void GetUserInfo(IRSRequestContext requestContext, out IIdentity userIdentity, out IntPtr userId)
{
    userIdentity = null;
    if (requestContext.User != null)
    {
        userIdentity = requestContext.User;
    }

    // initialize a pointer to the current user id to zero
    userId = IntPtr.Zero;
}
```

## <a name="deployment-and-configuration"></a>Déploiement et configuration

Les configurations de base nécessaires pour l’extension de sécurité personnalisée sont les mêmes que les versions précédentes. Des modifications sont nécessaires pour web.config et rsreportserver.config ; pour plus d’informations, consultez [Configurer l’authentification personnalisée ou par formulaire sur le serveur de rapports](../../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md).

Il n’existe plus de web.config distinct pour le Gestionnaire de rapports : le portail hérite les mêmes paramètres que le point de terminaison du serveur de rapports.

## <a name="machine-keys"></a>Clés d’ordinateur

Dans le cas de l’authentification par formulaire, qui nécessite le déchiffrement du cookie d’authentification, les deux processus doivent être configurés avec les mêmes clé d’ordinateur et algorithme de déchiffrement. Cette étape, familière pour ceux qui ont déjà configuré Reporting Services dans le cadre d’environnements de scale-out, est désormais obligatoire, même pour les déploiements sur un seul ordinateur.

Vous devez utiliser une clé de validation propre à votre déploiement ; vous disposez de plusieurs outils pour générer les clés, tels que le Gestionnaire des services IIS (IIS). D’autres outils sont disponibles sur Internet.

### <a name="sql-server-reporting-services-2017-and-later"></a>SQL Server Reporting Services 2017 et ultérieur

**\ReportServer\rsReportServer.config**

Ajoutez la clé sous `<configuration>`.

```
<machineKey validationKey="[YOUR KEY]" decryptionKey=="[YOUR KEY]" validation="AES" decryption="AES" />
```

### <a name="sql-server-reporting-services-2016"></a>SQL Server Reporting Services 2016

**\ReportServer\web.config**

Ajoutez la clé sous `<system.web>`.
    
```
    <machineKey validationKey="[YOUR KEY]" decryptionKey=="[YOUR KEY]" validation="AES" decryption="AES" />
```

**\RSWebApp\Microsoft.ReportingServices.Portal.WebHost.exe.config**

Ajoutez la clé sous `<configuration>`.

```
    <system.web>
        <machineKey validationKey=="[YOUR KEY]" decryptionKey=="[YOUR KEY]" validation="AES" decryption="AES" />
    </system.web>
```

### <a name="power-bi-report-server"></a>Power BI Report Server

Disponible depuis la version de juin 2017 (Build 14.0.600.301).

**\ReportServer\rsReportServer.config**

Ajoutez la clé sous `<configuration>`.

```
<machineKey validationKey="[YOUR KEY]" decryptionKey=="[YOUR KEY]" validation="AES" decryption="AES" />
```

## <a name="configure-passthrough-cookies"></a>Configurer les cookies Passthrough

Le nouveau portail et le serveur de rapports communiquent à l’aide d’API SOAP internes pour certaines de leurs opérations (à l’image de la version précédente du Gestionnaire de rapports). Quand des cookies supplémentaires doivent être transmis depuis le portail vers le serveur, les propriétés PassThroughCookies sont toujours disponibles. Pour plus d’informations, consultez [Configurer le portail web pour passer des cookies d’authentification personnalisée](../../../reporting-services/security/configure-the-web-portal-to-pass-custom-authentication-cookies.md).

```
<UI>
   <CustomAuthenticationUI>
      <PassThroughCookies>
         <PassThroughCookie>sqlAuthCookie</PassThroughCookie>
      </PassThroughCookies>
   </CustomAuthenticationUI>
</UI>
```

## <a name="next-steps"></a>Étapes suivantes

[Configurer l'authentification personnalisée ou par formulaire sur le serveur de rapports](../../../reporting-services/security/configure-custom-or-forms-authentication-on-the-report-server.md)  
[Configurer le Gestionnaire de rapports pour passer des cookies d’authentification personnalisée](https://msdn.microsoft.com/library/ms345241(v=sql.120).aspx)

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
