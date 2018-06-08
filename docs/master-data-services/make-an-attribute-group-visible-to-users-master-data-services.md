---
title: Rendre un groupe d’attributs visible pour les utilisateurs (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b2f6cc27-dbc9-4f3f-961e-e81e76375248
caps.latest.revision: 6
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: c68a17800113f6f413daa8534a61f296f96e9494
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="make-an-attribute-group-visible-to-users-master-data-services"></a>Rendre un groupe d'attributs visible pour les utilisateurs (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], rendez un groupe d'attributs visible aux utilisateurs ou aux groupes lorsque vous souhaitez que les utilisateurs disposent d'onglets au-dessus de la grille dans la zone fonctionnelle **Explorateur** .  
  
 Lorsque vous créez un groupe d'attributs, il est automatiquement masqué pour tous les utilisateurs, à l'exception de son créateur.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Au moins un groupe d'attributs doit exister. Pour plus d’informations, consultez [Créer un groupe d’attributs &#40;Master Data Services&#41;](../master-data-services/create-an-attribute-group-master-data-services.md).  
  
### <a name="to-make-an-attribute-group-visible-to-users"></a>Pour rendre un groupe d'attributs visible pour les utilisateurs  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Sur la page **Gérer le modèle** , sélectionnez un modèle dans la grille et cliquez sur **Entités**.  
  
3.  Dans la grille de la page **Gérer l’entité** , sélectionnez la ligne de l’entité pour laquelle vous souhaitez modifier un groupe d’attributs.  
  
4.  Cliquez sur **Groupes d’attributs**.  
  
5.  Sur la page **Gérer les groupes d’attributs** , sélectionnez le type de membre dans la liste déroulante **Types de membres** pour développer **Feuille**, **Consolidé** ou **Collection**, selon le type de groupe que vous voulez rendre visible.  
  
6.  Sélectionnez le groupe d’attributs que vous souhaitez modifier dans la grille, puis cliquez sur **Modifier**.  
  
7.  Cliquez sur un utilisateur ou un groupe dans la zone **Disponible** , puis cliquez sur la flèche **Ajouter** . Pour les ajouter tous, cliquez sur la flèche **Ajouter tout** .  
  
8.  Cliquez sur **Enregistrer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Groupes d’attributs &#40;Master Data Services&#41;](../master-data-services/attribute-groups-master-data-services.md)   
 [Créer un groupe d’attributs &#40;Master Data Services&#41;](../master-data-services/create-an-attribute-group-master-data-services.md)  
  
  
