---
title: Réactiver un membre ou une collection (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- collections [Master Data Services], reactivating
- consolidated members [Master Data Services], reactivating
- reactivating members [Master Data Services]
- members [Master Data Services], reactivating
- reactivating collections [Master Data Services]
- leaf members [Master Data Services], reactivating
ms.assetid: bb4884c0-3658-4763-92d1-636804278b1c
caps.latest.revision: 11
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: ccc919dc259eb551cc95fff24cf1f3df3ff55a7f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="reactivate-a-member-or-collection-master-data-services"></a>Réactiver un membre ou une collection (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], vous pouvez réactiver un membre qui a été :  
  
-   Désactivé par le biais du processus de site  
  
-   Supprimé dans le [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]MDS  
  
-   Supprimé dans l’application web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]  
  
 La réactivation d’un membre permet de restaurer ses attributs et son appartenance aux hiérarchies et collections.  
  
 Vous pouvez également réactiver des collections. Lorsque vous procédez ainsi, les attributs et les membres de la collection qui appartiennent à la collection sont restaurés.  
  
 Lorsqu'une collection ou un membre est réactivé, toutes les transactions précédentes sont restaurées.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], vous devez avoir l’autorisation d’accéder à la zone fonctionnelle **Gestion des versions** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-reactivate-a-member-or-collection"></a>Pour réactiver un membre ou une collection  
  
1.  Dans la page d’accueil de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , cliquez sur **Gestion des versions**.  
  
2.  Dans la barre de menus, cliquez sur **Transactions**.  
  
3.  Dans la page **Transactions** , dans la liste **Modèle** , sélectionnez un modèle.  
  
4.  Dans la liste **Version** , sélectionnez une version.  
  
5.  Dans le volet **Transactions** , cliquez sur la ligne correspondant au membre ou à la collection à réactiver. Cette ligne doit afficher **Actif** dans la colonne **Valeur précédente** et **Désactivé** dans la colonne **Nouvelle valeur** .  
  
6.  Cliquez sur **Inverser la transaction**.  
  
7.  Dans la boîte de dialogue de confirmation, cliquez sur **OK**. Une nouvelle transaction est ajoutée, affichant **Activé** dans la colonne **Nouvelle valeur** .  
  
## <a name="see-also"></a> Voir aussi  
 [Supprimer un membre ou une collection &#40;Master Data Services&#41;](../master-data-services/delete-a-member-or-collection-master-data-services.md)   
 [Membres &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)   
 [Collections &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md)  
  
  
