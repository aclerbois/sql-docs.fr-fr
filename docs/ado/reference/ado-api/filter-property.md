---
title: Propriété filtre | Documents Microsoft
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
- Recordset15::Filter
helpviewer_keywords:
- Filter property
ms.assetid: 80263a7a-5d21-45d1-84fc-34b7a9be4c22
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 9dc176d7c64d1845ddb863cd58fd41313967ccce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="filter-property"></a>Filter, propriété
Indique un filtre pour les données dans un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md).  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour

Définit ou retourne un **Variant** valeur, qui peut contenir un des éléments suivants :  
  
-   **Chaîne de critères :** une chaîne composée d’une ou plusieurs clauses individuelles concaténées avec **AND** ou **OR** opérateurs.  
  
-   **Tableau de signets :** un tableau de signet unique des valeurs qui pointent vers les enregistrements dans la **Recordset** objet.  
  
-   A [FilterGroupEnum](../../../ado/reference/ado-api/filtergroupenum.md) valeur.  
  
## <a name="remarks"></a>Notes

Utilisez le **filtre** propriété à filtrer les enregistrements dans une **Recordset** objet. La liste filtrée **Recordset** devient le curseur actuel. Autres propriétés qui retournent des valeurs basées sur actuel **curseur** sont affectés, tels que [propriété AbsolutePosition (ADO)](../../../ado/reference/ado-api/absoluteposition-property-ado.md), [propriété AbsolutePage (ADO)](../../../ado/reference/ado-api/absolutepage-property-ado.md), [ RecordCount, propriété (ADO)](../../../ado/reference/ado-api/recordcount-property-ado.md), et [PageCount, propriété (ADO)](../../../ado/reference/ado-api/pagecount-property-ado.md). Définition de la **filtre** propriété à une valeur spécifique déplace l’enregistrement actif vers le premier enregistrement qui satisfait à la nouvelle valeur.
  
La chaîne de critères est composée de clauses sous la forme *NomChamp-Opérateur-valeur* (par exemple, `"LastName = 'Smith'"`). Vous pouvez créer des clauses composées en concaténant des clauses avec **AND** (par exemple, `"LastName = 'Smith' AND FirstName = 'John'"`) ou **OR** (par exemple, `"LastName = 'Smith' OR LastName = 'Jones'"`). Pour les chaînes de critères, utilisez les instructions suivantes :

-   *FieldName* doit être un nom de champ valide à partir de la **Recordset**. Si le nom du champ contient des espaces, vous devez placer le nom entre crochets.  
  
-   Opérateur doit être une des valeurs suivantes : \<, >, \<=, > =, <>, =, ou **comme**.  
  
