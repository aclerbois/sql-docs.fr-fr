---
title: Propriétés de la source OData | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 4fde5bb0-6d78-4ec4-8f0b-67f91c53fe99
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6c1035a694f53c274e47a89b45fc7b62022a7b21
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odata-source-properties"></a>Propriétés de la source OData
Quand vous cliquez avec le bouton droit sur **Source OData** dans le flux de données, puis cliquez sur **Propriétés**, vous voyez les propriétés du composant **Source OData** dans la fenêtre **Propriétés**.  

## <a name="properties"></a>Propriétés 
|Propriété|Description|  
|-|-|  
|CollectionName|Nom de la collection à extraire du service OData. La propriété **CollectionName** est utilisée quand **UseResourcePath** a la valeur false.<br /><br /> Cette propriété utilise des expressions, ce qui vous permet de définir la valeur au moment de l’exécution. Cependant, si les métadonnées de la collection ne correspondent pas aux métadonnées utilisées au moment de la conception, une erreur de validation est générée et l’exécution du flux de données échoue.|  
|DefaultStringLength|Cette valeur spécifie la longueur par défaut des colonnes de chaîne qui n'ont pas de longueur maximale.<br /><br /> **Valeur par défaut :** 4000|  
|Requête|Paramètres de requête OData. Cette propriété utilise des expressions et peut être définie au moment de l’exécution.|  
|ResourcePath|Utilisez cette propriété lorsque vous devez spécifier un chemin d'accès de ressources complet, plutôt que sélectionner uniquement un nom de collection. Cette propriété est utilisée lorsque **UseResourcePath** a la valeur True.|  
|UseResourcePath|Lorsque la valeur est définie à True, la valeur **ResourcePath** est ajoutée à l'URL de base pour déterminer l'emplacement du flux OData. Lorsque la valeur est False, la valeur **CollectionName** est utilisée.<br /><br /> **Valeur par défaut :** False|  
  
## <a name="see-also"></a> Voir aussi
[Source OData](odata-source.md)
