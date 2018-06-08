---
title: L’objet de champ | Documents Microsoft
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
- Field object [ADO]
ms.assetid: 7d1c4ad5-4be3-42ab-b516-e7133ca300bc
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 872bb5e1ccede336f85b7bbcbdc7e91c89dd1688
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="the-field-object"></a>Le champ de l’objet
Chaque **champ** objet correspond généralement à une colonne dans une table de base de données. Toutefois, un **champ** peut également représenter un pointeur vers un autre **Recordset**, appelé un chapitre. Exceptions, telles que les colonnes de chapitres, seront abordées plus loin dans ce guide.  
  
 Utilisez le **valeur** propriété du **champ** objets pour définir ou retourner les données de l’enregistrement actif. Selon les fonctionnalités le fournisseur expose des regroupements, des méthodes ou propriétés d’un **champ** objet n’est peut-être pas disponible.  
  
 Avec les collections, les méthodes et les propriétés d’un **champ** de l’objet, vous pouvez procédez comme suit :  
  
-   Retourner le nom d’un champ à l’aide de la **nom** propriété.  
  
-   Afficher ou modifier les données dans le champ à l’aide de la **valeur** propriété. **Valeur** est la propriété par défaut de la **champ** objet.  
  
-   Retourner les caractéristiques de base d’un champ à l’aide de la **Type**, **précision**, et **NumericScale** propriétés.  
  
-   Retourner la taille déclarée d’un champ à l’aide de la **DefinedSize** propriété.  
  
-   Retourner la taille réelle des données dans un champ donné à l’aide de la **ActualSize** propriété.  
  
-   Déterminer les types de fonctionnalités sont prises en charge pour un champ donné à l’aide de la **attributs** propriété et **propriétés** collection.  
  
-   Manipuler les valeurs des champs contenant des données binaires longues ou caractères longs à l’aide de la **AppendChunk** et **GetChunk** méthodes.  
  
 Résoudre les différences dans les valeurs de champ pendant une mise à jour par lot à l’aide de la **OriginalValue** et **UnderlyingValue** propriétés, si le fournisseur prend en charge les mises à jour par lots.  
  
## <a name="describing-a-field"></a>Description d’un champ  
 Les rubriques suivantes abordent les propriétés de la [champ](../../../ado/reference/ado-api/field-object.md) objet qui représente des informations qui décrivent le **champ** objet proprement dit, autrement dit, les métadonnées relatives au champ. Ces informations peuvent être utilisées pour déterminer la bonne partie du schéma de la **Recordset**. Ces propriétés incluent **Type**, **DefinedSize** et **ActualSize**, **nom**, et **NumericScale**et **précision**.  
  
### <a name="discovering-the-data-type"></a>Découvrir le Type de données  
 Le **Type** propriété indique le type de données du champ. Les constantes énumérées sont pris en charge par ADO sont décrites dans le type de données [DataTypeEnum](../../../ado/reference/ado-api/datatypeenum.md) dans les *de référence du programmeur ADO*.  
  
 Pour les types numériques tels de virgule flottante **adNumeric**, vous pouvez obtenir plus d’informations. Le **NumericScale** propriété indique le nombre de chiffres à droite de la virgule décimale sera utilisé pour représenter des valeurs pour le **champ**. Le **précision** propriété spécifie le nombre maximal de chiffres utilisés pour représenter des valeurs pour le **champ**.  
  
### <a name="determining-field-size"></a>Détermination de la taille de champ  
 Utilisez le **DefinedSize** propriété pour déterminer la capacité de données d’une **champ** objet.  
  
 Utilisez le **ActualSize** propriété pour retourner la longueur réelle d’un **champ** valeur de l’objet. Pour tous les champs, les **ActualSize** propriété est en lecture seule. Si ADO ne peut pas déterminer la longueur de la **champ** valeur de l’objet, le **ActualSize** propriété renvoie **adUnknown**.  
  
 Le **DefinedSize** et **ActualSize** propriétés ont des objectifs différents. Par exemple, considérez un **champ** objet avec un type déclaré de **adVarChar** et un **DefinedSize** valeur de la propriété de 50, contenant un seul caractère. Le **ActualSize** valeur de propriété retournée est la longueur en octets du caractère unique.  
  
