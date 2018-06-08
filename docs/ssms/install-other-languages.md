---
title: Installer des versions de SQL Server Management Studio (SSMS) dans d’autres langues que l’anglais | Microsoft Docs
description: Installer des versions de SQL Server Management Studio (SSMS) dans d’autres langues que l’anglais
ms.custom: ''
ms.date: 12/08/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: ''
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 74cda5bc234b468e89d70b5289a5a70478328b11
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="install-non-english-language-versions-of-sql-server-management-studio-ssms"></a>Installer des versions de SQL Server Management Studio (SSMS) dans d’autres langues que l’anglais 

[SSMS est disponible dans plusieurs langues](download-sql-server-management-studio-ssms.md#available-languages), mais le programme d’installation de SSMS bloque l’installation sur les ordinateurs dont les paramètres régionaux système ne correspondent pas à la langue de SSMS. 

Les instructions suivantes dépendent de la version de Windows. Celles qui sont présentées ici valent pour Windows 10.

## <a name="install-non-english-ssms-on-a-computer-running-an-english-operating-system-os"></a>Installer SSMS dans une autre langue que l’anglais sur un ordinateur doté d’un système d’exploitation (OS) en anglais

1. Installez le module linguistique de Windows associé à la langue que SSMS devra utiliser : 
   - **Paramètres** > **Heure et langue** > **Région et langue** > **Ajouter une langue** 
2. Définissez maintenant les paramètres régionaux système afin d’utiliser le module linguistique installé à l’étape précédente en cliquant sur la langue que vous venez d’installer, puis sélectionnez **Définir comme valeur par défaut**. (Après l’installation de SSMS, vous pourrez sélectionner à nouveau l’anglais dans les paramètres régionaux système.)
3. Lorsque votre système d’exploitation fonctionne dans la langue souhaitée, [installez la version SSMS dans cette langue](download-sql-server-management-studio-ssms.md#available-languages). La première fois que vous installez une nouvelle langue SSMS, utilisez le package complet. Vous pourrez utiliser le package de mise à niveau pour les installations suivantes.
4. Exécutez SSMS ; il devrait s’afficher dans la langue installée à l’étape précédente.
5. Sélectionnez à nouveau l’anglais dans les paramètres régionaux système de votre ordinateur.

## <a name="install-ssms-in-a-language-other-than-the-language-of-the-installed-os"></a>Installer SSMS dans une langue autre que celle du système d’exploitation installé

1. Installez le module linguistique de Windows associé à la langue que SSMS devra utiliser : 
   - **Paramètres** > **Heure et langue** > **Région et langue** > **Ajouter une langue** 
2. Définissez maintenant les paramètres régionaux système afin d’utiliser le module linguistique installé à l’étape précédente en cliquant sur la langue que vous venez d’installer, puis sélectionnez **Définir comme valeur par défaut**. 
3. Lorsque votre système d’exploitation fonctionne dans la langue souhaitée, [installez la version SSMS dans cette langue](download-sql-server-management-studio-ssms.md#available-languages). La première fois que vous installez une nouvelle langue SSMS, utilisez le package complet. Vous pourrez utiliser le package de mise à niveau pour les installations suivantes.
4. Pour chaque langue à installer qui ne correspond pas à la langue de la première version de SSMS installée, installez le module linguistique Visual Studio 2015 Shell (isolé) correspondant :
   - Accédez à [ https://connect.microsoft.com/VisualStudio/ExtendVS ](https://connect.microsoft.com/VisualStudio/ExtendVS) (vous devrez peut-être vous connecter et effectuer la procédure de *connexion d’inscription*).
   - Téléchargez le module linguistique Visual Studio 2015 Shell (isolé) souhaité, et installez-le.

   > [!IMPORTANT]
   > Suivez les étapes précédentes pour installer le module linguistique Visual Studio 2015 Shell isolé ; n’utilisez pas le lien **Obtenir des langues supplémentaires** qui se trouve sur **Outils** | **Options**  |  **Paramètres internationaux**. 

5. Exécutez SSMS et sélectionnez la langue que vous souhaitez utiliser dans :
   - **Outils** | **Options** | **Paramètres internationaux**
1. Fermez et redémarrez SSMS.

## <a name="next-steps"></a>Étapes suivantes

- [Didacticiel : SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/tutorials/tutorial-sql-server-management-studio)