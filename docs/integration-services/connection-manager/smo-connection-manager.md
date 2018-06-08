---
title: Gestionnaire de connexions SMO | Microsoft Docs
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
- sql13.dts.designer.smoconnection.f1
helpviewer_keywords:
- connections [Integration Services], SMO
- SMO connection manager
- connection managers [Integration Services], SMO
ms.assetid: d273f1fb-a6a8-4f2f-a5ff-55c2e3de4723
caps.latest.revision: 21
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: cc70089ca05b20a81f93f65bbdb53cc819d3d7d2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="smo-connection-manager"></a>Gestionnaire de connexions SMO
  Un gestionnaire de connexions SMO permet à un package de se connecter à un serveur SMO (SQL Management Object). Les tâches de transfert incluses dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilisent un gestionnaire de connexions SMO. Par exemple, la tâche de transfert de connexions qui transfère des connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un gestionnaire de connexions SMO.  
  
 Quand vous ajoutez un gestionnaire de connexions SMO à un package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un gestionnaire de connexions qui est résolu en tant que connexion SMO au moment de l’exécution, définit les propriétés du gestionnaire de connexions et ajoute le gestionnaire de connexions à la collection **Connexions** sur le package. La propriété **ConnectionManagerType** du gestionnaire de connexions a pour valeur **SMOServer**.  
  
 Vous pouvez configurer le gestionnaire de connexions SMO de plusieurs manières :  
  
-   Spécifiez le nom d'un serveur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé.  
  
-   Sélectionnez le mode d'authentification pour la connexion au serveur.  
  
## <a name="configuration-of-the-smo-connection-manager"></a>Configuration du gestionnaire de connexions SMO  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d’informations sur les propriétés que vous pouvez définir dans le Concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Éditeur du gestionnaire de connexions SMO](../../integration-services/connection-manager/smo-connection-manager-editor.md).  
  
 Pour plus d’informations sur la configuration d’un gestionnaire de connexions par programmation, consultez <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> et [Ajout de connexions par programme](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="smo-connection-manager-editor"></a>Éditeur du gestionnaire de connexions SMO
  Utilisez l' **Éditeur du gestionnaire de connexions SMO** pour configurer une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à utiliser par les différentes tâches qui transfèrent des objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Pour en savoir plus sur le gestionnaire de connexions SMO, consultez [SMO Connection Manager](../../integration-services/connection-manager/smo-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Nom du serveur**  
 Tapez le nom de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou sélectionnez le nom du serveur dans la liste.  
  
 **Actualiser**  
 Actualisez la liste des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponibles qui peuvent être détectées sur le réseau.  
  
 **Utiliser l'authentification Windows**  
 Utilisez l'authentification Windows pour vous connecter à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sélectionnée.  
  
 **Utiliser l’authentification SQL Server**  
 Utilisez l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour vous connecter à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sélectionnée.  
  
 **User name**  
 Si vous avez sélectionné l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , entrez le nom d'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Mot de passe**  
 Si vous avez sélectionné l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , entrez le mot de passe.  
  
 **Tester la connexion**  
 Testez la connexion telle qu'elle est configurée.  
  
## <a name="see-also"></a> Voir aussi  
 [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
  
