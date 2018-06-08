---
title: Les décalages de liaison de paramètre | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- offsets of parameters [ODBC]
- binding offsets of parameters [ODBC]
ms.assetid: 309339e9-9ccd-4a58-8aa4-b6dc88f4eb7c
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 63284508fe2614a84eecb890545bbce2f8e0147b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="parameter-binding-offsets"></a>Décalages de liaison de paramètre
Une application peut spécifier qu’un décalage est ajouté lié des adresses de mémoire tampon de paramètre et l’indicateur de longueur/correspondante des adresses de mémoire tampon quand **SQLExecDirect** ou **SQLExecute** est appelée. Le résultat de ces ajouts détermine les adresses utilisées dans ces opérations.  
  
 Les décalages de liaison permettent à une application modifier les liaisons sans appeler **SQLBindParameter** pour les paramètres précédemment liées. Un appel à **SQLBindParameter** pour relier un paramètre change l’adresse de la mémoire tampon et le pointeur de longueur / d’indicateur. La reliaison avec un décalage, ajoute quant à eux, simplement un offset à l’adresse de mémoire tampon de paramètres liés existant et indicateur/longueur de la mémoire tampon. Lorsque les décalages sont utilisés, les liaisons sont un « modèle » de la disposition des mémoires tampons de l’application et l’application peut passer cet « modèle » à différentes zones de mémoire en modifiant le décalage. Un nouveau décalage peut être spécifié à tout moment et est toujours ajouté pour les valeurs liées à l’origine.  
  
 Pour spécifier un décalage de la liaison, l’application définit l’attribut d’instruction SQL_ATTR_PARAM_BIND_OFFSET_PTR à l’adresse d’une mémoire tampon SQLINTEGER. Avant que l’application appelle une fonction qui utilise les liaisons, il place un offset en octets dans cette mémoire tampon, que l’adresse de mémoire tampon de paramètre, ni l’adresse de mémoire tampon de longueur / d’indicateur est 0, et le paramètre lié est dans l’instruction SQL. La somme de l’adresse et le décalage doit être une adresse valide. (Cela signifie qu’un ou les deux le décalage et l’adresse à laquelle le décalage est ajouté peuvent être non valides, tant que leur somme est une adresse valide.)  
  
> [!NOTE]  
>  Les décalages de liaison ne sont pas pris en charge par ODBC 2. *x* pilotes.
