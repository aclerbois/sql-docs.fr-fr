---
title: "Mots réservés (Master Data Services) | Microsoft Docs"
ms.custom: 
ms.date: 03/01/2017
ms.prod: sql-non-specified
ms.prod_service: mds
ms.service: 
ms.component: non-specific
ms.reviewer: 
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- reserved words [Master Data Services]
- Master Data Services, reserved words
ms.assetid: 88afd0d0-4362-4394-8357-4e65388fc0fc
caps.latest.revision: 
author: leolimsft
ms.author: lle
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: 6bfb58ced9b444d0fa9cf14852a871da06ffedf2
ms.sourcegitcommit: 6ac1956307d8255dc544e1063922493b30907b80
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2018
---
# <a name="reserved-words-master-data-services"></a>Mots réservés (Master Data Services)
  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], lorsque vous créez des objets de modèle ou des membres, certains mots ne peuvent pas être utilisés. Ces mots peuvent provoquer des erreurs.  
  
> [!NOTE]  
>  Vous devez également limiter votre utilisation des caractères spéciaux (symboles, césure, etc.).  
  
-   [Modèles](../master-data-services/reserved-words-master-data-services.md#models)  
  
-   [Entités](../master-data-services/reserved-words-master-data-services.md#entities)  
  
-   [Hiérarchies explicites](../master-data-services/reserved-words-master-data-services.md#exhierarchies)  
  
-   [Attributs](../master-data-services/reserved-words-master-data-services.md#attributes)  
  
-   [Membres](../master-data-services/reserved-words-master-data-services.md#members)  
  
##  <a name="models"></a> Modèles  
 Si vous créez un modèle dont le nom est défini sur **Nom** ou **Code**, ne sélectionnez pas **Créer une entité portant le même nom que le modèle** , car **Nom** ou **Code** ne peut pas être utilisé pour le nom d’une entité.  
  
##  <a name="entities"></a> Entités  
 Pour les noms d'entité, vous ne pouvez pas utiliser **Name** ou **Code**.  
  
##  <a name="exhierarchies"></a> Hiérarchies explicites  
 Pour les noms de hiérarchies explicites, vous ne pouvez pas utiliser **Name** ou **Code**.  
  
##  <a name="attributes"></a> Attributs  
  
-   **ID**  
  
-   **Code**  
  
-   **EnterUserName**  
  
-   **LastChgUserName**  
  
-   **Nom**  
  
-   **EnterDTM**  
  
-   **EnterUserID**  
  
-   **EnterUserName**  
  
-   **LastChgDTM**  
  
-   **LastChgUserID**  
  
-   **Status_ID**  
  
-   **ValidationStatus_ID**  
  
-   **Version_ID**  
  
##  <a name="members"></a> Membres  
 Pour les membres, vous ne pouvez pas utiliser **MDMMemberStatus**, **MDMUnused**ou **ROOT** pour la valeur d’attribut **Code** .  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble de Master Data Services &#40;MDS&#41;](../master-data-services/master-data-services-overview-mds.md)  
  
  
