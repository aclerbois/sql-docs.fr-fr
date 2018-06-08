---
title: Présentation du modèle d’objet tabulaire (TOM) dans Analysis Services AMO | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9a075f78694727d723b70bd0ffcb7c23d10210f1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="understanding-tabular-object-model-tom-in-analysis-services-amo"></a>Présentation du modèle d’objet tabulaire (TOM) dans Analysis Services AMO
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Le modèle d’objet tabulaire (TOM) est une extension de la bibliothèque cliente objet AMO (Analysis Services Management Objects), créée pour prendre en charge des scénarios de programmation pour les modèles tabulaires créés au niveau de compatibilité 1200 et supérieur. Comme avec AMO, TOM fournit un moyen par programmation pour gérer les fonctions d’administration telles que la création de modèles, l’importation et l’actualisation des données et attribution des rôles et autorisations.  
  
TOM expose des métadonnées tabulaires native, tel que **modèle**, **tables**, **colonnes**, et **relations** objets.  Une vue d’ensemble de l’arborescence du modèle objet, vous trouverez ci-dessous, illustre la façon dont les éléments sont liés.  
  
 TOM étant une extension d’AMO, toutes les classes qui représentent des nouveaux objets tabulaires introduites dans SQL Server 2016 sont implémentées dans un nouvel assembly Microsoft.AnalysisServices.Tabular.dll. Les classes à usage général d’AMO ont été déplacées à l’assembly de Microsoft.AnalysisServices.Core. Votre code doit faire référence aux deux assemblys.
