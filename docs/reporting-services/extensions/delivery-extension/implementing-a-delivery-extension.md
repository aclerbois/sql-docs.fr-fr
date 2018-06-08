---
title: Implémentation d’une extension de remise | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
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
- delivery [Reporting Services]
- custom delivery extensions [Reporting Services]
- extensions [Reporting Services], delivery
- delivery extensions [Reporting Services]
ms.assetid: 600cd229-efcd-480e-8c95-3c3c39ff4e7a
caps.latest.revision: 32
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 2e508497f7429f596285717978cc6c2953c9c320
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="implementing-a-delivery-extension"></a>Implémentation d'une extension de remise
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] permet aux utilisateurs de créer et de publier des rapports qui, une fois créés et publiés, peuvent être remis à différents emplacements. De plus, [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] inclut plusieurs extensions de remise et une API de remise qui permettent aux développeurs de créer des extensions de remise supplémentaires pour étendre les fonctionnalités de remise proposées dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 Pour un exemple d’implémentation d’extension de remise, consultez [SQL Server Reporting Services Product Samples](http://go.microsoft.com/fwlink/?LinkId=177889) (Exemples Reporting Services pour le produit SQL Server).  
  
## <a name="in-this-section"></a>Dans cette section  
 [Vue d'ensemble des extensions de remise](../../../reporting-services/extensions/delivery-extension/delivery-extensions-overview.md)  
 Explique comment écrire une extension de remise personnalisée pour [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 [Préparation pour la mise en œuvre d'une extension de remise](../../../reporting-services/extensions/delivery-extension/preparing-to-implement-a-delivery-extension.md)  
 Décrit les interfaces et les classes disponibles lors de l'implémentation d'une extension de remise [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], ainsi que les problèmes à prendre en considération avant l'implémentation.  
  
 [Création d'une bibliothèque d'extensions de remise](../../../reporting-services/extensions/delivery-extension/creating-a-delivery-extension-library.md)  
 Décrit comment assigner un espace de noms à votre extension de remise [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] et comment compiler votre extension de remise dans une DLL de bibliothèque.  
  
 [Mise en œuvre de l'interface IDeliveryExtension pour une extension de remise](../../../reporting-services/extensions/delivery-extension/implementing-the-ideliveryextension-interface-for-a-delivery-extension.md)  
 Décrit les attributs d'une extension de remise et comment implémenter votre propre classe d'extension de remise.  
  
 [Utilisation d'une classe de notification pour une extension de remise](../../../reporting-services/extensions/delivery-extension/using-a-notification-class-for-a-delivery-extension.md)  
 Décrit les attributs d’une classe **Notification** et comment l’utiliser dans l’implémentation de votre extension de remise.  
  
 [Utilisation de la classe Paramètre pour une extension de remise](../../../reporting-services/extensions/delivery-extension/using-the-setting-class-for-a-delivery-extension.md)  
 Décrit les attributs d’une classe **Setting** et comment l’utiliser dans l’implémentation de votre extension de remise.  
  
 [Utilisation de l'interface IDeliveryReportServerInformation pour une extension de remise](../../../reporting-services/extensions/delivery-extension/using-the-ideliveryreportserverinformation-interface-for-a-delivery-extension.md)  
 Décrit les attributs d’une interface **IDeliveryReportServerInformation** et comment l’utiliser dans l’implémentation de votre extension de remise.  
  
 [Utilisation de la classe Rapport pour une extension de remise](../../../reporting-services/extensions/delivery-extension/using-the-report-class-for-a-delivery-extension.md)  
 Décrit les attributs d’une classe **Report** et comment l’utiliser dans l’implémentation de votre extension de remise.  
  
 [Utilisation de la classe RenderedOutputFile pour une extension de remise](../../../reporting-services/extensions/delivery-extension/using-the-renderedoutputfile-class-for-a-delivery-extension.md)  
 Décrit les attributs d’une classe **RenderedOutputFile** et comment l’utiliser dans l’implémentation de votre extension de remise.  
  
 [Implémentation de l’interface ISubscriptionBaseUIUserControl pour une extension de remise](../../../reporting-services/extensions/delivery-extension/implementing-the-isubscriptionbaseuiusercontrol-interface.md)  
 Décrit les attributs d'un contrôle utilisateur d'extension de remise et comment implémenter votre propre interface utilisateur pour un abonnement.  
  
 [Déploiement d'une extension de remise](../../../reporting-services/extensions/delivery-extension/deploying-a-delivery-extension.md)  
 Décrit comment déployer votre extension de remise.  
  
 [Débogage du code d'extension de remise](../../../reporting-services/extensions/delivery-extension/debugging-delivery-extension-code.md)  
 Décrit comment déboguer le code dans votre extension de remise.  
  
 [Suppression d'une extension de remise](../../../reporting-services/extensions/delivery-extension/removing-a-delivery-extension.md)  
 Décrit comment supprimer une extension de remise d'un serveur de rapports.  
  
## <a name="see-also"></a> Voir aussi  
 [Extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
