---
title: Utilisation des ensembles de lignes de schéma dans ADOMD.NET | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: bdaef1315d1a16e6e3318652bacb105c6af0bc20
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="retrieving-metadata---working-with-schema-rowsets"></a>La récupération des métadonnées - utilisation des ensembles de lignes de schéma
  Lorsque vous avez besoin de plus de métadonnées que celles disponibles dans le modèle objet ADOMD.NET, ADOMD.NET offre la possibilité de récupérer la gamme complète d'ensembles de lignes de schéma XMLA (XML for Analysis), OLE DB, OLE DB pour OLAP et OLE DB pour l'exploration de données :  
  
 **Métadonnées XMLA**  
 Les ensembles de lignes de schéma XMLA offrent une méthode de récupération d'informations de bas niveau sur le serveur. Parmi les informations disponibles figurent entre autres les sources de données disponibles sur le serveur, les mots clés réservés par le fournisseur et les littéraux pris en charge par le fournisseur. Vous pouvez même utiliser un ensemble de lignes de schéma XMLA pour découvrir tous les ensembles de lignes de schéma pris en charge par le fournisseur.  
  
 Pour plus d’informations : [XML for Analysis ensembles de lignes de schéma](../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)  
  
 **Métadonnées OLE DB**  
 Les ensembles de lignes de schéma OLE DB fournissent une méthode standard de récupération d'informations auprès de divers fournisseurs.  
  
 Pour plus d’informations : [OLE DB Schema Rowsets](../../analysis-services/schema-rowsets/ole-db/ole-db-schema-rowsets.md)  
  
 **Métadonnées OLAP**  
 Parmi les informations de schéma fournies pour une source de données analytiques figurent notamment les bases de données ou les catalogues disponibles auprès de la source de données analytiques, des cubes et modèles d'exploration de données d'une base de données et des rôles qui existent pour les cubes au niveau de la source de données.  
  
 Pour plus d’informations : [OLE DB pour OLAP Schema Rowsets](../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)  
  
 **Métadonnées d’exploration de données**  
 En plus des métadonnées OLAP, les métadonnées d'exploration de données peuvent être récupérées par le biais d'ensembles de lignes de schéma. Les ensembles de lignes disponibles présentent des informations sur les modèles d'exploration de données disponibles dans la base de données, les algorithmes d'exploration de données disponibles, les paramètres requis par les algorithmes, les structures d'exploration de données, entre autres.  
  
 Pour plus d’informations : [ensembles de lignes de schéma d’exploration de données](../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md)  
  
 Pour chacun de ces différents ensembles de lignes de schéma, les métadonnées sont récupérées à partir de l'ensemble de lignes en passant un GUID ou un nom XMLA via la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.GetSchemaDataSet%2A> de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection>.  
  
## <a name="retrieving-metadata-by-passing-guids"></a>Récupération de métadonnées en passant des GUID  
 La classe <xref:Microsoft.AnalysisServices.AdomdClient.AdomdSchemaGuid> contient une liste de champs qui représentent les ensembles de lignes de schéma les plus communément pris en charge par les fournisseurs et les sources de données analytiques. Pour récupérer les métadonnées générales et spécifiques au fournisseur auprès d'un fournisseur ou d'une source de données analytiques, vous devez utiliser les GUID contenus dans l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdSchemaGuid> avec l'une des méthodes suivantes :  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.GetSchemaDataSet%2A>  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.GetSchemaDataSet%2A>  
  
> [!NOTE]  
>  Le fournisseur de données ADOMD.NET expose les informations de schéma via les fonctionnalités mises à disposition par votre fournisseur et votre source de données analytiques spécifiques. Chaque fournisseur et source de données peut fournir des métadonnées différentes.  
  
## <a name="retrieving-metadata-by-passing-xmla-names"></a>Récupération de métadonnées en passant des noms XMLA  
 Les méthodes suivantes prennent pour arguments le nom de schéma XMLA qui identifie les informations de schéma à retourner, ainsi qu'un tableau de restrictions sur ces colonnes retournées :  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.GetSchemaDataSet%2A>  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.GetSchemaDataSet%2A>  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.GetSchemaDataSet%2A>  
  
-   <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.GetSchemaDataSet%2A>  
  
 Chacune de ces méthodes retourne une instance d'un objet **DataSet** rempli des informations de schéma. L'objet **DataSet** provient de l'espace de noms **System.Data** de la bibliothèque de classes Microsoft .NET Framework.  
  
## <a name="example"></a>Exemple  
 Dans l’exemple suivant, la fonction GetActions prend une connexion, le nom du cube, une coordonnée et un type de coordonnée, récupère un [de lignes MDSCHEMA_ACTIONS](../../analysis-services/schema-rowsets/ole-db-olap/mdschema-actions-rowset.md)et retourne les actions disponibles sur la coordonnée sélectionnée.  
  
 [!code-cs[Adomd.NetClient#GetActions](../../analysis-services/multidimensional-models-adomd-net-client/codesnippet/csharp/retrieving-metadata-work_0_1.cs)]  
  
## <a name="see-also"></a>Voir aussi  
 [Récupération de métadonnées à partir d’une source de données analytiques](../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-from-an-analytical-data-source.md)  
  
  
