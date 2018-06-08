---
title: La Collection de champs | Documents Microsoft
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
- Field object [ADO], fields collection
- Fields collection [ADO]
ms.assetid: 574cf36e-e5f5-403b-983c-749ef93c108f
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 35be5189e20c9f028c5d73a68aab90ac502b4dfc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="the-fields-collection"></a>La Collection de champs
Le **champs** collection est une des collections intrinsèques d’ADO. Une collection est un ensemble ordonné d’éléments qui peuvent être référencés en tant qu’unité. Pour plus d’informations sur les collections ADO, consultez [modèle d’objet ADO](../../../ado/guide/data/ado-objects-and-collections.md).  
  
 Le **champs** collection contient un **champ** pour chaque champ (colonne) de l’objet dans le **Recordset**. Comme toutes les collections ADO, elle a **nombre** et **élément** propriétés, ainsi que **Append** et **Actualiser** méthodes. Il a également **CancelUpdate**, **supprimer**, **Resync**, et **mise à jour** , ces méthodes ne sont pas disponibles pour les autres collections ADO.  
  
## <a name="examining-the-fields-collection"></a>Examen de la Collection de champs  
 Prendre en compte la **champs** collection de l’exemple **Recordset** présentés dans cette section. L’exemple **Recordset** a été dérivée à partir de l’instruction SQL  
  
```  
SELECT ProductID, ProductName, UnitPrice FROM Products WHERE CategoryID = 7  
```  
  
 Par conséquent, vous devez constater que le **Fields du Recordset** collection contient trois champs.  
  
```  
'BeginWalkFields  
    Dim objFields As ADODB.Fields  
    Dim intLoop As Integer  
  
    objRs.Open strSQL, strConnStr, adOpenForwardOnly, adLockReadOnly, adCmdText  
  
    Set objFields = objRs.Fields  
  
    For intLoop = 0 To (objFields.Count - 1)  
        Debug.Print objFields.Item(intLoop).Name  
    Next  
'EndWalkFields  
```  
  
 Ce code détermine simplement le nombre de **champ** des objets dans le **champs** à l’aide de la collection le **nombre** propriété et parcourt la collection, en retournant la valeur de le **nom** propriété pour chaque **champ** objet. Vous pouvez utiliser bien plus encore **champ** propriétés pour obtenir des informations sur un champ. Pour plus d’informations sur l’interrogation un **champ**, consultez [le champ objet](../../../ado/guide/data/the-field-object.md).  
  
## <a name="counting-columns"></a>Comptage des colonnes  
 Comme vous pouvez l’imaginer, la **nombre** propriété retourne le nombre réel de **champ** des objets dans le **champs** collection. Étant donné que la numérotation des membres d’une collection commence à zéro, vous devez toujours coder les boucles en commençant par le membre zéro et en terminant par la valeur de la **nombre** propriété moins 1. Si vous utilisez Microsoft Visual Basic et que vous souhaitez parcourir les membres d’une collection sans vérifier le **nombre** propriété, utilisez la **For Each... Suivant** commande.  
  
 Si le **nombre** propriété est zéro, il y a aucun objet dans la collection.  
  
## <a name="getting-to-the-field"></a>Mise en route pour le champ  
 Comme avec n’importe quelle collection ADO, la **élément** propriété est la propriété par défaut de la collection. Elle retourne la personne **champ** objet spécifié par le nom ou l’index lui est passé. Par conséquent, les instructions suivantes sont équivalentes pour l’exemple **Recordset**:  
  
```  
objField = objRecordset.Fields.Item("ProductID")  
objField = objRecordset.Fields("ProductID")  
objField = objRecordset.Fields.Item(0)  
objField = objRecordset.Fields(0)  
```  
  
 Si ces méthodes sont équivalentes, ce qui convient le mieux ? Ça dépend. À l’aide d’un index pour extraire un **champ** à partir de la collection est plus rapide car elle accède à la **champ** directement sans avoir à effectuer une recherche de chaîne. En revanche, l’ordre des **champs** au sein de la collection doit être connu, et si l’ordre des modifications, la référence à la **du champ** index devra être modifiée en conséquence. Bien que les légèrement plus lentes, en utilisant le nom de la **champ** est plus souple, car elle ne dépend pas de l’ordre de la **champs** dans la collection.  
  
