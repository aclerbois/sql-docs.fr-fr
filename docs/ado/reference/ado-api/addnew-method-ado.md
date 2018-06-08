---
title: AddNew (méthode) (ADO) | Documents Microsoft
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
- Recordset15::AddNew
- Recordset15::raw_AddNew
helpviewer_keywords:
- AddNew method [ADO]
ms.assetid: a9f54be9-5763-45d0-a6eb-09981b03bc08
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 315eec5a4cddccb55be1ad9b8bd9d8bc4ded99cf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="addnew-method-ado"></a>AddNew (méthode) (ADO)
Crée un nouvel enregistrement pour être une mise à jour [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.AddNew FieldList, Values  
```  
  
#### <a name="parameters"></a>Paramètres  
 *recordset*  
 A **Recordset** objet.  
  
 *FieldList*  
 Ce paramètre est facultatif. Un nom unique ou un tableau de noms ou de positions ordinales des champs dans le nouvel enregistrement.  
  
 *Valeurs*  
 Ce paramètre est facultatif. Une valeur unique, ou un tableau de valeurs pour les champs dans le nouvel enregistrement. Si *liste de champs* est un tableau, *valeurs* doit également être un tableau avec le même nombre de membres ; sinon, une erreur se produit. L’ordre des noms de champs doit correspondre à l’ordre des valeurs de champs dans chaque tableau.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **AddNew** méthode pour créer et initialiser un nouvel enregistrement. Utilisez le [prend en charge](../../../ado/reference/ado-api/supports-method.md) méthode avec **adAddNew** (un [CursorOptionEnum](../../../ado/reference/ado-api/cursoroptionenum.md) valeur) pour vérifier si vous pouvez ajouter des enregistrements à actuel **Recordset**objet.  
  
 Après avoir appelé la **AddNew** (méthode), le nouvel enregistrement devient l’enregistrement actif et reste actif après avoir appelé la [mise à jour](../../../ado/reference/ado-api/update-method.md) (méthode). Étant donné que le nouvel enregistrement est ajouté à la **Recordset**, un appel à **MoveNext** après la mise à jour ne dépassera la fin de la **Recordset**, rendant **EOF**  True. Si le **Recordset** objet ne prend pas en charge les signets, vous n’êtes peut-être pas en mesure d’accéder au nouvel enregistrement une fois que vous déplacez vers un autre enregistrement. Selon le type de curseur, vous devez appeler la [Requery](../../../ado/reference/ado-api/requery-method.md) méthode pour rendre le nouvel enregistrement accessible.  
  
 Si vous appelez **AddNew** lors de la modification de l’enregistrement en cours ou lors de l’ajout d’un nouvel enregistrement, ADO appelle la **mise à jour** méthode pour enregistrer les modifications et crée ensuite le nouvel enregistrement.  
  
 Le comportement de la **AddNew** méthode varie selon le mode de mise à jour de la **Recordset** objet et si vous passez le *liste de champs* et *valeurs*arguments.  
  
 Dans *en mode de mise à jour immédiate* (dans lequel le fournisseur écrit les modifications à la source de données sous-jacente lorsque vous appelez le **mettre à jour** (méthode)), l’appel le **AddNew** sans (méthode) arguments affecte le [EditMode](../../../ado/reference/ado-api/editmode-property.md) propriété **adEditAdd** (un [EditModeEnum](../../../ado/reference/ado-api/editmodeenum.md) valeur). Le fournisseur met en cache localement les modifications des valeurs de champ. Appel de la **mise à jour** méthode publie le nouvel enregistrement dans la base de données et réinitialise le **EditMode** propriété **adEditNone** (un **EditModeEnum**valeur). Si vous passez le *liste de champs* et *valeurs* arguments, ADO publie immédiatement le nouvel enregistrement dans la base de données (aucun **mise à jour** appel n’est nécessaire) ; le **EditMode**  valeur de propriété ne change pas (**adEditNone**).  
  
 Dans *mode de mise à jour par lot* (dans lequel le fournisseur met en cache plusieurs modifications et les écrit dans la source de données sous-jacente uniquement lorsque vous appelez le [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) méthode), l’appel du **AddNew** méthode sans arguments affecte le **EditMode** propriété **adEditAdd**. Le fournisseur met en cache localement les modifications des valeurs de champ. Appel de la **mise à jour** méthode ajoute le nouvel enregistrement actuel **Recordset**, mais le fournisseur ne pas valider les modifications apportées à la base de données sous-jacente, ou redéfinissez le **EditMode** pour **adEditNone**, jusqu'à ce que vous appeliez la **UpdateBatch** (méthode). Si vous passez le *liste de champs* et *valeurs* arguments, ADO envoie le nouvel enregistrement dans le fournisseur de stockage dans un cache et définit les **EditMode** à **adEditAdd** ; vous devez appeler la **UpdateBatch** méthode pour valider le nouvel enregistrement dans la base de données sous-jacente.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre comment utiliser la méthode AddNew avec la liste de champs et de la liste de valeurs inclus, pour voir comment inclure la liste de champs et une liste de valeurs sous forme de tableaux.  
  
```  
create table aa1 (intf int, charf char(10))  
insert into aa1 values (2, 'aa')  
  
Dim cn As New adodb.Connection  
Dim rs As New adodb.Recordset  
Dim cmd As New adodb.Command  
  
cn.ConnectionString = "Provider=SQLOLEDB;Data Source=alexverb2;uid=sa;pwd=foo$bar00;"  
  
cn.Open  
rs.Open "select * from xxx..aa1", cn, adOpenKeyset, adLockOptimistic  
  
Dim fieldsArray(1) As Variant  
fieldsArray(0) = "intf"  
fieldsArray(1) = "charf"  
Dim values(1) As Variant  
values(0) = 4  
values(1) = "as"  
rs.AddNew fieldsArray, values  
rs.Update  
```  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple AddNew (méthode) (VB)](../../../ado/reference/ado-api/addnew-method-example-vb.md)   
 [Exemple de méthode AddNew (VBScript)](../../../ado/reference/ado-api/addnew-method-example-vbscript.md)   
 [Exemple de méthode AddNew (VC ++)](../../../ado/reference/ado-api/addnew-method-example-vc.md)   
 [CancelUpdate, méthode (ADO)](../../../ado/reference/ado-api/cancelupdate-method-ado.md)   
 [EditMode, propriété](../../../ado/reference/ado-api/editmode-property.md)   
 [Requery (méthode)](../../../ado/reference/ado-api/requery-method.md)   
 [Prend en charge (méthode)](../../../ado/reference/ado-api/supports-method.md)   
 [Update (méthode)](../../../ado/reference/ado-api/update-method.md)   
 [UpdateBatch, méthode](../../../ado/reference/ado-api/updatebatch-method.md)
