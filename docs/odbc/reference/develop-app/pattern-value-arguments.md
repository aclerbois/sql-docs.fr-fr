---
title: Arguments de la valeur de modèle | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- catalog functions [ODBC], arguments
- arguments in catalog functions [ODBC], pattern value
- pattern value arguments [ODBC]
ms.assetid: 1d3f0ea6-87af-4836-807f-955e7df2b5df
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5befe078127bb526f4fe9a103ec823f2142039f1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="pattern-value-arguments"></a>Arguments de valeur de modèle
Certains arguments dans le catalogue des fonctions, telles que la *TableName* argument dans **SQLTables**, accepte les modèles de recherche. Ces arguments accepte les modèles de recherche si l’attribut d’instruction SQL_ATTR_METADATA_ID a la valeur SQL_FALSE ; ils représentent les arguments d’identificateur qui n’acceptent pas d’un modèle de recherche si cet attribut a la valeur SQL_TRUE.  
  
 Les caractères de modèle de recherche sont :  
  
-   Un trait de soulignement (_), qui représente n’importe quel caractère unique.  
  
-   Un signe de pourcentage (%), qui représente n’importe quelle séquence de zéro ou plusieurs caractères.  
  
-   Un caractère d’échappement, qui est spécifique au pilote et est utilisé pour inclure des traits de soulignement, des signes de pourcentage, et le caractère d’échappement en tant que littéraux. Si le caractère d’échappement précède un caractère non spéciale, le caractère d’échappement n’a aucune signification particulière. Si le caractère d’échappement précède un caractère spécial, il remplace le caractère spécial. Par exemple, « \a » sont considérés comme deux caractères, «\\» et « a », mais «\\% » est considérée comme non spéciale caractère « % ».  
  
 Le caractère d’échappement est récupéré avec l’option SQL_SEARCH_PATTERN_ESCAPE dans **SQLGetInfo**. Il doit précéder n’importe quel trait de soulignement, un signe de pourcentage ou un caractère d’échappement dans un argument qui accepte les modèles de recherche pour inclure ce caractère en tant que littéral. Exemples sont présentés dans le tableau suivant.  
  
|Modèle de recherche| Description|  
|--------------------|-----------------|  
|% DE %A|Tous les identificateurs contenant la lettre A|  
|ABC_|Tous les identificateurs de quatre caractères commençant par ABC|  
|ABC\\_|L’identificateur ABC_, en supposant que le caractère d’échappement est une barre oblique inverse (\\)|  
|\\\\%|Tous les identificateurs en commençant par une barre oblique inverse (\\), en supposant que le caractère d’échappement est une barre oblique inverse|  
  
 Une attention particulière doit être prise pour échapper les caractères de modèle de recherche dans les arguments qui acceptent des modèles de recherche. Cela est particulièrement vrai pour le caractère de soulignement, qui est couramment utilisé dans les identificateurs. Une erreur courante dans les applications consiste à récupérer une valeur d’une fonction de catalogue et passer cette valeur à un argument de modèle de recherche dans une autre fonction de catalogue. Par exemple, une application récupère le nom de table MY_TABLE à partir du résultat défini dans **SQLTables** et le transmet à **SQLColumns** pour récupérer une liste de colonnes dans MY_TABLE. Au lieu d’obtenir les colonnes pour MY_TABLE, l’application obtiendra les colonnes pour toutes les tables qui correspondent au modèle de recherche MY_TABLE, par exemple MY_TABLE, MY1TABLE, MY2TABLE et ainsi de suite.  
  
> [!NOTE]  
>  ODBC 2. *x* pilotes ne prennent pas en charge les modèles de recherche dans les *CatalogName* argument dans **SQLTables**. ODBC 3 *.x* pilotes accepte les modèles de recherche dans cet argument si l’attribut d’environnement le paramètre version_odbc SQL_ATTR_ est défini à SQL_OV_ODBC3 ; ils n’acceptent pas les modèles de recherche dans cet argument si elle est définie sur SQL_OV_ODBC2.  
  
 En passant un pointeur null à un argument de modèle de recherche ne limite pas la recherche pour cet argument ; Autrement dit, un pointeur null et que le pourcentage de modèle de recherche (caractères) sont équivalentes. Cependant, une longueur nulle rechercher le modèle : autrement dit, un pointeur valide vers une chaîne de longueur zéro — correspond uniquement à la chaîne vide (« »).
