---
title: Analyser dans Excel | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9823e6493e59440407ac847c9c8233983e0ca236
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="analyze-in-excel"></a>Analyser dans Excel
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  La fonctionnalité analyser dans Excel, dans SSDT, fournit aux auteurs de modèle tabulaire permet d’analyser rapidement les projets de modèle au cours du développement. La fonctionnalité Analyser dans Excel ouvre l'application Microsoft Excel, crée une connexion de source de données à la base de données model de l'espace de travail, puis ajoute automatiquement un tableau croisé dynamique à la feuille de calcul. Les objets de base de données de l'espace de travail (tables, colonnes et mesures) sont inclus en tant que champs dans la liste de champs du tableau croisé dynamique. Les objets et les données peuvent ensuite être affichés dans le contexte du rôle ou de l'utilisateur effectif et de la perspective.  
  
 Cet article suppose que vous êtes déjà familiarisé avec Microsoft Excel, des tableaux croisés dynamiques et graphiques croisés dynamiques. Pour en savoir plus sur l'utilisation d'Excel, consultez l'aide relative à cette application.  
  
##  <a name="bkmk_benefits"></a> Avantages  
 La fonctionnalité Analyser dans Excel permet aux créateurs de modèles de tester l'efficacité d'un projet de modèle à l'aide de l'application courante d'analyse de données, Microsoft Excel. Pour pouvoir utiliser la fonctionnalité analyser dans Excel, vous devez disposer de Microsoft Office 2003 ou version ultérieure sur le même ordinateur en tant que SSDT.  
  
 La fonctionnalité Analyser dans Excel ouvre Excel et crée un nouveau classeur Excel (.xls). Une connexion de données du classeur à la base de données model de l'espace de travail est créée. Un tableau croisé dynamique vide est ajouté à la feuille de calcul et des métadonnées d'objet de modèle alimentent la liste de champs de tableau croisé dynamique. Vous pouvez ajouter des données visibles et des segments au tableau croisé dynamique.  
  
 Lors de l'utilisation de la fonctionnalité Analyser dans Excel, par défaut, le compte d'utilisateur actuellement connecté correspond à l'utilisateur effectif. Ce compte est généralement un administrateur sans restrictions d'affichage sur les objets de modèle ou les données. Vous pouvez toutefois spécifier un rôle ou un nom d'utilisateur effectif différent. Cela vous permet d'explorer un projet de modèle dans Excel dans le contexte d'un utilisateur ou d'un rôle spécifique. La spécification de l'utilisateur effectif inclut les options suivantes :  
  
 **Utilisateur Windows actuel**  
 Utilise le compte d'utilisateur avec lequel vous êtes connecté.  
  
 **Autre utilisateur Windows**  
 Utilise un nom d'utilisateur Windows spécifié au lieu de l'utilisateur actuellement connecté. L'utilisation d'un autre utilisateur Windows ne requiert pas de mot de passe. Les objets et les données ne peuvent être affichés dans Excel que dans le contexte du nom d'utilisateur effectif. Aucune modification aux objets de modèle ou aux données ne peut être effectuée dans Excel.  
  
 **Rôle**  
 Un rôle est utilisé pour définir les autorisations utilisateur sur les métadonnées et les données d'objets. Les rôles sont généralement définis pour un utilisateur ou un groupe d'utilisateurs Windows particulier. Certains rôles peuvent inclure des filtres supplémentaires au niveau de la ligne définis dans une formule DAX. Lors de l'utilisation de la fonctionnalité Analyser dans Excel, vous pouvez éventuellement sélectionner un rôle à utiliser. Les métadonnées et les vues de données d'objet sont limitées par l'autorisation et les filtres définis pour le rôle. Pour plus d’informations, consultez [créer et gérer les rôles](../../analysis-services/tabular-models/create-and-manage-roles-ssas-tabular.md).  
  
 En plus du rôle ou de l'utilisateur effectif, vous pouvez spécifier une perspective. Les perspectives permettent aux créateurs de modèles de définir des vues particulières de scénario d'entreprise des objets de modèle et des données. Par défaut, aucune perspective n'est utilisée. Pour utiliser une perspective avec la fonctionnalité analyser dans Excel, les perspectives doivent déjà être définies à l’aide de la boîte de dialogue Perspectives dans SSDT. Si une perspective est spécifiée, la liste de champs de tableau croisé dynamique contient uniquement les objets sélectionnés dans la perspective. Pour plus d’informations, consultez [créer et gérer des Perspectives](../../analysis-services/tabular-models/create-and-manage-perspectives-ssas-tabular.md).  
  
##  <a name="bkmk_rt"></a> Related tasks  
  
|**Rubrique**|**Description**|  
|---------------|---------------------|  
|[Analyser un modèle tabulaire dans Excel](../../analysis-services/tabular-models/analyze-a-tabular-model-in-excel-ssas-tabular.md)|Cet article décrit comment utiliser la fonctionnalité analyser dans Excel dans le Concepteur de modèle pour ouvrir Excel, créer une connexion de source de données à la base de données de modèle espace de travail et ajouter un tableau croisé dynamique à la feuille de calcul.|  
  
## <a name="see-also"></a>Voir aussi  
 [Analyser un modèle tabulaire dans Excel](../../analysis-services/tabular-models/analyze-a-tabular-model-in-excel-ssas-tabular.md)   
 [Rôles](../../analysis-services/tabular-models/roles-ssas-tabular.md)   
 [Perspectives](../../analysis-services/tabular-models/perspectives-ssas-tabular.md)  
  
  
