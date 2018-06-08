---
title: Débogage du code d’extension de remise | Microsoft Docs
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
- delivery extensions [Reporting Services], debugging
- debugging delivery extensions [Reporting Services]
- troubleshooting [Reporting Services], delivery extensions
ms.assetid: a7d959da-5005-4a50-aca7-2cef36aa9947
caps.latest.revision: 35
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 72741f4ad984fde27c24518e2b2ed4758d54ac9a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="debugging-delivery-extension-code"></a>Débogage du code d'extension de remise
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] fournit plusieurs outils de débogage qui peuvent vous aider à analyser le code de vos extensions de remise et à localiser les erreurs qu’il contient. L'outil le plus approprié dépend de ce que vous essayez d'accomplir. Cet exemple utilise [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)].  
  
#### <a name="to-debug-your-delivery-extension-code"></a>Pour déboguer votre code d'extension de remise  
  
1.  Lancez [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)] et ouvrez votre projet d’extension de remise.  
  
2.  Générez le projet et déployez votre assembly d'extension de remise ainsi que le fichier .pdb associé sur le serveur de rapports et le Générateur de rapports. Pour plus d’informations sur le déploiement, consultez [Déploiement d’une extension de remise](../../../reporting-services/extensions/delivery-extension/deploying-a-delivery-extension.md).  
  
3.  Si vous avez écrit une interface utilisateur d'abonnement pour étendre le Gestionnaire de rapports, ouvrez Internet Explorer et naviguez jusqu'au Gestionnaire de rapports en laissant votre code d'extension de remise ouvert dans [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)]. Si vous n'avez pas d'interface utilisateur d'abonnement déployée pour le Gestionnaire de rapports, ouvrez simplement l'application cliente dans laquelle vous appelez votre extension de remise à l'aide de l'API SOAP.  
  
4.  Naviguez jusqu'à [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] et votre projet d'extension de remise et définissez des points d'arrêt dans votre code.  
  
5.  Le projet d’extension de remise demeurant dans la fenêtre active, cliquez sur **Attacher au processus** dans le menu **Déboguer**.  
  
     La boîte de dialogue **Attacher au processus** s’ouvre.  
  
6.  Dans la liste de processus, sélectionnez aspnet_wp.exe (ou w3wp.exe, si votre application est déployée sur IIS 6.0) et cliquez sur **Attacher**.  
  
7.  Définissez un nouvel abonnement à l'aide de votre extension de remise. Vous utiliserez très probablement le Gestionnaire de rapports ou l'API SOAP. Celle-ci doit appeler le débogueur et exécuter le code correspondant à vos points d'arrêt.  
  
8.  Exécutez le code pas à pas à l’aide de la touche **F11**. Pour plus d'informations sur l'utilisation de [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] pour le débogage, consultez votre documentation [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a> Voir aussi  
 [Implémentation d’une extension de remise](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
