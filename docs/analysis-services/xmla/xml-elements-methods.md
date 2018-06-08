---
title: Méthodes (XMLA) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 15f930695e13d824e70da748dad20cf574e9bc38
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579141"
---
# <a name="xml-elements---methods"></a>Éléments XML - méthodes
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Le protocole XML for Analysis (XMLA) utilise deux méthodes, **Discover** et **Execute**, pour offrir un moyen standard pour les applications d’accéder aux informations sur une instance d’Analysis Services. Ces méthodes étant appelées à l'aide du protocole SOAP (Simple Object Access Protocol), elles acceptent des entrées et affichent les sorties au format XML. Analysis Services implémente les deux méthodes conformément à la spécification XML for Analysis 1.1.  
  
## <a name="in-this-section"></a>Contenu de cette section  
 Les rubriques suivantes décrivent les méthodes XMLA implémentés par Analysis Services.  
  
|Méthode|Description|  
|------------|-----------------|  
|[Méthode Discover &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-methods-discover.md)|Récupère les informations, telles que la liste des bases de données disponibles ou des détails concernant un objet spécifique, à partir d’une instance d’Analysis Services. Les données récupérées avec le **Discover** méthode varie selon les valeurs des paramètres passés à ce dernier.|  
|[Exécuter la méthode &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-methods-execute.md)|Envoie des commandes XMLA à une instance d’Analysis Services. Ceci inclut des demandes qui impliquent un transfert de données (par exemple, la récupération ou la mise à jour de données sur le serveur).|  
  
## <a name="see-also"></a>Voir aussi
 [Éléments XML &#40;XMLA&#41;](http://msdn.microsoft.com/library/40ab2360-efb6-4ba6-bf23-e84964e51008)   
 [Types de données XML &#40;XMLA&#41;](../../analysis-services/xmla/xml-data-types/xml-data-types-xmla.md)   
 [Éléments XML &#40;XMLA&#41;](http://msdn.microsoft.com/library/40ab2360-efb6-4ba6-bf23-e84964e51008)  
  
  
