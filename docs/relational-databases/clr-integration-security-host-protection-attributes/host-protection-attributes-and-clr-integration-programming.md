---
title: Héberger des attributs de Protection et de la programmation de l’intégration CLR | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- host protection attributes [CLR integration]
- HostProtectionAttribute [CLR integration]
- common language runtime [SQL Server], host protection attributes
- disallowed types and members [CLR integration]
- common language runtime [SQL Server], disallowed types and members
- HPAs [CLR integration]
ms.assetid: 268078df-63ca-4c03-a8e7-7108bcea9697
caps.latest.revision: 28
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 8c844f2f5a3491c7eea71b6d3ffab7efff254a97
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="host-protection-attributes-and-clr-integration-programming"></a>Attributs de protection de l'hôte et programmation de l'intégration CLR
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le Common Language Runtime (CLR) fournit un mécanisme pour annoter des interfaces de programmation d'applications (API) managées qui font partie du .NET Framework avec certains attributs qui peuvent intéresser un hôte du CLR, tel que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Voici quelques exemples d'attributs de protection de l'hôte (HPA, Host Protection Attribute) :  
  
-   **SharedState**, qui indique si l’API expose la capacité de créer ou gérer l’état (par exemple, les champs de classe statique) partagé.  
  
-   **Synchronisation**, ce qui indique si l’API expose la capacité à effectuer la synchronisation entre les threads.  
  
-   **ExternalProcessMgmt**, ce qui indique si l’API expose une méthode pour contrôler le processus hôte.  
  
 Étant donné ces attributs, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifie une liste de HPA rejetés dans l'environnement hébergé par le biais de la sécurité d'accès du code. Les exigences d’autorités de certification sont spécifiées par un des trois [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] jeux d’autorisations : **SAFE**, **EXTERNAL_ACCESS**, ou **UNSAFE**. Un de ces trois niveaux de sécurité est spécifié lorsque l’assembly est inscrit sur le serveur, à l’aide de la **CREATE ASSEMBLY** instruction. Code qui s’exécute au sein de la **SAFE** ou **EXTERNAL_ACCESS** jeux d’autorisations doivent éviter certains types ou membres qui ont le **System.Security.Permissions.HostProtectionAttribute** attribut appliqué. Pour plus d’informations, consultez [création d’un Assembly](../../relational-databases/clr-integration/assemblies/creating-an-assembly.md) et [Restrictions du modèle de programmation CLR Integration](../../relational-databases/clr-integration/database-objects/clr-integration-programming-model-restrictions.md).  
  
 Le **HostProtectionAttribute** n’est pas une autorisation de sécurité comme un moyen d’améliorer la fiabilité, car elle identifie le code spécifique est construit, types ou méthodes, que l’hôte peut rejeter. L’utilisation de la **HostProtectionAttribute** applique un modèle de programmation qui contribue à protéger la stabilité de l’hôte.  
  
## <a name="host-protection-attributes"></a>Attributs de protection de l'hôte  
 Les attributs de protection de l'hôte identifient des types ou des membres qui ne sont pas adaptés au modèle de programmation hôte et représentent les niveaux croissants suivants de menace en termes de fiabilité :  
  
-   Sans gravité par ailleurs.  
  
-   Susceptible de déstabiliser le code utilisateur géré par le serveur.  
  
-   Susceptible de déstabiliser le processus serveur lui-même.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’autorise pas l’utilisation d’un type ou un membre qui a un **HostProtectionAttribute** qui spécifie un **System.Security.Permissions.HostProtectionResource** énumération avec la valeur  **ExternalProcessMgmt**, **ExternalThreading**, **MayLeakOnAbort**, **SecurityInfrastructure**,  **SelfAffectingProcessMgmnt**, **SelfAffectingThreading**, **SharedState**, **synchronisation**, ou **l’interface utilisateur**. Cela empêche les assemblys d'appeler des membres qui activent l'état de partage, exécutent la synchronisation, peuvent entraîner une fuite de ressources ou affectent l'intégrité du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="disallowed-types-and-members"></a>Types et membres rejetés  
 Les rubriques suivantes identifient les types et membres dont **HostProtectionResource** valeurs ne sont pas autorisées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Les listes de ces rubriques ont été générées à partir des assemblys pris en charge.  Pour plus d’informations, consultez [prise en charge des bibliothèques .NET Framework](../../relational-databases/clr-integration/database-objects/supported-net-framework-libraries.md).  
  
## <a name="in-this-section"></a>Dans cette section  
 [Types et membres dans Microsoft.VisualBasic.dll interdits](../../relational-databases/clr-integration-security-host-protection-attributes/disallowed-types-and-members-in-microsoft-visualbasic-dll.md)  
 Répertorie les types et membres dans Microsoft.VisualBasic.dll dont les valeurs HPA sont rejetées.  
  
 [Types et membres dans mscorlib.dll interdits](../../relational-databases/clr-integration-security-host-protection-attributes/disallowed-types-and-members-in-mscorlib-dll.md)  
 Répertorie les types et membres dans mscorlib.dll dont les valeurs HPA sont rejetées.  
  
 [Types et membres dans System.dll interdits](../../relational-databases/clr-integration-security-host-protection-attributes/disallowed-types-and-members-in-system-dll.md)  
 Répertorie les types et membres dans System.dll dont les valeurs HPA sont rejetées.  
  
 [Types et membres dans System.Data.dll interdits](../../relational-databases/clr-integration-security-host-protection-attributes/disallowed-types-and-members-in-system-data-dll.md)  
 Répertorie les types et membres dans System.Data.dll dont les valeurs HPA sont rejetées.  
  
 [Types et membres dans System.Core.dll interdits](../../relational-databases/clr-integration-security-host-protection-attributes/disallowed-types-and-members-in-system-core-dll.md)  
 Répertorie les types et membres dans System.Core.dll dont les valeurs HPA sont rejetées.  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité d’accès du Code CLR Integration](../../relational-databases/clr-integration/security/clr-integration-code-access-security.md)   
 [Restrictions du modèle de programmation CLR Integration](../../relational-databases/clr-integration/database-objects/clr-integration-programming-model-restrictions.md)   
 [Création d’un Assembly](../../relational-databases/clr-integration/assemblies/creating-an-assembly.md)  
  
  
