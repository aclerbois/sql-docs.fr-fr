---
title: Créer un membre feuille (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
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
- leaf members [Master Data Services], creating
- creating leaf members [Master Data Services]
- members [Master Data Services], creating leaf members
ms.assetid: 0499d3b3-d508-4d43-a740-19cf53ade9f1
caps.latest.revision: 14
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: b626839df972a21c010837d65a121a4cf0cee057
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-leaf-member-master-data-services"></a>Créer un membre feuille (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], créez un membre feuille lorsque vous souhaitez ajouter des données de référence à votre système. Si vous souhaitez ajouter des données en bloc, utilisez à la place des tables de mise en lots. Pour plus d’informations, consultez [Importer des données à partir de tables &#40;Master Data Services&#41;](../master-data-services/import-data-from-tables-master-data-services.md)  
  
 Vous pouvez également utiliser [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] pour importer des données.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Explorateur** .  
  
-   Vous devez au minimum disposer de l’autorisation **Créer** ou **Mettre à jour** concernant l’objet modèle feuille de l’entité à laquelle vous ajoutez un membre. L’autorisation Créer vous permet de créer un membre et de modifier l’attribut Code uniquement. L’autorisation Mettre à jour, quant à elle, vous permet de mettre à jour d’autres attributs.  
  
     Pour plus d’informations, consultez [Importer des données à partir de tables &#40;Master Data Services&#41;](../master-data-services/security-master-data-services.md).  
  
### <a name="to-create-a-leaf-member"></a>Pour créer un membre feuille  
  
1.  Dans la page d'accueil de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , dans la liste **Modèle** , sélectionnez un modèle.  
  
2.  Si vous êtes un utilisateur, sélectionnez une version ouverte dans la liste **Version** . Si vous êtes un administrateur, sélectionnez une version avec l'état ouvert ou verrouillé dans la liste **Version** .  
  
3.  Cliquez sur **Explorateur**.  
  
4.  Dans la barre de menus, pointez sur **Entités** , puis cliquez sur le nom de l'entité à laquelle vous souhaitez ajouter un membre.  
  
5.  Cliquez sur **Ajouter un membre**.  
  
6.  Dans le volet **Détails** , renseignez les champs.  
  
     Si un attribut basé sur un domaine et un filtre a été appliqué à l’attribut, la liste des valeurs d’attribut est restreinte par l’attribut parent du filtre.  
  
     Pour plus d’informations sur les attributs parents de filtre et les attributs basés sur un domaine, consultez [Créer un attribut basé sur un domaine &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md).  
  
7.  Facultatif. Dans la zone **Annotations** , tapez un commentaire pour expliquer l'ajout du membre. Tous les utilisateurs ayant accès au membre peuvent afficher l'annotation.  
  
8.  Cliquez sur **OK**.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer un membre consolidé &#40;Master Data Services&#41;](../master-data-services/create-a-consolidated-member-master-data-services.md)   
 [Membres &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)  
  
  
