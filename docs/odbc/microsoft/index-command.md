---
title: Commande INDEX | Documents Microsoft
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
- index command [ODBC]
ms.assetid: 694e8cf5-2f69-4001-9c1e-b735a4da3aff
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 555cccdbeae2b70b18616b6c81a79ff84e230f4a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="index-command"></a>Commande INDEX
Crée un fichier d’index pour afficher et accéder aux enregistrements de la table dans un ordre logique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
INDEX ON eExpression TO IDXFileName | TAG TagName [OF CDXFileName]  
   [FOR lExpression]  
   [COMPACT]  
   [ASCENDING | DESCENDING]  
   [UNIQUE | CANDIDATE]  
   [ADDITIVE]  
```  
  
## <a name="arguments"></a>Arguments  
 *eExpression*  
 Spécifie une expression d’index qui peut inclure le nom d’un champ ou des champs de la table actuelle. Une clé d’index en fonction de l’expression d’index est créée dans le fichier d’index pour chaque enregistrement dans la table. Visual FoxPro utilise ces clés pour afficher et accéder à des enregistrements dans la table.  
  
> [!NOTE]  
>  Bien que déconseillée, *eExpression* peut également être une variable de la mémoire, un élément de tableau, ou un champ ou une expression de champ d’une table dans une autre zone de travail. Les champs de type Memo ne peut pas être utilisés seul dans les expressions de fichier d’index ; elles doivent être combinées avec d’autres expressions de caractères. Si vous accédez à un index qui contient une variable ou un champ qui n’est plus existe ou ne peut pas être localisé, Visual FoxPro génère un message d’erreur.  
  
 Si vous tentez de créer un index avec une clé qui varie selon la longueur, la clé sera complétée avec des espaces. Clés d’index de longueur variable ne sont pas pris en charge dans Visual FoxPro.  
  
 Il est possible de créer une clé d’index avec une longueur nulle. Par exemple, une clé d’index de longueur zéro est créée lors de l’expression d’index est une sous-chaîne d’un champ de type memo vide. Une clé d’index de longueur zéro génère un message d’erreur. Lorsque Visual FoxPro crée un index, il évalue les champs dans le premier enregistrement dans la table. Si un champ est vide, il peut être nécessaire d’entrer des données temporaires dans le champ dans le premier enregistrement pour empêcher une clé d’index de longueur 0.  
  
 POUR *IDXFileName*  
 Crée un fichier d’index .idx. Le fichier d’index est donné le .idx d’extension par défaut.  
  
 BALISE *TagName*[OF *CDXFileName*]  
 Crée un fichier d’index composé. Un fichier d’index composite est un fichier d’index unique qui se compose d’un nombre quelconque de balises distinctes (entrées d’index). Chaque balise est identifié par son nom de balise unique. Noms de balise doivent commencer par une lettre ou un trait de soulignement et peuvent être constitué de n’importe quelle combinaison de lettres, des chiffres ou des traits de soulignement jusqu'à 10. Le nombre de balises dans un fichier d’index composé est limité uniquement par la mémoire disponible et l’espace disque.  
  
 Fichiers d’index composée d’entrées multiples sont toujours compacts. Il n’est pas nécessaire d’inclure COMPACT lors de la création d’un fichier d’index composés. Noms de fichiers d’index composés sont fournis extension .cdx.  
  
 Deux types de fichiers d’index composés peuvent être créés : structurelle et structurelles.  
  
 **Indexer les fichiers composés structurelle** vous pouvez créer un fichier d’index composés structurelle dont la balise *TagName* en excluant le facultatif de *CDXFileName* clause. Un fichier d’index composés structurelle a le même nom que la table toujours et est ouvert automatiquement quand la table est ouverte.  
  
 **Fichiers d’Index composite non structurelle** vous pouvez créer un fichier d’index composite non structurelle en incluant de *CDXFileName* après la balise *TagName*. Contrairement à un index composé structurelle, un fichier d’index composite non structurelle doit être ouverte explicitement avec la clause de l’INDEX en cours d’utilisation.  
  
 Si un fichier d’index composé a déjà été créé et ouvert, l’émission d’INDEX avec la balise *TagName* ajoute une balise dans le fichier d’index composé.  
  
 POUR *lExpression*  
 Spécifie une condition selon laquelle seuls les enregistrements qui répondent à l’expression de filtre *lExpression* sont disponibles pour l’affichage et l’accès ; les clés d’index sont créés dans le fichier d’index pour que ces enregistrements correspondant à l’expression de filtre.  
  
 Visual FoxPro Rushmore technologie optimise un INDEX en cours... POUR *lExpression* commande si *lExpression* est une expression de conditions. Pour de meilleures performances, utilisez une expression de conditions dans la clause FOR.  
  
 COMPACT  
 Crée un fichier .idx compact.  
  
 L’ORDRE CROISSANT  
 Spécifie un ordre croissant pour le fichier .cdx. Par défaut, les balises .cdx sont créés dans l’ordre croissant. (Vous pouvez inclure croissant en guise de rappel de l’index ordre du fichier). Une table peut être indexée dans l’ordre inverse en incluant l’ordre décroissant.  
  
 TRI DÉCROISSANT  
 Spécifie un ordre décroissant pour le fichier .cdx. Vous ne pouvez inclure décroissant lors de la création des fichiers d’index .idx.  
  
 UNIQUE  
 Spécifie que le premier enregistrement rencontré avec une valeur de clé d’index particulier est inclus dans un fichier .idx ou une balise .cdx. UNIQUE peut être utilisé pour empêcher l’affichage d’ou d’un accès à des enregistrements en double. Tous les enregistrements ajoutés avec les clés d’index en double sont exclues à partir du fichier d’index. À l’aide de l’UNIQUE option d’INDEX est identique à l’exécution de UNIQUE définie à ON avant d’émettre des INDEX ou la RÉINDEXATION.  
  
 Lorsqu’un index UNIQUE ou une balise d’index est actif et un enregistrement en double est modifié de manière à ce que les modifications apportées à la clé d’index, l’index ou la balise de l’index est mis à jour. Toutefois, l’enregistrement suivant en double avec la clé d’index d’origine ne peut pas accessibles ou affiché jusqu'à ce que vous réindexer le fichier à l’aide de la RÉINDEXATION.  
  
 CANDIDAT  
 Crée une balise de structure d’index candidats. Le mot clé CANDIDATE peut être inclus seulement lors de la création d’une balise de structure d’index. dans le cas contraire, Visual FoxPro génère un message d’erreur.  
  
 Une balise d’index candidats empêche des valeurs en double dans le champ ou une combinaison des champs spécifiés dans l’expression d’index *eExpression*. Le terme *candidat* fait référence au type d’index ; comme index candidats empêchent les valeurs en double, ils qualifiées de « candidat » d’un index principal.  
  
 Visual FoxPro génère une erreur si vous créez une balise d’index candidats pour un champ ou une combinaison des champs qui contiennent déjà des valeurs en double.  
  
 ADDITIF  
 Conserve ouvre tous les fichiers d’index précédemment ouverts. Si vous omettez la clause additif lorsque vous créez un fichier d’index ou des fichiers pour une table avec INDEX, tous les fichiers (à l’exception de l’index composé structurelle) index précédemment ouverts sont fermés.  
  
## <a name="remarks"></a>Notes  
 Enregistrements dans une table qui comporte un fichier d’index sont affichés et accessibles dans l’ordre spécifié par l’expression d’index. L’ordre physique des enregistrements dans la table n’est pas remplacé par un fichier d’index.  
  
## <a name="index-types"></a>Types d’index  
 Visual FoxPro vous permet de créer deux types de fichiers d’index :  
  
-   Fichiers d’index .cdx contenant plusieurs entrées d’index appelées « balises » composés  
  
-   fichiers d’index .idx contenant une entrée d’index  
  
 Vous pouvez également créer un fichier index composés structurel, qui s’ouvre automatiquement avec la table.  
  
> [!NOTE]  
>  Étant donné que les fichiers d’index composés structurelle sont ouvrent automatiquement lors de l’ouverture de la table, ils sont le type d’index par défaut.  
  
 Inclure COMPACT pour créer des fichiers d’index .idx compact. Fichiers d’index composés sont toujours compacts.  
  
## <a name="index-order-and-updating"></a>Ordre d’index et la mise à jour  
 Un seul index (fichier l’index principal) ou une balise (la balise master) contrôle l’ordre dans lequel la table est affichée ou accessible. Certaines commandes (recherche, par exemple) utilisent le fichier d’index principale ou la balise pour rechercher des enregistrements. Toutefois, tous les ouvrez .idx et fichiers d’index .cdx sont mis à jour lorsque des modifications sont apportées à la table.  
  
## <a name="user-defined-functions"></a>Fonctions définies par l'utilisateur  
 Bien qu’une expression d’index permettre contenir une fonction définie par l’utilisateur, vous ne devez pas utiliser les fonctions définies par l’utilisateur dans une expression d’index. Fonctions définies par l’utilisateur dans une expression d’index augmentent le temps que nécessaire pour créer ou mettre à jour l’index. En outre, les mises à jour de l’index ne soient pas exécutées lorsqu’une fonction définie par l’utilisateur est utilisée pour une expression d’index.  
  
 Si vous utilisez une fonction définie par l’utilisateur dans une expression d’index, Visual FoxPro doit être en mesure de trouver la fonction définie par l’utilisateur. Lorsque Visual FoxPro crée un index, l’expression d’index est enregistrée dans le fichier d’index, mais uniquement une référence à la fonction définie par l’utilisateur est incluse dans l’expression d’index.  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER TABLE - commande SQL](../../odbc/microsoft/alter-table-sql-command.md)   
 [SUPPRIMER des commandes de balise](../../odbc/microsoft/delete-tag-command.md)   
 [Commande SET COLLATE](../../odbc/microsoft/set-collate-command.md)   
 [SET UNIQUE, commande](../../odbc/microsoft/set-unique-command.md)
