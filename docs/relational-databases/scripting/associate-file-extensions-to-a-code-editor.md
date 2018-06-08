---
title: Associer des extensions de fichier à un éditeur de code | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- file extensions [SQL Server]
- associating file extensions [SQL Server]
- Query Editor [SQL Server Management Studio], associating file extensions
ms.assetid: 193630f4-93de-4950-8f36-68702531f925
caps.latest.revision: 23
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e3034bf9884f89a9dc0ba9bc456c789ab40f180d
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="associate-file-extensions-to-a-code-editor"></a>Associer des extensions de fichier à un éditeur de code
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  L'association d'extensions de fichier à un éditeur de code spécifique permet d'ouvrir un fichier avec l'éditeur [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] adapté lorsque vous double-cliquez sur un fichier dans l'Explorateur Windows. Les extensions couramment utilisées par [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], par exemple .sql and .mdx,  sont associées lors de l'installation. Les nouvelles extensions doivent également être associées à [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] dans le système de fichiers. Vous pouvez utiliser cette fonctionnalité pour ouvrir des fichiers créés avec d'autres éditeurs. Vous pouvez également ouvrir des fichiers que vous avez renommés (ex. sauvegardes de fichiers .sql renommés avec l'extension .bak).  
  
 Cette procédure se déroule en deux étapes. Associez d'abord l'extension avec [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]et associez-la ensuite à un éditeur de code spécifique.  
  
### <a name="to-associate-a-new-file-extension-with-sql-server-management-studio"></a>Pour associer une nouvelle extension à SQL Server Management Studio  
  
1.  Dans le menu **Démarrer** , pointez sur **Tous les programmes**, puis sur **Accessoires**. Cliquez ensuite sur **Explorateur Windows**.  
  
2.  Dans le menu **Outils** de l’Explorateur Windows, cliquez sur **Options des dossiers**.  
  
3.  Dans la boîte de dialogue **Options des dossiers** , cliquez sur l’onglet **Types de fichiers** , puis sur **Nouveau**.  
  
4.  Dans la zone **Extension du fichier** de la boîte de dialogue **Créer une nouvelle extension** , tapez la nouvelle extension que vous voulez associer. Cliquez ensuite sur **OK**. Ne placez pas un point devant l'extension.  
  
5.  Dans la zone **Types de fichiers enregistrés** , cliquez sur la nouvelle extension, puis sur **Modifier**.  
  
6.  Dans la boîte de dialogue **Ouvrir avec** , cliquez sur **SSMS – SQL Server Management Studio**, puis sur **OK**.  
  
7.  Cliquez sur **OK** pour fermer la boîte de dialogue **Options des dossiers** , puis fermez l’Explorateur Windows.  
  
### <a name="to-associate-a-new-file-extension-with-a-code-editor-in-sql-server-management-studio"></a>Pour associer une nouvelle extension à un éditeur de code dans SQL Server Management Studio  
  
1.  Dans le menu [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]Outils **de** , cliquez sur **Options**.  
  
2.  Dans la boîte de dialogue **Options** , cliquez sur **Éditeur de texte**, puis sur **Extension de fichier**.  
  
3.  Dans la zone **Extension** , tapez la nouvelle extension de fichier.  
  
4.  Dans la zone **Éditeur** , cliquez sur l’éditeur de code que vous voulez utiliser pour ouvrir ce type de fichier. Cliquez sur **Ajouter**, puis sur **OK**.  
  
## <a name="see-also"></a> Voir aussi  
 [Utilitaire Ssms](../../tools/sql-server-management-studio/ssms-utility.md)  
  
  
