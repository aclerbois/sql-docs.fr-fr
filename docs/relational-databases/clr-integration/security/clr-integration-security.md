---
title: Sécurité d’intégration du CLR | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- security [CLR integration]
- authorization [CLR integration]
- common language runtime [SQL Server], security
- database objects [CLR integration], security
ms.assetid: 05d7a471-c5d5-4730-b903-e4edc8157bb4
caps.latest.revision: 55
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: a7df2bb8e3620b374fa534326b9195b7b2887f91
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clr-integration-security"></a>Sécurité de l'intégration du CLR
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le modèle de sécurité de l'intégration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec le Common Language Runtime (CLR) [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] gère et sécurise l'accès entre types différents d'objets CLR et non-CLR qui s'exécutent dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ces objets peuvent être appelés par une instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou un autre objet CLR qui s'exécute dans le serveur. Les appels entre objets portent le nom de liens. Les types de vérifications de sécurité effectués sur ces objets dépendent des types de liens impliqués.  
  
 Le modèle de sécurité d'intégration du CLR a les objectifs suivants :  
  
-   Par défaut, l'exécution de code utilisateur managé sur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne doit pas compromettre l'intégrité et la stabilité de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. L'exécution d'opérations susceptibles de compromettre la robustesse de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit être protégée par des autorisations de haut niveau appropriées.  
  
-   Le code utilisateur managé ne doit pas accéder de façon non autorisée aux données utilisateur ou autre code utilisateur dans la base de données. Le code défini par l'utilisateur doit s'exécuter sous le contexte de sécurité de la session utilisateur qui l'a appelé et avec les privilèges corrects pour ce contexte de sécurité.  
  
-   Il doit y avoir des contrôles pour restreindre le code utilisateur à accéder à toute ressource située à l'extérieur du serveur, de sorte qu'il soit utilisé strictement pour l'accès aux données et le calcul locaux.  
  
-   Le code défini par l'utilisateur ne doit pas être en mesure d'accéder de façon non autorisée aux ressources système du fait de son exécution dans le processus [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] intègre maintenant le modèle de sécurité basé sur utilisateur de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec le modèle de sécurité basé sur l'accès du code du CLR. Quelques-uns des avantages offerts par cette approche combinée de la sécurité sont discutés dans cette section.  
  
 Le tableau suivant décrit les rubriques de cette section.  
  
 [Sécurité d’accès du code d’intégration du CLR](../../../relational-databases/clr-integration/security/clr-integration-code-access-security.md)  
 Discute du modèle de sécurité d'accès du code pour le code managé.  
  
 [Attributs de protection de l’hôte et programmation de l’intégration du CLR](../../../relational-databases/clr-integration-security-host-protection-attributes/host-protection-attributes-and-clr-integration-programming.md)  
 Fournit des informations à propos des valeurs d'attributs de protection de l'hôte (HPA) interdites dans les assemblys SAFE et EXTERNAL_ACCESS.  
  
 [Liens de sécurité d’intégration du CLR](http://msdn.microsoft.com/library/168efd01-d12e-4bdf-a1b3-0b5c76474eaf)  
 Décrit comment les segments de code utilisateur peuvent s'appeler dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [L’emprunt d’identité et de sécurité d’intégration du CLR](http://msdn.microsoft.com/library/1495a7af-2248-4cee-afdb-9269fb3a7774)  
 Discute la manière dont le code managé accède aux ressources externes à l'aide de l'emprunt d'identité.  
  
 [Autoriser partiellement approuvé appelants](http://msdn.microsoft.com/library/20b0248f-36da-4fc3-97d2-3789fcf6e084)  
 Discute des problèmes qui surviennent lorsqu'une méthode managée appelle une méthode dans une classe contenue dans un autre assembly.  
  
 [Domaines d’application et de sécurité d’intégration du CLR](http://msdn.microsoft.com/library/54ee904e-e21a-4ee7-b4ad-a6f6f71bd473)  
 Décrit la façon dont les assemblys sont chargés dans les domaines d'application.  
  
## <a name="see-also"></a>Voir aussi  
 [La gestion des assemblys d’intégration du CLR](../../../relational-databases/clr-integration/assemblies/managing-clr-integration-assemblies.md)  
  
  