### <a name="determining-field-contents"></a>Déterminer le contenu du champ  
 L’identificateur de la colonne à partir de la source de données est représenté par le **nom** propriété de la **champ**. Le **valeur** propriété de la **champ** objet retourne ou définit le contenu de données réel du champ. Il s’agit de la propriété par défaut.  
  
 Pour modifier les données dans un champ, définissez la **valeur** propriété est égale à une nouvelle valeur du type approprié. Le type de curseur doit prendre en charge les mises à jour pour modifier le contenu d’un champ. Validation de la base de données n'est pas effectuée ici en mode batch, vous devez vérifier les erreurs lorsque vous appelez **UpdateBatch** dans ce cas. Certains fournisseurs prennent également en charge ADO **champ** l’objet **UnderlyingValue** et **OriginalValue** propriétés pour vous aider à résoudre les conflits lorsque vous tentez effectuer des mises à jour par lots. Pour plus d’informations sur la façon de résoudre ces conflits, consultez [modification des données](../../../ado/guide/data/editing-data.md).  
  
> [!NOTE]
>  **Recordset Field** valeurs ne peut pas être définies lors de l’ajout de nouveaux **champs** à un **Recordset**. Au lieu de cela, nouvelle **champs** peuvent être ajoutés à un fermé **Recordset**. Le **Recordset** doit être les valeurs ouvert, puis uniquement et peuvent être affectées à ces **champs**.  
  
### <a name="getting-more-field-information"></a>Informations complémentaires sur le champ  
 Objets ADO possèdent deux types de propriétés : intégrées et dynamiques. À ce stade, seules les propriétés intégrées de la **champ** objet ont été présentés.  
  
 Propriétés intégrées sont les propriétés implémentées dans ADO et immédiatement disponibles pour tout nouvel objet, à l’aide du `MyObject.Property` syntaxe. Ils n’apparaissent pas en tant que **propriété** les objets d’un objet **propriétés** collection.  
  
 Les propriétés dynamiques sont définies par le fournisseur de données sous-jacent et apparaissent dans le **propriétés** collection pour l’objet ADO approprié. Par exemple, une propriété spécifique au fournisseur peut indiquer si un **Recordset** objet prend en charge les transactions ou la mise à jour. Ces propriétés supplémentaires apparaissent en tant que **propriété** objets dans ce **Recordset** l’objet **propriétés** collection. Propriétés dynamiques peuvent être référencées uniquement par le biais de la collection, à l’aide de la syntaxe `MyObject.Properties(0)` ou `MyObject.Properties("Name")`.  
  
 Vous ne pouvez pas supprimer les deux types de propriété.  
  
 Un dynamique **propriété** objet possède quatre propriétés intégrées :  
  
-   Le **nom** propriété est une chaîne qui identifie la propriété.  
  
-   Le **Type** propriété est un entier qui spécifie le type de données de propriété.  
  
-   Le **valeur** propriété est une variante qui contient le paramètre de propriété. **Valeur** est la propriété par défaut pour un **propriété** objet.  
  
-   Le **attributs** propriété est un **Long** valeur qui indique les caractéristiques de la propriété spécifique au fournisseur.  
  
 Le **propriétés** collection pour le **champ** objet contient des métadonnées supplémentaires sur le champ. Le contenu de cette collection varie en fonction du fournisseur. L’exemple de code suivant examine la **propriétés** collection de l’exemple **Recordset** présenté au début de cette section. Il examine tout d’abord le contenu de la collection. Ce code utilise le [fournisseur OLE DB pour SQL Server](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md), donc le **propriétés** collection contient des informations relatives à ce fournisseur.  
  
