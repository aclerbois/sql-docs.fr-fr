---
title: Effectuer une recherche incrémentielle dans un document actif | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- searches [SQL Server Management Studio], incremental
- Query Editor [SQL Server Management Studio], incremental search
- incremental searches [SQL Server Management Studio]
ms.assetid: 490bb36c-dd43-4219-9e2a-ff27046b9395
caps.latest.revision: 24
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 46a461a5d06bafffc2d8f665c367e6f7e64522e1
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="search-an-active-document-incrementally"></a>Effectuer une recherche incrémentielle dans un document actif
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Vous pouvez effectuer une recherche incrémentielle dans un document ou une fenêtre unique en entrant du texte. L'opération de recherche met en surbrillance le premier ensemble de caractères qui correspond aux caractères entrés pendant la recherche incrémentielle dans le document ou la fenêtre. La recherche incrémentielle recherche automatiquement l'ensemble du texte dans un document ou une fenêtre, à l'exception du texte caché.  
  
 Quand l’option **Respecter la casse** est activée, la recherche incrémentielle utilise les critères de votre recherche précédente. Par exemple, si vous avez effectué une recherche dans plusieurs fichiers à l’aide de la boîte de dialogue **Rechercher dans les fichiers** et que vous avez sélectionné l’option **Respecter la casse**, la prochaine recherche incrémentielle respecte la casse.  
  
### <a name="to-search-incrementally"></a>Pour effectuer une recherche incrémentielle  
  
1.  Ouvrez le fichier ou la fenêtre dans laquelle vous souhaitez effectuer la recherche.  
  
2.  Dans le menu **Edition** , pointez sur **Avancée**, puis cliquez sur **Recherche incrémentielle**.  
  
     L'icône du curseur se transforme en jumelles avec une flèche qui indique le sens de la recherche et la barre d'état affiche le texte « Recherche incrémentielle ».  
  
3.  Commencez à taper la chaîne de texte à rechercher.  
  
     La barre d'état affiche le texte que vous entrez et l'éditeur met en surbrillance la première occurrence trouvée. À mesure que vous tapez, l'éditeur progresse et surligne l'occurrence suivante. S'il ne trouve pas de chaîne identique, la barre d'état affiche le texte suivant.  
  
    ```  
    Incremental Search: <text> (not found)  
    ```  
  
 Les recherches incrémentielles sont effectuées à partir de l'emplacement courant du document, de haut en bas et de gauche à droite. Les recherches incrémentielles peuvent être effectuées à l'aide de touches de raccourci clavier.  
  
> [!NOTE]  
>  Pour obtenir la liste complète des touches de raccourci clavier, consultez [Raccourcis clavier dans SQL Server Management Studio](../../tools/sql-server-management-studio/sql-server-management-studio-keyboard-shortcuts.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Recherche et remplacement](../../relational-databases/scripting/search-and-replace.md)   
 [Effectuer une recherche de façon interactive dans des documents](../../relational-databases/scripting/search-documents-interactively.md)   
 [Effectuer une recherche dans des documents à l'aide des listes de résultats](../../relational-databases/scripting/search-documents-using-results-lists.md)   
 [Rechercher du texte avec des caractères génériques](../../relational-databases/scripting/search-text-with-wildcards.md)   
 [Rechercher du texte avec des expressions régulières](../../relational-databases/scripting/search-text-with-regular-expressions.md)  
  
  
