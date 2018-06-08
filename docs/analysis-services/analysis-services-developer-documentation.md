---
title: Analysis Services Documentation pour développeurs | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 978819f3b8af369942dde2c3c18d6ca117489633
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="analysis-services-developer-documentation"></a>Documentation du développeur Analysis Services
[!INCLUDE[ssas-appliesto-sqlas-aas](../includes/ssas-appliesto-sqlas-aas.md)]

Dans Analysis Services, presque tous les objets et la charge de travail sont programmable, et il existe souvent plusieurs approche sélectionnables.  Options incluent l’écriture de code managé, un script ou à l’aide de normes ouvertes, comme XMLA et MSOLAP si les besoins de votre solution empêchent l’utilisation du .NET framework.

## <a name="what-you-can-accomplish-in-code"></a>Vous pouvez effectuer dans le code
Scénarios de programmation classiques incluent le serveur et déploiement de la base de données, administration, modèle de création de la base de données et des accès aux données de vos applications personnalisées et les rapports qui utilisent des données Analysis Services. Commune à tous ces scénarios est une hiérarchie définition architecture et objet fixe, avec les opérations de bien comprendre qui s’étendent sur la définition de données, de traitement et de charges de travail de requête.

Bien que les objets et les charges de travail sont programmables, ils ne sont pas extensibles. Plus précisément, vous ne pouvez pas créer cartouches personnalisé permettant de récupèrent des données à partir de sources de données non pris en charge, personnaliser ou remplacement les comportements de moteur de formule ou de stockage, ni créer des types de métadonnées d’objets sur un serveur, la base de données ou le modèle.

Pour développer davantage le dernier point sur la création de nouveaux types d’objets : alors que vous ne pouvez pas créer un nouveau type d’objet, vous pouvez créer des objets calculés créés à partir des expressions ou du code en cours d’exécution. Pas tous les éléments de votre modèle doivent être prédéfinies et mappée à une structure de données existante. En outre, vous pouvez étendre le schéma via les Annotations dans AMO pour passer des informations spécifiques à l’objet à votre application cliente.

## <a name="choose-a-platform-or-approach-to-development"></a>Choisir une plateforme ou une approche du développement
Analysis Services fournit plusieurs façons de personnaliser une solution dans le code, mais la plupart des développeurs utilisent l’API managé ou un script.

