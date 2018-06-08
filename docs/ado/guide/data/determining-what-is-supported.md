---
title: Déterminer ce qui est pris en charge | Documents Microsoft
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
- editing data [ADO], Supports method
- Supports method [ADO]
ms.assetid: 65090cba-6d46-4775-8d61-f6838e7752a6
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: dd86d96489e59926935567a0f8aa7cb23bf9f3ae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="determining-what-is-supported"></a>Déterminer ce qui est pris en charge
Le **prend en charge** méthode est utilisée pour déterminer si un élément spécifié **Recordset** objet prend en charge un type particulier de fonctionnalité. Il présente la syntaxe suivante :  
  
```  
  
boolean = recordset.Supports(CursorOptions )  
```  
  
## <a name="remarks"></a>Notes  
 Le **prend en charge** méthode retourne une valeur booléenne qui indique si le fournisseur prend en charge toutes les fonctionnalités identifiées par l’argument CursorOptions. Vous pouvez utiliser la **prend en charge** méthode pour déterminer les types de fonctionnalités un **Recordset** prend en charge de l’objet. Si le **Recordset** objet prend en charge les fonctionnalités dont les constantes correspondantes se trouvent dans *CursorOptions*, le **prend en charge** méthode retourne **True**. Sinon, elle retourne **False**.  
  
 À l’aide de la **prend en charge** (méthode), vous pouvez vérifier la capacité de la **Recordset** objet à ajouter de nouveaux enregistrements, utiliser des signets, utiliser le **trouver** méthode, utilisez le défilement, utilisez le  **Index** propriété et d’effectuer des mises à jour par lots. Pour obtenir une liste complète des constantes et leur signification, consultez [CursorOptionEnum](../../../ado/reference/ado-api/cursoroptionenum.md).  
  
 Bien que le **prend en charge** méthode peut retourner **True** pour une fonctionnalité donnée, il ne pas garantit que le fournisseur peut rendre la fonctionnalité disponible en toutes circonstances. Le **prend en charge** méthode retourne simplement si le fournisseur peut prendre en charge la fonctionnalité spécifiée, en supposant que certaines conditions sont remplies. Par exemple, le **prend en charge** méthode peut-être indiquer qu’un **Recordset** objet prend en charge les mises à jour, même si le curseur est basé sur une jointure de plusieurs tables, dont certaines colonnes ne sont pas modifiables.