-   Valeur est la valeur à laquelle vous comparez les valeurs de champ (par exemple, « Smith », n° 8/24/95 #, 12,345 ou $50,00). Utilisez des guillemets simples avec les chaînes et les signes dièse (##) avec des dates. Pour les nombres, vous pouvez utiliser le point décimal, signes dollar et la notation scientifique. Si l’opérateur est **comme**, valeur peut utiliser des caractères génériques. Uniquement l’astérisque (*) et signe de pourcentage (%) les caractères génériques sont autorisés, et ils doivent être le dernier caractère de la chaîne. Valeur ne peut pas être null.  
  
> [!NOTE]
>  Pour inclure des guillemets simples (') dans le filtre de valeur, utilisez deux guillemets simples pour représenter un. Par exemple, pour filtrer sur Malley, la chaîne de critères doit être `"col1 = 'O''Malley'"`. Pour inclure des guillemets simples au début et à la fin de la valeur de filtre, délimitez la chaîne avec les signes dièse (#). Par exemple, pour filtrer sur '1', la chaîne de critères doit être `"col1 = #'1'#"`.  
  
-   Il n’existe aucun ordre de priorité et et ou. Clauses peuvent être regroupées dans les parenthèses. Toutefois, vous ne peut pas regrouper les clauses liées par OR et joignez ensuite le groupe à une autre clause avec AND, comme dans l’extrait de code suivant :  
 `(LastName = 'Smith' OR LastName = 'Jones') AND FirstName = 'John'`  
  
-   Au lieu de cela, vous devez construire ce filtre en tant que  
 `(LastName = 'Smith' AND FirstName = 'John') OR (LastName = 'Jones' AND FirstName = 'John')`  
  
-   Dans un **comme** clause, vous pouvez utiliser un caractère générique au début et à la fin du modèle. Par exemple, vous pouvez utiliser `LastName Like '*mit*'`. Ou avec **comme** vous pouvez utiliser un caractère générique uniquement à la fin du modèle. Par exemple, `LastName Like 'Smit*'`.  
  
 Les constantes de filtre facilitent la résolution des conflits d’enregistrements individuels lors des mises à jour par lot en vous permettant d’afficher, par exemple, uniquement les enregistrements qui ont été affectés pendant la dernière [méthode UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) appel de méthode.  
  
Définition de la **filtre** propriété proprement dite peut échouer en raison d’un conflit avec les données sous-jacentes. Par exemple, il peut se produire lorsqu’un enregistrement a déjà été supprimé par un autre utilisateur. Dans ce cas, le fournisseur retourne des avertissements dans le [Collection d’erreurs (ADO)](../../../ado/reference/ado-api/errors-collection-ado.md) collection, mais n’interrompt ne pas l’exécution du programme. Une erreur au moment de l’exécution se produit uniquement en cas de conflit sur tous les enregistrements demandés. Utilisez le [Status, propriété (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/status-property-ado-recordset.md) propriété pour localiser les enregistrements en conflit.  
  
Définissant le **filtre** propriété dans une chaîne de longueur nulle (« ») a le même effet que l’utilisation de la **adFilterNone** constante.
  
Chaque fois que le **filtre** propriété est définie, la position actuelle se déplace vers le premier enregistrement dans le sous-ensemble d’enregistrements dans la **Recordset**. De même, lorsque le **filtre** propriété est désactivée, la position actuelle se déplace vers le premier enregistrement dans le **Recordset**.

Supposons qu’un **Recordset** est filtrée selon un champ d’un type variant, telles que le type sql_variant. Une erreur (DISP_E_TYPEMISMATCH ou 80020005) se produit lorsque les sous-types des valeurs de champ et de filtre utilisés dans la chaîne de critères ne correspondent pas. Par exemple, supposons que :

- A **Recordset** objet (rs) contienne une colonne (C) du type sql_variant.
- Et une valeur de 1 de type I4 a été assigné à un champ de cette colonne. La chaîne de critères est définie `rs.Filter = "C='A'"` sur le champ.

Cette configuration génère l’erreur pendant l’exécution. Toutefois, `rs.Filter = "C=2"` appliquée sur le même champ ne produira pas une erreur. Et le champ est filtrage dans le jeu d’enregistrements actuel.

Consultez le [signet propriété (ADO)](../../../ado/reference/ado-api/bookmark-property-ado.md) propriété pour obtenir une explication des valeurs de signet à partir de laquelle vous pouvez créer un tableau à utiliser avec la propriété du filtre.

Seuls les filtres sous la forme de chaînes de critères affectent le contenu d’un rendu persistant **Recordset**. Un exemple d’une chaîne de critères est `OrderDate > '12/31/1999'`. Les filtres créés avec un tableau de signets, ou à l’aide d’une valeur à partir de la **FilterGroupEnum**, n’affectent pas le contenu du rendu persistant **Recordset**. Ces règles s’appliquent aux jeux d’enregistrements créés avec des curseurs côté client ou côté serveur.
  
> [!NOTE]
>  Lorsque vous appliquez l’indicateur adFilterPendingRecords à un filtré et modifié **Recordset** dans le mode de mise à jour par lot, résultant **Recordset** est vide si le filtrage est basé sur le champ clé d’un table à clé unique et la modification a été effectuée sur les valeurs de champ clé. Résultant **Recordset** sera vide si une des affirmations suivantes est vraie :  
  
-   Le filtrage était basé sur des champs dans une table à clé unique.  
  
-   Le filtrage était basé sur tous les champs dans une table à clés multiples.  
  
-   Les modifications ont été effectuées sur des champs dans une table à clé unique.  
  
-   Les modifications apportées sur tous les champs dans une table à clés multiples.  
  
Le tableau suivant récapitule les effets de **adFilterPendingRecords** dans différentes combinaisons de filtrage et de modifications. La colonne de gauche indique les modifications possibles. Modifications peuvent être effectuées sur un des champs non-clés, sur le champ clé dans une table à clé unique ou sur un des champs clés dans une table à clés multiples. La ligne du haut affiche le critère de filtrage. Le filtrage peut être basé sur tous les champs non-clés, le champ de clé dans une table à clé unique ou l’un des champs clés dans une table à clés multiples. Les cellules qui montrent les résultats. A **+** signe signifie que l’application **adFilterPendingRecords** entraîne un vide **Recordset**. A **-** signe signifie vide **Recordset**.  
  
||Non clés|Clé unique|Plusieurs clés|
|-|--------------|----------------|-------------------|
|**Non clés**|+|+|+|
|**Clé unique**|+|-|Néant|
|**Plusieurs clés**|+|Néant|+|
|||||
  
## <a name="applies-to"></a>S'applique à

[Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi

[Filter et RecordCount propriétés exemple (VB)](../../../ado/reference/ado-api/filter-and-recordcount-properties-example-vb.md)
[Filter et RecordCount, propriétés-exemple (VC ++)](../../../ado/reference/ado-api/filter-and-recordcount-properties-example-vc.md)
[Clear, méthode (ADO)](../../../ado/reference/ado-api/clear-method-ado.md) 
 [Optimiser la propriété dynamique (ADO)](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md)
