---
title: Générer automatiquement les valeurs des attributs autres que Code (Master Data Services) | Microsoft Docs
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
ms.assetid: b82f6f81-6e9c-4918-9ea9-4ab5f5d11b15
caps.latest.revision: 5
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: 9db614c74a302758ba4f9163896c13027ccdf04e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="automatically-generate-attribute-values-other-than-code-master-data-services"></a>Générer automatiquement les valeurs des attributs autres que Code (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], générez automatiquement les valeurs des valeurs d’attribut d’une entité lorsque vous souhaitez qu’un entier soit attribué automatiquement comme valeur chaque fois que les règles d’entreprise sont appliquées.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Un attribut numérique doit exister. Pour plus d’informations, consultez [Créer un attribut numérique &#40;Master Data Services&#41;](../master-data-services/create-a-numeric-attribute-master-data-services.md).  
  
### <a name="to-automatically-generate-an-attribute-value"></a>Pour générer automatiquement une valeur d'attribut  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la barre de menus, pointez sur **Gérer** et cliquez sur **Règles d'entreprise**.  
  
3.  Dans la page **Maintenance des règles d’entreprise** , dans la liste **Modèle** , sélectionnez un modèle.  
  
4.  Dans la liste **Entité** , sélectionnez une entité.  
  
5.  Dans la liste **Type de membre** , sélectionnez un type de membre auquel appliquer la règle d’entreprise.  
  
6.  Dans la liste **Attribut** , conservez l’option par défaut **Tout**.  
  
7.  Cliquez sur **Ajouter une règle d’entreprise**.  
  
8.  Cliquez sur **Modifier la règle d’entreprise sélectionnée**.  
  
9. Dans le volet **Composants** , développez le nœud **Actions** .  
  
10. Dans le nœud Valeur par défaut, cliquez sur **prend par défaut une valeur générée** et faites-la glisser vers le libellé **Actions** du volet **THEN** .  
  
11. Dans le volet **Attributs** , cliquez sur l’attribut avec la valeur à générer et faites-le glisser vers le libellé **Sélectionner un attribut** du volet **Modifier l’action** .  
  
12. Tapez une valeur dans les zones **Démarrer avec** et **Incrémenter** . Si les membres existent déjà, la valeur sera définie en fonction de la valeur existante la plus élevée. Par exemple, si la valeur existante la plus élevée est 299 et que vous définissez **Incrémenter de** sur **1**, la valeur du membre suivant sera définie sur 300.  
  
13. Dans le volet **Modifier l’action** , cliquez sur **Enregistrer l’élément**.  
  
14. Cliquez sur **Précédent**.  
  
15. (Facultatif) Dans la page **Maintenance des règles d’entreprise** , sur la ligne qui contient votre règle d’entreprise, double-cliquez sur une cellule dans la colonne **Nom**, **Description**ou **Notification** pour mettre à jour la valeur.  
  
16. Cliquez sur **Publier les règles d’entreprise**.  
  
17. Dans la boîte de dialogue de confirmation, cliquez sur **OK**. L’état de la règle devient **Actif**.  
  
## <a name="next-steps"></a>Next Steps  
  
-   [Valider des membres spécifiques par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
-   [Valider une version par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Création automatique de code &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md)   
 [Règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)   
 [Validation &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)  
  
  
