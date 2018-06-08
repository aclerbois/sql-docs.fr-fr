---
title: Modifier les fuseaux horaires et les paramètres d’horloge sur un serveur de rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: subscriptions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- time zones [Reporting Services]
- clocks [Reporting Services]
- schedules [Reporting Services], clock settings
- schedules [Reporting Services], time zones
ms.assetid: 69a19468-baa1-40f6-b158-8afdab0f8968
caps.latest.revision: 22
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: f9e613a2212a852f386343de6463d06b9b57b3ea
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="change-time-zones-and-clock-settings-on-a-report-server"></a>Modifier les fuseaux horaires et les paramètres d'horloge sur un serveur de rapports
  Un serveur de rapports utilise toujours l'heure locale de l'ordinateur sur lequel il est installé. Vous ne pouvez pas le configurer de manière à utiliser un autre fuseau horaire. Si une application cliente pointe vers un serveur de rapports se trouvant dans un autre fuseau horaire, c'est le fuseau horaire du serveur de rapports qui sera utilisé pour effectuer une opération planifiée. Dans les pages de gestion SharePoint et le Gestionnaire de rapports, le fuseau horaire est indiqué sur chaque page de planification de sorte que vous savez exactement à quel moment doit se produire une opération planifiée. Par exemple, la page consacrée à la création de planifications personnalisées indiquera « Les heures sont exprimées en (UTC-08:00) Heure du Pacifique (États-Unis et Canada). »  
  
## <a name="changing-the-time-zone-native-mode"></a>Modification du fuseau horaire (mode natif)  
 Si vous changez le fuseau horaire sur un ordinateur qui héberge un serveur de rapports, vous devez redémarrer le service Report Server pour que le changement de fuseau horaire soit pris en compte.  
  
 Les valeurs d'horodateur des instantanés d'historique de rapport existants sont synchronisées au nouveau paramètre de fuseau horaire. Si vous avez créé un instantané de l'historique d'un rapport à 09h00 et que vous avez ensuite avancé d'un fuseau horaire, l'horodateur qui figure sur l'instantané produit passe de 09h00 à 10h00. à 10h00.  
  
 Les planifications conservent les paramètres existants, mais ceux-ci sont mappés sur le nouveau fuseau horaire. Par exemple, si une planification s'exécute à 2h00 du matin. Heure standard du Pacifique et vous modifiez le fuseau horaire à l'heure standard est d'Australie, la planification s'exécute à 2h00 du matin Heure standard est d'Australie.  
  
 Les valeurs d'horodatage de propriété (par exemple, l'heure à laquelle un dossier ou un élément de rapport lié est créé) ne sont pas synchronisées sur un nouveau fuseau horaire. Si vous créez un élément le 25 juin à 09:00, puis réinitialisez le fuseau horaire ou l'horloge, la valeur d'horodatage demeure le 25 juin à 09:00.  
  
## <a name="changing-the-time-zone-sharepoint-mode"></a>Modification du fuseau horaire (mode SharePoint)  
 La configuration de fuseau horaire pour le mode [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint est gérée dans le cadre des paramètres régionaux de SharePoint. Pour plus d'informations, consultez [Paramètres régionaux (SharePoint Server 2010 (http://technet.microsoft.com/library/cc824907.aspx)](http://technet.microsoft.com/library/cc824907.aspx).  
  
## <a name="changing-the-clock-settings"></a>Modification des paramètres d'horloge  
 Le changement de l'heure de l'horloge de l'ordinateur est sans effet sur les valeurs d'horodatage existantes (par exemple, si vous avancez l'horloge d'une heure, les valeurs d'horodatage des instantanés d'historique de rapport ne changent pas). On peut constater un délai de 10 secondes avant que le processeur de planification et de remise utilise le nouveau paramètre. Le délai véritable peut varier si vous avez modifié les paramètres de fréquence d'interrogation dans les fichiers de configuration.  
  
## <a name="see-also"></a> Voir aussi  
 [Démarrer et arrêter le service Report Server](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)   
 [Planifications](../../reporting-services/subscriptions/schedules.md)  
  
  
