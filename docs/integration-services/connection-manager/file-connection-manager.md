---
title: Gestionnaire de connexions de fichiers | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.fileconnectionmanager.f1
helpviewer_keywords:
- folders [Integration Services], connections
- files [Integration Services], connections
- files [Integration Services]
- connection managers [Integration Services], File
- connections [Integration Services], files
- File connection manager
ms.assetid: 019078bc-44ee-4975-9169-0f9a89e3f3be
caps.latest.revision: 50
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 465a881b9a999331596bcbbc2329406e0ff5fe65
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="file-connection-manager"></a>Gestionnaire de connexions de fichiers
  Un gestionnaire de connexions de fichiers permet à un package de référencer un fichier ou dossier existant ou de créer un fichier ou dossier au moment de l'exécution. Par exemple, vous pouvez référencer un fichier Excel. Certains composants de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilisent les informations figurant dans les fichiers pour réaliser leur travail. Par exemple, une tâche d'exécution SQL peut référencer un fichier contenant les instructions SQL exécutées par la tâche. D'autres composants exécutent des opérations sur les fichiers. Par exemple, la tâche de système de fichiers peut référencer un fichier pour le copier à un nouvel emplacement.  
  
## <a name="usage-types-of-the-file-connection-manager"></a>Types d'utilisations du gestionnaire de connexions de fichiers  
 La propriété **FileUsageType** du gestionnaire de connexions de fichiers spécifie la manière dont la connexion de fichiers est utilisée. Le gestionnaire de connexions de fichiers peut créer un fichier, créer un dossier, utiliser un fichier existant ou utiliser un dossier existant.  
  
 Le tableau suivant répertorie les valeurs de **FileUsageType**.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Le gestionnaire de connexions de fichiers utilise un fichier existant.|  
|**1**|Le gestionnaire de connexions de fichiers crée un fichier.|  
|**2**|Le gestionnaire de connexions de fichiers utilise un dossier existant.|  
|**3**|Le gestionnaire de connexions de fichiers crée un dossier.|  
  
## <a name="multiple-file-or-folder-connections"></a>Connexions de fichiers ou de dossiers multiples  
 Le gestionnaire de connexions de fichiers peut référencer un seul fichier ou dossier. Pour référencer plusieurs fichiers ou dossiers, utilisez un gestionnaire de connexions de fichiers multiples au lieu du gestionnaire de connexions de fichiers. Pour plus d’informations, consultez [Gestionnaire de connexions de fichiers multiples](../../integration-services/connection-manager/multiple-files-connection-manager.md).  
  
## <a name="configuration-of-the-file-connection-manager"></a>Configuration du gestionnaire de connexions de fichiers  
 Lorsque vous ajoutez un gestionnaire de connexions de fichiers à un package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un gestionnaire de connexions qui sera converti en connexion de fichiers au moment de l’exécution, définit les propriétés de la connexion de fichiers et ajoute la connexion de fichiers à la collection **Connections** du package.  
  
 La propriété **ConnectionManagerType** du gestionnaire de connexions a pour valeur **FILE**.  
  
 Vous pouvez configurer un gestionnaire de connexions de fichiers de plusieurs manières :  
  
-   Spécifiez le type d'utilisation.  
  
-   Spécifiez un fichier ou un dossier.  
  
 Vous pouvez définir la propriété ConnectionString du gestionnaire de connexions de fichiers en spécifiant une expression dans la fenêtre Propriétés de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Toutefois, pour éviter une erreur de validation lorsque vous utilisez une expression pour spécifier le fichier ou dossier, dans **l’Éditeur du gestionnaire de connexions de fichiers**, pour **Fichier/Dossier**, ajoutez un chemin de fichier ou de dossier.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d’informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Éditeur du gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager-editor.md).  
  
 Pour plus d’informations sur la configuration d’un gestionnaire de connexions par programmation, consultez <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> et [Ajout de connexions par programme](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="file-connection-manager-editor"></a>Éditeur du gestionnaire de connexions de fichiers
  Utilisez la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers** pour spécifier les propriétés à utiliser pour se connecter à un fichier ou à un dossier.  
  
> [!NOTE]  
>  Vous pouvez définir la propriété ConnectionString du gestionnaire de connexions de fichiers en spécifiant une expression dans la fenêtre Propriétés de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Toutefois, pour éviter une erreur de validation quand vous utilisez une expression pour spécifier le fichier ou dossier, dans **l’Éditeur du gestionnaire de connexions de fichiers**, pour **Fichier/Dossier**, ajoutez un chemin du fichier ou du dossier.  
  
 Pour en savoir plus sur le gestionnaire de connexions de fichiers, consultez [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Type d'utilisation**  
 Indiquez si le **Gestionnaire de connexions de fichiers** se connecte à un fichier ou dossier existant ou s’il crée un nouveau fichier ou dossier.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Créer un fichier|Crée un nouveau fichier au moment de l'exécution.|  
|Fichier existant|Utilise un fichier existant.|  
|Créer un dossier|Crée un nouveau dossier au moment de l'exécution.|  
|Dossier existant|Utilise un dossier existant.|  
  
 **Fichier / Dossier**  
 Si **Fichier**, spécifiez le fichier à utiliser.  
  
 Si **Dossier**, spécifiez le dossier à utiliser.  
  
 **Parcourir**  
 Sélectionnez le fichier ou le dossier à l’aide de la boîte de dialogue **Sélectionner un fichier** ou **Rechercher un dossier** .  
  
  
