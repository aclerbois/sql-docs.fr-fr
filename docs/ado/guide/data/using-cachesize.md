---
title: À l’aide de CacheSize | Documents Microsoft
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
helpviewer_keywords:
- locks [ADO], CacheSize property
- CacheSize property [ADO]
ms.assetid: ca1c3422-b6a4-4ba6-af55-54f975b698b1
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 043634736f9ad5f26ced4707349405793ff6e556
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-cachesize"></a>Utilisation de CacheSize
Utilisez le **CacheSize** propriété pour contrôler le nombre d’enregistrements à récupérer simultanément dans la mémoire locale à partir du fournisseur. Par exemple, si le **CacheSize** est 10, après la première ouverture du **Recordset** de l’objet, le fournisseur extrait les 10 premiers enregistrements dans la mémoire locale. À mesure que vous parcourez les **Recordset** de l’objet, le fournisseur retourne les données de la mémoire tampon locale. Dès que vous déplacez au-delà du dernier enregistrement dans le cache, le fournisseur extrait les 10 enregistrements suivants à partir de la source de données dans le cache.  
  
> [!NOTE]
>  **CacheSize** est basé sur le **nombre maximal de lignes ouvrir** propriété spécifique au fournisseur (dans le **propriétés** collection de la **Recordset** objet). Vous ne pouvez pas définir **CacheSize** à une valeur supérieure à **nombre maximal de lignes ouvert.** Pour modifier le nombre de lignes qui peuvent être ouverts par le fournisseur, définissez **nombre maximal de lignes ouvrir**.  
  
 La valeur de **CacheSize** peut être ajustée pendant la durée de vie de la **Recordset** objet, mais la modification de cette valeur affecte uniquement le nombre d’enregistrements dans le cache après plusieurs extractions successives de la source de données. Modifiez la valeur de propriété autonome ne changera pas le contenu actuel du cache.  
  
 S’il existe moins d’enregistrements à récupérer que **CacheSize** Spécifie, le fournisseur retourne les enregistrements restants et aucune erreur ne se produit.  
  
 A **CacheSize** valeur égale à zéro n’est pas autorisée et retourne une erreur.  
  
 Enregistrements récupérés du cache ne reflètent pas les modifications simultanées apportées par d’autres utilisateurs à la source de données. Pour forcer une mise à jour de toutes les données mises en cache, utilisez la [Resync](../../../ado/reference/ado-api/resync-method.md) (méthode).  
  
 Si **CacheSize** est défini sur une valeur supérieure à 1, les méthodes de navigation ([déplacer](../../../ado/reference/ado-api/move-method-ado.md), [MoveFirst, MoveLast, MoveNext et MovePrevious](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)) peut entraîner la navigation vers un élément supprimé enregistrement, si la suppression a lieu après la récupération des enregistrements. Après l’extraction initiale, les suppressions suivantes seront répercutées dans votre cache de données jusqu'à ce que vous tentez d’accéder à une valeur de données à partir d’une ligne supprimée. Toutefois, l’affectation **CacheSize** 1 élimine ce problème, car les lignes supprimées ne peuvent pas être extraites.
