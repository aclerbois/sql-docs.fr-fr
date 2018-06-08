---
title: Activer Resource Governor | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, enabling
ms.assetid: 4d17af53-cf11-4ce4-aab4-deda94a49836
caps.latest.revision: 12
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 4b28938d3cc3010410d55aead4d676e611b2027e
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="enable-resource-governor"></a>Activer Resource Governor
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le gouverneur de ressources est désactivé par défaut. Vous pouvez activer Resource Governor à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de Transact-SQL.  
  
-   **Before you begin:**  [Limitations and Restrictions](#LimitationsRestrictions), [Permissions](#Permissions)  
  
-   **Pour activer Resource Governor, utilisez :**  [Explorateur d’objets](#RGOnObjEx), [Propriétés de Resource Governor](#RGOnProp), [Transact-SQL](#RGOnTSQL)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
 L'activation de Resource Governor entraîne les résultats suivants :  
  
-   La fonction classifieur est exécutée pour les nouvelles connexions afin que leurs charges de travail puissent être assignées aux groupes de charge de travail.  
  
-   Les limites de ressource qui sont spécifiées dans la configuration de Resource Governor sont honorées et appliquées.  
  
-   Les demandes qui ont existé avant d'activer Resource Governor sont affectées par les éventuelles modifications de configuration qui ont été effectuées lorsque Resource Governor était désactivé.  
  
###  <a name="LimitationsRestrictions"></a> Limitations et restrictions  
 Vous ne pouvez pas utiliser l’instruction **ALTER RESOURCE GOVERNOR** pour activer Resource Governor dans une transaction utilisateur.  
  
###  <a name="Permissions"></a> Permissions  
 L'activation de Resource Governor requiert l'autorisation CONTROL SERVER.  
  
##  <a name="RGOnObjEx"></a> Activer Resource Governor à l'aide de l'Explorateur d'objets  
 **Pour activer Resource Governor à l'aide de l'Explorateur d'objets**  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ouvrez l'Explorateur d'objets et développez de manière récursive le nœud **Gestion** jusqu'au **Resource Governor**.  
  
2.  Cliquez avec le bouton droit sur **Resource Governor**, puis sélectionnez **Activer**.  
  
##  <a name="RGOnProp"></a> Activer Resource Governor à l'aide des propriétés de Resource Governor  
 **Pour activer Resource Governor à l'aide de la page Propriétés de Resource Governor**  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ouvrez l'Explorateur d'objets et développez de manière récursive le nœud **Gestion** jusqu'à **Resource Governor**.  
  
2.  Cliquez avec le bouton droit sur **Resource Governor** , puis sélectionnez **Propriétés**afin d’ouvrir la page **Propriétés de Resource Governor** .  
  
3.  Activez la case à cocher **Activer Resource Governor** , puis cliquez **OK**.  
  
##  <a name="RGOnTSQL"></a> Activer Resource Governor à l'aide de Transact-SQL  
 **Pour activer Resource Governor à l'aide de Transact-SQL**  
  
1.  Exécutez l'instruction **ALTER RESOURCE GOVERNOR RECONFIGURE** .  
  
### <a name="example-transact-sql"></a>Exemple (Transact-SQL)  
 L'exemple suivant active Resource Governor.  
  
```  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [Désactiver Resource Governor](../../relational-databases/resource-governor/disable-resource-governor.md)   
 [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [Groupe de charge de travail de Resource Governor](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [Fonction classifieur de Resource Governor](../../relational-databases/resource-governor/resource-governor-classifier-function.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  
