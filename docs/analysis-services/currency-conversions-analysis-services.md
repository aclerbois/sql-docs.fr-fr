---
title: Conversions monétaires (Analysis Services) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ebcce8d042b7a87771f99bac53b78bb59af6f7e1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="currency-conversions-analysis-services"></a>Conversions monétaires (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas-aas](../includes/ssas-appliesto-sqlas-aas.md)]

  [!INCLUDE[applies](../includes/applies-md.md)] Multidimensionnel uniquement  
  
 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] utilise une combinaison de fonctionnalités, guidées par des scripts MDX (Multidimensional Expressions), pour assurer la prise en charge des conversions monétaires dans les cubes qui prennent en charge plusieurs devises.  
  
## <a name="currency-conversion-terminology"></a>Terminologie propre aux conversions monétaires  
 La terminologie répertoriée ci-dessous est utilisée dans [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] pour décrire la fonctionnalité de conversion monétaire :  
  
 Devise pivot  
 Devise dans laquelle les taux de change sont entrés dans le groupe de mesures de taux.  
  
 Devise locale  
 Devise utilisée pour stocker les transactions sur lesquelles se basent les mesures à convertir.  
  
 La devise locale peut être identifiée par un des éléments suivants :  
  
-   Un identificateur de devise dans la table de faits stockée avec la transaction, comme cela est communément le cas avec les applications bancaires où la transaction identifie elle-même la devise qu'elle utilise.  
  
-   Un identificateur de devise associé à un attribut dans une table de dimensions qui est ensuite associée à une transaction dans la table de faits, comme cela est communément le cas dans les applications financières où un emplacement ou un autre identificateur, tel qu'une filiale, identifie la devise utilisée pour une transaction associée.  
  
 Devise pour les rapports  
 Devise dans laquelle les transactions sont converties à partir de la devise pivot.  
  
> [!NOTE]  
>  Pour les conversions monétaires plusieurs-à-un, la devise pivot et la devise pour les rapports sont les mêmes.  
  
 Dimension monétaire  
 Dimension de base de données définie avec les paramètres suivants :  
  
-   La propriété **Type** de la dimension a la valeur Currency.  
  
-   La propriété **Type** d'un attribut de la dimension a la valeur CurrencyName.  
  
    > [!IMPORTANT]  
    >  Les valeurs de cet attribut doivent être utilisées dans toutes les colonnes qui doivent contenir un identificateur de devise.  
  
 Groupe de mesures de taux  
 Groupe de mesures dans un cube, défini avec les paramètres suivants :  
  
-   Une relation de dimension régulière existe entre une dimension monétaire et le groupe de mesures de taux.  
  
-   Une relation de dimension régulière existe entre une dimension de temps et le groupe de mesures de taux.  
  
-   En option, la propriété **Type** a la valeur ExchangeRate. Alors que l'Assistant Business Intelligence utilise les relations avec les dimensions monétaire et de temps pour identifier les groupes de mesures de taux probables, attribuer à la propriété **Type** la valeur ExchangeRate permet aux applications clientes d'identifier plus facilement les groupes de mesures de taux.  
  
-   Une ou plusieurs mesures, représentant les taux de change contenus par le groupe de mesures de taux.  
  
 Dimension monétaire de rapport  
 Dimension, définie par l'Assistant Business Intelligence après qu'une conversion monétaire a été définie, qui contient les devises pour les rapports relatives à cette conversion monétaire. La dimension monétaire de rapport se base sur une requête nommée, définie dans la vue de source de données sur laquelle se base la dimension monétaire associée au groupe de mesures de taux, à partir de la table principale de dimensions de la dimension monétaire. La dimension est définie avec les paramètres suivants :  
  
-   La propriété **Type** de la dimension a la valeur Currency.  
  
-   La propriété **Type** de l'attribut clé de la dimension a la valeur CurrencyName.  
  
-   La propriété **Type** d'un attribut au sein de la dimension a la valeur CurrencyDestination et la colonne liée à cet attribut contient les identificateurs de devises qui représentent les devises pour les rapports de la conversion monétaire.  
  
## <a name="defining-currency-conversions"></a>Définition des conversions monétaires  
 Vous pouvez utiliser l'Assistant Business Intelligence pour définir la fonctionnalité de conversion monétaire pour un cube ou vous pouvez définir manuellement les conversions monétaires à l'aide de scripts MDX.  
  
