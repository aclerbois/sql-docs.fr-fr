---
title: Préparé, propriété (ADO) | Documents Microsoft
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
- Command15::Prepared
helpviewer_keywords:
- Prepared property [ADO]
ms.assetid: 11ca8825-765e-4bb4-a6ce-3f6564ad8755
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9a5f02d8c1536192832622e26fa4e128a7c04c66
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="prepared-property-ado"></a>Prepared, propriété (ADO)
Indique s’il faut enregistrer la version compilée d’un [commande](../../../ado/reference/ado-api/command-object-ado.md) avant l’exécution.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un **booléenne** valeur, si la valeur **True**, indique que la commande doit être préparée.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **Prepared** propriété pour que le fournisseur enregistre une version préparée (ou compilée) de la requête spécifiée dans le [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) propriété avant une [commande](../../../ado/reference/ado-api/command-object-ado.md) l’objet première exécution. Cela peut ralentir l’exécution du premier d’une commande, mais une fois que le fournisseur compile une commande, le fournisseur utilise la version compilée de la commande pour des exécutions suivantes, ce qui entraîne une amélioration des performances.  
  
 Si la propriété est **False**, le fournisseur s’exécutera la **commande** objet directement sans création d’une version compilée.  
  
 Si le fournisseur ne prend pas en charge la préparation des commandes, elle peut retourner une erreur lorsque cette propriété est définie sur **True**. Si le fournisseur ne retourne pas d’erreur, il ignore simplement la requête de préparation de la commande et le définit le **Prepared** propriété **False**.  
  
## <a name="applies-to"></a>S'applique à  
 [Command, objet (ADO)](../../../ado/reference/ado-api/command-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété préparé (VB)](../../../ado/reference/ado-api/prepared-property-example-vb.md)   
 [Prepared, exemple de propriété (VC++)](../../../ado/reference/ado-api/prepared-property-example-vc.md)   