- Inclure des API managées [AMO et TOM](http://msdn.microsoft.com/library/mt436122.aspx) pour la définition de données et les tâches d’administration, et [ADOMD.NET](http://msdn.microsoft.com/library/mt465769.aspx) pour la prise en charge des requêtes à partir du code client. Dans SQL Server 2016, AMO est mis à jour pour utiliser les nouvelles métadonnées tabulaires pour les modèles créés ou mis à niveau vers le niveau de compatibilité 1200 et supérieur.

- Script peut atteindre souvent les mêmes résultats que d’un programme exécutable, avec éventuellement moins de travail.

  - Vous pouvez écrire le script PowerShell à l’aide des composants PowerShell Analysis Services qui appellent directement des types AMO. Dans PowerShell, vous pouvez également créer et exécuter/XMLA ASSL ou TMSL (dans JSON).

  - ASSL et TMSL sont des langages de script qui fournissent des objets utilisés dans les découvrir et exécutent des opérations. Le type de script que vous utilisez dépend du serveur, base de données ou modèle sous-jacent.

  - Les modèles tabulaires ou des bases de données au niveau de compatibilité 1200 et les versions ultérieures utilisent le script langage TMSL (Tabular Model), qui est au format JSON.

  - Les modèles multidimensionnels et tabulaires niveaux de compatibilité 1050-1103 utiliser Analysis Services Scripting Language (ASSL), qui est l’extension de la norme ouverte XMLA Analysis Services.

  - Vous pouvez générer le script ASSL ou TMSL dans Management Studio. Vous pouvez également utiliser **afficher le Code** dans SQL Server Data Tools pour afficher la définition du modèle dans ASSL ou TMSL.

- Bien qu’il soit possible de créer une solution basée sur les normes ouvertes de XMLA et MDX, il est très rare de faire. Il n’existe aucune documentation de XMLA et dessine des référence MDX pour aider à vous et la plupart des et Communauté forum prennent en charge à partir d’expériences .NET ou des technologies natives (MSOLAP).

## <a name="programming-in-analysis-services"></a>Programmation dans Analysis Services
[Programmation de données d’exploration de données](../analysis-services/data-mining-programming.md) décrit les approches de création de solutions qui incluent des objets d’exploration de données.

[Programmation de modèle multidimensionnel](../analysis-services/multidimensional-models/multidimensional-model-programming.md) décrit les tâches de développement et les approches permettant d’intégrer les objets de modèle multidimensionnel dans une solution personnalisée.

[Programmation modèle tabulaire pour 1200 de niveau de compatibilité et versions ultérieures](../analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200.md)
**nouveau dans SQL Server 2016**.  Récapitule les interfaces et langages de script utilisés pour travailler avec tabulaire 1200 et les modèles plus élevées par programme.

[Programmation de modèle tabulaire de 1050 de niveaux de compatibilité 1103 par le biais](../analysis-services/tabular-model-programming-compatibility-levels-1050-1103/tabular-model-programming-for-compatibility-levels-1050-through-1103.md) cette documentation s’adresse aux développeurs qui prennent en charge les modèles tabulaires aux niveaux de compatibilité antérieure. Elle décrit les extensions CSDL qui définissent un modèle tabulaire dans la syntaxe XML. Il inclut également des informations sur la syntaxe et les définitions de modèle d’objet tabulaire.

[Analysis Services Management Objects (AMO)](https://msdn.microsoft.com/library/mt436122.aspx) documentation référence du développeur pour le fournisseur managé, Analysis Services Management Objects (AMO), pour l’administration, notamment le traitement et de définition de données.

[ADOMD.NET](http://msdn.microsoft.com/library/mt465769.aspx) documentation référence du développeur pour le fournisseur managé, ADOMD.NET, utilisé pour les données par programme les charges de travail access et la requête.

[Ensembles de lignes de schéma Analysis Services](../analysis-services/schema-rowsets/analysis-services-schema-rowsets.md) décrit les ensembles de lignes de schéma qui fournissent des informations sur l’état du serveur, les opérations de serveur et les objets de base de données.

[XML for Analysis &#40;XMLA&#41; référence](../analysis-services/xmla/xml-for-analysis-xmla-reference.md) concepts décrit de XMLA qui peuvent vous aider à comprennent comment XMLA contribue à votre solution personnalisée. Décrit également le niveau de compatibilité avec la spécification XMLA 1.1.

[Analysis Services Scripting Language &#40;ASSL de XMLA&#41; ](../analysis-services/scripting/analysis-services-scripting-language-assl-for-xmla.md) décrit les extensions ASSL à XMLA. ASSL propose un langage de manipulation et de définition de données pour les modèles multidimensionnels Analysis Services qui complète la spécification XMLA.

[Langage de script de modèle tabulaire &#40;TMSL&#41; référence](../analysis-services/tabular-model-scripting-language-tmsl-reference.md) TMSL est une représentation JSON de modèles tabulaires au niveau de compatibilité 1200 et supérieur. Définitions d’objet sont basées sur les constructions de métadonnées tabulaires comme table, colonne et relation plutôt que des métadonnées multidimensionnelles qui peuvent être peu si vous débutez avec modélisation de données Analysis Services en mode tabulaire.

[Analysis Services PowerShell Reference](../analysis-services/powershell/analysis-services-powershell-reference.md) documente les applets de commande utilisé pour les fonctions d’administration, ainsi que l’usage général **Invoke-ASCmd** applet de commande qui accepte n’importe quel script ou une requête en tant qu’entrée.

## <a name="see-also"></a>Voir aussi
[Informations techniques de référence ](../analysis-services/powershell/technical-reference-ssas.md) 
 [de requêtes et de référence de langage d’Expression &#40;Analysis Services&#41;](http://msdn.microsoft.com/library/gg492188.aspx)
