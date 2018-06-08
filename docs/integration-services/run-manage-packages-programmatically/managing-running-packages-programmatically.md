---
title: Gestion des packages en cours d’exécution par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: run-manage-packages-programmatically
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- packages [Integration Services], managing
- running packages [Integration Services]
ms.assetid: 0e91f4ac-6f29-40d7-8c28-9b82e4802c35
caps.latest.revision: 19
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 12f2104f4cbbf65d73e50e6db53dd191955b5d71
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="managing-running-packages-programmatically"></a>Gestion des packages en cours d'exécution par programme
  Lors de l'utilisation de packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] par programme, il peut être utile d'identifier les packages en cours d'exécution. La classe <xref:Microsoft.SqlServer.Dts.Runtime.Application> de l'espace de noms <xref:Microsoft.SqlServer.Dts.Runtime> fournit des méthodes et des classes pour répondre à ces impératifs.  
  
 Pour plus d’informations sur la surveillance des packages, consultez [Gestion de packages &#40;Service SSIS&#41;](../../integration-services/service/package-management-ssis-service.md).  
  
 Toutes les méthodes décrites dans cette rubrique nécessitent une référence à l’assembly **Microsoft.SqlServer.ManagedDTS**. Après avoir ajouté la référence dans un nouveau projet, importez l’espace de noms <xref:Microsoft.SqlServer.Dts.Runtime> à l’aide d’une instruction **using** ou **Imports**.  
  
> [!IMPORTANT]  
>  Les méthodes de la classe <xref:Microsoft.SqlServer.Dts.Runtime.Application> qui permettent d’utiliser le magasin de packages SSIS prennent uniquement en charge « . », localhost ou le nom du serveur local. Vous ne pouvez pas utiliser « (local) ».  
  
## <a name="determining-which-packages-are-currently-running"></a>Identification des packages en cours d'exécution  
 Pour identifier les packages en cours d'exécution sur le serveur spécifié, appelez la méthode <xref:Microsoft.SqlServer.Dts.Runtime.Application.GetRunningPackages%2A>. Cette méthode retourne une collection <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackages> d'objets <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage>.  
  
> [!NOTE]  
>  Les administrateurs peuvent afficher tous les packages en cours d'exécution sur l'ordinateur ; alors que les autres utilisateurs ne voient que les packages qu'ils ont lancés.  
  
## <a name="working-with-running-packages"></a>Utilisation de packages en cours d'exécution  
 Après avoir identifié les packages en cours d'exécution, vous pouvez extraire des informations relatives aux packages et demander l'arrêt d'un package.  
  
### <a name="getting-information-about-a-running-package"></a>Obtention d'informations sur un package en cours d'exécution  
 Pendant que vous parcourez la collection <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackages>, vous pouvez utiliser les propriétés de l'objet <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage> pour rechercher un package ou obtenir des informations supplémentaires sur les packages en cours d'exécution :  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.ExecutionDuration%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.ExecutionStartTime%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.InstanceID%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageDescription%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageID%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.PackageName%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.UserName%2A>  
  
### <a name="stopping-a-running-package"></a>Arrêt d'un package en cours d'exécution  
 Vous pouvez appeler la méthode <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage.Stop%2A> d'un objet <xref:Microsoft.SqlServer.Dts.Runtime.RunningPackage> pour demander l'arrêt du package. Il peut y avoir un délai entre le moment où l'arrêt est demandé et le moment où le package s'arrête réellement.  
  
## <a name="see-also"></a> Voir aussi  
 [Gestion de packages &#40;Service SSIS&#41;](../../integration-services/service/package-management-ssis-service.md)   
 [Énumération des packages disponibles par programmation](../../integration-services/run-manage-packages-programmatically/enumerating-available-packages-programmatically.md)  
  
  
