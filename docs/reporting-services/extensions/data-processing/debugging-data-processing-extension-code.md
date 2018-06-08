---
title: Débogage du code des extensions pour le traitement des données | Microsoft Docs
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
- debugging data processing extensions [Reporting Services]
- troubleshooting [Reporting Services], data processing extensions
- data processing extensions [Reporting Services], debugging
ms.assetid: e963e205-9ae0-446d-97df-028a1d2727d9
caps.latest.revision: 40
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 2fb83b02372a5ecb7313185f18d841884ffab775
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="debugging-data-processing-extension-code"></a>Débogage du code des extensions pour le traitement des données
  Le [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] fournit plusieurs outils de débogage qui peuvent vous aider à analyser le code de vos extensions pour le traitement des données et à localiser les erreurs qu’il contient. L'outil le plus approprié dépend de ce que vous essayez d'accomplir. Cet exemple utilise [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)].  
  
#### <a name="to-debug-your-data-processing-extension-code"></a>Pour déboguer le code de votre extension pour le traitement des données  
  
1.  Lancez [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)] et ouvrez votre projet d'extension pour le traitement des données.  
  
2.  Générez le projet et déployez votre assembly d'extension pour le traitement des données ainsi que le fichier .pdb associé sur le Générateur de rapports. Pour plus d’informations sur le déploiement, consultez [Procédure : déployer une extension pour le traitement des données sur le Concepteur de rapports](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension-to-report-designer.md).  
  
3.  Ouvrez un nouveau projet de rapport dans [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] tout en laissant ouvert le code de votre extension pour le traitement des données dans une fenêtre distincte de [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].  
  
4.  Accédez à la fenêtre de [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] qui contient votre projet d'extension pour le traitement des données et définissez des points d'arrêt dans votre code.  
  
5.  La fenêtre du projet d’extension pour le traitement des données étant toujours active, cliquez sur **Attacher au processus** dans le menu **Déboguer**.  
  
     La boîte de dialogue **Attacher au processus** s’ouvre.  
  
6.  Dans la liste des processus, sélectionnez le processus devenv.exe qui correspond à votre projet de rapport, puis cliquez sur **Attacher**.  
  
7.  Définissez votre source de données de rapport à l’aide de l’onglet **Données du rapport** du projet de rapport. Vous utiliserez très probablement le Concepteur de requêtes générique pour exécuter une requête sur votre source de données personnalisée. Celle-ci doit appeler le débogueur et exécuter le code correspondant à vos points d'arrêt.  
  
8.  Exécutez le code pas à pas à l'aide de la touche F11. Pour plus d'informations sur l'utilisation de [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] pour le débogage, consultez votre documentation [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a> Voir aussi  
 [Déploiement d’une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)   
 [Extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implémentation d’une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
