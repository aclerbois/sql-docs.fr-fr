---
title: Historique de ADO | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ADO, what's new
ms.assetid: 667673f2-3151-432b-894a-3fc60b704ea4
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cc6116c80af9ae7ca6a6504a1caa2d912b0d3731
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-features-for-each-release"></a>Fonctionnalités d’ADO pour chaque version
Cette rubrique répertorie les nouvelles fonctionnalités introduites par chaque version de ADO, ADO MD et ADOX.

## <a name="ado-60"></a>ADO 6.0
 ADO 6.0 est inclus dans Windows Vista, dans le cadre de Windows Data Access Components (Windows DAC) 6.0. ADO 6.0 est fonctionnellement équivalente à ADO 2.8.

## <a name="ado-28"></a>ADO 2.8
 ADO 2.8 a été inclus dans Windows XP et Windows Server 2003, dans le cadre de la 2.8 Microsoft Data Access composants (MDAC). Une version redistribuable de MDAC 2.8 est également disponible ; Notez que cette version redistribuable doit uniquement être installée sur Windows 2000. ADO 2.8 résout plusieurs problèmes liés à la sécurité :

 *Accès de disque dur n’est pas autorisé en dehors d’une zone de confiance.*
Dans le script impliquant des sites non approuvés entre domaines, les opérations suivantes sont désactivées : **Stream.SaveToFile**, **Stream.LoadFromFile**, **Recordset.Save**, et **Recordset.Open**, utilisé conjointement avec la **adCmdFile** indicateur ou avec le fournisseur Microsoft OLE DB persistance (MSPersist).

 **Recordset.Open** *,***Recordset.Save** *,***Stream.SaveToFile** *, et* **Stream.LoadFromFile***ne fonctionne pas sur les fichiers physiques.*
Ces méthodes maintenant vérifier que les descripteurs de fichiers pointent vers les fichiers physiques.

 **Recordset.ActiveCommand***renvoie une erreur lorsqu’elle est appelée à partir d’une page HTML/ASP.*
Cela empêche le **commande** objet à partir de l’utilisation abusive.

 *Le nombre de***jeux d’enregistrements***retourné par une liste imbriquée***forme***command dispose d’une limite supérieure.*
Une commande imbriquée shape retourne maintenant un maximum de 512 **jeux d’enregistrements**. Cela signifie qu’un **forme** n’est plus possible d’imbriquer des commandes à toute profondeur. Au lieu de cela, la profondeur maximale de niveau est 512, si chaque commande se traduit par un seul (enfant) **Recordset**. If, n’importe quel niveau, un **forme** commande retourne plusieurs **jeux d’enregistrements**, le niveau maximal de profondeur sera inférieure à 512.

## <a name="ado-27"></a>ADO 2.7
 *prise en charge de la plateforme 64 bits* ADO 2.7 introduit la prise en charge des processeurs 64 bits.

## <a name="ado-26"></a>ADO 2.6
 **CubDef.GetSchemaObject***méthode* à partir de ADO 2.6, objets ADO MD peuvent être récupérées à l’aide des noms uniques, comme spécifié par le [UniqueName, propriété (ADO MD)](../../ado/reference/ado-md-api/uniquename-property-ado-md.md). Les noms des objets parents n’avez pas besoin de connaître et collections de parent n’avez pas besoin d’être rempli pour récupérer un objet de schéma. Consultez [GetSchemaObject, méthode (ADO MD)](../../ado/reference/ado-md-api/getschemaobject-method-ado-md.md).

 *Flux de commandes* le **commande** objet prend en charge des commandes dans un format de flux de données en guise d’alternative à l’utilisation de la **CommandText** propriété. Le [CommandStream, propriété (ADO)](../../ado/reference/ado-api/commandstream-property-ado.md) peut être utilisé pour spécifier les modèles XML ou des programmes en tant que le **commande** d’entrée avec le fournisseur Microsoft OLE DB pour SQL Server.

 **Dialecte***propriété* [dialecte](../../ado/reference/ado-api/dialect-property.md) est une propriété qui définit la syntaxe et les règles générales que le fournisseur utilise pour analyser la chaîne ou le flux.

 **Command.Execute***méthode* le [exécuter la méthode](../../ado/reference/ado-api/execute-method-ado-command.md) de ADO **commande** objet a été amélioré pour utiliser des flux d’entrée et de sortie.

 *Champ statusvalues* si l’utilisateur rencontre une erreur DB_E_ERRORSOCCURRED lorsque vous modifiez un **champ** d’un **Recordset**, ADO remplira maintenant la **Field.Status**propriété avec les informations d’état approprié afin que l’utilisateur aura plus d’informations sur la cause du problème. Consultez [Status, propriété (champ ADO)](../../ado/reference/ado-api/status-property-ado-field.md).

 **NamedParameters***propriété* [NamedParameters](../../ado/reference/ado-api/namedparameters-property-ado.md) une nouvelle propriété de la **commande** nommé de l’objet qui indique que le fournisseur doit utiliser. paramètres.

 *Jeux de résultats dans des flux* ADO peut retourner des jeux de résultats à partir d’une source de données dans un **flux**, plutôt qu’un **Recordset** objet. À l’aide de la version la plus récente du fournisseur Microsoft OLE DB pour SQL Server, vous pouvez obtenir des résultats XML à partir du fournisseur en exécutant une requête « Pour le XML ». A **flux** qui reçoit le jeu de résultats peut être ouvert avec une commande « Pour XML » comme source. Consultez [la récupération des jeux de résultats en flux](../../ado/guide/data/retrieving-resultsets-into-streams.md).

 *Jeu de résultats de ligne unique* ADO le **enregistrement** objet peut maintenant être ouverts sur une chaîne de commande ou **commande** objet qui retourne une ligne de données à partir du fournisseur. Cela entraîne de meilleures performances avec les fournisseurs de MDAC 2.6. Consultez [Open (méthode) (enregistrement ADO)](../../ado/reference/ado-api/open-method-ado-record.md).

## <a name="ado-25"></a>ADO 2.5
 **Enregistrement** *objet* ADO 2.5 présente le **enregistrement** objet pour représenter et gérer une ligne d’un **Recordset** ou un fournisseur de données, ou un objet en encapsulant un des données semi-structurées, par exemple un fichier ou répertoire.

 **Flux** *objet* ADO 2.5 présente également la **flux** objet pour représenter un flux de données binaire ou texte.

 *La liaison d’URL* ADO 2.5 présente l’utilisation d’une URL, au lieu d’un texte à chaîne et la commande de connexion, pour nommer des objets de magasin de données. Une URL peut être utilisée avec le **connexion** et **Recordset** objets, ainsi qu’avec la nouvelle **enregistrement** et **flux** objets.

 *Fournisseurs de données prenant en charge la liaison d’URL* ADO 2.5 prend en charge les fournisseurs OLE DB qui reconnaissent les schémas d’URL. Cela inclut le fournisseur OLE DB pour la publication Internet qui accède au système de fichiers Windows 2000 et identifie le schéma HTTP existant.
