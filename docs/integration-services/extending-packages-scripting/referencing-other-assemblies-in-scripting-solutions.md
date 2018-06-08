---
title: Référencement d’autres assemblys dans les solutions de script | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-scripting
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
dev_langs:
- VB
helpviewer_keywords:
- SSIS Script task, .NET Framework
- Script task [Integration Services], adding references
- referencing custom assemblies
- SSIS Script task, VisualBasic namespace
- assemblies [Integration Services]
- VisualBasic namespace
- Script task [Integration Services], VisualBasic namespace
- Microsoft.VisualBasic namespace
- Script task [Integration Services], .NET Framework
- .NET Framework [Integration Services]
- referencing Web services
ms.assetid: 9b655bcd-19f6-43d8-9f89-1b4d299c6380
caps.latest.revision: 68
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b7f5fc6b9174364e223f001c1ba455b14d36f635
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="referencing-other-assemblies-in-scripting-solutions"></a>Référencement d'autres assemblys dans les solutions de script
  La bibliothèque de classes [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] fournit au développeur de scripts un ensemble d’outils performants permettant d’implémenter des fonctionnalités personnalisées dans des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. La tâche de script et le composant Script peuvent également utiliser des assemblys managés personnalisés.  
  
> [!NOTE]  
>  Pour permettre à vos packages d’utiliser les objets et méthodes d’un service web, utilisez la commande **Ajouter une référence web** disponible dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA). Dans les versions antérieures de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vous deviez générer une classe proxy pour utiliser un service Web.  
  
## <a name="using-a-managed-assembly"></a>Utilisation d'un assembly managé  
 Pour que [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] trouve un assembly managé au moment de la conception, vous devez effectuer les étapes suivantes :  
  
1.  Stockez l'assembly managé dans un dossier sur votre ordinateur.  
  
    > [!NOTE]  
    >  Dans les versions antérieures de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vous pouviez uniquement ajouter une référence à un assembly managé qui était stocké dans le dossier %windir%\Microsoft.NET\Framework\vx.x.xxxxx ou le dossier %ProgramFiles%\Microsoft SQL Server\100\SDK\Assemblies.  
  
2.  Ajoutez une référence à l'assembly managé.  
  
     Pour ajouter la référence, dans VSTA, dans la boîte de dialogue **Ajouter une référence**, sous l’onglet **Parcourir**, localisez et ajoutez l’assembly managé.  
  
 Pour que [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] recherche l'assembly managé au moment de l'exécution, vous devez procéder comme suit :  
  
1.  Signez l'assembly managé avec un nom fort.  
  
2.  Installez l'assembly dans le Global Assembly Cache sur l'ordinateur sur lequel le package est exécuté.  
  
     Pour plus d’informations, consultez [Génération, déploiement et débogage d’objets personnalisés](../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md).  
  
## <a name="using-the-microsoft-net-framework-class-library"></a>Utilisation de la bibliothèque de classes Microsoft .NET Framework  
 La tâche de script et le composant Script peuvent tirer parti de tous les autres objets et fonctionnalités exposés par la bibliothèque de classes [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Par exemple, en utilisant [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], vous pouvez extraire des informations sur votre environnement et interagir avec l'ordinateur qui exécute le package.  
  
 Cette liste décrit plusieurs classes [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] figurant parmi les plus fréquemment utilisées :  
  
-   **System.Data** Contient l’architecture ADO.NET.  
  
-   **System.IO** Fournit une interface au système et aux flux de fichiers.  
  
-   **System.Windows.Forms** Fournit la création de formulaires.  
  
-   **System.Text.RegularExpressions** Fournit des classes à utiliser avec des expressions régulières.  
  
-   **System.Environment** Renvoie des informations sur l’ordinateur local, l’utilisateur actif et les paramètres de l’ordinateur et de l’utilisateur.  
  
-   **System.Net** Fournit une communication réseau.  
  
-   **System.DirectoryServices** Expose Active Directory.  
  
-   **System.Drawing** Fournit de vastes bibliothèques de manipulation d’images.  
  
-   **System.Threading** Permet une programmation multithread.  
  
 Pour plus d'informations sur le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], consultez MSDN Library.  
  
## <a name="see-also"></a> Voir aussi  
 [Extension de packages avec des scripts](../../integration-services/extending-packages-scripting/extending-packages-with-scripting.md)  
  
  
