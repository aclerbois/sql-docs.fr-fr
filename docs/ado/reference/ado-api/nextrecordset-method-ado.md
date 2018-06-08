---
title: NextRecordset, méthode (ADO) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- NextRecordset
- Recordset15::NextRecordset
- Recordset15::raw_NextRecordset
helpviewer_keywords:
- NextRecordset method [ADO]
ms.assetid: ab1fa449-a695-4987-b1ee-bc68f89418dd
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1773c2bd8709a429a2e388b8a38a192107f2e7f3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="nextrecordset-method-ado"></a>NextRecordset, méthode (ADO)
Efface l’actuel [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) de l’objet et renvoie la prochaine **Recordset** en exécutant une série de commandes.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set recordset2 = recordset1.NextRecordset(RecordsAffected )  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un **Recordset** objet. Dans le modèle de syntaxe, *recordset1* et *recordset2* peuvent être les mêmes **Recordset** objet, ou vous pouvez utiliser des objets distincts. Lors de l’utilisation de distinct **Recordset** objets, la réinitialisation de la **ActiveConnection** propriété sur l’original **Recordset** (*recordset1*) une fois **NextRecordset** a été appelé va générer une erreur.  
  
#### <a name="parameters"></a>Paramètres  
 *RecordsAffected*  
 Ce paramètre est facultatif. A **Long** variable sur laquelle le fournisseur retourne le nombre d’enregistrements affectée par l’opération en cours.  
  
> [!NOTE]
>  Ce paramètre retourne uniquement le nombre d’enregistrements affectés par une opération ; Il ne retourne pas le nombre d’enregistrements à partir d’une instruction select utilisée pour générer le **Recordset**.  
  
## <a name="remarks"></a>Notes  
 Utilisez le **NextRecordset** méthode pour retourner les résultats de la commande suivante dans une instruction composée de commande ou d’une procédure stockée qui retourne plusieurs résultats. Si vous ouvrez un **Recordset** objet basé sur une instruction composée de commande (par exemple, « sélectionnez \* à partir de table1 ; Sélectionnez \* de table2 ») à l’aide de la [Execute](../../../ado/reference/ado-api/execute-method-ado-command.md) méthode sur un [commande](../../../ado/reference/ado-api/command-object-ado.md) ou [ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md) méthode sur un **Recordset**, ADO exécute uniquement la première commande et retourne les résultats à *recordset*. Pour accéder aux résultats des commandes suivantes dans l’instruction, appelez le **NextRecordset** (méthode).  
  
 Tant que des résultats supplémentaires et la **Recordset** contenant les instructions composées n’est pas déconnecté ni marshalé à travers les limites de processus, le **NextRecordset** méthode continue de retourner **Recordset** objets. Si une commande retourne des lignes s’exécute correctement mais ne renvoie aucun enregistrement, retourné **Recordset** objet sera ouvert mais vide. Test pour ce cas en vérifiant que le [BOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md) et [EOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md) propriétés sont toutes deux **True**. Si une commande sans renvoi à la ligne s’exécute correctement, retourné **Recordset** objet sera fermé, vous pouvez vérifier en testant la [état](../../../ado/reference/ado-api/state-property-ado.md) propriété sur le **Recordset**. Lorsqu’il n’y a plus aucun résultat, *recordset* a la valeur *rien*.  
  
 Le **NextRecordset** méthode n’est pas disponible sur un déconnecté **Recordset** objet, où [ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md) a été défini sur **rien**(dans Microsoft Visual Basic) ou NULL (dans d’autres langues).  
  
 Si une modification est en cours d’exécution en mode de mise à jour immédiate, l’appel le **NextRecordset** méthode génère une erreur ; appeler le [mettre à jour](../../../ado/reference/ado-api/update-method.md) ou [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) méthode premier.  
  
 Pour passer des paramètres à plusieurs commandes dans l’instruction composée en complétant la [paramètres](../../../ado/reference/ado-api/parameters-collection-ado.md) collection, ou en passant un tableau avec la version d’origine **ouvrir** ou **Execute** appel, les paramètres doivent être dans le même ordre dans la collection ou un tableau en tant que leurs commandes correspondantes dans la série de commandes. Vous devez terminer la lecture de tous les résultats avant de lire les valeurs de paramètre de sortie.  
  
 Votre fournisseur OLE DB détermine quand chaque commande dans une instruction composée est exécutée. Le [fournisseur Microsoft OLE DB pour SQL Server](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md), par exemple, s’exécute toutes les commandes dans un lot lorsqu’il reçoit l’instruction composée. Résultant **jeux d’enregistrements** sont retournées lorsque vous appelez **NextRecordset**.  
  
 Toutefois, les autres fournisseurs peuvent exécuter la commande suivante dans une instruction uniquement après l’appel de NextRecordset. Pour ces fournisseurs, si vous fermez explicitement le **Recordset** avant d’exécuter l’instruction de la commande entière, par l’objet ADO n’exécute jamais les commandes restantes.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de méthode NextRecordset (VB)](../../../ado/reference/ado-api/nextrecordset-method-example-vb.md)   
 [NextRecordset, exemple de méthode (VC++)](../../../ado/reference/ado-api/nextrecordset-method-example-vc.md)   