### <a name="prerequisites"></a>Conditions préalables  
 Avant de pouvoir définir une conversion monétaire dans un cube à l'aide de l'Assistant Business Intelligence, vous devez définir au préalable au moins une dimension monétaire, au moins une dimension de temps et au moins un groupe de mesures de taux. À partir de ces objets, l'Assistant Business Intelligence peut récupérer les données et métadonnées utilisées pour générer la dimension monétaire de rapport et le script MDX nécessaires pour assurer la fonctionnalité de conversion monétaire.  
  
### <a name="decisions"></a>Décisions  
 Vous devez prendre les décisions ci-dessous pour que l'Assistant Business Intelligence puisse générer la dimension monétaire de rapport et le script MDX nécessaires pour assurer la fonctionnalité de conversion monétaire :  
  
-   Sens du taux de change  
  
-   Membres convertis  
  
-   Type de conversion  
  
-   Devises locales  
  
-   Devises pour les rapports  
  
### <a name="exchange-rate-directions"></a>Sens des taux de change  
 Le groupe de mesures de taux contient des mesures représentant les taux de change entre les devises locales et la devise pivot (souvent appelée « devise d'entreprise »). La combinaison du sens du taux de change et du type de conversion détermine l'opération effectuée sur les mesures que doit convertir le script MDX généré à l'aide de l'Assistant Business Intelligence. Le tableau ci-dessous décrit les opérations effectuées en fonction du sens du taux de change et du type de conversion, selon les options de sens de taux de change et les sens de conversion disponibles dans l'Assistant Business Intelligence.  
  
|||||  
|-|-|-|-|  
|Sens du taux de change|**Plusieurs-à-un**|**Un-à-plusieurs**|**Plusieurs-à-plusieurs**|  
|**Devise pivot n vers 1 devise d'exemple**|Multipliez la mesure à convertir par la mesure du taux de change pour la devise locale afin de convertir la mesure dans la devise pivot.|Divisez la mesure à convertir par la mesure du taux de change pour la devise pour les rapports afin de convertir la mesure dans la devise pour les rapports.|Multipliez la mesure à convertir par la mesure du taux de change pour la devise locale afin de convertir la mesure dans la devise pivot, puis divisez la mesure convertie par la mesure du taux de change pour la devise pour les rapports afin de convertir la mesure dans la devise pour les rapports.|  
|**Devise d'exemple n vers 1 devise pivot**|Divisez la mesure à convertir par la mesure du taux de change pour la devise locale afin de convertir la mesure dans la devise pivot.|Multipliez la mesure à convertir par la mesure du taux de change pour la devise pour les rapports afin de convertir la mesure dans la devise pour les rapports.|Divisez la mesure à convertir par la mesure du taux de change pour la devise locale afin de convertir la mesure dans la devise pivot, puis multipliez la mesure convertie par la mesure du taux de change pour la devise pour les rapports afin de convertir la mesure dans la devise pour les rapports.|  
  
 Vous pouvez choisir le sens du taux de change à la page **Définir les options de conversion monétaire** de l'Assistant Business Intelligence. Pour plus d’informations sur la spécification du sens de conversion, consultez [Définir les options de conversion monétaire &#40;Assistant Business Intelligence&#41;](http://msdn.microsoft.com/library/a49d4e1f-bdda-4a83-ab4f-ce8c500e1d6d).  
  
### <a name="converted-members"></a>Membres convertis  
 Vous pouvez utiliser l'Assistant Business Intelligence pour spécifier les mesures du groupe de mesures de taux qui sont utilisées pour convertir les valeurs pour :  
  
-   les mesures d'autres groupes de mesures ;  
  
-   les membres d'une hiérarchie d'attribut pour un attribut de compte dans une dimension de base de données ;  
  
-   les types de comptes, utilisés par les membres d'une hiérarchie d'attribut pour un attribut de compte dans une dimension de base de données.  
  
 L'Assistant Business Intelligence utilise ces informations dans le script MDX qu'il génère pour déterminer l'étendue du calcul de conversion monétaire. Pour plus d’informations sur la spécification des membres de conversion monétaire, consultez [Sélectionner les membres &#40;Assistant Business Intelligence&#41;](http://msdn.microsoft.com/library/1a147461-d594-41e7-a41d-09d2d003e1e0).  
  
### <a name="conversion-types"></a>Types de conversion  
 L'Assistant Business Intelligence prend en charge trois types différents de conversion monétaire :  
  
-   **Un-à-plusieurs**  
  
     Les transactions sont stockées dans la table de faits dans la devise pivot, puis converties vers une ou plusieurs devises pour les rapports.  
  
     Par exemple, la devise pivot peut correspondre à des dollars américains (USD) et la table de faits peut stocker les transactions en USD. Ce type de conversion convertit ces transactions à partir de la devise pivot vers les devises pour les rapports spécifiées. Il en résulte que les transactions peuvent être stockées dans la devise pivot spécifiée et affichées dans la devise pivot spécifiée ou dans les devises pour les rapports spécifiées dans la dimension monétaire de rapport définie pour la conversion monétaire.  
  
-   **Plusieurs-à-un**  
  
     Les transactions sont stockées dans la table de faits dans des devises locales, puis converties dans la devise pivot. La devise pivot sert d'unique devise pour les rapports spécifiée dans la dimension monétaire de rapport.  
  
     Par exemple, la devise pivot peut correspondre à des dollars américains (USD) et la table de faits peut stocker les transactions en euros (EUR), en dollars australiens (AUD) et en pesos mexicains (MXN). Ce type de conversion convertit ces transactions à partir de leurs devises locales spécifiées dans la devise pivot. Il en résulte que les transactions peuvent être stockées dans les devises locales spécifiées et affichées dans la devise pivot spécifiée dans la dimension monétaire de rapport définie pour la conversion monétaire.  
  
-   **Plusieurs-à-plusieurs**  
  
     Les transactions sont stockées dans la table de faits dans des devises locales. La fonctionnalité de conversion monétaire convertit ces transactions dans la devise pivot, puis dans une ou plusieurs autres devises pour les rapports.  
  
     Par exemple, la devise pivot peut correspondre à des dollars américains (USD) et la table de faits peut stocker les transactions en euros (EUR), en dollars australiens (AUD) et en pesos mexicains (MXN). Ce type de conversion convertit ces transactions à partir de leurs devises locales spécifiées dans la devise pivot, puis convertit à nouveau les transactions converties de la devise pivot dans les devises pour les rapports spécifiées. Il en résulte que les transactions peuvent être stockées dans les devises locales spécifiées et affichées dans la devise pivot spécifiée ou dans les devises pour les rapports spécifiées dans la dimension monétaire de rapport définie pour la conversion monétaire.  
  
 La spécification du type de conversion permet à l'Assistant Business Intelligence de définir la requête nommée et la structure de dimensions de la dimension monétaire de rapport, ainsi que la structure du script MDX défini pour la conversion monétaire.  
  
### <a name="local-currencies"></a>Devises locales  
 Si vous choisissez un type de conversion plusieurs-à-plusieurs ou plusieurs-à-un pour la conversion monétaire, vous devez spécifier comment identifier les devises locales à partir desquelles le script MDX généré par l'Assistant Business Intelligence doit effectuer les calculs de conversion monétaire. La devise locale d'une transaction dans une table de faits peut être identifiée de deux manières différentes :  
  
-   Le groupe de mesures contient une relation de dimension régulière avec la dimension monétaire. Par exemple, dans la base de données [!INCLUDE[ssAWDWsp](../includes/ssawdwsp-md.md)] de l'exemple [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , le groupe de mesures Internet Sales (Ventes Internet) possède une relation de dimension régulière avec la dimension monétaire. La table de faits pour ce groupe de mesures contient une colonne clé étrangère qui fait référence aux identificateurs de devises dans la table de dimensions pour cette dimension. Dans ce cas, vous pouvez sélectionner l'attribut à partir de la dimension monétaire qui est référencé par le groupe de mesures pour identifier la devise locale pour les transactions dans la table de faits pour ce groupe de mesures. Cette situation a lieu le plus souvent dans les applications bancaires, où la transaction détermine elle-même la devise qu'elle utilise.  
  
-   Le groupe de mesures contient une relation de dimension référencée avec la dimension monétaire, via une autre dimension qui fait référence directement à la dimension monétaire. Par exemple, dans l'exemple [!INCLUDE[ssAWDWsp](../includes/ssawdwsp-md.md)] de base de données [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , le groupe de mesures Financial Reporting a une relation de dimension référencée avec la dimension Devise via la dimension Organisation. La table de faits pour ce groupe de mesures contient une colonne clé étrangère qui fait référence aux membres de la table de dimensions pour la dimension Organization. La table de dimensions pour la dimension Organization, à son tour, contient une colonne clé étrangère qui fait référence aux identificateurs de devises dans la table de dimensions pour la dimension Currency. Cette situation a lieu le plus souvent dans les applications de comptabilité, où l'emplacement ou la filiale pour une transaction détermine la devise utilisée pour la transaction. Dans ce cas, vous pouvez sélectionner l'attribut qui fait référence à la dimension monétaire à partir de la dimension pour l'entité commerciale.  
  
### <a name="reporting-currencies"></a>Devises pour les rapports  
 Si vous choisissez un type de conversion plusieurs-à-plusieurs ou un-à-plusieurs pour la conversion monétaire, vous devez spécifier les devises pour les rapports pour lesquelles le script MDX généré par l'Assistant Business Intelligence doit effectuer les calculs de conversion monétaire. Vous pouvez spécifier tous les membres de la dimension monétaire liée au groupe de mesures de taux ou sélectionner des membres individuels à partir de la dimension.  
  
 L'Assistant Business Intelligence crée une dimension monétaire de rapport, basée sur une requête nommée construite à partir de la table de dimensions pour la dimension monétaire à l'aide des devises pour les rapports sélectionnées.  
  
> [!NOTE]  
>  Si vous sélectionnez le type de conversion un-à-plusieurs, une dimension monétaire de rapport est également créée. La dimension contient un seul membre, qui représente la devise pivot, car la devise pivot est également utilisée comme devise pour les rapports pour une conversion monétaire un-à-plusieurs.  
  
 Une dimension monétaire de rapport distincte est définie pour chaque conversion monétaire définie dans un cube. Vous pouvez modifier le nom des dimensions monétaires de rapport après leur création, mais dans ce cas vous devez également mettre à jour le script MDX généré pour cette conversion monétaire pour garantir que le nom correct est utilisé par la commande de script lors d'une référence à la dimension monétaire de rapport.  
  
## <a name="defining-multiple-currency-conversions"></a>Définition de plusieurs conversions monétaires  
 Grâce à l'Assistant Business Intelligence, vous pouvez définir autant de conversions monétaires qu'en nécessite votre solution Business Intelligence. Vous pouvez remplacer une conversion monétaire existante ou ajouter une nouvelle conversion monétaire au script MDX pour un cube. Plusieurs conversions monétaires définies dans un même cube confèrent de la flexibilité aux applications Business Intelligence qui ont des conditions requises complexes en matière de rapports, telles que des applications de comptabilité qui prennent en charge plusieurs conditions requises de conversions distinctes pour la rédaction de rapports internationaux.  
  
### <a name="identifying-currency-conversions"></a>Identification des conversions monétaires  
 L'Assistant Business Intelligence identifie chaque conversion monétaire en encadrant les commandes de script liées à la conversion monétaire par les commentaires suivants :  
  
 `//<Currency conversion>`  
  
 `...`  
  
 `[MDX statements for the currency conversion]`  
  
 `...`  
  
 `//</Currency conversion>`  
  
 La modification ou la suppression de ces commentaires empêche l'Assistant Business Intelligence de détecter la conversion monétaire. Par conséquent, vous ne devez pas modifier ces commentaires.  
  
 L'Assistant stocke également les métadonnées dans des commentaires au sein de ces commentaires, y compris la date et l'heure de création, l'utilisateur et le type de conversion. Ces commentaires ne doivent pas être modifiés non plus, car l'Assistant Business Intelligence utilise ces métadonnées pour afficher les conversions monétaires existantes.  
  
 Vous pouvez modifier les commandes de script incluses dans une conversion monétaire, selon les besoins. Toutefois, si vous remplacez la conversion monétaire, vos modifications seront perdues.  
  
## <a name="see-also"></a>Voir aussi  
 [Scénarios de globalisation pour Analysis Services](../analysis-services/globalization-scenarios-for-analysis-services.md)  
  
  