```  
'BeginFieldProps  
    Dim objProp As ADODB.Property  
  
    For intLoop = 0 To (objFields.Count - 1)  
        Debug.Print objFields.Item(intLoop).Name  
  
        For Each objProp In objFields(intLoop).Properties  
            Debug.Print vbTab & objProp.Name & " = " & objProp.Value  
        Next objProp  
    Next intLoop  
'EndFieldProps  
```  
  
### <a name="dealing-with-binary-data"></a>Traitement des données binaires  
 Utilisez le [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) méthode sur un **champ** objet à remplir avec des données binaires longues ou de caractères. Dans les situations où la mémoire système est limitée, vous pouvez utiliser la **AppendChunk** méthode pour manipuler les valeurs longues dans les parties plutôt que dans leur intégralité.  
  
 Si le **adFldLong** bit dans le **attributs** propriété d’un **champ** objet a la valeur **True**, que vous pouvez utiliser la  **AppendChunk** méthode pour ce champ.  
  
 La première **AppendChunk** appeler sur un **champ** objet écrit des données dans le champ, remplaçant les données existantes. Ultérieures **AppendChunk** ajoutent des appels aux données existantes. Si vous ajoutez des données à un champ et puis de définir ou de lire la valeur d’un autre champ dans l’enregistrement actif, ADO suppose que vous avez terminé l’ajout de données dans le premier champ. Si vous appelez le **AppendChunk** méthode sur le premier champ, ADO interprète l’appel en tant que nouvelle **AppendChunk** opération et remplace les données existantes. L’accès à d’autres champs **Recordset** les objets qui ne sont pas des clones du premier **Recordset** objet n’interrompra pas **AppendChunk** operations.  
  
 Utilisez le **GetChunk** méthode sur un **champ** objet pour extraire tout ou partie de ses données binaires longues ou de caractères. Dans les situations où la mémoire système est limitée, vous pouvez utiliser la **GetChunk** méthode pour manipuler les valeurs longues dans des parties, plutôt que dans leur intégralité.  
  
 Les données qui un **GetChunk** appel retourne est affectée à *variable*. Si *taille* est supérieur aux données restantes, la **GetChunk** méthode retourne uniquement les données restantes sans remplissage *variable* avec des espaces vides. Si le champ est vide, le **GetChunk** méthode retourne une valeur null.  
  
 Chaque **GetChunk** appel récupère les données en commençant à partir de laquelle la précédente **GetChunk** appel s’est arrêté. Toutefois, si vous récupérez des données à partir d’un champ et ensuite définissez ou lire la valeur d’un autre champ dans l’enregistrement actif, ADO suppose que vous avez terminé la récupération de données à partir du premier champ. Si vous appelez le **GetChunk** méthode sur le premier champ, ADO interprète l’appel en tant que nouvelle **GetChunk** opération et démarre la lecture à partir du début des données. L’accès à d’autres champs **Recordset** les objets qui ne sont pas des clones du premier **Recordset** objet n’interrompra pas **GetChunk** operations.  
  
 Si le **adFldLong** bit dans le **attributs** propriété d’un **champ** objet a la valeur **True**, que vous pouvez utiliser la **GetChunk**  (méthode) pour ce champ.  
  
 S’il n’existe aucun enregistrement actif lorsque vous utilisez la **GetChunk** ou **AppendChunk** méthode sur un **champ** de l’objet, l’erreur 3021 (aucun enregistrement actif) survient.  
  
 Pour obtenir un exemple de l’utilisation de ces méthodes pour manipuler des données binaires, consultez la [AppendChunk, méthode](../../../ado/reference/ado-api/appendchunk-method-ado.md) et [méthode GetChunk](../../../ado/reference/ado-api/getchunk-method-ado.md) exemples dans le *de référence du programmeur ADO*.
