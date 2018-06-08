---
title: Propriétés de Analysis Server (onglet Service) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 8dbe4bc5-6aa9-48ee-857e-0b4ea764b9cb
caps.latest.revision: 19
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 335c6a9abbc7a1df3a3a0efb2dd12c6d1d3918d5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MTE
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="analysis-server-properties-service-tab"></a>Propriétés de Analysis Server (onglet Service)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
  Ce service est le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Ce service doit être en cours d'exécution pour que [!INCLUDE[ssAS](../../includes/ssas-md.md)] fonctionne correctement. Les valeurs des propriétés en gris clair ne peuvent pas être modifiées à l'aide de cette application.  
  
## <a name="options"></a>Options  
 **Chemin d’accès binaire**  
 Affiche l'emplacement des fichiers programme utilisés par ce service.  
  
 **Contrôle d’erreurs**  
 1 indique `SERVICE_ERROR_NORMAL`. Si le lancement du service échoue pendant le démarrage de l'ordinateur, le programme de démarrage consigne l'erreur et affiche une boîte de message, mais poursuit l'opération de démarrage. Cette valeur ne peut pas être modifiée.  
  
 **Code de sortie**  
 Lorsqu'une erreur se produit, le numéro d'erreur apparaît dans cette zone. Pour dépanner des incidents, recherchez ce numéro dans la Base de connaissances [!INCLUDE[msCoName](../../includes/msconame-md.md)] ou communiquez-le au personnel en charge du support technique.  
  
 **Host Name**  
 Affiche le nom de l'ordinateur ou du cluster exécutant [!INCLUDE[ssAS](../../includes/ssas-md.md)].  
  
 **Nom**  
 Indique le nom d'affichage du service.  
  
 **ID de processus**  
 Affiche le numéro qu’utilise [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows pour effectuer le suivi des processus de ce programme.  
  
 **Type de service SQL**  
 Affiche le type de service fourni aux processus appelants. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe plusieurs services.  
  
 **Mode de démarrage**  
 Les options disponibles pour ce service sont les suivantes :  
  
-   Manuel : ce service n'est pas automatiquement lancé au démarrage de l'ordinateur. Vous devez démarrer le service à l'aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d'un autre outil.  
  
-   Automatique : ce service essaie de se lancer au démarrage de cet ordinateur.  
  
-   Désactivé : ce service ne peut pas être démarré.  
  
 **État**  
 Indique si ce service est en cours d'exécution, arrêté ou désactivé.  
  
  
