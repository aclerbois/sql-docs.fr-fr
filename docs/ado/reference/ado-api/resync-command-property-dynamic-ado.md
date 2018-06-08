---
title: Resync commande propriété dynamique (ADO) | Documents Microsoft
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
helpviewer_keywords:
- Resync Command property [ADO]
ms.assetid: 4e2bb601-0fe8-4d61-b00e-38341d85a6bb
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 240c4d6ce4aa392f01ebb27a4a52fd73c684b981
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="resync-command-property-dynamic-ado"></a>Resync commande propriété dynamique (ADO)
Spécifie une commande fournie par l’utilisateur de chaîne qui le [Resync](../../../ado/reference/ado-api/resync-method.md) les problèmes de méthode pour actualiser les données dans la table nommée dans la [Unique Table](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) propriété dynamique.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un **chaîne** valeur qui est une chaîne de commande.  
  
## <a name="remarks"></a>Notes  
 Le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet est le résultat d’une opération JOIN exécutée sur plusieurs tables de base. Les lignes affectées dépendent du *AffectRecords* paramètre de la [Resync](../../../ado/reference/ado-api/resync-method.md) (méthode). La norme **Resync** méthode est exécutée si la [Unique Table](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) et **Resync Command** propriétés ne sont pas définies.  
  
 La chaîne de commande de la **Resync Command** propriété est une commande paramétrée ou une procédure stockée qui identifie de façon unique la ligne actualisée et renvoie une seule ligne contenant le même nombre et l’ordre des colonnes en tant que la ligne à actualisé. La chaîne de commande contient un paramètre pour chaque colonne de clé primaire dans le **Unique Table**; sinon, une erreur d’exécution est renvoyée. Les paramètres sont automatiquement renseignés avec les valeurs de clé primaire de la ligne à actualiser.  
  
 Voici deux exemples basés sur SQL :  
  
 1\) le **Recordset** est défini par une commande :  
  
```  
SELECT * FROM Customers JOIN Orders ON   
   Customers.CustomerID = Orders.CustomerID  
   WHERE city = 'Seattle'  
   ORDER BY CustomerID  
```  
  
 Le **Resync Command** est définie sur :  
  
```  
"SELECT * FROM   
   (SELECT * FROM Customers JOIN Orders   
   ON Customers.CustomerID = Orders.CustomerID  
   city = 'Seattle' ORDER BY CustomerID)  
WHERE Orders.OrderID = ?"  
```  
  
 Le **Unique Table** est *commandes* et sa clé primaire, *OrderID*, est paramétrable. L’expression Sub-SELECT fournit un moyen simple de vous assurer par programmation que le même nombre et l’ordre des colonnes sont renvoyées par la commande d’origine.  
  
 2\) le **Recordset** est défini par une procédure stockée :  
  
```  
CREATE PROC Custorders @CustomerID char(5) AS   
SELECT * FROM Customers JOIN Orders ON   
Customers.CustomerID = Orders.CustomerID   
WHERE Customers.CustomerID = @CustomerID  
```  
  
 Le **Resync** méthode doit exécuter la procédure stockée suivante :  
  
```  
CREATE PROC CustordersResync @ordid int AS   
SELECT * FROM Customers JOIN Orders ON   
Customers.CustomerID = Orders.CustomerID  
WHERE Orders.ordid  = @ordid  
```  
  
 Le **Resync Command** est définie sur :  
  
```  
"{call CustordersResync (?)}"  
```  
  
 Une fois encore, la **Unique Table** est *commandes* et sa clé primaire, *OrderID*, est paramétrable.  
  
 **Resync commande** est une propriété dynamique ajoutée à la **Recordset** objet [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection lorsque le [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) est définie sur **adUseClient**.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
