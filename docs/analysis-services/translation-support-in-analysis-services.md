---
title: Prise en charge de la traduction dans Analysis Services | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 882434c2bd0a194a2ecbe21a62d3b22964b30f09
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="translation-support-in-analysis-services"></a>Prise en charge des traductions dans Analysis Services
[!INCLUDE[ssas-appliesto-sqlas-aas](../includes/ssas-appliesto-sqlas-aas.md)]

  Dans des modèles de données [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , vous pouvez incorporer plusieurs traductions d’une légende ou d’une description pour fournir des chaînes spécifiques aux paramètres régionaux en fonction de l’identificateur LCID. Dans le cas des modèles multidimensionnels, vous pouvez ajouter des traductions pour le nom de la base de données, les objets cube et les objets de dimension de base de données. Pour les modèles tabulaires, vous pouvez traduire les descriptions et légendes des tables et des colonnes.  
  
 La définition d’une traduction crée les métadonnées et la légende traduite à l’intérieur du modèle. Cependant, pour restituer des chaînes localisées dans une application cliente, vous devez définir la propriété **Language** sur l’objet ou passer un paramètre **Culture** ou **Locale Identifier** sur la chaîne de connexion (par exemple, en définissant `LocaleIdentifier=1036` afin de retourner des chaînes en français).  
  
 Utilisez **Locale Identifier** si vous souhaitez prendre en charge plusieurs traductions simultanées du même objet dans différentes langues. Définir la propriété **Language** fonctionne, mais cela affecte aussi le traitement et les requêtes, ce qui pourrait avoir des conséquences inattendues. Définir **Locale Identifier** constitue le meilleur choix, car il est utilisé uniquement pour retourner des chaînes traduites.  
  
 Une traduction est composée d'un identificateur de paramètres régionaux (LCID), d'une légende traduite pour l'objet (par exemple le nom de la dimension ou de l'attribut) et éventuellement d'une liaison à une colonne qui fournit des valeurs de données dans la langue cible. Vous pouvez avoir plusieurs traductions, mais vous ne pouvez en utiliser qu'une seule pour une connexion donnée. Il n'existe aucune limite théorique quant au nombre de traductions que vous pouvez incorporer dans le modèle, mais chaque traduction ajoute une complexité au test et toutes les traductions doivent partager le même classement. Vous devez donc garder ces contraintes naturelles à l'esprit lors de la conception de votre solution.  
  
> [!TIP]  
>  Vous pouvez utiliser des applications clientes telles qu'Excel, Management Studio et SQL Server Profiler pour retourner des chaînes traduites. Pour plus d’informations, consultez [Conseils et meilleures pratiques en matière de globalisation &#40;Analysis Services&#41;](../analysis-services/globalization-tips-and-best-practices-analysis-services.md) .  
  
## <a name="how-to-add-translated-metadata-to-model-in-analysis-services"></a>Comment ajouter des métadonnées traduites à un modèle dans Analysis Services  
 Pour obtenir des instructions pas à pas, cliquez sur les liens suivants :  
  
-   [Traductions dans les modèles tabulaires &#40;Analysis Services&#41;](../analysis-services/tabular-models/translations-in-tabular-models-analysis-services.md)  
  
-   [Traductions dans les modèles multidimensionnels &#40;Analysis Services&#41;](../analysis-services/multidimensional-models/translations-in-multidimensional-models-analysis-services.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Scénarios de globalisation pour Analysis Services](../analysis-services/globalization-scenarios-for-analysis-services.md)   
 [Langues et classements & #40 ; Analysis Services & #41 ;](../analysis-services/languages-and-collations-analysis-services.md)   
 [Définir ou modifier le classement des colonnes](../relational-databases/collations/set-or-change-the-column-collation.md)   
 [Globalisation conseils et meilleures pratiques & #40 ; Analysis Services & #41 ;](../analysis-services/globalization-tips-and-best-practices-analysis-services.md)  
  
  
