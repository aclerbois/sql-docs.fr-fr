---
title: Désactiver les fonctionnalités par défaut (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3238e09bea0ef150dde01c78ef5d2c802c1c1853
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="features-off-by-default-analysis-services"></a>Fonctionnalités désactivées par défaut (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est conçue de façon à être sécurisée par défaut. Par conséquent, les fonctionnalités qui pourraient compromettre la sécurité sont désactivées par défaut. Les fonctionnalités suivantes sont désactivées après leur installation et doivent être activées explicitement si vous voulez les utiliser.  
  
## <a name="feature-list"></a>Liste des fonctionnalités  
 Pour activer les fonctionnalités suivantes, connectez-vous à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Cliquez avec le bouton droit sur le nom de l’instance, puis sélectionnez **Facettes**. Vous pouvez également activer ces fonctionnalités via les propriétés du serveur, comme décrit dans la section suivante.  
  
-   Requêtes d'exploration de données ad hoc (OpenRowset)  
  
-   Objets liés (Vers)  
  
-   Objets liés (De)  
  
-   Écouter uniquement les connexions locales  
  
-   Fonctions définies par l'utilisateur  
  
## <a name="server-properties"></a>Propriétés du serveur  
 Les autres fonctionnalités qui sont désactivées par défaut peuvent être activées via les propriétés du serveur. Se connecter à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] Cliquez avec le bouton droit sur le nom de l’instance, puis sélectionnez **Propriétés**. Cliquez sur **Général**, puis cliquez sur **Afficher les options avancées** pour afficher une liste de propriétés plus longue.  
  
-   Requêtes d'exploration de données ad hoc (OpenRowset)  
  
-   Autoriser les modèles d'exploration de données de session (exploration de données)  
  
-   Objets liés (Vers)  
  
-   Objets liés (De)  
  
-   Fonctions COM définies par l'utilisateur  
  
-   Définitions de traces de boîte noire (modèles).  
  
-   Journalisation des requêtes  
  
-   Écouter uniquement les connexions locales  
  
-   XML binaire  
  
-   Compression  
  
-   Affinité de groupe. Pour plus d'informations, consultez [Thread Pool Properties](../../analysis-services/server-properties/thread-pool-properties.md) .  
  
  
