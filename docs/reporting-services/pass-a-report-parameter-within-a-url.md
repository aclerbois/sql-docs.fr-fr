---
title: Passer un paramètre de rapport dans une URL | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: reporting-services
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: article
helpviewer_keywords:
- URL access [Reporting Services], passing parameters
- passing parameters [Reporting Services]
ms.assetid: f93a94cc-27b5-435a-aa85-69e6ec6459ad
caps.latest.revision: 36
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 4f7efcc4ff59aa48e09ee1e053a9f9522fad49cb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pass-a-report-parameter-within-a-url"></a>Passer un paramètre de rapport dans une URL
  Vous pouvez passer des paramètres de rapport à un rapport en les incluant dans une URL de rapport. Ces paramètres URL ne sont pas préfixés parce qu'ils sont directement passés au moteur de traitement des rapports.  
  
> [!IMPORTANT]  
>  Il est important que l'URL inclue la syntaxe de proxy `_vti_bin` pour acheminer la requête via SharePoint et le proxy HTTP [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . Le proxy ajoute à la requête HTTP le contexte nécessaire pour garantir une exécution correcte du rapport pour les serveurs de rapports en mode SharePoint.  
>   
>  Si vous n’incluez pas la syntaxe de proxy, vous devez faire précéder le paramètre de *rp:*.  
  
 Tous les paramètres de requête peuvent avoir des paramètres de rapport correspondants. Vous passez un paramètre de requête à un rapport en transmettant le paramètre de rapport correspondant. Pour plus d’informations, consultez [Générer une requête dans le Concepteur de requêtes relationnelles &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-data/build-a-query-in-the-relational-query-designer-report-builder-and-ssrs.md).  
  
> [!IMPORTANT]  
>  Les paramètres de rapport respectent la casse.  
  
> [!NOTE]  
>  Les paramètres de rapport respectent la casse et utilisent les caractères spéciaux suivants :  
>   
>  -   Tout espace figurant dans la chaîne d'URL est remplacé par le caractère « % 20 », conformément aux normes d'encodage des URL.  
> -   Un espace dans la partie Paramètre de l'URL est remplacé par un caractère Plus (+).  
> -   Un point-virgule dans toute partie de la chaîne est remplacé par les caractères « %3A ».  
> -   Les navigateurs doivent effectuer automatiquement l'encodage d'URL approprié. Vous n'avez pas besoin d'encoder manuellement les caractères.  
  
 Pour définir un paramètre de rapport au sein d'une URL, utilisez la syntaxe suivante :  
  
```  
  
parameter=value  
```  
  
 Par exemple, pour spécifier deux paramètres, « ReportMonth » et « ReportYear », définis dans un rapport, utilisez l'URL suivante pour un serveur de rapports en mode natif :  
  
```  
http://myrshost/ReportServer?/AdventureWorks 2008R2/Employee_Sales_Summary_2008R2&ReportMonth=3&ReportYear=2008  
```  
  
 Par exemple, pour spécifier les deux paramètres définis dans un rapport, utilisez l'URL suivante pour un serveur de rapports SharePoint intégré : Notez le `/_vti_bin`:  
  
```  
http://myspsite/subsite/_vti_bin/reportserver?http://myspsite/subsite/AdventureWorks 2008R2/Employee_Sales_Summary_2008R2.rdl&ReportMonth=3&ReportYear=2008  
```  
  
 Pour passer une valeur NULL pour un paramètre, utilisez la syntaxe suivante :  
  
```  
  
parameter  
:isnull=true  
  
```  
  
 Par exemple,  
  
```  
SalesOrderNumber:isnull=true  
```  
  
 Pour passer une valeur **booléenne** , utilisez 0 pour false et 1 pour true. Pour passer une valeur **flottante** , incluez le séparateur décimal des paramètres régionaux du serveur  
  
> [!NOTE]  
>  Si votre rapport contient un paramètre de rapport qui a une valeur par défaut et que la valeur de la propriété **Demander** est **false** (autrement dit, si la propriété Demander à l’utilisateur n’est pas sélectionnée dans le Gestionnaire de rapports), vous ne pouvez pas passer de valeur pour ce paramètre de rapport dans une URL. Cela permet aux administrateurs d'empêcher les utilisateurs finaux d'ajouter ou de modifier les valeurs de certains paramètres de rapport.  
  
##  <a name="bkmk_examples"></a> Autres exemples  
 L'exemple d'URL suivante comprend des espaces et plusieurs paramètres  
  
-   Le nom de dossier « SQL Server User Education Team » comprend des espaces ; par conséquent, un « + » remplace chaque espace.  
  
-   Le nom de rapport « team project report » comprend des espaces ; par conséquent, un « + » remplace chaque espace.  
  
-   Passe deux paramètres de « teamgrouping2 » avec la valeur « xgroup » et « teamgrouping1 » avec la valeur « ygroup ».  
  
```  
https://myserver/Reportserver?/SQL+Server+User+Education+Team/_ContentTeams/folder123/team+project+report&teamgrouping2=xgroup&teamgrouping1=ygroup  
```  
  
 L'exemple d'URL suivante comprend un paramètre à valeurs multiples nommé OrderID. Le format d'un paramètre à valeurs multiples consiste à répéter le nom du paramètre pour chaque valeur.  
  
```  
https://myserver/Reportserver?/SQL+Server+User+Education+Team/_ContentTeams/folder123/team+project+report&teamgrouping2=xgroup&teamgrouping1=ygroup&OrderID=747&OrderID=787&OrderID=12  
```  
  
 L’exemple d’URL suivant passe un seul paramètre de *SellStartDate* avec une valeur « 7/1/2005 » pour un serveur de rapports en mode natif.  
  
```  
http://myserver/ReportServer/Pages/ReportViewer.aspx?%2fProduct_and_Sales_Report_AdventureWorks&SellStartDate=7/1/2005  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Accès URL &#40;SSRS&#41;](../reporting-services/url-access-ssrs.md)   
 [Référence de paramètres d'accès URL](../reporting-services/url-access-parameter-reference.md)  
  
  
