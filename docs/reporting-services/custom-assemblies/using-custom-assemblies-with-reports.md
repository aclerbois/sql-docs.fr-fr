---
title: Utilisation d’assemblages personnalisés avec des rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.component: custom-assemblies
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- custom assemblies [Reporting Services]
- assemblies [Reporting Services], custom
- custom assemblies [Reporting Services], about custom assemblies
ms.assetid: 53d141d0-2185-466a-84dc-7b90d284da3d
caps.latest.revision: 32
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 4fc146a14a8237f6def33a0d53acc529cf82780f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-custom-assemblies-with-reports"></a>Utilisation d'assemblys personnalisés avec des rapports
  Dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vous pouvez écrire du code personnalisé pour les valeurs, les styles et la mise en forme des éléments de rapport. Par exemple, vous pouvez utiliser du code personnalisé pour mettre en forme les devises en fonction de paramètres régionaux, marquer certaines valeurs avec une mise en forme spéciale ou appliquer d'autres règles d'entreprise en pratique dans votre société. L’un des moyens d’inclure ce code dans vos rapports consiste à créer un assembly de code personnalisé à l’aide du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] que vous pouvez référencer depuis vos fichiers de définition de rapport. Le serveur appelle les fonctions de votre assembly personnalisé lors de l'exécution d'un rapport. Les assemblys personnalisés peuvent être utilisés pour extraire des fonctions spéciales que vous envisagez d'utiliser dans vos rapports.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Référencement d'assemblages dans un fichier RDL](../../reporting-services/custom-assemblies/referencing-assemblies-in-an-rdl-file.md)  
 Décrit comment référencer vos assemblys personnalisés dans un fichier RDL (Report Definition Language File).  
  
 [Déploiement d'un assembly personnalisé](../../reporting-services/custom-assemblies/deploying-a-custom-assembly.md)  
 Décrit comment déployer un assembly personnalisé sur le Concepteur de rapports et le serveur de rapports.  
  
 [Utilisation d'assemblages personnalisés avec noms forts](../../reporting-services/custom-assemblies/using-strong-named-custom-assemblies.md)  
 Décrit comment utiliser des assemblys personnalisés avec des noms forts.  
  
 [Déclaration d'autorisations dans les assemblages personnalisés](../../reporting-services/custom-assemblies/asserting-permissions-in-custom-assemblies.md)  
 Décrit comment déployer des assemblys personnalisés avec des autorisations limitées et spécifiques, et comment déclarer ces autorisations dans le code.  
  
 [Accès aux assemblages personnalisés par le biais d'expressions](../../reporting-services/custom-assemblies/accessing-custom-assemblies-through-expressions.md)  
 Décrit comment appeler des méthodes d'assembly personnalisé sous la forme d'expressions de rapport dans vos définitions de rapport.  
  
 [Initialisation d'objets d’assemblage personnalisés](../../reporting-services/custom-assemblies/initializing-custom-assembly-objects.md)  
 Décrit comment initialiser les valeurs des objets d'assembly personnalisé appelés à partir d'un rapport.  
  
 [Procédure : déboguer des assemblages personnalisés](../../reporting-services/custom-assemblies/how-to-debug-custom-assemblies.md)  
 Décrit comment déboguer le code de votre assembly personnalisé.  
  
## <a name="see-also"></a> Voir aussi  
 [RDL (Report Definition Language) &#40;SSRS&#41;](../../reporting-services/reports/report-definition-language-ssrs.md)  
  
  