## <a name="using-the-refresh-method"></a>À l’aide de la méthode d’actualisation  
 Contrairement à d’autres collections ADO, à l’aide de la **Actualiser** méthode sur le **champs** collection n’a aucun effet visible. Pour récupérer les modifications de la structure de base de données sous-jacente, vous devez utiliser soit le **Requery** (méthode), ou si le **Recordset** objet ne prend pas en charge les signets, la **MoveFirst**(méthode), ce qui provoque la commande à exécuter à nouveau sur le fournisseur.  
  
## <a name="adding-fields-to-a-recordset"></a>Ajout de champs dans un jeu d’enregistrements  
 Le **Append** méthode est utilisée pour ajouter des champs à un **Recordset**.  
  
 Vous pouvez utiliser la **Append** méthode pour fabriquer une **Recordset** par programme sans ouvrir de connexion à une source de données. Une erreur d’exécution se produit si le **Append** méthode est appelée sur le **champs** collection ouvert **Recordset** ou sur un **Recordset** où les **ActiveConnection** propriété a été définie. Vous pouvez ajouter des champs uniquement à un **Recordset** qui n’est pas ouvert et n’a pas encore été connecté à une source de données. Toutefois, pour spécifier des valeurs pour récemment ajoutés **champs**, le **Recordset** doit être ouvert.  
  
 Les développeurs ont souvent besoin d’un emplacement pour stocker des données temporairement ou certaines données se comportent comme si elle provenance d’un serveur afin de pouvoir faire partie de la liaison de données dans une interface utilisateur. ADO (conjointement avec la [le Service de curseur Microsoft pour OLE DB](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)) permet au développeur de créer vide **Recordset** objet en spécifiant les informations de colonne et en appelant **ouvrir**. Dans l’exemple suivant, trois nouveaux champs sont ajoutés à un nouveau **Recordset** objet. Le **Recordset** est ouvert, deux nouveaux enregistrements sont ajoutés et le **Recordset** est rendue persistante dans un fichier. (Pour plus d’informations sur **Recordset** persistance, consultez [mise à jour et persistance des données](../../../ado/guide/data/updating-and-persisting-data.md).)  
  
```  
'BeginFabricate  
    Dim objRs As ADODB.Recordset  
    Set objRs = New ADODB.Recordset  
  
    With objRs.Fields  
        .Append "StudentID", adChar, 11, adFldUpdatable  
        .Append "FullName", adVarChar, 50, adFldUpdatable  
        .Append "PhoneNmbr", adVarChar, 20, adFldUpdatable  
    End With  
  
    With objRs  
        .Open  
  
        .AddNew  
        .Fields(0) = "123-45-6789"  
        .Fields(1) = "John Doe"  
        .Fields(2) = "(425) 555-5555"  
        .Update  
  
        .AddNew  
        .Fields(0) = "123-45-6780"  
        .Fields(1) = "Jane Doe"  
        .Fields(2) = "(615) 555-1212"  
        .Update  
    End With  
  
    objRs.Save App.Path & "FabriTest.adtg", adPersistADTG  
  
    objRs.Close  
'EndFabricate  
```  
  
 L’utilisation de la **ajouter des champs** méthode diffère entre le **Recordset** objet et la **enregistrement** objet. Pour plus d’informations sur la **enregistrement** d’objets, consultez [enregistrements et flux](../../../ado/guide/data/records-and-streams.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Fabrication de recordsets hiérarchiques](../../../ado/guide/data/fabricating-hierarchical-recordsets.md)
