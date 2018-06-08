---
title: Options de sauvegarde | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 44251ff0abf155564102f0cf94ca8cd998718462
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="backup-options"></a>Options de sauvegarde
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez sauvegarder les bases de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de différentes manières, mais toutes les méthodes nécessitent des autorisations d’administration de serveur ou de base de données. Vous pouvez ouvrir la boîte de dialogue **Sauvegarder** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], sélectionner les options appropriées et effectuer la sauvegarde dans la boîte de dialogue elle-même. Ou bien, vous pouvez créer un script en utilisant les paramètres définis dans le fichier, le script pouvant être enregistré et exécuté aussi souvent que nécessaire.  
  
## <a name="backup-and-synchronize"></a>Sauvegarde et synchronisation  
 Si la base de données se trouve sur une instance distante [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vous pouvez utiliser la fonction de synchronisation pour sauvegarder la base de données sur l’instance locale. Les versions de développement d'une base de données peuvent être transférées vers l'environnement de production de cette manière. Vous pouvez également utiliser la sauvegarde et la restauration de fichiers classiques pour transférer la version de développement vers l'environnement de production, mais la synchronisation fournit des fonctions supplémentaires. Vous pouvez, par exemple, disposer de paramètres de sécurité différents pour les ordinateurs de développement et de production ; la synchronisation vous donne la possibilité de gérer ces paramètres et de synchroniser tous les objets autres que les rôles. En outre, la synchronisation met généralement à jour ces objets, qui sont différents pour les ordinateurs sources et les ordinateurs de destination, de manière incrémentielle. Ce type de sauvegarde incrémentielle n'est pas disponible avec la fonction de sauvegarde/restauration. Pour plus d’informations, consultez [Synchroniser des bases de données Analysis Services](../../analysis-services/multidimensional-models/synchronize-analysis-services-databases.md).  
  
> [!IMPORTANT]  
>  Le compte de service Analysis Services doit avoir l'autorisation d'écrire dans l'emplacement de sauvegarde spécifié pour chaque fichier. Par ailleurs, l'utilisateur doit avoir l'un des rôles suivants : rôle d'administrateur sur l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou membre d'un rôle de base de données avec les autorisations de contrôle total (Administrateur) sur la base de données à sauvegarder.  
  
## <a name="see-also"></a>Voir aussi  
 [Boîte de dialogue de sauvegarde de la base de données & #40 ; Analysis Services - données multidimensionnelles & #41 ;](http://msdn.microsoft.com/library/7811ce7d-6c37-4189-bfa6-ef36fb4932db)   
 [Sauvegarde et restauration de bases de données Analysis Services](../../analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases.md)   
 [Élément Backup & #40 ; XMLA & #41 ;](../../analysis-services/xmla/xml-elements-commands/backup-element-xmla.md)   
 [Sauvegarde, restauration et synchronisation de bases de données & #40 ; XMLA & #41 ;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/backing-up-restoring-and-synchronizing-databases-xmla.md)  
  
  
