---
title: Gestionnaire de connexions MSMQ | Microsoft Docs
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
- sql13.dts.designer.msmqconnectionmanager.f1
helpviewer_keywords:
- connections [Integration Services], message queues
- connection managers [Integration Services], MSMQ
- MSMQ connection manager
- message queue connections [Integration Services]
ms.assetid: a86900e2-450e-479f-b207-e1b02361d395
caps.latest.revision: 35
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7aa34309776b1fa9748f302941a09db1f008dd54
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="msmq-connection-manager"></a>Gestionnaire de connexions MSMQ
  Un gestionnaire de connexions MSMQ permet à un package de se connecter à une file d'attente de messages qui utilise Message Queuing (MSMQ). La tâche MSMQ incluse dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise un gestionnaire de connexions MSMQ.  
  
 Quand vous ajoutez un gestionnaire de connexions MSMQ à un package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un gestionnaire de connexions qui sera résolu en une connexion MSMQ au moment de l’exécution, définit les propriétés du gestionnaire de connexions et ajoute le gestionnaire de connexions à la collection **Connections** sur le package. La propriété **ConnectionManagerType** du gestionnaire de connexions a pour valeur **MSMQ**.  
  
 Vous pouvez configurer le gestionnaire de connexions MSMQ de plusieurs manières :  
  
-   Spécifiez une chaîne de connexion.  
  
-   Spécifiez le chemin d'accès à la file d'attente de messages à laquelle se connecter.  
  
 Le format du chemin dépend du type de file d'attente, comme le montre le tableau suivant.  
  
|Type de file d'attente|Exemple de chemin d'accès|  
|----------------|-----------------|  
|Public|\<nom_ordinateur >\\<nom_file_attente\>|  
|Privé|\<nom_ordinateur >\Private$\\<nom_file_attente\>|  
  
 Vous pouvez utiliser un point (.) pour représenter l'ordinateur local.  
  
## <a name="configuration-of-the-msmq-connection-manager"></a>Configuration du gestionnaire de connexions MSMQ  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d’informations sur les propriétés que vous pouvez définir dans le Concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , consultez [Éditeur du gestionnaire de connexions MSMQ](../../integration-services/connection-manager/msmq-connection-manager-editor.md).  
  
 Pour plus d’informations sur la configuration d’un gestionnaire de connexions par programmation, consultez <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> et [Ajout de connexions par programme](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="msmq-connection-manager-editor"></a>Éditeur du gestionnaire de connexions MSMQ
  La boîte de dialogue **Gestionnaire de connexions MSMQ** permet de spécifier le chemin d’une file d’attente de messages MSMQ (Message Queuing).  
  
 Pour en savoir plus sur le gestionnaire de connexions MSMQ, consultez [MSMQ Connection Manager](../../integration-services/connection-manager/msmq-connection-manager.md).  
  
> [!NOTE]  
>  Le gestionnaire de connexions MSMQ prend en charge les files d'attente privées et publiques locales et les files d'attente publiques distantes. Il ne prend pas en charge les files d'attente privées distantes. Pour une solution de contournement qui utilise la tâche de script, consultez [Sending to a Remote Private Message Queue with the Script Task](../../integration-services/extending-packages-scripting-task-examples/sending-to-a-remote-private-message-queue-with-the-script-task.md).  
  
### <a name="options"></a>Options  
 **Nom**  
 Fournissez un nom unique pour le gestionnaire de connexions MSMQ dans le flux de travail. Le nom fourni sera affiché dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Description**  
 Décrivez le gestionnaire de connexions. Il est recommandé d'indiquer ici l'usage auquel le gestionnaire de connexions est destiné, de sorte que les packages soient correctement documentés et plus faciles à gérer.  
  
 **Chemin d'accès**  
 Tapez le chemin d'accès complet de la file d'attente de messages. Le format du chemin d'accès dépend du type de file d'attente.  
  
|Type de file d'attente|Exemple de chemin d'accès|  
|----------------|-----------------|  
|Public|\<nom_ordinateur >\\<nom_file_attente\>|  
|Privé|\<nom_ordinateur >\Private$\\<nom_file_attente\>|  
  
 Vous pouvez utiliser "." pour représenter l'ordinateur local.  
  
 **Test**  
 Après avoir configuré le gestionnaire de connexions MSMQ, vérifiez si la connexion est opérationnelle en cliquant sur **Tester**.  
  
## <a name="see-also"></a> Voir aussi  
 [Tâche MSMQ](../../integration-services/control-flow/message-queue-task.md)   
 [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
  
