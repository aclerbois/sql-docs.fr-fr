---
title: Définir la langue des paramètres de rapport dans une URL | Microsoft Docs
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
- overriding report language settings
- report servers [Reporting Services], language settings
- languages [Reporting Services]
- URL access [Reporting Services], language overrides
- international considerations [Reporting Services]
- global considerations [Reporting Services]
ms.assetid: e1ccf22f-80d6-45bc-aae0-f5f263275092
caps.latest.revision: 29
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 51392ff1c0aac9c8c4d8297f1e9735fb7c0b2216
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-the-language-for-report-parameters-in-a-url"></a>Définir la langue des paramètres de rapport dans une URL
  Le paramètre de l’accès URL *rs:ParameterLanguage* permet d’atténuer un problème dans lequel des paramètres de rapports liés à la culture, tels que les dates, les heures, les monnaies et les nombres, sont interprétés à l’aide de la langue du navigateur. Avec *rs:ParameterLanguage*, l’URL est à présent interprétée indépendamment du navigateur. Par exemple, si le serveur de rapports est défini sur un paramètre régional allemand, mais qu'un utilisateur accède à un rapport via une URL en utilisant un navigateur défini sur l'anglais américain, les valeurs des paramètres transmises à un serveur de rapports sont mal interprétées.  
  
 Considérez l'URL suivante d'un rapport :  
  
```  
http://myrshost/Reportserver?/SampleReports/Product+Line+Sales&rs:Command=Render&StartDate=4/10/2008&EndDate=11/10/2008  
```  
  
 Dans le cas ci-dessus, le serveur, s'exécutant sous une culture « de-de », génère une URL via un abonnement par courrier électronique ou un lien hypertexte. Le lien hypertexte indique que le rapport doit être paramétré avec une date de début du 4 octobre 2008 et une date de fin du 11 octobre 2008 d'après le format allemand de date/heure standard. Toutefois, un utilisateur qui accède à l'URL via un navigateur défini sur « en-us » oblige le serveur à interpréter ces valeurs comme le 10 avril 2008 et le 10 novembre 2008, selon le format standard de date/heure de l'anglais américain. Pour résoudre ce problème, vous pouvez utiliser *rs:ParameterLanguage* pour remplacer la langue du navigateur afin d’interpréter le paramètre :  
  
```  
http://myrshost/Reportserver?/SampleReports/Product+Line+Sales&rs:Command=Render&StartDate=4/10/2008&EndDate=11/10/2008&rs:ParameterLanguage=de-DE  
```  
  
 Outre une valeur **true** et **false** pour le paramètre de l’accès URL *rc:Parameters*, vous pouvez à présent passer une valeur **Collapsed**. Quand vous utilisez *rc:Parameters*=**Collapsed** sur une URL, la zone d’invite des paramètres de la Visionneuse HTML est réduite, mais l’utilisateur peut encore basculer vers elle. La valeur **false** supprime la zone d’invite des paramètres de la barre d’outils de la Visionneuse HTML et la rend indisponible pour l’utilisateur final.  
  
## <a name="see-also"></a> Voir aussi  
 [Accès URL &#40;SSRS&#41;](../reporting-services/url-access-ssrs.md)   
 [Référence de paramètres d'accès URL](../reporting-services/url-access-parameter-reference.md)  
  
  
