---
title: Fichiers divers | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-solutions
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- files [SQL Server Management Studio], miscellaneous
- projects [SQL Server Management Studio], files
- solutions [SQL Server Management Studio], files
- miscellaneous files folder [SQL Server]
ms.assetid: 3c952b0b-8f5f-4d86-9e5d-616c10b9df0d
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 842d34729bef0ddf0fac8bd2ffa47b537c096e6c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="miscellaneous-files"></a>Fichiers divers
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Les fichiers externes à un projet sont appelés *fichiers divers*. Lorsqu'une solution est ouverte, vous pouvez ouvrir et modifier les fichiers divers associés au projet. Un fichier est considéré comme un fichier divers si son extension n'est pas associée à l'éditeur de code du projet. Par exemple, dans les projets de script [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , les fichiers dotés de l’extension .txt ou .mdx sont traités comme des fichiers divers. Dans un projet MDX, les fichiers dotés de l'extension .txt ou .sql sont également traités comme des fichiers divers. Pour associer une extension de fichier à un éditeur de code, consultez [Procédure : associer des extensions de fichier à un éditeur de code](http://msdn.microsoft.com/en-us/193630f4-93de-4950-8f36-68702531f925).  
  
Il est utile de pouvoir ajouter des fichiers divers à un projet pour plusieurs raisons. Vous pouvez avoir un fichier qui n'est pas nécessairement un script reconnu mais qui est indispensable au développement de la solution, tel que des instructions ou conseils de développement, des fichiers de données ou des clips de code.  
  
Les fichiers divers offrent une certaine souplesse. Supposons, par exemple, que votre projet de script [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] contient plusieurs scripts pour la création de tables et de procédures stockées dans votre base de données. Vous disposez également de plusieurs fichiers de données pour les tables (dotés de l'extension de fichier .BCP) et d'instructions d'exécution dans un fichier LISEZMOI. Vous pouvez alors attacher les fichiers de données et le fichier LISEZMOI en tant que fichiers divers au projet pour tirer parti du contrôle de code source et d'autres fonctionnalités du système de projet.  
  
[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)] Les menus et les barres d’outils changent selon le format du fichier que vous ouvrez. Lorsque vous ouvrez un fichier texte, par exemple, la barre d'outils Éditeur de texte s'affiche. Si vous ouvrez un schéma XML, la barre d'outils correspondante s'affiche. Lorsque vous modifiez le schéma XML, la barre d'outils Éditeur de texte n'est pas disponible. Lorsque vous passez d'un fichier de projet à un fichier divers, toutes les commandes et les barres d'outils associées au projet disparaissent et seules celles directement associées au fichier divers s'affichent.  
  
## <a name="see-also"></a> Voir aussi  
[Fichiers gérant les solutions et les projets](../../ssms/solution/files-that-manage-solutions-and-projects.md)  
[Solutions &#40;SQL Server Management Studio&#41;](../../ssms/solution/solutions-sql-server-management-studio.md)  
[Projets &#40;SQL Server Management Studio&#41;](../../ssms/solution/projects-sql-server-management-studio.md)  
  
