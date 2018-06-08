---
title: Valider des membres spécifiques par rapport aux règles d’entreprise (Master Data Services) | Microsoft Docs
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
helpviewer_keywords:
- applying business rules [Master Data Services]
- business rules [Master Data Services], applying to select members
ms.assetid: 2288ef43-5392-47ea-b651-ec25e5692a14
caps.latest.revision: 7
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 37bf689ff457cda1717609b293661a7f4344dbc9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="validate-specific-members-against-business-rules-master-data-services"></a>Valider des membres spécifiques par rapport aux règles d'entreprise (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], appliquez des règles d'entreprise sélectivement lorsque vous souhaitez mettre à jour ou valider des sous-ensembles de membres par rapport à des règles d'entreprise.  
  
> [!NOTE]  
>  Si vous souhaitez appliquer des règles d’entreprise à tous les membres dans une version d’un modèle, consultez [Valider une version par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md).  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Explorateur** .  
  
-   Vous devez au minimum disposer de l'autorisation **Mettre à jour** sur l'objet modèle auquel vous appliquez les règles d'entreprise.  
  
### <a name="to-apply-business-rules-selectively"></a>Pour appliquer des règles d'entreprise sélectivement  
  
1.  Dans la page d’accueil de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , dans la liste déroulante **Modèle** , sélectionnez un modèle.  
  
2.  Dans la liste déroulante **Version** , sélectionnez une version.  
  
3.  Cliquez sur l’onglet **Explorateur** .  
  
4.  Dans la barre de menus, pointez sur **Entités** , puis cliquez sur le nom de l'entité qui contient les membres auxquels vous souhaitez appliquer les règles.  
  
5.  Cliquez sur **Appliquer les règles**. Les règles d'entreprise sont appliquées uniquement aux membres affichés dans la grille.  
  
## <a name="see-also"></a> Voir aussi  
 [Valider une version par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)   
 [Business Rules &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
