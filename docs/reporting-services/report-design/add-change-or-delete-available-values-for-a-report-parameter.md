---
title: Ajouter, modifier ou supprimer les valeurs disponibles d’un paramètre de rapport | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.reportparameters.availablevalues.f1
- "10455"
- "10071"
ms.assetid: 0e03264c-523f-4c59-b71b-ceef600f75f6
caps.latest.revision: 11
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 4d3466964e2b2550e5b0dd6b90cbaac3a063e68e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-change-or-delete-available-values-for-a-report-parameter"></a>Ajouter, modifier ou supprimer les valeurs disponibles d'un paramètre de rapport
  Après avoir créé un paramètre de rapport, vous pouvez spécifier une liste de valeurs disponibles visibles par l'utilisateur. Une liste de valeurs disponibles limite les choix qu'un utilisateur peut faire aux valeurs valides pour le paramètre.  
  
 Les valeurs disponibles s'affichent dans une liste déroulante en regard du paramètre de rapport dans la barre d'outils lors de l'exécution du rapport. Les paramètres de rapport peuvent représenter une ou plusieurs valeurs. Le haut de liste commence par une fonctionnalité **Sélectionner tout** afin que l’utilisateur puisse sélectionner ou effacer toutes les valeurs d’un simple clic.  
  
 Vous pouvez fournir une liste statique de valeurs ou une liste provenant d'un dataset de rapport. Vous pouvez éventuellement fournir un nom convivial pour les valeurs en spécifiant un champ d'étiquette. Par exemple, vous pouvez afficher le champ `ProductID` dans l'étiquette de paramètre pour un paramètre basé sur un champ `ProductName` . Lorsque le rapport s'exécute, l'utilisateur peut choisir entre les noms de produits, mais la valeur choisie réelle est l'élément `ProductID`correspondant.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
 Après la publication d'un rapport, vous pouvez remplacer les valeurs disponibles définies dans l'outil de création de rapports en définissant les valeurs de propriété de paramètre sur le serveur de rapports. Pour plus d'informations, consultez [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
### <a name="to-add-or-change-the-available-values-for-a-report-parameter"></a>Pour ajouter ou modifier les valeurs disponibles d'un paramètre de rapport  
  
1.  Dans le volet Données du rapport, développez le nœud Paramètres. Cliquez avec le bouton droit sur le paramètre, puis cliquez sur **Propriétés du paramètre**. La boîte de dialogue **Propriétés du paramètre de rapport** s'ouvre.  
  
    > [!NOTE]  
    >  Si le volet Données du rapport n’est pas visible, cliquez sur **Affichage** , puis sur **Données du rapport**.  
  
2.  Cliquez sur **Valeurs disponibles**. Sélectionnez une option relative aux valeurs disponibles :  
  
    -   Cliquez sur **Spécifier les valeurs** pour fournir manuellement une liste de valeurs et, éventuellement, des noms conviviaux (les étiquettes) pour les valeurs.  
  
         Cliquez sur **Ajouter** , puis entrez la valeur dans la zone de texte **Valeur** et, éventuellement, l’étiquette dans la zone de texte **Étiquette** . Si vous ne spécifiez pas d'étiquette, la valeur est utilisée. Vous pouvez écrire une expression pour une valeur. Le type de données doit correspondre au type de données du paramètre. Les noms de champs ne peuvent pas être utilisés dans une expression pour un paramètre. Pour obtenir des exemples, consultez [Filtres couramment utilisés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/commonly-used-filters-report-builder-and-ssrs.md).  
  
         Répétez cette étape pour toutes les valeurs que vous souhaitez fournir. L'ordre des éléments que vous voyez dans cette liste détermine l'ordre dans lequel l'utilisateur les voit dans la liste déroulante. Pour modifier l’ordre d’un élément dans la liste, cliquez sur la zone de texte **Valeur** ou **Étiquette** pour sélectionner l’élément, puis utilisez les boutons fléchés orientés vers le bas et le haut pour déplacer l’élément dans la liste.  
  
    -   Cliquez sur **Obtenir les valeurs à partir d’une requête** pour fournir le nom d’un dataset existant qui récupère les valeurs et, éventuellement, les noms conviviaux pour ce paramètre.  
  
        > [!IMPORTANT]  
        >  Si le même dataset contient le paramètre de requête correspondant pour le paramètre de rapport, le rapport affiche un message d'erreur lorsque vous tentez de l'exécuter. Pour résoudre cette erreur, utilisez un dataset différent pour récupérer les valeurs.  
  
         Dans **Dataset**, choisissez le nom du dataset.  
  
         Dans **Champ de valeur**, choisissez le nom du champ qui fournit les valeurs de paramètre.  
  
         Dans **Champ d’étiquette**, choisissez le nom du champ qui fournit les noms conviviaux pour le paramètre. S’il n’existe aucun champ séparé pour les noms conviviaux, choisissez le même champ que celui pour le champ **Valeur** .  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Lorsque vous affichez un aperçu du rapport, une liste déroulante de valeurs disponibles pour le paramètre s'affiche.  
  
### <a name="to-remove-the-available-values-for-a-report-parameter"></a>Pour supprimer les valeurs disponibles pour un paramètre de rapport  
  
1.  Dans le volet Données du rapport, développez le nœud Paramètres. Cliquez avec le bouton droit sur le paramètre, puis cliquez sur **Propriétés du paramètre**. La boîte de dialogue **Paramètres du rapport** s’ouvre.  
  
2.  Cliquez sur **Valeurs disponibles**.  
  
3.  Dans **Sélectionnez l’une des options suivantes**, cliquez sur **Aucun**.  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Lorsque vous affichez un aperçu du rapport, la liste déroulante de valeurs disponibles pour le paramètre ne s'affiche plus.  
  
## <a name="see-also"></a> Voir aussi  
 [Modifier l’ordre d’un paramètre de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/change-the-order-of-a-report-parameter-report-builder-and-ssrs.md)   
 [Ajouter, modifier ou supprimer un paramètre de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-report-parameter-report-builder-and-ssrs.md)   
 [Ajouter des paramètres en cascade à un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-cascading-parameters-to-a-report-report-builder-and-ssrs.md)   
 [Ajouter, modifier ou supprimer les valeurs par défaut d’un paramètre de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-default-values-for-a-report-parameter.md)   
 [Références à la collection Parameters&#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-parameters-collection-references-report-builder.md)   
 [Didacticiel : ajouter un paramètre à un rapport &#40;Générateur de rapports&#41;](../../reporting-services/tutorial-add-a-parameter-to-your-report-report-builder.md)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
  
