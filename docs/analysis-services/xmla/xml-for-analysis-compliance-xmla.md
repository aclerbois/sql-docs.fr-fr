---
title: Conformité XML for Analysis (XMLA) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b618fdd8cca3faed72afb0276f18cd928a3f31f7
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34576881"
---
# <a name="xml-for-analysis-xmla-compliance"></a>Conformité XML for Analysis (XMLA)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]  

  La spécification XML for Analysis 1.1 décrit une norme ouverte qui prend en charge l'accès aux sources de données qui résident sur Internet. Cette rubrique détaille le niveau de compatibilité avec la spécification XML for Analysis 1.1 prise en charge par Azure Analysis Services et SQL Server Analysis Services.  
  
## <a name="compliant-items"></a>Éléments conformes  
Analysis Services est compatible avec tous les éléments obligatoires répertoriés dans la spécification XML for Analysis 1.1. En outre, Analysis Services implémente l’élément facultatif suivant décrit dans la spécification XML for Analysis 1.1.  
  
|Élément|Spécification|Implémentation|  
|----------|-------------------|--------------------|  
|Prise en charge de la session|Les éléments d'en-tête SOAP répertoriés dans la section relative à la prise en charge de l'état dans XML for Analysis pour la spécification XML for Analysis 1.1.|Tous les éléments d’en-tête SOAP répertoriés sont pris en charge par Analysis Services, comme décrit dans la spécification.|  
  
## <a name="extensions"></a>Extensions  
 Analysis Services permet également d’étendre la spécification XML for Analysis 1.1 pour prendre en charge les fonctionnalités supplémentaires suivantes.  
  
|Élément|Spécification|Implémentation|  
|----------|-------------------|--------------------|  
|Négociation de protocole|Aucune information contenue dans la spécification XML for Analysis 1.1|Élément d’en-tête ProtocolCapabilities ajouté par Analysis Services pour prendre en charge la négociation des fonctionnalités de protocole.|  
|Commandes XML for Analysis (XMLA) prises en charge par la méthode Discover|Les commandes suivantes sont prises en charge par la spécification XML for Analysis 1.1 :<br /><br /> [Élément d’instruction &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/statement-element-xmla.md)|Les commandes suivantes sont prises en charge par Analysis Services :<br /><br /> [Élément ALTER &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/alter-element-xmla.md)<br /><br /> [Élément de sauvegarde &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/backup-element-xmla.md)<br /><br /> [Élément du lot &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/batch-element-xmla.md)<br /><br /> [Élément BeginTransaction &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/begintransaction-element-xmla.md)<br /><br /> [Élément Cancel &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/cancel-element-xmla.md)<br /><br /> [Élément ClearCache &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/clearcache-element-xmla.md)<br /><br /> [Élément CommitTransaction &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/committransaction-element-xmla.md)<br /><br /> [Créer l’élément &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/create-element-xmla.md)<br /><br /> [Supprimer l’élément &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/delete-element-xmla.md)<br /><br /> [Élément DesignAggregations &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/designaggregations-element-xmla.md)<br /><br /> [Élément DROP &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/drop-element-xmla.md)<br /><br /> [Insérer l’élément &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/insert-element-xmla.md)<br /><br /> [Verrouiller l’élément &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/lock-element-xmla.md)<br /><br /> [Élément MergePartitions &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/mergepartitions-element-xmla.md)<br /><br /> [Élément NotifyTableChange &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/notifytablechange-element-xmla.md)<br /><br /> [Traiter l’élément &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/process-element-xmla.md)<br /><br /> [Élément Restore &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/restore-element-xmla.md)<br /><br /> [Élément RollbackTransaction &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/rollbacktransaction-element-xmla.md)<br /><br /> [Élément d’instruction &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/statement-element-xmla.md)<br /><br /> [Élément Subscribe &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/subscribe-element-xmla.md)<br /><br /> [Élément Synchronize &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/synchronize-element-xmla.md)<br /><br /> [Élément Unlock &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/unlock-element-xmla.md)<br /><br /> [Mettre à jour d’élément &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/update-element-xmla.md)<br /><br /> [Élément UpdateCells &#40;XMLA&#41;](../../analysis-services/xmla/xml-elements-commands/updatecells-element-xmla.md)|  
|Erreurs de colonne dans les ensembles de lignes tabulaires|Non répertoriées dans la spécification XML for Analysis 1.1.|Le [erreur](../../analysis-services/xmla/xml-elements-properties/error-element-xmla.md) élément est utilisé par Analysis Services pour signaler des erreurs pour les éléments de colonne contenus dans un [ligne](../../analysis-services/xmla/xml-elements-properties/error-element-xmla.md) élément.|  
  
## <a name="see-also"></a>Voir aussi
 [Référence XML for Analysis &#40;XMLA&#41;](../../analysis-services/xmla/xml-for-analysis-xmla-reference.md)  
  
  
