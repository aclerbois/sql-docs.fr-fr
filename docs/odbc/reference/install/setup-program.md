---
title: Programme d’installation | Documents Microsoft
ms.custom: ''
ms.date: 08/31/2016
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- installing ODBC components [ODBC], setup program
ms.assetid: 9cc5d75d-b293-41e5-927c-10f4af2e7af1
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 175991bd2aafe25f25848b1476d07c9e2e1e29b5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setup-program"></a>Programme d’installation
> **Remarque :** en commençant par Windows XP et Windows Server 2003, **ODBC est inclus dans le système d’exploitation Windows**. Vous devez explicitement uniquement installer ODBC dans les versions antérieures de Windows.  
  
 L’utilisateur exécute le programme d’installation pour démarrer le processus d’installation. Le programme d’installation est écrit par le développeur de l’application ou le pilote. Outre l’installation des composants ODBC, il peut installer d’autres logiciels. Par exemple, les développeurs d’applications peuvent utiliser le même programme d’installation pour installer les composants ODBC et installer leurs applications.  
  
 Les développeurs peuvent écrire à partir de zéro, le programme d’installation à l’aide de l’utilitaire d’installation de Microsoft® Windows® SDK ou logiciel d’installation à partir d’autres fournisseurs. Ainsi, les développeurs un contrôle complet sur l’apparence du programme d’installation. Le programme d’installation peut être écrits pour installer des logiciels supplémentaires, par exemple, une application ODBC. Pour plus d’informations sur les utilitaires du programme d’installation du Kit de développement logiciel Windows, consultez la documentation du Kit de développement logiciel Windows.  
  
 La quantité de l’installation s’effectue par le programme d’installation dépend de ce que les appels dans le programme d’installation DLL de fonctions. Le programme d’installation DLL contient des fonctions pour l’installation de composants ODBC. Le programme d’installation appelle simplement **SQLInstallDriverManager**, **SQLInstallDriverEx**, ou **SQLInstallTranslatorEx** dans le programme d’installation de la DLL pour récupérer le chemin d’accès du répertoire dans lequel le composant à installer et à ajouter des informations sur le composant dans le Registre. Ces fonctions ne copient pas réellement les fichiers ; le programme d’installation pour cela en utilisant les informations dans les arguments de ces fonctions.  
  
 Le programme d’installation DLL contient également des fonctions pour supprimer des composants ODBC. Le programme d’installation appelle **SQLRemoveDriverManager**, **SQLRemoveDriver**, ou **SQLRemoveTranslator** dans le programme d’installation DLL pour décrémenter le décompte d’utilisation d’un composant dans le Registre et, si le nouveau nombre de l’utilisation du composant tombe à 0, supprimez toutes les informations sur le composant à partir du Registre. Ces fonctions ne suppriment pas réellement les fichiers pour le composant ; le programme d’installation pour cela, si le décompte d’utilisation se situe à 0.
