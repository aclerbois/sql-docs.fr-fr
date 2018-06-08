---
title: Objet de paramètre | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Parameter
helpviewer_keywords:
- Parameter object [ADO]
ms.assetid: e010e794-7f0f-4026-8b5b-37328e437d63
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 59474a000d3def675caf66085380c6a3791805c3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="parameter-object"></a>Objet Parameter
Représente un paramètre ou un argument associé à un [commande](../../../ado/reference/ado-api/command-object-ado.md) objet basé sur une procédure stockée ou une requête paramétrable.  
  
## <a name="remarks"></a>Notes  
 De nombreux fournisseurs prennent en charge les commandes paramétrées. Il s’agit de commandes dans lequel l’action souhaitée est définie une seule fois, mais les variables (ou paramètres) sont utilisées pour modifier certains détails de la commande. Par exemple, une instruction SQL SELECT peut utiliser un paramètre pour définir les critères de correspondance d’une clause WHERE et l’autre pour définir le nom de colonne pour une clause de tri.  
  
 **Paramètre** objets représentent des paramètres associés aux requêtes paramétrables ou les arguments d’entrée/sortie et les valeurs de retour de procédures stockées. Selon les fonctionnalités du fournisseur, certaines collections, des méthodes ou propriétés d’un **paramètre** objet n’est peut-être pas disponible.  
  
 Avec les collections, les méthodes et les propriétés d’un **paramètre** de l’objet, vous pouvez procédez comme suit :  
  
-   Définir ou retourner le nom d’un paramètre avec le [nom](../../../ado/reference/ado-api/name-property-ado.md) propriété.  
  
-   Définir ou retourner la valeur d’un paramètre avec le [valeur](../../../ado/reference/ado-api/value-property-ado.md) propriété. **Valeur** est la propriété par défaut de la **paramètre** objet.  
  
-   Définir ou retourner les caractéristiques des paramètres avec les [attributs](../../../ado/reference/ado-api/attributes-property-ado.md), [Direction](../../../ado/reference/ado-api/direction-property.md), [précision](../../../ado/reference/ado-api/precision-property-ado.md), [NumericScale](../../../ado/reference/ado-api/numericscale-property-ado.md), [ Taille](../../../ado/reference/ado-api/size-property-ado-parameter.md), et [Type](../../../ado/reference/ado-api/type-property-ado.md) propriétés.  
  
-   Passer des données binaires longues ou de caractères à un paramètre avec le [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) (méthode).  
  
-   Accéder aux attributs spécifiques au fournisseur à l’aide de la [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection.  
  
 Si vous connaissez les noms et les propriétés des paramètres associés avec la procédure stockée ou une requête paramétrable que vous souhaitez appeler, vous pouvez utiliser la [CreateParameter](../../../ado/reference/ado-api/createparameter-method-ado.md) méthode pour créer **paramètre** objets avec les paramètres de propriété appropriés et utilisez la [Append](../../../ado/reference/ado-api/append-method-ado.md) méthode pour les ajouter à la [paramètres](../../../ado/reference/ado-api/parameters-collection-ado.md) collection. Cela vous permet de définir et retourner des valeurs de paramètre sans devoir appeler le [Actualiser](../../../ado/reference/ado-api/refresh-method-ado.md) méthode sur le **paramètres** collection pour récupérer les informations de paramètre à partir du fournisseur, un potentiellement opération gourmande en ressources.  
  
 Le **paramètre** objet n’est pas sécurisé pour le script.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements](../../../ado/reference/ado-api/parameter-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Objet de commande (ADO)](../../../ado/reference/ado-api/command-object-ado.md)   
 [CreateParameter, méthode (ADO)](../../../ado/reference/ado-api/createparameter-method-ado.md)   
 [Collection de paramètres (ADO)](../../../ado/reference/ado-api/parameters-collection-ado.md)   
 [Properties, collection (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)
