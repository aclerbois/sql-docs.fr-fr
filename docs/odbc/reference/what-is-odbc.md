---
title: Nouveautés d’ODBC ? | Microsoft Docs
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
- ODBC [ODBC], about ODBC
ms.assetid: badf3a45-f941-44ae-a31d-393116f68a18
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: eed93c1d5b096e132f6d514057abd73c090519c5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="what-is-odbc"></a>Nouveautés d’ODBC ?
Nombreuses idées sur ODBC existent dans le monde informatique. À l’utilisateur final, il est une icône dans le panneau de configuration Microsoft® Windows®. Pour les programmeurs d’applications, il est une bibliothèque contenant des routines d’accès aux données. Beaucoup d’autres, il est la réponse à tous les problèmes d’accès de base de données pensiez.  
  
 Tout d’abord, ODBC est une spécification d’une API de base de données. Cette API méthode est indépendante de tout un SGBD ou du système d’exploitation ; Bien que ce manuel utilise C, l’API ODBC est indépendant du langage. L’API ODBC est basée sur les spécifications CLI à partir d’Open Group et de la norme ISO/IEC. ODBC 3. *x* implémente entièrement les deux de ces spécifications : les versions antérieures d’ODBC étaient basées sur des versions préliminaires de ces spécifications, mais s’est pas entièrement les implémenter et ajoute des fonctionnalités fréquemment utilisées par les développeurs d’applications de base de données basée sur l’écran, tels que les curseurs de défilement.  
  
 Les fonctions de l’API ODBC sont implémentées par les développeurs de pilotes spécifiques au SGBD. Applications appellent les fonctions dans ces pilotes pour accéder aux données d’une manière indépendante de SGBD. Un gestionnaire de pilote gère la communication entre les applications et des pilotes.  
  
 Bien que Microsoft fournit un gestionnaire de pilotes pour les ordinateurs exécutant Microsoft Windows® 95 et versions ultérieures, a écrit plusieurs pilotes ODBC et appelle les fonctions ODBC à partir de certaines de ses applications, tout le monde peut écrire des pilotes et des applications ODBC. En fait, la grande majorité des applications ODBC et des pilotes disponibles aujourd'hui sont écrites par des sociétés que Microsoft. En outre, les applications et les pilotes ODBC existent sur le Macintosh® et diverses plateformes UNIX.  
  
 Pour aider les développeurs d’applications et de pilotes, Microsoft propose un Kit de développement de logiciel (SDK) ODBC pour les ordinateurs exécutant Windows 95 et versions ultérieures qui fournit le Gestionnaire de pilotes, DLL, outils de test et installer exemples d’applications. Microsoft a associées à des logiciels Visigenic pour déplacer ces kits de développement logiciel pour les ordinateurs Macintosh et diverses plateformes UNIX.  
  
 Il est important de comprendre que ODBC est conçu pour exposer les fonctionnalités de base de données, pas les compléter. Par conséquent, créateurs d’applications ne doivent pas attendre que l’utilisation d’ODBC soudainement transforme une base de données simple dans un moteur de base de données relationnelle complet. Ni sont les rédacteurs de pilotes attendus pour implémenter la fonctionnalité introuvable dans la base de données sous-jacente. Une exception à cela est que les développeurs qui écrivent des pilotes qui accèdent directement aux données de fichier (par exemple, les données dans un fichier Xbase) sont requis pour écrire un moteur de base de données qui prend en charge des fonctionnalités SQL au moins minimales. Une autre exception qui est le composant ODBC du SDK Windows, précédemment inclus dans le Kit de développement logiciel Microsoft données Access Components (MDAC), fournit une bibliothèque de curseurs qui simule les curseurs de défilement pour les pilotes qui implémentent un certain niveau de fonctionnalité.  
  
 Les applications qui utilisent ODBC sont responsables de toutes les fonctionnalités de bases de données croisées. Par exemple, ODBC n’est pas un moteur de jointure hétérogène, ni est-il un processeur de transaction distribuée. Toutefois, comme il est indépendant du SGBD, il peut être utilisé pour construire des outils de ces bases de données croisées.
