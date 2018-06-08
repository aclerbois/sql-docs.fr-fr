---
title: Traductions de dimension | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b58303abfd94710452143e014fee0019826a2a97
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="dimension-translations"></a>Traductions de dimension
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une traduction est un mécanisme simple permettant de modifier les étiquettes et les légendes affichées d'une langue à une autre. Chaque traduction est définie comme une paire de valeurs : une chaîne avec le texte traduit et un nombre avec l'ID de langue. Les traductions sont disponibles pour tous les objets dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Les valeurs d'attribut peuvent également être traduites pour les dimensions. L'application cliente est chargée de rechercher le paramètre de langue que l'utilisateur a défini et bascule pour afficher toutes les légendes et les étiquettes dans cette langue. Un objet peut avoir autant de traductions que vous le souhaitez.  
  
 Un objet <xref:Microsoft.AnalysisServices.Translation> simple est composé d'un numéro d'ID de langue et de la légende traduite. Le numéro d’ID de langue est une **entier** avec l’ID de langue. La légende traduite est le texte traduit.  
  
 Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], une traduction de dimension est une représentation spécifique à la langue du nom d’une dimension, le nom d’un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] objet ou l’un de ses membres, tels que d’un niveau de la légende, membre ou une hiérarchie. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] prend également en charge les traductions d’objets de cube.  
  
 Les traductions permettent au serveur de prendre en charge les applications clientes qui autorisent l'emploi de plusieurs langues. Souvent, des utilisateurs de différents pays affichent un cube et ses dimensions. Il est utile de pouvoir traduire divers éléments d'un cube et ses dimensions dans une langue différente, de sorte que ces utilisateurs puissent afficher et comprendre le cube. Par exemple, un utilisateur situé en France peut accéder à un cube à partir d’une station de travail avec les paramètres régionaux Français et voir les valeurs de propriété d’objet en Français. En revanche, un utilisateur d'une entreprise en Allemagne qui accède au même cube depuis une station de travail avec les paramètres régionaux allemands peut voir les mêmes valeurs des propriétés des objets en allemand.  
  
 Les informations de classement et de langue pour l'ordinateur client sont stockées sous la forme d'un identificateur local (LCID). Lors de la connexion, le client envoie cet identificateur à l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Cette instance utilise ensuite l'identificateur LCID pour déterminer quel jeu de traductions utiliser en fournissant les métadonnées pour des objets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Si un objet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne contient pas la traduction spécifiée, la langue par défaut est utilisée pour retourner le contenu au client.  
  
## <a name="see-also"></a>Voir aussi  
 [Traductions de cube](../../analysis-services/multidimensional-models-olap-logical-cube-objects/cube-translations.md)   
 [Prise en charge de la traduction dans Analysis Services](../../analysis-services/translation-support-in-analysis-services.md)   
 [Globalisation conseils et meilleures pratiques & #40 ; Analysis Services & #41 ;](../../analysis-services/globalization-tips-and-best-practices-analysis-services.md)  
  
  
