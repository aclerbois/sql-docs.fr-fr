---
title: Gestionnaire de connexions Excel | Microsoft Docs
ms.date: 04/02/2018
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: ''
ms.suite: sql
ms.custom: ''
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.excelconnection.f1
helpviewer_keywords:
- files [Integration Services], connections
- connections [Integration Services], Excel
- Excel [Integration Services]
- connection managers [Integration Services], Excel
ms.assetid: 667419f2-74fb-4b50-b963-9197d1368cda
caps.latest.revision: 41
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 744705953ad52d2582c4790359d6f5b8e35f018a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="excel-connection-manager"></a>Gestionnaire de connexions Excel
  Un gestionnaire de connexions Excel permet à un package de se connecter à un fichier de classeur [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel. La source et la destination Excel incluses dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilisent le gestionnaire de connexions Excel.  
 
> [!IMPORTANT]
> Pour obtenir des informations détaillées sur la connexion à des fichiers Excel, et sur les limitations et les problèmes connus liés au chargement de données depuis ou vers des fichiers Excel, consultez [Charger des données depuis ou vers Excel avec SQL Server Integration Services (SSIS)](../load-data-to-from-excel-with-ssis.md).

 Quand vous ajoutez un gestionnaire de connexions Excel à un package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un gestionnaire de connexions qui est converti en connexion Excel au moment de l’exécution, définit les propriétés du gestionnaire de connexions et ajoute le gestionnaire de connexions à la collection **Connections** du package.  
  
 La propriété **ConnectionManagerType** du gestionnaire de connexions a pour valeur **EXCEL**.  
  
## <a name="configure-the-excel-connection-manager"></a>Configurer le gestionnaire de connexions Excel  
 Vous pouvez configurer le gestionnaire de connexions Excel de plusieurs manières :  
  
-   Spécifiez le chemin d'accès du fichier Excel.  
  
-   Spécifiez la version d'Excel utilisée pour créer le fichier.  
  
-   Indiquez si la première ligne de données dans les feuilles de calcul ou plages sélectionnées contient des noms de colonnes.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d’informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Éditeur du gestionnaire de connexions Excel](../../integration-services/connection-manager/excel-connection-manager-editor.md).  
  
 Pour plus d’informations sur la configuration d’un gestionnaire de connexions par programmation, consultez <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> et [Ajout de connexions par programme](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="excel-connection-manager-editor"></a>Éditeur du gestionnaire de connexions Excel
  La boîte de dialogue **Éditeur du gestionnaire de connexions Excel** vous permet d'ajouter une connexion à un classeur [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] existant ou nouveau.  
  
### <a name="options"></a>Options  
 **Chemin de fichier Excel**  
 Tapez le chemin et le nom d’un fichier de classeur Excel existant ou nouveau.  
   
 **Parcourir**  
 Utilisez la boîte de dialogue **Ouvrir** pour accéder au dossier contenant le fichier Excel existant ou au dossier où vous souhaitez créer le fichier.  
  
 **Version Excel**  
 Spécifiez la version de Microsoft Excel qui a été utilisée pour créer le fichier.  
  
 **La première ligne possède des noms de colonnes**  
 Permet de spécifier si la première ligne de données dans la feuille de calcul sélectionnée contient les noms des colonnes. La valeur par défaut de cette option est **True**.  
  
## <a name="related-tasks"></a>Related Tasks  
[Charger des données depuis ou vers Excel avec SQL Server Integration Services (SSIS)](../load-data-to-from-excel-with-ssis.md)  
[Source Excel](../data-flow/excel-source.md)  
[Destination Excel](../data-flow/excel-destination.md)