Consultez [installer, de distribuer et de faire référence à l’objet de modèle tabulaire &#40;Microsoft.AnalysisServices.Tabular&#41; ](../../analysis-services/tabular-model-programming-compatibility-level-1200/install-distribute-and-reference-the-tabular-object-model.md) pour plus d’informations.  
  
 Actuellement, l’API est disponible uniquement pour le code managé via le .NET framework. Pour consulter la liste complète des options, y compris le script et requête prise en charge, de programmation voir [programmation de modèle tabulaire pour 1200 de niveau de compatibilité](../../analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200.md).  
  
## <a name="tabular-object-model-hierarchy"></a>Hiérarchie du modèle objet tabulaire  
 À partir d’un point de vue logique, tous les objets tabulaires forment une arborescence, dont la racine est un **modèle**, d’une base de données. **Serveur** et **base de données** ne sont pas considérés comme « tableau », car ces objets peuvent également représenter une base de données multidimensionnelle hébergé sur un serveur s’exécutant en mode multidimensionnel ou d’un modèle tabulaire au niveau de compatibilité inférieur niveau qui n’utilise pas les métadonnées tabulaires pour les définitions d’objet. 
  
 À l’exception de **AttributeHierarchy**, **KPI**, et **LinguisticMetadata**, chaque objet enfant peut être un membre d’une collection. Par exemple, le **modèle** objet contient une collection de **Table** objets (via la **Tables** propriété), avec chaque **Table** objet contenant une collection de **colonne** objets et ainsi de suite.  
  
 Le descendant plus bas niveau de n’importe quel objet parent dans la hiérarchie est une **Annotation** objet qui peut être utilisé pour éventuellement étendre le schéma, que vous fournissez le code pour le gérer.  
  
 ![diagramme de hiérarchie d’objets](../../analysis-services/tabular-model-programming-compatibility-level-1200/media/ssastomobjectmodeldiagram.png "diagramme de hiérarchie d’objets")  
  
## <a name="tom-and-other-related-technologies"></a>TOM et autres technologies connexes

TOM repose sur l’infrastructure d’AMO, ce qui autorise également la multidimensionnel et les bases de données tabulaires à des niveaux de compatibilité inférieur à 1 200.

Cela a certaines implications pratiques.
Première de toutes les, lorsque vous gérez des objets qui ne sont pas spécifiés dans les métadonnées tabulaires (comme un **Server** ou **base de données**), vous devez tirer parti des parties de la pile AMO existante qui décrivent ces objets. Avec l’API héritée est le concept d’objets principaux et secondaires qui fournissent des descriptions granulaires de l’état objet découvert à partir du serveur, ou lors de l’enregistrement sur le serveur. Expose des méthodes pour la classe MajorObject sous l’espace de noms Microsoft.AnalysisServices **Actualiser** et **mise à jour**. Les objets secondaires sont uniquement l’actualisation ou enregistré via l’objet principal qui les contient.

En revanche, lorsque vous gérez des objets qui font partie des métadonnées tabulaires (tel que **modèle** ou **Table**), vous tirer parti d’une pile tabulaire entièrement nouvelle. Dans cette pile, les mises à jour sont affinées, ce qui signifie que chaque objet de métadonnées (dérivées de la **MetadataObject** classe sous l’espace de noms Microsoft.AnalysisServices.Tabular) peuvent être enregistrés individuellement sur le serveur. En règle générale, vous serez découvrir l’ensemble de le **modèle**, puis apportez les modifications des objets de métadonnées individuels dans cette section (tel que **Table** ou **colonne**), puis appelez **Model.SaveChanges()** méthode (qui comprend les modifications apportées par vous-même au niveau de granularité fin), envoyer des commandes vers le serveur de mise à jour uniquement les objets qui a changent.

### <a name="tom-and-xmla"></a>TOM et XMLA

Sur le câble, TOM utilise le protocole XMLA pour communiquer avec le serveur Analysis Services et de gérer des objets. Lors de la gestion des objets non tabulaires, TOM utilise [ASSL](../scripting/analysis-services-scripting-language-assl-for-xmla.md), l’extension d’Analysis Services Scripting Language de XMLA. Lors de la gestion des objets tabulaires, TOM utilise un protocole tabulaire MS-SSAS, également une extension du XMLA. Consultez [tabulaires SSAS-T-MS SQL Server Analysis Services documentation du protocole](https://msdn.microsoft.com/library/mt719260.aspx) pour plus d’informations.

### <a name="tom-and-json"></a>TOM et JSON

Les métadonnées tabulaires sont structurée comme documents JSON, a une syntaxe nouvelle de définition de commande et l’objet modèle via le langage de script de modèle tabulaire [TMSL](../tabular-model-scripting-language-tmsl-reference.md). Le langage de script utilise JSON pour le corps des demandes et réponses.

Bien que TMSL et TOM exposent les mêmes objets (**Table**, **colonne**, et ainsi de suite) et les mêmes opérations (**créer**, **supprimer**, **Actualiser**), TOM n’utilise pas TMSL sur le câble (il utilise le protocole tabulaire SSAS MS au lieu de cela, comme indiqué précédemment).

En tant qu’utilisateur, vous pouvez choisir gérer les bases de données tabulaires via la bibliothèque TOM à partir de votre programme c# ou un script PowerShell ou via un script TMSL exécutées via PowerShell, SQL Server Management Studio (SSMS) ou d’un travail de l’Agent SQL Server.

La décision d’utiliser un ou l’autre se résument aux caractéristiques de vos besoins. La bibliothèque de TOM fournit des fonctionnalités plus riches par rapport à TMSL. Plus précisément, tandis que TMSL propose uniquement les opérations au niveau de la base de données, de Table, de Partition ou de rôle à granularité grossière, TOM autorise les opérations beaucoup plus fines. Pour générer ou mettre à jour des modèles par programme, vous devez l’éventail complet de l’API dans la bibliothèque TOM.
  
## <a name="see-also"></a>Voir aussi  
 [Programmation de modèle tabulaire de niveau de compatibilité 1200](../../analysis-services/tabular-model-programming-compatibility-level-1200/tabular-model-programming-for-compatibility-level-1200.md)   
 [Niveau de compatibilité pour les modèles tabulaires dans Analysis Services](../../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md)  
[PowerShell Analysis Services](../../analysis-services/powershell/analysis-services-powershell-reference.md)
  
