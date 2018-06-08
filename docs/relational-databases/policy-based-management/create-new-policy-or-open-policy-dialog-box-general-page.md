---
title: Créer une stratégie ou Ouvrir une stratégie, boîte de dialogue (page Général) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.policy.f1
- sql13.swb.dmf.policy.filter.f1
- sql13.swb.dmf.newgroup.f1
ms.assetid: c00bebd0-d04b-4c64-840e-8b7a2c603436
caps.latest.revision: 42
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 1e1f14d52a6bd9198b529ad748175d194d5eeaee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-new-policy-or-open-policy-dialog-box-general-page"></a>Créer une stratégie ou Ouvrir une stratégie, boîte de dialogue (page Général)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez cette boîte de dialogue pour créer une stratégie de la Gestion basée sur des stratégies ou pour modifier une stratégie existante. Utilisez les zones **Par rapport aux cibles** et **Restriction sur le serveur** comme filtre pour limiter les stratégies à un sous-ensemble de toutes les cibles possibles. Pour être utilisées comme filtres cibles, des conditions doivent être définies sur une facette physique, ne doivent pas contenir de fonctions et ne doivent pas contenir l'opérateur LIKE. Lorsque le système calcule le jeu d'objets pour une stratégie, les objets système sont exclus par défaut.  Par exemple, si le jeu d'objets de la stratégie fait référence à toutes les tables, la stratégie ne s'applique pas aux tables système. Si les utilisateurs souhaitent évaluer une stratégie sur les objets système, ils peuvent les ajouter explicitement au jeu d'objets. Toutefois, bien que toutes les stratégies soient prises en charge pour le mode d'évaluation **vérifier la planification** , pour des raisons de performances, toutes les stratégies comportant des jeux d'objets arbitraires ne sont pas prises en charge pour le mode d'évaluation **vérifier la planification** . Pour plus d’informations, consultez [http://blogs.msdn.com/b/sqlpbm/archive/2009/04/13/policy-evaluation-modes.aspx](http://blogs.msdn.com/b/sqlpbm/archive/2009/04/13/policy-evaluation-modes.aspx)  
  
## <a name="options"></a>Options  
 **Nom**  
 Pour une nouvelle stratégie, tapez le nom de la nouvelle stratégie. Pour une stratégie existante, le nom est affiché.  
  
 **Activé**  
 Cochez la case **Activé** pour activer la stratégie. Désactivez la case à cocher **Activé** pour désactiver la stratégie. La case à cocher **Activée** s'applique à l'automation de stratégie. Elle crée ou supprime le système d'automation pour la stratégie. L'automation utilise les mécanismes suivants :  
  
 **Sur modification - Empêcher**  
 Un déclencheur de base de données applique la conformité.  
  
 **Sur modification - Journal uniquement**  
 Un événement des services de notification vérifie la conformité.  
  
 **Selon planification**  
 Un travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est créé pour vérifier la conformité selon une planification.  
  
 Les stratégies exécutées avec le mode d'évaluation **À la demande** n'utilisent pas cette case à cocher.  
  
 **Vérifier la condition**  
 Sélectionnez la condition de la Gestion basée sur des stratégies utilisée par cette stratégie. Toutes les conditions sur le serveur pour la facette de la Gestion basée sur des stratégies associée sont répertoriées. Cliquez sur **Nouvelle condition** pour créer une condition. Cliquez sur le bouton**…**pour modifier la condition.  
  
 **Par rapport aux cibles**  
 Sélectionnez les types de cibles disponibles pour cette facette afin d'achever une expression de filtre.  
  
 **Mode d'évaluation**  
 Sélectionnez le mode d'évaluation à utiliser pour la stratégie. Certaines stratégies peuvent être vérifiées mais pas appliquées. Les modes d'évaluation sont les suivantes :  
  
 **À la demande**  
 La stratégie n’est exécutée que lorsque vous l’exécutez depuis la boîte de dialogue **Évaluer** .  
  
 **Selon planification**  
 Évalue périodiquement la stratégie, enregistre une entrée de journal pour les stratégies non conformes et crée un rapport. Active la zone **Planification** .  
  
 **Sur modification - Journal uniquement**  
 Lorsque des modifications sont essayées, cette option n'empêche pas les modifications hors de-conformité, mais elle enregistre les violations de stratégie.  
  
 **Sur modification - Empêcher**  
 Lorsque des modifications sont essayées, cette option empêche toute modification qui enfreindrait la stratégie.  
  
 **Planification**  
 Cette option apparaît lorsque le mode d’évaluation **Selon la planification** est sélectionné. Tapez le nom de la planification, cliquez sur **Choisir** pour sélectionner une planification dans la liste ou sur **Nouveau** pour créer une planification. Pour activer la zone de planification, l'option **Selon la planification** doit être sélectionnée.  
  
 **Restriction sur le serveur**  
 Sélectionnez les types de serveurs appropriés pour cette stratégie. Vous pouvez choisir **Aucun** ou sélectionner une condition qui filtre les serveurs possibles.  
  
## <a name="see-also"></a> Voir aussi  
 [Administrer des serveurs à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)  
  
  
