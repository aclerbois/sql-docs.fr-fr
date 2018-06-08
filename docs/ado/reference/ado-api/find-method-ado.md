---
title: Find (méthode) (ADO) | Documents Microsoft
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
- Recordset15::raw_Find
- Recordset15::Find
helpviewer_keywords:
- Find method [ADO]
ms.assetid: 55c9810a-d8ca-46c2-a9dc-80e7ee7aa188
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 953398f5ed01cc3e0f7c0da1fee769d5e64209af
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="find-method-ado"></a>Find (méthode) (ADO)
Recherche un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) pour la ligne qui répond aux critères spécifiés. Si vous le souhaitez, la direction de recherche, de la ligne de début et du décalage à partir de la ligne de début peut être spécifiée. Si les critères sont satisfaits, la position de ligne actuelle est définie sur l’enregistrement trouvé. dans le cas contraire, la position est définie à la fin (ou démarrer) de la **Recordset**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Find (Criteria, SkipRows, SearchDirection, Start)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Critères*  
 A **chaîne** valeur qui contient une instruction en spécifiant le nom de colonne, un opérateur de comparaison et une valeur à utiliser dans la recherche.  
  
 *SkipRows*  
 Facultatif *.* A **Long** valeur, dont la valeur par défaut est zéro, ce qui spécifie le décalage de ligne de la ligne actuelle ou *Démarrer* signet pour commencer la recherche. Par défaut, la recherche commence sur la ligne actuelle.  
  
 *SearchDirection*  
 Facultatif *.* A [SearchDirectionEnum](../../../ado/reference/ado-api/searchdirectionenum.md) valeur qui spécifie si la recherche doit commencer sur la ligne actuelle ou de la ligne suivante dans la direction de la recherche. Une recherche s’arrête à la fin de la **Recordset** si la valeur est **adSearchForward**. Une recherche s’arrête au début de la **Recordset** si la valeur est **adSearchBackward**.  
  
 *Démarrer*  
 Ce paramètre est facultatif. A **Variant** signet qui indique la position de départ pour la recherche.  
  
## <a name="remarks"></a>Notes  
 Seul un nom de colonne unique peut être spécifié dans *critères*. Cette méthode ne prend pas en charge les recherches sur plusieurs colonnes.  
  
 L’opérateur de comparaison dans *critères* peut être «**>**« (supérieur à), »**\<**» (inférieur à), « = » (égal), « > = » (supérieur ou égal à), « < = » (inférieur ou égal à), « <> » (différent de) ou « like » (correspondance).  
  
 La valeur de *critères* peut être une chaîne, un nombre à virgule flottante ou une date. Les valeurs de chaîne sont délimitées par des guillemets simples ou « # » (dièse) (par exemple, « état = 'WA' » ou « état = #WA # »). Les valeurs de date sont séparés par des signes « # » (dièse) (par exemple, « start_date > #7/22/97 # »). Ces valeurs peuvent contenir des heures, minutes et secondes pour indiquer les horodatages, mais ne doivent pas contenir de millisecondes ou des erreurs se produisent.  
  
 Si l’opérateur de comparaison est « comme », la valeur de chaîne peut contenir un astérisque (*) pour rechercher une ou plusieurs occurrences d’un caractère ou d’une sous-chaîne. Par exemple, « state like'm\*' » correspond à Maine et du Massachusetts. Vous pouvez également utiliser des astérisques de début et de fin pour rechercher une sous-chaîne contenue dans les valeurs. Par exemple, « état comme '\*comme\*' » correspond à Alaska, Arkansas et Massachusetts.  
  
 Astérisques peuvent être utilisés uniquement à la fin d’une chaîne de critères ou au début et à la fin d’une chaîne de critères, comme indiqué ci-dessus. Vous ne pouvez pas utiliser l’astérisque comme caractère générique de début ('* str'), ou comme un s incorporé\*r »). Cela entraîne une erreur.  
  
> [!NOTE]
>  Une erreur se produit si la position de ligne actuelle n’est pas définie avant d’appeler **trouver**. Toute méthode qui définit la position de la ligne, tel que [MoveFirst](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md), doit être appelée avant d’appeler **trouver**.  
  
> [!NOTE]
>  Si vous appelez le **trouver** méthode sur un jeu d’enregistrements et la position actuelle dans le jeu d’enregistrements est au dernier enregistrement ou à la fin du fichier (EOF), vous ne trouverez pas quoi que ce soit. Vous avez besoin d’appeler le **MoveFirst** pour définir la position/le curseur actuel au début de l’objet recordset.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple (VB) de la méthode Find](../../../ado/reference/ado-api/find-method-example-vb.md)   
 [Propriété index](../../../ado/reference/ado-api/index-property.md)   
 [Optimiser la propriété dynamique (ADO)](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md)   
 [Seek, méthode](../../../ado/reference/ado-api/seek-method.md)
