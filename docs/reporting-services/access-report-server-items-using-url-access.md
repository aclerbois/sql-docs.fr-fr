---
title: Accéder à des éléments de serveurs de rapports à l’aide de l’accès URL | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- referencing URL items for report server access
- URL access [Reporting Services], report servers
ms.assetid: a58b4ca6-129d-45e9-95c7-e9169fe5bba4
caps.latest.revision: 40
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: edd381205ea4cb5f8ae3709543d4c6a50e8933df
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="access-report-server-items-using-url-access"></a>Accéder à des éléments de serveur de rapports à l'aide de l'accès URL
  Cette rubrique explique comment accéder aux éléments du catalogue de types différents dans une base de données du serveur de rapports ou dans un site SharePoint en utilisant *rs:Command*=*Value*. Il n'est pas nécessaire d'ajouter cette chaîne de paramètres. Si vous l'omettez, le serveur de rapports évalue le type d'élément et sélectionne automatiquement la valeur du paramètre appropriée. Toutefois, l’utilisation de la chaîne *rs:Command*=*Valeur* dans l’URL améliore les performances du serveur de rapports.  
  
 Notez la syntaxe de proxy `_vti_bin` dans les exemples ci-dessous. Pour plus d’informations sur l’utilisation de la syntaxe de proxy, consultez [Informations de référence sur les paramètres d’accès URL](../reporting-services/url-access-parameter-reference.md).  
  
## <a name="access-a-report"></a>Accéder à un rapport  
 Pour afficher un rapport dans le navigateur, utilisez le paramètre *rs:Command*=*Render* . Exemple :  
  
 - **Natif** `http://myrshost/reportserver?/Sales/YearlySalesByCategory&rs:Command=Render`  
 - **SharePoint** `http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales/YearlySalesByCategory&rs:Command=Render`  
  
> [!TIP]  
>  Il est important que l'URL inclue la syntaxe de proxy `_vti_bin` pour acheminer la requête via SharePoint et le proxy HTTP [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . Le proxy ajoute à la requête HTTP le contexte nécessaire pour garantir une exécution correcte du rapport pour les serveurs de rapports en mode SharePoint.  
  
## <a name="access-a-resource"></a>Accéder à une ressource  
 Pour accéder à une ressource, utilisez le paramètre *rs:Command*=*GetResourceContents* . Si la ressource est compatible avec le navigateur (une image par exemple), elle est ouverte dans le navigateur. Sinon, vous êtes invité à ouvrir ou enregistrer le fichier ou la ressource sur le disque.  
  
 **Natif** `http://myrshost/reportserver?/Sales/StorePicture&rs:Command=GetResourceContents`  
  
 **SharePoint** `http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales/StorePicture.jpg&rs:Command=GetResourceContents`  
  
## <a name="access-a-data-source"></a>Accéder à une source de données  
 Pour accéder à une source de données, utilisez le paramètre *rs:Command*=*GetDataSourceContents* . Si votre navigateur prend en charge XML, la définition de la source de données est affichée si vous êtes un utilisateur authentifié avec l’autorisation **Read Contents** sur la source des données. Exemple :  
  
 **Natif** `http://myrshost/reportserver?/Sales/AdventureWorks2012&rs:Command=GetDataSourceContents`  
  
 **SharePoint** `http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales/AdventureWorks2012&rs:Command=GetDataSourceContents`  
  
 La structure XML peut ressembler à l'exemple suivant :  
  
```  
<DataSourceDefinition>  
   <Extension>SQL</Extension>  
   <ConnectString>Provider=SQLOLEDB.1;Integrated Security=SSPI;Persist Security Info=False;Initial Catalog=AdventureWorks2012;Data Source=MYSERVER1;</ConnectString>  
   <CredentialRetrieval>Integrated</CredentialRetrieval>  
   <WindowsCredentials>False</WindowsCredentials>  
   <ImpersonateUser>False</ImpersonateUser>  
   <Prompt />  
   <Enabled>True</Enabled>  
</DataSourceDefinition>  
```  
  
 La chaîne de connexion est retournée selon le paramètre **SecureConnectionLevel** du serveur de rapports. Pour plus d’informations sur le paramètre **SecureConnectionLevel** , consultez [Utilisation des méthodes de service web sécurisées](../reporting-services/report-server-web-service/net-framework/using-secure-web-service-methods.md).  
  
## <a name="access-the-contents-of-a-folder"></a>Accéder au contenu d'un dossier  
 Pour accéder au contenu d’un dossier, utilisez le paramètre *rs:Command*=*GetChildren* . Il retourne une page générique de navigation des dossiers qui contient des liens vers les sous-dossiers, rapports, sources de données et ressources dans le dossier demandé. Exemple :  
  
 **Natif** `http://myrshost/reportserver?/Sales&rs:Command=GetChildren`  
  
 **SharePoint** `http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/Sales&rs:Command=GetChildren`  
  
 L'interface utilisateur qui s'affiche est similaire au mode d'exploration de répertoires utilisé par [!INCLUDE[msCoName](../includes/msconame-md.md)] Internet Information Server (IIS). Le numéro de version, y compris le numéro de build spécifique, du serveur de rapports est aussi affiché sous la liste des dossiers.  
  
## <a name="see-also"></a> Voir aussi  
 [Accès URL &#40;SSRS&#41;](../reporting-services/url-access-ssrs.md)   
 [Référence de paramètres d'accès URL](../reporting-services/url-access-parameter-reference.md) 
