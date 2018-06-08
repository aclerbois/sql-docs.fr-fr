---
title: Analysis Services des ensembles de lignes de schéma | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ea3d947e8cfbea4b183f4416ebabf6bb0ebf7b61
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="analysis-services-schema-rowsets"></a>Ensembles de lignes de schéma Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les ensembles de lignes de schéma sont des tables prédéfinies qui contiennent des informations sur les objets et l'état du serveur Analysis Services, notamment les schémas de base de données, les sessions actives, les connexions, les commandes et les travaux qui s'exécutent sur le serveur. Vous pouvez interroger les tables d'ensembles de lignes de schéma dans une fenêtre de script XML/A dans SQL Server Management Studio, exécuter une requête DMV sur un ensemble de lignes de schéma, ou créer une application personnalisée qui incorpore les informations d'ensemble de lignes de schéma (par exemple, une application de création de rapport qui récupère la liste des dimensions disponibles pouvant être utilisées pour créer un rapport).  
  
> [!NOTE]  
>  Si vous utilisez des ensembles de lignes de schéma XML/a un script, les informations qui sont retournées dans le *résultat* paramètre de la [Discover](../../analysis-services/xmla/xml-elements-methods-discover.md) (méthode) est structurée selon les dispositions de colonne d’ensemble de lignes décrites dans cette section. Le fournisseur XMLA ([!INCLUDE[msCoName](../../includes/msconame-md.md)] XML for Analysis) prend en charge les ensembles de lignes requis par la spécification XML for Analysis. Le fournisseur XMLA prend également en charge certains ensembles de lignes de schéma standard pour les fournisseurs de sources de données OLE DB, OLE DB pour OLAP et OLE DB pour l'exploration de données. Les ensembles de lignes pris en charge sont décrits dans les rubriques suivantes :  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[XML for Analysis ensembles de lignes de schéma](../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)|Décrit les ensembles de lignes XMLA pris en charge par le fournisseur XMLA.|  
|[Ensembles de lignes de schéma OLE DB](../../analysis-services/schema-rowsets/ole-db/ole-db-schema-rowsets.md)|Décrit les ensembles de lignes de schéma OLE DB pris en charge par le fournisseur XMLA.|  
|[OLE DB pour OLAP Schema Rowsets](../../analysis-services/schema-rowsets/ole-db-olap/ole-db-for-olap-schema-rowsets.md)|Décrit les ensembles de lignes de schéma OLE DB pour OLAP pris en charge par le fournisseur XMLA.|  
|[Ensembles de lignes de schéma de données d’exploration de données](../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md)|Décrit les ensembles de lignes de schéma pour l'exploration de données pris en charge par le fournisseur XMLA.|  
  
## <a name="see-also"></a>Voir aussi  
 [Accès aux données de modèle multidimensionnel & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../analysis-services/multidimensional-models/mdx/multidimensional-model-data-access-analysis-services-multidimensional-data.md)   
 [Utiliser dynamique vues de gestion & #40 ; vues de gestion dynamique & #41 ; pour surveiller Analysis Services](../../analysis-services/instances/use-dynamic-management-views-dmvs-to-monitor-analysis-services.md)  
  
  
