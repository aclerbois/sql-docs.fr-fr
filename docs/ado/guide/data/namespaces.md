---
title: Espaces de noms | Documents Microsoft
ms.prod: sql-non-specified
ms.prod_service: drivers
ms.service: 
ms.component: ado
ms.technology:
- drivers
ms.custom: 
ms.date: 01/19/2017
ms.reviewer: 
ms.suite: sql
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- namespaces in ADO
ms.assetid: efff5569-db52-451d-a039-2e74870534da
caps.latest.revision: 
author: MightyPen
ms.author: genemi
manager: craigg
ms.workload: Inactive
ms.openlocfilehash: c34bb680f7a066eeb694cf62fba39cabb0d4cbea
ms.sourcegitcommit: acab4bcab1385d645fafe2925130f102e114f122
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
---
# <a name="namespaces"></a>Espaces de noms
Le format XML de persistance dans ADO utilise les quatre espaces de noms.  
  
## <a name="remarks"></a>Notes  
 Le format XML de persistance dans ADO utilise les quatre espaces de noms.  
  
|Prefix| Description|  
|------------|-----------------|  
|s|Fait référence à l’espace de noms « XML-Data » contenant les éléments et les attributs qui définissent le schéma de l’ensemble d’enregistrements en cours.|  
|dt|Fait référence à la spécification de définitions de type de données.|  
|rs|Fait référence à l’espace de noms des éléments qui le contient et les attributs spécifiques aux propriétés du jeu d’enregistrements ADO et les attributs.|  
|z|Fait référence au schéma de l’ensemble de lignes en cours.|  
  
 Un client ne doit pas ajouter ses propres balises à ces espaces de noms, tel que défini par la spécification. Par exemple, un client ne doit pas définir un espace de noms « urn : schemas-microsoft-rowset », puis écrivez quelque chose comme « rs : MaPropreBalise ». Pour en savoir plus sur les espaces de noms, consultez le [espaces de noms W3C dans la recommandation XML](http://www.w3.org/TR/REC-xml-names/).  
  
> [!IMPORTANT]
>  L’ID de la balise de schéma doit être « RowsetSchema » et l’espace de noms utilisé pour faire référence au schéma de l’ensemble de lignes en cours doit pointer vers « #RowsetSchema ».  
  
 Notez que le préfixe de l’espace de noms, la partie entre le signe deux-points et le signe égal, est arbitraire.  
  
```  
xmlns:rs="urn:schemas-microsoft-com:rowset"  
```  
  
 L’utilisateur peut définir cette option pour être n’importe quel nom tant que ce nom est utilisé de manière cohérente dans tout le document XML. ADO écrit toujours « s », « rs », « dt » et « z », mais ces préfixes ne sont pas codées en dur dans le composant de chargement.  
  
## <a name="see-also"></a>Voir aussi  
 [Persistance des enregistrements au format XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
