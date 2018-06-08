---
title: Spécifications de capacité maximale (Analysis Services) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 187a3a76f63853c63b9fe92dbd7ccfd4a0cfcdb5
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="maximum-capacity-specifications-analysis-services"></a>Spécifications de capacité maximale (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Les tableaux suivants spécifient les tailles maximales et les nombres des différents objets définis dans les composants [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] selon des modes de déploiement de serveur différents.  
  
 Cette rubrique contient les sections suivantes :  
  
 [Multidimensionnel et exploration de données (DeploymentMode = 0)](#bkmk_OLAP)  
  
 [SharePoint (DeploymentMode = 1)](#bkmk_sharepoint)  
  
 [Tabulaire (DeploymentMode = 2)](#bkmk_vertipaq)  
  
##  <a name="bkmk_OLAP"></a> Multidimensionnel et exploration de données (DeploymentMode = 0)  
 Le mode de stockage MOLAP, qui stocke des données et des métadonnées, a des limites physiques supplémentaires concernant les tailles de fichier. Les fichiers du magasin de chaînes ont par défaut une taille maximale de 4 Go. Si vous avez besoin de fichiers de plus grande taille pour les magasins de chaînes, vous pouvez spécifier une architecture de stockage de chaînes différente. Pour plus d’informations, consultez [configurer le stockage des chaînes pour les Partitions et Dimensions](../../../analysis-services/multidimensional-models/configure-string-storage-for-dimensions-and-partitions.md).  
  
|Objet|Tailles maximales/nombres maximaux|  
|------------|----------------------------|  
|Bases de données dans une instance|2^31-1 = 2,147,483,647|  
|Dimensions dans une base de données|2^31-1 = 2,147,483,647|  
|Attributs dans une dimension|2^31-1 = 2,147,483,647|  
|Membres dans un attribut de dimension|2^31-1 = 2,147,483,647|  
|Hiérarchies définies par l'utilisateur dans une dimension|2^31-1 = 2,147,483,647|  
|Niveaux dans une hiérarchie définie par l'utilisateur|2^31-1 = 2,147,483,647|  
|Cubes dans une base de données|2^31-1 = 2,147,483,647|  
|Groupes de mesures dans un cube|2^31-1 = 2,147,483,647|  
|Mesures dans un groupe de mesures|2^31-1 = 2,147,483,647|  
|Calculs dans un cube|2^31-1 = 2,147,483,647|  
|KPI dans un cube|2^31-1 = 2,147,483,647|  
|Actions dans un cube|2^31-1 = 2,147,483,647|  
|Partitions dans un cube|2^31-1 = 2,147,483,647|  
|Traductions dans un cube|2^31-1 = 2,147,483,647|  
|Agrégations dans une partition|2^31-1 = 2,147,483,647|  
|Cellules renvoyées par une requête|2^31-1 = 2,147,483,647|  
|Taille d'enregistrement de la requête source|64 Ko|  
|Longueur des noms d’objet|100 caractères|  
|Nombre maximal d'états distincts dans une colonne d'attribut de modèle d'exploration de données|2^31-1 = 2,147,483,647|  
|Nombre maximal d'attributs pris en compte (sélection de fonctionnalité)|2^31-1 = 2,147,483,647|  
  
 Pour plus d’informations sur les instructions d’affectation de noms d’objet, consultez [objets ASSL et caractéristiques des objets](../../../analysis-services/multidimensional-models/scripting-language-assl/assl-objects-and-object-characteristics.md).  
  
 Pour plus d’informations sur les limites de source de données pour le traitement analytique en ligne (OLAP) et d’exploration de données, consultez [prise en charge des Sources de données &#40;SSAS - multidimensionnel&#41;](../../../analysis-services/multidimensional-models/supported-data-sources-ssas-multidimensional.md), [prise en charge des Sources de données &#40;SSAS - multidimensionnel&#41;](../../../analysis-services/multidimensional-models/supported-data-sources-ssas-multidimensional.md), et [objets ASSL et caractéristiques des objets](../../../analysis-services/multidimensional-models/scripting-language-assl/assl-objects-and-object-characteristics.md).  
  
##  <a name="bkmk_sharepoint"></a> SharePoint (DeploymentMode = 1)  
  
|Objet|Tailles maximales/nombres maximaux|  
|------------|----------------------------|  
|Bases de données dans une instance|2^31-1 = 2,147,483,647|  
|Tables dans une base de données|2^31-1 = 2,147,483,647|  
|Colonnes dans une table|2^31-1 = 2,147,483,647<br /><br /> **Avertissement :** nombre Total de colonnes dans une table dépend du nombre total de mesures et colonnes calculées associées à la même table.<br /><br /> Le nombre maximal de « Colonnes + Mesures + Colonnes calculées » pour une table est 2^31-1 = 2 147 483 647|  
|Lignes dans une table|Illimité<br /><br /> **Avertissement :** avec la restriction qu’aucune colonne ne peut contenir plus de 1 999 999 997 valeurs distinctes.|  
|Hiérarchies dans une table|2^31-1 = 2,147,483,647|  
|Niveaux dans une hiérarchie|2^31-1 = 2,147,483,647|  
|Relations|2^31-1 = 2,147,483,647|  
|Colonnes clés dans une table|2^31-1 = 2,147,483,647|  
|Mesures dans une table|2^31-1 = 2,147,483,647<br /><br /> **Avertissement :** nombre Total de mesures dans une table dépend du nombre total de colonnes et les colonnes calculées associées à la même table.<br /><br /> Le nombre maximal de « Colonnes + Mesures + Colonnes calculées » pour une table est 2^31-1 = 2 147 483 647|  
|Colonnes calculées dans une table|2^31-1 = 2,147,483,647<br /><br /> **Avertissement :** nombre Total de colonnes calculées dans une table dépend du nombre total de colonnes et mesures associés à la même table.<br /><br /> Le nombre maximal de « Colonnes + Mesures + Colonnes calculées » pour une table est 2^31-1 = 2 147 483 647|  
|Cellules renvoyées par une requête|2^31-1 = 2,147,483,647|  
|Taille d'enregistrement de la requête source|64 Ko|  
|Longueur des noms d’objet|100 caractères|  
  
##  <a name="bkmk_vertipaq"></a> Tabulaire (DeploymentMode = 2)  
Limite théorique sont les suivantes. Performances seront diminuée sur les nombres inférieurs.   

|Objet|Tailles maximales/nombres maximaux|  
|------------|----------------------------|  
|Bases de données dans une instance|16,000|  
|Nombre total de tables et colonnes dans une base de données|16,000|  
|Lignes dans une table|Illimité<br /><br /> **Avertissement :** avec la restriction qu’aucune colonne dans la table ne peut avoir plus de 1 999 999 997 valeurs distinctes.|  
|Hiérarchies dans une table|15,999|  
|Niveaux dans une hiérarchie|15,999|  
|Relations|8,000|  
|Colonnes de clé de table toutes les|15,999|  
|Mesures dans des tables|2^31-1 = 2,147,483,647|  
|Cellules renvoyées par une requête|2^31-1 = 2,147,483,647|  
|Taille d'enregistrement de la requête source|64 Ko|  
|Longueur des noms d’objet|512 caractères|  
  
## <a name="see-also"></a>Voir aussi  
 [Déterminer le mode serveur d'une instance Analysis Services](../../../analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance.md)   
 [Propriétés générales](../../../analysis-services/server-properties/general-properties.md)  
  
  
