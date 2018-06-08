---
title: En choisissant des données Source ou le pilote | Documents Microsoft
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
- connecting to driver [ODBC], selecting driver
- connecting to data source [ODBC], selecting data source
- data sources [ODBC], selecting
- ODBC drivers [ODBC], selecting
ms.assetid: 10aaf570-01ab-4478-8339-bdde2a5e3dd1
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d3e9c5964d529fa70bd82c3aec7d25a42cb10c08
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="choosing-a-data-source-or-driver"></a>En choisissant des données Source ou pilote
La source de données ou le pilote utilisé par une application est parfois codé en dur dans l’application. Par exemple, une application personnalisée écrite par un service MIS à transférer des données à partir d’une source de données à l’autre contient les noms de ces sources de données, l’application ne fonctionnerait pas avec d’autres sources de données. Un autre exemple est une application verticale, tel que celui utilisé pour l’entrée de commande. Une telle application toujours utilise la même source de données, qui dispose d’un schéma prédéfini connu par l’application.  
  
 Autres applications sélectionner la source de données ou le pilote en cours d’exécution. En règle générale, il s’agit d’applications génériques qui effectuent des requêtes ad hoc, par exemple une feuille de calcul qui utilise ODBC pour importer des données. De telles applications généralement répertorient les sources de données disponibles ou les pilotes et laisser les utilisateurs choisir les celles qu’ils souhaitent utiliser. Si une application générique répertorie les sources de données, les pilotes ou les deux fréquemment dépend de si l’application utilise des pilotes basés sur SGBD ou basée sur le fichier.  
  
 Pilotes basés sur SGBD requièrent généralement un jeu complexe d’informations de connexion, telles que l’adresse réseau, protocole réseau, nom de la base de données et ainsi de suite. L’objectif d’une source de données est de masquer ces informations. Par conséquent, le paradigme de source de données se prête à utiliser avec les pilotes basés sur SGBD. Une application peut afficher une liste de sources de données à l’utilisateur de deux manières. Elle peut appeler **SQLDriverConnect** avec la **DSN** (mot clé) (nom de Source de données) et aucune valeur associée ; le Gestionnaire de pilotes affiche une liste de noms de source de données. Si l’application souhaite contrôler l’apparence de la liste, il appelle **SQLDataSources** pour récupérer la liste des sources de données et construit sa propre boîte de dialogue. Cette fonction est implémentée par le Gestionnaire de pilotes et peut être appelée avant que tous les pilotes sont chargés. Ensuite, l’application appelle une fonction de connexion et lui transmet le nom de la source de données choisi.  
  
 Si une source de données n’est pas spécifiée, la source de données par défaut indiquée par les informations système est utilisée. (Pour plus d’informations, consultez [par défaut le sous-clé](../../../odbc/reference/install/default-subkey.md).) Si **SQLConnect** est appelée en utilisant un *nom_serveur* argument qui est introuvable, est un pointeur null ou est « DEFAULT », le Gestionnaire de pilotes se connecte à la source de données par défaut. La valeur par défaut de source de données est également utilisée si la chaîne de connexion qui est utilisée dans un appel à **SQLDriverConnect** ou **SQLBrowseConnect** contient le **DSN** mot clé la valeur « DEFAULT » ou si la source de données spécifiée est introuvable. En outre, la valeur par défaut de source de données est utilisée si la chaîne de connexion qui est utilisé dans un appel à **SQLDriverConnect** ne contient pas le **DSN** (mot clé).  
  
 Pilotes basée sur le fichier, il est possible d’utiliser un paradigme de fichier. Pour les données stockées sur l’ordinateur local, les utilisateurs savent fréquemment que leurs données sont dans un fichier particulier, tel que Employee.dbf. Au lieu de sélectionner une source de données inconnu, il est plus facile à ces utilisateurs pour sélectionner le fichier qu’ils savent. Pour cela, elle appelle d’abord implémenter **SQLDrivers**. Cette fonction est implémentée par le Gestionnaire de pilotes et peut être appelée avant que tous les pilotes sont chargés. **SQLDrivers** retourne une liste des pilotes disponibles ; il retourne également des valeurs pour le **FileUsage** et **FileExtns** mots clés. Le **FileUsage** mot clé explique si pilotes basés sur le fichier de traitent les fichiers en tant que tables, comme est Xbase ou en tant que bases de données, comme le fait Microsoft® Access. Le **FileExtns** (mot clé) répertorie les extensions de nom de fichier du pilote reconnaît, telles que .dbf pour un pilote Xbase. À l’aide de ces informations, l’application crée une boîte de dialogue par le biais duquel l’utilisateur choisit un fichier. En fonction de l’extension du fichier sélectionné, puis l’application se connecte au pilote en appelant **SQLDriverConnect** avec la **pilote** (mot clé).  
  
 Rien à arrêter une application à partir de l’aide d’une source de données avec un pilote de fichier ou l’appel de **SQLDriverConnect** avec la **pilote** (mot clé) pour se connecter à un pilote de SGBD. Voici plusieurs utilisations courantes de la **pilote** mot clé pour pilotes basés sur SGBD :  
  
-   **Ne pas créer de sources de données.** Par exemple, une application personnalisée peut utiliser un pilote spécifique et la base de données. Si le nom du pilote et toutes les informations requises pour se connecter à la base de données est codé en dur dans l’application, les utilisateurs n’ont pas créer une source de données sur leur ordinateur pour exécuter l’application. Tout ce qu’ils doivent faire est d’installer l’application et le pilote.  
  
     L’inconvénient de cette méthode est que l’application doit être recompilée et si les informations de connexion changent. Si un nom de source de données est codé en dur dans l’application au lieu des informations de connexion complète, chaque utilisateur doit modifier que les informations dans la source de données.  
  
-   **L’accès à un SGBD particulier une seule fois.** Par exemple, une feuille de calcul qui Récupère des données en appelant les fonctions ODBC peut contenir le **pilote** mot clé pour identifier un pilote spécifique. Étant donné que le nom du pilote est significatif pour les utilisateurs qui disposent de ce pilote, la feuille de calcul peut être passée entre les utilisateurs. Si la feuille de calcul contenue un nom de source de données, chaque utilisateur devra créer la même source de données pour utiliser la feuille de calcul.  
  
-   **Parcourir le système pour toutes les bases de données accessibles à un pilote spécifique.** Pour plus d’informations, consultez [connexion avec SQLBrowseConnect](../../../odbc/reference/develop-app/connecting-with-sqlbrowseconnect.md), plus loin dans cette section.
