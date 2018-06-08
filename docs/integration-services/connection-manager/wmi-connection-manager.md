---
title: Gestionnaire de connexions WMI | Microsoft Docs
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
- sql13.dts.designer.wmiconnection.f1
helpviewer_keywords:
- connections [Integration Services], WMI
- connection managers [Integration Services], WMI
- WMI connection manager [Integration Services]
ms.assetid: fbfa4ba7-3d0d-4d6b-94ad-50741a88d03d
caps.latest.revision: 39
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fd6db991c0d9df908b781065d3ca65cd8d0b03c9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="wmi-connection-manager"></a>Gestionnaire de connexions WMI
  Un gestionnaire de connexions WMI permet à un package d'utiliser WMI (Windows Management Instrumentation) pour gérer des informations dans un environnement d'entreprise. La tâche de service web incluse dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise un gestionnaire de connexions WMI.  
  
 Quand vous ajoutez un gestionnaire de connexions WMI à un package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un gestionnaire de connexions qui sera résolu en une connexion WMI au moment de l’exécution, définit les propriétés du gestionnaire de connexions et ajoute le gestionnaire de connexions à la collection **Connexions** sur le package. La propriété **ConnectionManagerType** du gestionnaire de connexions est définie sur **WMI**.  
  
## <a name="configuration-of-the-wmi-connection-manager"></a>Configuration du gestionnaire de connexions WMI  
 Vous pouvez configurer un gestionnaire de connexions WMI de plusieurs manières :  
  
-   Spécifiez le nom d'un serveur.  
  
-   Spécifiez un espace de noms sur le serveur.  
  
-   Sélectionnez le mode d'authentification pour la connexion au serveur.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d’informations sur les propriétés que vous pouvez définir dans le Concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Éditeur du gestionnaire de connexions WMI](../../integration-services/connection-manager/wmi-connection-manager-editor.md).  
  
 Pour plus d’informations sur la configuration d’un gestionnaire de connexions par programmation, consultez <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> et [Ajout de connexions par programme](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="wmi-connection-manager-editor"></a>Éditeur du gestionnaire de connexions WMI
  Utilisez la boîte de dialogue **Gestionnaire de connexions WMI** pour spécifier une connexion Microsoft Windows Management Instrumentation (WMI) à un serveur.  
  
 Pour en savoir plus sur le gestionnaire de connexions WMI, consultez [WMI Connection Manager](../../integration-services/connection-manager/wmi-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Nom**  
 Donnez un nom unique au gestionnaire de connexions.  
  
 **Description**  
 Décrivez le gestionnaire de connexions. Il est recommandé d'indiquer ici l'usage auquel le gestionnaire de connexions est destiné, de sorte que les packages soient correctement documentés et plus faciles à gérer.  
  
 **Nom du serveur**  
 Fournissez le nom du serveur pour lequel vous souhaitez établir la connexion WMI.  
  
 **Espace de noms**  
 Spécifiez l'espace de noms WMI.  
  
 **Utiliser l’authentification Windows**  
 Sélectionnez cette option pour utiliser l'authentification Windows. Si vous utilisez l'authentification Windows, vous n'avez pas besoin de fournir un nom d'utilisateur ou un mot de passe pour la connexion.  
  
 **User name**  
 Si vous n'utilisez pas l'authentification Windows, vous devez fournir un nom d'utilisateur pour la connexion.  
  
 **Mot de passe**  
 Si vous n'utilisez pas l'authentification Windows, vous devez fournir le mot de passe de la connexion.  
  
 **Tester**  
 Testez les paramètres du gestionnaire de connexions.  
  
## <a name="see-also"></a> Voir aussi  
 [Tâche de service Web](../../integration-services/control-flow/web-service-task.md)   
 [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
