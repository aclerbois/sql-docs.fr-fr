---
title: Détection et résolution des conflits | Documents Microsoft
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
- conflicts [ADO], detecting and resolving
- ADO, detecting and resolving conflicts
ms.assetid: b28fdd26-c1a4-40ce-a700-2b0c9d201514
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 05e79fec4c5ddf9d33c9cfaa17581b6d50e0e42b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="detecting-and-resolving-conflicts"></a>Détection et résolution des conflits
Si vous êtes confronté à votre jeu d’enregistrements en mode exécution, il est beaucoup moins le risque de problèmes d’accès concurrentiel se produise. En revanche, si votre application utilise le mode de traitement par lots mise à jour, il peut être une bonne chance qu’un utilisateur modifie un enregistrement avant l’enregistrement des modifications apportées par un autre utilisateur, ce même enregistrement. Dans ce cas, vous préférerez votre application de façon à gérer le conflit. Il peut être votre souhait de la dernière personne à envoyer une mise à jour sur le serveur « remporte ». Ou vous pouvez souhaiter permettre à l’utilisateur la plus récente afin de déterminer la mise à jour est prioritaire en lui offrant un choix entre les deux valeurs en conflit.  
  
 Quel que soit le cas, ADO fournit les propriétés invoquez de l’objet de champ à gérer ces types de conflits. Utilisez ces propriétés en combinaison avec la méthode Resync et la propriété Filter de l’objet Recordset.  
  
## <a name="remarks"></a>Notes  
 Lorsque ADO rencontre un conflit pendant une mise à jour par lots, un avertissement sera ajouté à la collection d’erreurs. Par conséquent, vous devez toujours vérifier les erreurs immédiatement après que vous appelez BatchUpdate et si vous en trouvez, commencez le test de l’hypothèse que vous avez rencontré un conflit. La première étape consiste à définir la propriété Filter sur l’objet Recordset la valeur adFilterConflictingRecords. Cela limite la vue sur le jeu d’enregistrements, seuls les enregistrements qui sont en conflit. Si la propriété RecordCount est égale à zéro après cette étape, vous savez que l’erreur a été levée par autre chose qu’un conflit.  
  
 Lorsque vous appelez BatchUpdate, ADO et le fournisseur génèrent des instructions SQL pour effectuer des mises à jour sur la source de données. N’oubliez pas que certaines sources de données ont des limitations sur lequel les types de colonnes peuvent être utilisés dans une clause WHERE.  
  
 Ensuite, appelez la méthode Resync sur l’ensemble d’enregistrements avec l’argument AffectRecords égal à adAffectGroup et l’argument ResyncValues égal à adResyncUnderlyingValues. La méthode Resync met à jour les données dans l’objet de jeu d’enregistrements actuel à partir de la base de données sous-jacente. À l’aide d’adAffectGroup, vous vous assurez que seuls les enregistrements visibles avec le filtre actuel définissant, autrement dit, seuls les enregistrements en conflit, sont resynchronisés avec la base de données. Cela permet d’améliorer considérablement les performances si vous êtes confronté à un jeu d’enregistrements volumineux. En définissant l’argument ResyncValues adResyncUnderlyingValues lors de l’appel de resynchronisation, vous assurez que la propriété UnderlyingValue contiendra la valeur (conflictuelle) à partir de la base de données, que la propriété de valeur met à jour la valeur entrée par l’utilisateur, et que la propriété OriginalValue contiendra la valeur d’origine pour le champ (la valeur qu’elle avait avant le dernier appel réussi de UpdateBatch). Vous pouvez ensuite utiliser ces valeurs pour résoudre le conflit par programme ou oblige l’utilisateur à sélectionner la valeur qui sera utilisée.  
  
 Cette technique est illustrée dans l’exemple de code suivant. L’exemple crée artificiellement un conflit à l’aide d’un objet Recordset distinct pour modifier une valeur dans la table sous-jacente avant l’appel de UpdateBatch.  
  
```  
'BeginConflicts  
    strConn = "Provider=SQLOLEDB;Initial Catalog=Northwind;" & _  
              "Data Source=MySQLServer;Integrated Security=SSPI;"  
  
    strSQL = "SELECT * FROM Shippers WHERE ShipperID = 2"  
  
    'Open Rs and change a value  
    Set objRs1 = New ADODB.Recordset  
    Set objRs2 = New ADODB.Recordset  
    objRs1.CursorLocation = adUseClient  
    objRs1.Open strSQL, strConn, adOpenStatic, adLockBatchOptimistic, adCmdText  
    objRs1("Phone") = "(111) 555-1111"  
  
    'Introduce a conflict at the db...  
    objRs2.Open strSQL, strConn, adOpenKeyset, adLockOptimistic, adCmdText  
    objRs2("Phone") = "(999) 555-9999"  
    objRs2.Update  
    objRs2.Close  
    Set objRs2 = Nothing  
  
    On Error Resume Next  
    objRs1.UpdateBatch  
  
    If objRs1.ActiveConnection.Errors.Count <> 0 Then  
        Dim intConflicts As Integer  
  
        intConflicts = 0  
  
        objRs1.Filter = adFilterConflictingRecords  
  
        intConflicts = objRs1.RecordCount  
  
        'Resync so we can see the UnderlyingValue and offer user a choice.  
        'This sample only displays all three values and resets to original.  
        objRs1.Resync adAffectGroup, adResyncUnderlyingValues  
  
        If intConflicts > 0 Then  
            strMsg = "A conflict occurred with updates for " & intConflicts & _  
                     " record(s)." & vbCrLf & "The values will be restored" & _  
                     " to their original values." & vbCrLf & vbCrLf  
  
            objRs1.MoveFirst  
            While Not objRs1.EOF  
              strMsg = strMsg & "SHIPPER = " & objRs1("CompanyName") & vbCrLf  
              strMsg = strMsg & "Value = " & objRs1("Phone").Value & vbCrLf  
              strMsg = strMsg & "UnderlyingValue = " & _  
                                 objRs1("Phone").UnderlyingValue & vbCrLf  
              strMsg = strMsg & "OriginalValue = " & _  
                                 objRs1("Phone").OriginalValue & vbCrLf  
              strMsg = strMsg & vbCrLf & "Original value has been restored."  
  
              MsgBox strMsg, vbOKOnly, _  
                    "Conflict " & objRs1.AbsolutePosition & _  
                    " of " & intConflicts  
  
              objRs1("Phone").Value = objRs1("Phone").OriginalValue  
              objRs1.MoveNext  
            Wend  
  
            objRs1.UpdateBatch adAffectGroup  
        Else  
            'Other error occurred. Minimal handling in this example.  
             strMsg = "Errors occurred during the update. " & _  
                        objRs1.ActiveConnection.Errors(0).Number & " " & _  
                        objRs1.ActiveConnection.Errors(0).Description  
        End If  
  
        On Error GoTo 0  
    End If  
  
    objRs1.MoveFirst  
    objRs1.Close  
    Set objRs1 = Nothing  
'EndConflicts  
```  
  
 Vous pouvez utiliser la propriété Status de l’enregistrement actif ou d’un champ spécifique pour déterminer quel type d’un conflit s’est produite.  
  
 Pour plus d’informations sur la gestion des erreurs, consultez [gestion des erreurs](../../../ado/guide/data/error-handling.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Mode batch](../../../ado/guide/data/batch-mode.md)
