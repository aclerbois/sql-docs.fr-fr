---
title: SQLConfigDataSource (pilote Paradox) | Documents Microsoft
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
- SQLConfigDataSource function [ODBC], Paradox Driver
- Paradox driver [ODBC], SQLConfigDataSource
ms.assetid: 59e84c4e-debe-49d7-b97b-84c736b0c793
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b308748a351617808ca74863c681e562a9df74a6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlconfigdatasource-paradox-driver"></a>SQLConfigDataSource (pilote Paradox)
> [!NOTE]  
>  Cette rubrique fournit des informations spécifiques au pilote de Corel. Pour obtenir des informations générales sur cette fonction, consultez la rubrique appropriée sous [référence de l’API ODBC](../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Le **SQLConfigDataSource** fonction qui est utilisée pour ajouter, modifier ou supprimer une source de données utilise dynamiquement les mots clés suivants.  
  
|Mot clé| Description|  
|-------------|-----------------|  
|COLLATINGSEQUENCE|La séquence dans laquelle les champs sont triés.<br /><br /> Lorsque le pilote Paradox est utilisé, la séquence peut être ASCII (par défaut), International, suédois-finnois ou Norvégien-Danois.<br /><br /> L’option est définie en tant que même **séquence de classement** dans la boîte de dialogue d’installation.|  
|DBQ|Le nom du fichier de base de données.<br /><br /> L’option est définie en tant que même **base de données** dans la boîte de dialogue d’installation.|  
|DEFAULTDIR|La spécification de chemin d’accès au répertoire.|  
|DESCRIPTION|Une description des données dans la source de données.<br /><br /> L’option est définie en tant que même **Description** dans la boîte de dialogue d’installation.|  
|DRIVER|La spécification de chemin d’accès à la DLL du pilote.|  
|DRIVERID|Un ID d’entier pour le pilote.<br /><br /> 26 (paradox 3.x)<br /><br /> 282 (paradox 4.x)<br /><br /> 538 (paradox 5.x)|  
|EXCLUSIF|Détermine si la base de données s’ouvre en mode exclusif (accédé par un seul utilisateur à la fois) ou mode (accédé par plusieurs utilisateurs à la fois) partagé. Peut être (mode exclusif) la valeur true ou false (mode partagé).<br /><br /> L’option est définie en tant que même **exclusif** dans la boîte de dialogue d’installation.|  
|FIL|Fichier type Paradox 3.x, Paradox 4.x ou Paradox 5.x|  
|TYPE DE FICHIER|Type de fichier pour le pilote de texte (texte).|  
|PAGETIMEOUT|Spécifie la période de temps, en dixièmes de seconde, une page (si non utilisé) reste dans la mémoire tampon avant d’être supprimée. La valeur par défaut est 600 dixièmes de seconde (60 secondes). Notez que cette option s’applique à toutes les sources de données qui utilisent le pilote ODBC.<br /><br /> L’option est définie en tant que même **Page Timeout** dans la boîte de dialogue d’installation.|  
|PARADOXNETPATH|Le chemin d’accès complet du répertoire contenant une base de données de verrou Paradox, car il contient le fichier PDOXUSRS.net (dans Paradox 4. *x*) ou le fichier PARADOX.net (dans Paradox 5. *x*). Si le répertoire ne contient pas un de ces fichiers, le pilote Paradox en crée un. Pour plus d’informations sur ces fichiers, consultez la documentation de Corel.<br /><br /> Avant de pouvoir sélectionner un répertoire réseau, un nom d’utilisateur Paradox doit être entré.<br /><br /> L’option est définie en tant que même **sélectionner un répertoire réseau** dans la boîte de dialogue d’installation.|  
|PARADOXNETSTYLE|Pour le pilote Paradox, le réseau d’accès style à utiliser lors de l’accès aux données Paradox : « 3.x » pour Paradox 3. *x* ou « 4.x » pour Paradox 4. *x* ou 5. *x*. Peut être définie à « 3.x » ou « 4.x » si la version est Paradox 4. *x* ou 5. *x*; si la version est Paradox 3. *x*, le style doit être « 3.x ».<br /><br /> L’option est définie en tant que même **Style Net** dans la boîte de dialogue d’installation.|  
|PARADOXUSERNAME|Pour le pilote Paradox, le nom d’utilisateur de Corel.<br /><br /> L’option est définie en tant que même **nom d’utilisateur** dans la boîte de dialogue d’installation.|  
|PWD|Mot de passe.<br /><br /> Ce mot clé facultatif et ne sera jamais être écrites dans le fichier par le pilote. Il est utilisé dans un appel à **SQLDriverConnect** mot de passe sécurisé de fichiers de Corel. Le mot de passe sera valide chaque fois qu’une table est ouvert. Si aucun mot de passe n’est transmis dans la chaîne de connexion, aucun mot de passe n’est générée pour cette table. Si les tables ont des mots de passe différents, plusieurs ne peut pas être ouvert dans la même session, ni si les tables peuvent être jointes.|  
|READONLY|TRUE pour rendre le fichier en lecture seule ; Valeur FALSE à rendre le fichier pas en lecture seule.<br /><br /> L’option est définie en tant que même **en lecture seule** dans la boîte de dialogue d’installation.|  
|THREADS|Le nombre de threads d’arrière-plan pour le moteur à utiliser. Cette valeur est 3 et ne peut pas être modifiée.<br /><br /> L’option est définie en tant que même **Threads** dans la boîte de dialogue d’installation.|
