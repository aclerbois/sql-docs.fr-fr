---
title: Ensemble de lignes DISCOVER_DATASOURCES | Documents Microsoft
ms.date: 05/03/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: schema-rowsets
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9e316e1e8927acb5dcb99046a8d53034126052ce
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discoverdatasources-rowset"></a>Ensemble de lignes DISCOVER_DATASOURCES
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Retourne la liste des sources de données du fournisseur XML for Analysis (XMLA) qui sont disponibles sur le serveur ou le service Web. Les sources de données publiées sont retournées à partir d'une URL du serveur Web d'application. Le client peut se connecter à l'une des sources de données indiquées dans cette liste.  
  
 Si vous appelez le [Discover](../../../analysis-services/xmla/xml-elements-methods-discover.md) méthode avec la **DISCOVER_DATASOURCES** valeur d’énumération dans le [RequestType](../../../analysis-services/xmla/xml-elements-properties/requesttype-element-xmla.md) élément, le **Discover** méthode retourne la **DISCOVER_DATASOURCES** ensemble de lignes.  
  
 **S'applique à :** modèles tabulaires, modèles multidimensionnels  
  
## <a name="rowset-columns"></a>Colonnes de l'ensemble de lignes  
 Le client sélectionne une source de données en définissant le **DataSourceInfo** propriété dans le [propriétés](../../../analysis-services/xmla/xml-elements-properties/properties-element-xmla.md) élément qui est envoyé avec la [commande](../../../analysis-services/xmla/xml-elements-properties/command-element-xmla.md) élément par le [Execute](../../../analysis-services/xmla/xml-elements-methods-execute.md) (méthode). Un client ne doit pas construire le contenu de la propriété **DataSourceInfo** à envoyer au serveur. À la place, le client doit utiliser la méthode **Discover** pour rechercher les sources de données que le fournisseur prend en charge. Le client renvoie ensuite pour la propriété **DataSourceInfo** la valeur qu'il obtient de l'ensemble de lignes **DISCOVER_DATASOURCES** .  
  
 L'ensemble de lignes **DISCOVER_DATASOURCES** contient les colonnes suivantes.  
  
|Nom de colonne|Indicateur de type|Restriction| Description|  
|-----------------|--------------------|-----------------|-----------------|  
|**DataSourceName**|**DBTYPE_WSTR**|Oui|Nom de la source de données, par exemple **Adventure Works**.|  
|**DataSourceDescription**|**DBTYPE_WSTR**||Description de la source de données entrée par le serveur de publication.<br /><br /> Peut retourner **NULL**.|  
|**URL**|**DBTYPE_WSTR**|Oui|Chemin d'accès unique qui indique où appeler les méthodes XML for Analysis (XMLA) pour cette source de données.<br /><br /> Peut retourner **NULL**.|  
|**DataSourceInfo**|**DBTYPE_WSTR**||Chaîne qui contient les informations supplémentaires requises pour établir la connexion à la source de données.<br /><br /> Peut retourner **NULL**.|  
|**ProviderName**|**DBTYPE_WSTR**|Oui|Nom du fournisseur de la source de données.<br /><br /> Exemple : `"MSOLAP"`<br /><br /> Peut retourner **NULL**.|  
|**ProviderType**|**DBTYPE_WSTR**|Oui|Types de données pris en charge par le fournisseur. Ce tableau peut inclure un ou plusieurs des types suivants :<br /><br /> **MDP**: fournisseur de données multidimensionnelles.<br /><br /> **TDP**: fournisseur de données sous forme de tableau.<br /><br /> **DMP**: fournisseur d'exploration de données (implémente la spécification OLE DB pour l'exploration de données).|  
|**AuthenticationMode**|**DBTYPE_WSTR**|Oui|Spécification du type de mode de sécurité que la source de données utilise. Il peut s'agir de l'une des valeurs suivantes :<br /><br /> **Unauthenticated**: aucun ID d'utilisateur ou mot de passe ne doit être envoyé.<br /><br /> **Authenticated**: l'ID utilisateur et le mot de passe doivent être inclus dans les informations requises pour établir la connexion à la source de données.<br /><br /> **Intégré**: la source de données utilise la sécurité sous-jacente pour déterminer l’autorisation, telles que la sécurité intégrée fournie par [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Internet Information Services (IIS).|  
  
 Cet ensemble de lignes de schéma n'est pas trié.  
  
> [!IMPORTANT]  
>  L'ensemble de lignes **DISCOVER_DATASOURCES** ne peut pas être interrogé à l'aide de requêtes DMV et de la syntaxe de la commande SELECT. Toutefois, le **DISCOVER_DATASOURCES** ensemble de lignes peut être interrogée à l’aide de <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.GetSchemaDataSet%2A>.  
  
## <a name="using-adomdnet-to-return-the-rowset"></a>Utilisation d'ADOMD.NET pour retourner l'ensemble de lignes  
 Lorsque vous utilisez ADOMD.NET et l'ensemble de lignes de schéma pour récupérer des métadonnées, vous pouvez utiliser un GUID ou une chaîne pour référencer un objet d'ensemble de lignes de schéma dans la méthode GetSchemaDataSet. Pour plus d'informations, consultez [Working with Schema Rowsets in ADOMD.NET](../../../analysis-services/multidimensional-models-adomd-net-client/retrieving-metadata-working-with-schema-rowsets.md).  
  
 Le tableau suivant fournit le GUID et les valeurs de chaîne qui identifient cet ensemble de lignes.  
  
|Argument|Valeur|  
|--------------|-----------|  
|GUID|06c03d41-f66d-49f3-b1b8-987f7af4cf18|  
|ADOMDNAME|DataSources|  
  
## <a name="see-also"></a>Voir aussi  
 [XML for Analysis ensembles de lignes de schéma](../../../analysis-services/schema-rowsets/xml/xml-for-analysis-schema-rowsets.md)  
  
  
