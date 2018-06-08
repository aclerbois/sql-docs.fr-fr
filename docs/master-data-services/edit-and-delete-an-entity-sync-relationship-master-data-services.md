---
title: Modifier et supprimer une relation de synchronisation d’entités (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 9a5e37f3-352e-45a6-b4a0-6f98f83b4bd8
caps.latest.revision: 6
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 54932f78bc39f78dfdbdc3de6a3d514be6c2fd12
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="edit-and-delete-an-entity-sync-relationship-master-data-services"></a>Modifier et supprimer une relation de synchronisation d’entités (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  La synchronisation d’entités est une synchronisation unidirectionnelle et reproductible entre des versions d'entités. Elle fournit un moyen de partager des données d’entités entre différents modèles. Vous pouvez modifier et supprimer une relation de synchronisation que vous avez créée.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Conditions préalables à la modification d’une relation de synchronisation d’entités.  
  
-   Vous devez avoir l’autorisation d’accéder à la zone fonctionnelle Administration de système. Pour plus d’informations, consultez [Autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   Vous devez être administrateur du modèle cible. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Vous devez disposer au minimum de l’accès en lecture pour l’entité source et l’ensemble de ses attributs et de ses membres.  
  
 Conditions préalables à la suppression d’une relation de synchronisation d’entités.  
  
-   Vous devez avoir l’autorisation d’accéder à la zone fonctionnelle Administration de système. Pour plus d’informations, consultez [Autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   Vous devez être administrateur du modèle cible. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
 Considérez les éléments suivants lorsque vous modifiez une relation de synchronisation d’entités.  
  
-   Les entités source et cible doivent se trouver dans des modèles différents.  
  
-   Le statut de la version de l’entité cible ne doit pas être validé.  
  
-   Une fois une relation de synchronisation établie, la cible est immédiatement synchronisée avec la source.  
  
-   Une version de l’entité cible ne peut pas être une version de l’entité source d’une autre relation de synchronisation.  
  
 Considérez les éléments suivants lorsque vous exécutez une relation de synchronisation d’entités.  
  
-   Seuls les membres feuilles seront copiés.  
  
-   Les attributs fondés sur les domaines ne seront pas copiés.  
  
-   Les membres supprimés de façon réversible ne seront pas copiés.  
  
-   La synchronisation ne génère pas de transactions/historiques de l'entité cible.  
  
 **Pour modifier une relation de synchronisation d’entités**  
  
1.  Dans Master Data Manager, cliquez sur **Administration de système**.  
  
2.  Sur la page **Vue du modèle** , dans la barre de menus, pointez sur **Gérer** et cliquez sur **Synchronisation d’entités**.  
  
3.  Sur la page **Maintenance de la synchronisation d’entités** , sélectionnez une relation de synchronisation dans la grille.  
  
4.  Cliquez sur **Modifier**. Un panneau s’affiche sur le côté droit.  
  
5.  Modifiez la valeur du champ **Fréquence**. Sélectionnez **Synchronisation à la demande**, ou sélectionnez **Synchronisation automatique** et définissez une fréquence.  
  
6.  Cliquez sur **Enregistrer**.  
  
 **Pour supprimer une relation de synchronisation d’entités**  
  
1.  Dans Master Data Manager, cliquez sur **Administration de système**.  
  
2.  Sur la page **Vue du modèle** , dans la barre de menus, pointez sur **Gérer** et cliquez sur **Synchronisation d’entités**.  
  
3.  Sur la page **Maintenance de la synchronisation d’entités** , sélectionnez une relation de synchronisation dans la grille.  
  
4.  Cliquez sur **Supprimer**.  
  
5.  Dans la boîte de dialogue de confirmation, cliquez sur **OK**.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer et exécuter une relation de synchronisation d’entités &#40;Master Data Services&#41;](../master-data-services/create-and-execute-an-entity-sync-relationship-master-data-services.md)   
 [Relation de synchronisation d’entités &#40;Master Data Services&#41;](../master-data-services/entity-sync-relationship-master-data-services.md)  
  
  
