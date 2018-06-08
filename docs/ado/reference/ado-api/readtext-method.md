---
title: Méthode ReadText | Documents Microsoft
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
- _Stream::raw_ReadText
- _Stream::ReadText
helpviewer_keywords:
- ReadText method [ADO]
ms.assetid: be5a409e-cf87-4859-9ea5-713401755a77
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 26221c32339aab70311a6ca9254bb5d514724070
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="readtext-method"></a>ReadText, méthode
Lit un nombre spécifié de caractères à partir d’un fichier texte [flux](../../../ado/reference/ado-api/stream-object-ado.md) objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
String = Stream.ReadText ( NumChars)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *NumChars*  
 Ce paramètre est facultatif. A **Long** valeur qui spécifie le nombre de caractères à lire à partir du fichier, ou un [StreamReadEnum](../../../ado/reference/ado-api/streamreadenum.md) valeur. La valeur par défaut est **adReadAll**.  
  
## <a name="return-value"></a>Valeur retournée  
 Le **ReadText** méthode lit un nombre spécifié de caractères, une ligne entière ou l’intégralité du flux d’un **flux** de l’objet et retourne la chaîne résultante.  
  
## <a name="remarks"></a>Notes  
 Si *NUMCHARS* est supérieur au nombre de caractères présents dans le flux, seuls les caractères restants sont retournés. La chaîne de lecture n’est pas remplie pour correspondre à la longueur spécifiée par *NUMCHARS*. S’il n’y a pas de caractères pour lire, une valeur de type variant null est retournée. **ReadText** ne peut pas être utilisé pour lire vers l’arrière.  
  
> [!NOTE]
>  Le **ReadText** méthode est utilisée avec des flux de texte ([Type](../../../ado/reference/ado-api/type-property-ado-stream.md) est **adTypeText**). Pour les flux binaires (**Type** est **adTypeBinary**), utilisez [en lecture](../../../ado/reference/ado-api/read-method.md).  
  
 Les requêtes qui résultent d’une grande quantité de données XML renvoyées par le biais du **ReadText** méthode de l’objet de flux de données objet ADO (ActiveX) peut prendre beaucoup de temps d’exécution ; si cette opération est effectuée dans un composant COM + qui est appelé à partir d’un Page ASP, de la session peut expirer. ADO convertit les données d’objet de flux de données à partir de l’encodage UTF-8 au format Unicode ; la réallocation de mémoire fréquent impliquée dans la conversion d’une grande quantité de données à la fois est très longue. Pour résoudre, effectuer des appels répétés à la **ReadText** méthode de ADO command, objet et spécifiez un plus petit nombre de caractères. Les tests ont montré qu’une valeur équivalente à 128K (131 072) est optimale. Temps de réponse réduit car cette valeur est réduite. Pour plus d’informations, consultez l’article de la Base de connaissances 280067, « PRB : la récupération de Documents XML très volumineux à partir de SQL Server 2000 en utilisant la méthode ReadText d’objet de flux de données ADO peut être lente », dans la Base de connaissances Microsoft à http://support.microsoft.com.  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Read, méthode](../../../ado/reference/ado-api/read-method.md)
