---
title: Save (méthode) | Documents Microsoft
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
- _Recordset::Save
- _Recordset::raw_Save
helpviewer_keywords:
- Save method [ADO]
ms.assetid: ed3d9678-5c28-4e61-8bb3-7dfb66d99cf5
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a24e0be39417235e86a86e239b8f6918fadda5dc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="save-method"></a>Save (méthode)
Enregistre le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) dans un fichier ou [flux](../../../ado/reference/ado-api/stream-object-ado.md) objet.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
recordset.Save Destination, PersistFormat  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Destination*  
 Ce paramètre est facultatif. A **Variant** qui représente le nom de chemin d’accès complet du fichier où le **Recordset** doit être enregistré, ou une référence à un **flux** objet.  
  
 *PersistFormat*  
 Ce paramètre est facultatif. A [PersistFormatEnum](../../../ado/reference/ado-api/persistformatenum.md) valeur qui spécifie le format dans lequel le **Recordset** doit être enregistré (XML ou ADTG). La valeur par défaut est **adPersistADTG**.  
  
## <a name="remarks"></a>Notes  
 Le [méthode Save](../../../ado/reference/ado-api/save-method.md) méthode peut uniquement être appelée sur open **Recordset**. Utilisez le [Open (méthode) (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md) méthode à la restauration ultérieure le **Recordset** de *Destination*.  
  
 Si le [propriété Filter](../../../ado/reference/ado-api/filter-property.md) propriété n’est en vigueur pour le **Recordset**, seules les lignes accessibles sous le filtre sont enregistrées. Si le **Recordset** est hiérarchique, puis l’enfant **Recordset** et ses enfants sont enregistrés, y compris le parent **Recordset**. Si la méthode Save d’un enfant **Recordset** est appelée, l’enfant et tous ses enfants sont enregistrés, mais le parent n’est pas.  
  
 La première fois que vous enregistrez le **Recordset**, il est facultatif spécifier *Destination*. Si vous omettez *Destination*, un nouveau fichier sera créé avec un nom de la valeur de la propriété de Source de la **Recordset**.  
  
 Omettre *Destination* lorsque vous appelez ensuite **enregistrer** après le premier enregistrement ou une erreur d’exécution se produira. Si vous appelez ensuite **enregistrer** avec un nouveau *Destination*, le **Recordset** est enregistré dans la nouvelle destination. Toutefois, la nouvelle destination et la destination d’origine à la fois sera ouverts.  
  
 **Enregistrer** ne ferme pas le **Recordset** ou *Destination*, de sorte que vous pouvez continuer à travailler avec les **Recordset** et enregistrer vos modifications les plus récentes. *Destination* reste ouverte jusqu'à ce que le **Recordset** est fermé.  
  
 Pour des raisons de sécurité, le **enregistrer** méthode autorise uniquement l’utilisation des paramètres de sécurité basse et personnalisée à partir d’un script exécuté par Microsoft Internet Explorer.  
  
 Si le **enregistrer** méthode est appelée en asynchrone **Recordset** fetch, exécuter ou mettre à jour est en cours, puis **enregistrer** attend que l’opération asynchrone est terminée.  
  
 Enregistrements sont enregistrés en commençant par la première ligne de la **Recordset**. Lorsque le **enregistrer** méthode est terminée, le curseur est déplacé vers la première ligne de la **Recordset**.  
  
 Pour de meilleurs résultats, définissez la [propriété CursorLocation (ADO)](../../../ado/reference/ado-api/cursorlocation-property-ado.md) propriété **adUseClient** avec **enregistrer**. Si votre fournisseur ne prend pas en charge toutes les fonctionnalités nécessaires pour enregistrer les **Recordset** des objets, le Service de curseur fournit cette fonctionnalité.  
  
 Lorsqu’un **Recordset** est rendue persistante avec les **CursorLocation** propriété la valeur **adUseServer**, la fonctionnalité de mise à jour pour le **Recordset**est limité. En règle générale, seules les mises à jour de la table unique, des insertions et suppressions sont autorisées (selon les fonctionnalités du fournisseur). Le [méthode Resync](../../../ado/reference/ado-api/resync-method.md) méthode n’est également disponible dans cette configuration.  
  
> [!NOTE]
>  Enregistrer un **Recordset** avec **champs** de type **adVariant**, **adIDispatch**, ou **adIUnknown** est non pris en charge par ADO et peut entraîner des résultats imprévisibles.  
  
 Filtre uniquement sous la forme de chaînes de critères (par exemple OrderDate > ' 12/31/1999 ') affectent le contenu d’un rendu persistant **Recordset**. Les filtres créés avec un tableau de **signets** ou à l’aide d’une valeur à partir de la [FilterGroupEnum](../../../ado/reference/ado-api/filtergroupenum.md) n’affectera pas le contenu du rendu persistant **Recordset**. Ces règles s’appliquent aux **Recordset**s créés avec des curseurs côté client ou côté serveur.  
  
 Étant donné que la *Destination* paramètre accepte tout objet qui prend en charge l’interface OLE DB IStream, vous pouvez enregistrer un **Recordset** directement à l’objet Response ASP. Pour plus d’informations, consultez la **scénario de persistance des objets Recordset XML**.  
  
 Vous pouvez également enregistrer une **Recordset** au format XML à une instance d’un objet DOM MSXML, comme est indiqué dans le code Visual Basic suivant :  
  
```  
Dim xDOM As New MSXML.DOMDocument  
Dim rsXML As New ADODB.Recordset  
Dim sSQL As String, sConn As String  
  
sSQL = "SELECT customerid, companyname, contactname FROM customers"  
sConn="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=Northwind.mdb"  
rsXML.Open sSQL, sConn  
rsXML.Save xDOM, adPersistXML   'Save Recordset directly into a DOM tree.  
...  
```  
  
> [!NOTE]
>  Deux limitations s’appliquent lors de l’enregistrement des jeux d’enregistrements (formes de données) au format XML. Vous ne pouvez pas enregistrer au format XML si l’hiérarchique **Recordset** contient en attente de mises à jour, et vous ne pouvez pas enregistrer un paramétrable hiérarchique **Recordset**.  
  
 A **Recordset** enregistré au format XML format est enregistré à l’aide du format UTF-8. Lorsqu’un tel fichier est chargé dans un flux de données ADO, l’objet Stream ne tente pas d’ouvrir une **Recordset** à partir du flux, sauf si la propriété de jeu de caractères du flux de données a la valeur appropriée pour le format UTF-8.  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)|[Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Enregistrer et ouvrir des méthodes-exemple (VB)](../../../ado/reference/ado-api/save-and-open-methods-example-vb.md)   
 [Enregistrez et ouvrez l’exemple de méthodes (VC ++)](../../../ado/reference/ado-api/save-and-open-methods-example-vc.md)   
 [Open (méthode) (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [Open (méthode) (flux ADO)](../../../ado/reference/ado-api/open-method-ado-stream.md)   
 [SaveToFile, méthode](../../../ado/reference/ado-api/savetofile-method.md)
