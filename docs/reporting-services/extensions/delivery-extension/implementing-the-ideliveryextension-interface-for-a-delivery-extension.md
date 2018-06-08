---
title: Mise en œuvre de l’interface IDeliveryExtension pour une extension de remise | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: extensions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- delivery extensions [Reporting Services], attributes
- delivery extensions [Reporting Services], class creation
- IDeliveryExtension interface
ms.assetid: ab0344db-510b-403f-8dbf-b9831553765d
caps.latest.revision: 37
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: d3456d06bf5a2e93bba7133ec794e93950e26d18
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="implementing-the-ideliveryextension-interface-for-a-delivery-extension"></a>Implémentation de l'interface IDeliveryExtension pour une extension de remise
  Votre classe d'extension de remise sert à remettre des notifications de rapport aux utilisateurs selon le contenu des notifications. La classe d'extension de remise fournit également l'infrastructure pour valider des paramètres utilisateurs passés à l'extension de remise. De plus, votre classe d'extension de remise doit contenir des propriétés spécifiques que les clients peuvent utiliser pour obtenir des informations sur le nom de l'extension, les paramètres pris en charge par l'extension, et les formats de rendu disponibles pour l'extension de remise.  
  
 ![Processus d’interface IDeliveryExtension](../../../reporting-services/extensions/delivery-extension/media/bk-ext-02.gif "Processus d’interface IDeliveryExtension")  
L'interface IDeliveryExtension autorise la validation des données utilisateur et permet aux clients de s'informer des paramètres de remise requis.  
  
 Pour créer une classe d'extension de remise, implémentez <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension> et <xref:Microsoft.ReportingServices.Interfaces.IExtension>. L’interface **IDeliveryExtension** permet à votre extension de remise de remettre des notifications de rapport à l’aide de la méthode <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension.Deliver%2A> et de valider des paramètres d’extension entrants à l’aide de la méthode <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension.ValidateUserData%2A>. L’interface **IExtension** permet à votre extension de remise d’implémenter un nom d’extension localisé et de traiter des informations de configuration spécifiques à l’extension stockées dans le fichier de configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. En implémentant **IExtension**, votre extension de remise contient la propriété <xref:Microsoft.ReportingServices.Interfaces.Extension.LocalizedName%2A>. Il est fortement recommandé que les extensions de remise [!INCLUDE[ssRS](../../../includes/ssrs-md.md)] prennent en charge la propriété **LocalizedName**, afin que les utilisateurs soient confrontés à un nom familier pour l’extension dans une interface utilisateur, telle que Gestionnaire de rapports.  
  
 Votre extension de remise doit également implémenter la propriété **ExtensionSettings** de l’interface **IDeliveryExtension**. Le serveur de rapports utilise la valeur retournée par la propriété <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension.ExtensionSettings%2A> pour évaluer les paramètres qu'une extension de remise requiert. Les clients qui interagissent avec les extensions de remise utilisent la méthode <xref:ReportService2010.ReportingService2010.GetExtensionSettings%2A> du service Web Report Server pour retourner une liste de paramètres pour l'extension de remise.  
  
 Vous pouvez également utiliser votre classe d'extension de remise pour extraire et traiter des données de configuration personnalisées stockées dans le fichier RSReportServer.config. Pour plus d'informations sur le traitement des données de configuration personnalisées, consultez la méthode <xref:Microsoft.ReportingServices.Interfaces.IExtension.SetConfiguration%2A>.  
  
 Pour un exemple d’implémentation de la classe **IDeliveryExtension**, consultez [SQL Server Reporting Services Product Samples](http://go.microsoft.com/fwlink/?LinkId=177889) (Exemples Reporting Services pour le produit SQL Server).  
  
## <a name="see-also"></a> Voir aussi  
 [Implémentation d’une extension de remise](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
