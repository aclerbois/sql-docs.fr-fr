---
title: Octroyer un accès personnalisé à des données de dimension (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: eb93b4aeeaae9d659a225763286fc15a7d9f52a3
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="grant-custom-access-to-dimension-data-analysis-services"></a>Octroyer un accès personnalisé à des données de dimension (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Après avoir activé l'accès en lecture à un cube, vous pouvez définir des autorisations supplémentaires qui accordent ou refusent explicitement l'accès aux membres de dimension (y compris les mesures contenues dans la dimension de mesures contenant toutes les mesures utilisées dans un cube). Par exemple, étant donné plusieurs catégories de revendeurs, vous pouvez définir des autorisations pour exclure les données d'un type spécifique. L'illustration suivante est une représentation avant/après du refus de l'accès au type d'entreprise Warehouse dans la dimension Reseller.  
  
 ![Les tableaux croisés dynamiques avec et sans un membre de dimension](../../analysis-services/multidimensional-models/media/ssas-permsdimdenied.png "des tableaux croisés dynamiques avec et sans un membre de dimension")  
  
 Par défaut, si vous pouvez lire les données d’un cube [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , vous disposez automatiquement d’autorisations d’accès en lecture pour toutes les mesures et tous les membres de dimension associés à ce cube. Ce comportement peut être suffisant pour de nombreux scénarios, cependant parfois des exigences de sécurité demandent une stratégie d'autorisation segmentée, avec différents niveaux d'accès pour différents utilisateurs dans la même dimension.  
  
 Vous pouvez limiter l’accès en choisissant les membres auxquels accorder (AllowedSet) ou refuser (DeniedSet) l’accès. Pour ce faire, vous sélectionnez ou désélectionnez les membres de la dimension à inclure ou exclure du rôle.  
  
 La sécurité de base de la dimension est la plus simple : il suffit de sélectionner les attributs de dimension et les hiérarchies d'attributs à inclure ou à exclure dans le rôle. La sécurité avancée est plus complexe et nécessite une connaissance des scripts MDX. Les deux approches sont décrites ci-dessous.  

> [!NOTE]  
>  Les instructions suivantes supposent une connexion client qui émet des requêtes dans MDX. Si le client utilise DAX, comme Power View dans Power BI, la sécurité de la dimension n’est pas évidente dans les résultats de la requête. Pour plus d’informations, consultez [Présentation de Power View pour les modèles multidimensionnels](understanding-power-view-for-multidimensional-models.md) .
      
## <a name="prerequisites"></a>Configuration requise  
 Vous ne pouvez pas utiliser toutes les mesures, ni tous les membres de dimension dans les scénarios d'accès personnalisés. Une connexion échoue si un rôle restreint l'accès à une mesure ou un membre par défaut, ou s'il restreint l'accès à des mesures qui font partie d'expressions de mesure.  
  
 **Vérifier les obstructions en matière de sécurité des dimensions : mesures par défaut, membres par défaut et mesures utilisées dans les expressions de mesure**  
  
1.  Dans SQL Server Management Studio, cliquez avec le bouton droit sur un cube, puis sélectionnez **Générer un script du cube en tant que** | **ALTER To** | **Nouvelle fenêtre d’éditeur de requête**.  
  
2.  Recherchez **DefaultMeasure**. Vous devriez en trouver une pour le cube, et une pour chaque perspective. Quand vous définissez la sécurité des dimensions, évitez de restreindre l'accès aux mesures par défaut.  
  
3.  Ensuite, recherchez **MeasureExpression**. Une expression de mesure est une mesure basée sur un calcul, qui inclut souvent d'autres mesures. Vérifiez que la mesure que vous souhaitez restreindre n'est pas utilisée dans une expression. Sinon, continuez et limitez l'accès. Toutefois, veillez à exclure également toutes les références à cette mesure dans l'ensemble du cube.  
  
4.  Enfin, recherchez **DefaultMember**. Notez tous les attributs qui servent de membre par défaut à un attribut. Évitez d'appliquer des restrictions à ces attributs quand vous définissez la sécurité des dimensions.  
  
## <a name="basic-dimension-security"></a>Sécurité de base de la dimension  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l’instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], développez **Rôles** pour la base de données appropriée dans l’Explorateur d’objets, puis cliquez sur un rôle de base de données (ou créez un rôle de base de données).  
  
     Le rôle doit déjà avoir l'accès en lecture sur le cube. Pour plus d’informations, consultez [Octroyer des autorisations de cube ou de modèle &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-cube-or-model-permissions-analysis-services.md) .  
  
2.  Dans **Données de la dimension** | **De base**, sélectionnez la dimension pour laquelle vous définissez des autorisations.  
  
3.  Choisissez la hiérarchie d'attribut. Tous les attributs ne seront pas disponibles. Seuls les attributs avec **AttributeHierarchyEnabled** s’affichent dans la liste **Hiérarchie d’attribut** .  
  
4.  Choisissez les membres auxquels autoriser ou refuser l'accès. L’autorisation d’accès, via l’option **Sélectionner tous les membres** , est l’option par défaut. Nous vous suggérons de conserver cette valeur par défaut, puis de désélectionner chaque membre individuel qui ne doit pas être visible aux comptes d’utilisateur et de groupe Windows dans le volet **Appartenances** via ce rôle. L'avantage de ce choix est que les nouveaux membres ajoutés dans des opérations de traitement futures sont automatiquement disponibles aux personnes qui se connectent via ce rôle.  
  
     Vous pouvez également **Désélectionner tous les membres** pour révoquer l’accès de façon globale, puis sélectionner les membres à autoriser. Dans les opérations de traitement futures, les nouveaux membres ne sont pas visibles tant que vous ne modifiez pas manuellement la sécurité des données de la dimension pour autoriser l'accès.  
  
5.  Vous pouvez également cliquer sur **Avancé** pour activer les **Valeurs totales affichées** pour cette hiérarchie d’attribut. Cette option recalcule les agrégations en fonction des membres disponibles via ce rôle.  
  
    > [!NOTE]  
    >  Lorsque vous appliquez des autorisations qui suppriment des membres de la dimension, les totaux agrégés ne sont pas automatiquement recalculés. Supposons que le membre **Tous** d’une hiérarchie d’attribut retourne un nombre égal à 200 avant que les autorisations soient appliquées. Une fois les autorisations appliquées qui refusent l’accès à certains membres, **Tous** retourne toujours 200, même si les valeurs de membre visibles à l’utilisateur sont beaucoup moins nombreuses. Pour ne pas dérouter les consommateurs de votre cube, vous pouvez configurer le membre **Tous** pour qu’il soit l’agrégat de ces seuls membres, au lieu d’être l’agrégat de tous les membres de la hiérarchie d’attribut. Pour appeler ce comportement, vous pouvez activer **Valeurs totales affichées** sous l’onglet **Avancé** lors de la configuration de la sécurité de la dimension. Une fois activé, l'agrégation est calculée au moment de la requête au lieu d'être extraite d'agrégations précalculées. Cela peut avoir un effet notable sur les performances de la requête, c'est pourquoi il est conseillé de l'utiliser uniquement lorsque cela est nécessaire.  
  
## <a name="hiding-measures"></a>Masquage des mesures  
 Dans [Octroyer un accès personnalisé à des données de cellule &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-custom-access-to-cell-data-analysis-services.md), il a été expliqué que le masquage complet de tous les aspects visuels d’une mesure, et pas seulement de ses données de cellule, nécessite des autorisations sur les membres de la dimension. Cette section explique comment refuser l'accès aux métadonnées d'objet d'une mesure.  
  
1.  Dans **Données de la dimension** | **De base**, faites défiler la liste Dimension jusqu’aux dimensions du cube, puis sélectionnez **Dimension de mesures**.  
  
2.  Dans la liste des mesures, décochez la case pour les mesures qui ne doivent pas s'afficher aux utilisateurs qui se connectent via ce rôle.  
  
> [!NOTE]  
>  Vérifiez les conditions préalables pour apprendre à identifier les mesures qui peuvent perturber la sécurité du rôle.  
  
## <a name="advanced-dimension-security"></a>Sécurité avancée de la dimension  
 Si vous êtes familier de MDX, vous pouvez également écrire des expressions MDX qui définissent les critères pour les membres auxquels l'accès est autorisé ou refusé. Cliquez sur **Créer un rôle** | **Données de la dimension** | **Avancé** pour fournir le script.  
  
 Vous pouvez utiliser le Générateur MDX pour écrire l'instruction MDX. Pour plus d’informations, consultez [Générateur MDX &#40;Analysis Services – Données multidimensionnelles&#41;](http://msdn.microsoft.com/library/fecbf093-65ea-4e1b-b637-f04876f1cb0f). L’onglet **Avancé** a les options suivantes :  
  
 **Attribut**  
 Sélectionnez l'attribut dont vous voulez gérer la sécurité des membres.  
  
 **Jeu de membres autorisé**  
 Le AllowedSet peut être résolu en : aucun membre (valeur par défaut), tous les membres ou certains membres. Si vous autorisez l'accès à un attribut et ne définissez pas les membres du jeu autorisé, tous les membres sont accessibles. Si vous autorisez l'accès à un attribut et définissez un jeu de membres d'attribut spécifique, seuls les membres autorisés explicitement sont visibles.  
  
 La création d’un AllowedSet a un effet de contagion quand l’attribut participe à une hiérarchie à plusieurs niveaux. Par exemple, supposons qu'un rôle autorise l'accès à l'état Washington (dans un scénario où le rôle accorde des autorisations au service des ventes d'une société dans l'état de Washington). Pour les personnes qui se connectent via ce rôle, les requêtes qui contiennent les ancêtres (United States) ou les descendants (Seattle et Redmond) ne verront que les membres dans une chaîne qui contient l'état Washington. Dans la mesure où d'autres états ne sont pas explicitement autorisés, l'effet sera le même que si l'accès leur avait été refusé.  
  
> [!NOTE]  
>  Si vous définissez un jeu vide ({}) des membres d’attribut, aucun membre de l’attribut ne sera visible pour le rôle de base de données. Un jeu autorisé absent ne correspond pas à un jeu vide.  
  
 **Jeu de membres refusé**  
 La propriété DeniedSet peut être résolue en : aucun membre, tous les membres (valeur par défaut) ou certains membres d’attribut. Quand le jeu refusé contient uniquement un jeu de membres d'attribut spécifique, le rôle de base de données n'a pas accès à ces membres spécifiques ainsi qu'à ses descendants si l'attribut se trouve dans une hiérarchie à plusieurs niveaux. Prenons l'exemple du service des ventes de l'état de Washington. Si Washington se trouve dans le DeniedSet, les personnes qui se connectent via ce rôle verront tous les autres états à l’exception de Washington et de ses attributs descendants.  
  
 Souvenez-vous que le jeu refusé est une collection fixe. Lors du traitement de nouveaux membres qui sont ajoutés ensuite et auxquels l'accès doit également être refusé, vous devez modifier ce rôle pour ajouter ces membres à la liste.  
  
 **Membre par défaut**  
 La propriété DefaultMember détermine le jeu de données retourné à un client quand un attribut n’est pas explicitement inclus dans une requête. Quand l’attribut n’est pas explicitement inclus, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise un des membres par défaut suivants pour l’attribut :  
  
-   Si le rôle de base de données définit un membre par défaut pour l'attribut, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise le membre par défaut.  
  
-   Si le rôle de base de données ne définit pas un membre par défaut pour l'attribut, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise le membre par défaut défini pour l'attribut lui-même. Le membre par défaut d’un attribut, sauf si vous spécifiez le contraire, est le membre **Tous** (à moins que l’attribut soit défini comme ne pouvant pas faire l’objet d’une agrégation).  
  
 Par exemple, supposons qu’un rôle de base de données définit **Male** comme membre par défaut de l’attribut **Gender** . Si une requête n’inclut pas explicitement l’attribut **Gender** et ne spécifie pas un membre différent pour cet attribut, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] retourne un ensemble de données qui inclut uniquement les clients hommes. Pour plus d’informations sur la définition du membre par défaut, consultez [Définir un membre par défaut](../../analysis-services/multidimensional-models/attribute-properties-define-a-default-member.md).  
  
 **Activer les valeurs visibles**  
 La propriété VisualTotals indique si les valeurs des cellules agrégées affichées sont calculées en fonction de toutes les valeurs des cellules ou en fonction des valeurs des cellules auxquelles le rôle de base de données peut accéder.  
  
 Par défaut, la propriété VisualTotals est désactivée (définie sur **False**). Cette valeur par défaut optimise les performances, car [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] peut calculer rapidement le total de toutes les valeurs des cellules, au lieu de passer du temps à sélectionner les valeurs des cellules à calculer.  
  
 Cependant, la désactivation de la propriété VisualTotals peut créer un problème de sécurité si un utilisateur peut utiliser les valeurs des cellules agrégées pour en déduire les valeurs pour les membres d’attribut auxquels le rôle de base de données de l’utilisateur ne peut pas accéder. Par exemple, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise les valeurs de trois membres d’attribut pour calculer une valeur de cellule agrégée. Le rôle de base de données peut afficher deux de ces membres. En utilisant la valeur de cellule agrégée, un membre de ce rôle de base de données peut déduire la valeur du troisième membre de l'attribut.  
  
 La définition de la propriété VisualTotals sur **True** peut éliminer ce risque. Quand vous activez la propriété VisualTotals, un rôle de base de données peut afficher seulement les totaux agrégés des membres de la dimension sur lesquels il dispose d’une autorisation.  
  
 **Vérifier**  
 Cliquez pour tester la syntaxe MDX définie dans cette page.  
  
## <a name="see-also"></a>Voir aussi  
 [Octroyer des autorisations de cube ou de modèle &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-cube-or-model-permissions-analysis-services.md)   
 [Octroyer un accès personnalisé aux données des cellules &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-custom-access-to-cell-data-analysis-services.md)   
 [Accorder des autorisations sur les structures d’exploration de données et modèles &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/grant-permissions-on-data-mining-structures-and-models-analysis-services.md)   
 [Accorder des autorisations sur un objet de source de données & #40 ; Analysis Services & #41 ;](../../analysis-services/multidimensional-models/grant-permissions-on-a-data-source-object-analysis-services.md)  
  
  
