---
title: SQLConfigDataSource (pilote Access) | Documents Microsoft
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
- SQLConfigDataSource function [ODBC], Access Driver
- Access driver [ODBC], SQLConfigDataSource
ms.assetid: 1b152fb7-fa12-46b9-b168-006bb1355e77
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a792a34bc80ae7e25641aaea7aae88ed7b2349ac
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlconfigdatasource-access-driver"></a>SQLConfigDataSource (pilote Access)
> [!NOTE]  
>  Cette rubrique fournit des informations spécifiques au pilote Access. Pour obtenir des informations générales sur cette fonction, consultez la rubrique appropriée sous [référence de l’API ODBC](../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Le **SQLConfigDataSource** fonction qui est utilisée pour ajouter, modifier ou supprimer une source de données utilise dynamiquement les mots clés suivants.  
  
|Mot clé| Description|  
|-------------|-----------------|  
|COLLATINGSEQUENCE|La séquence dans laquelle les champs sont triés.<br /><br /> L’option est définie en tant que même **séquence de classement** dans la boîte de dialogue d’installation.|  
|COMPACT_DB|Effectue une compression de données sur un fichier de base de données. A le format suivant : COMPACT_DB = < nom_chemin >< optionaL_sort_order >\<mot clé de chiffrement facultatif >.<br /><br /> Lorsque vous utilisez le mot clé COMPACT_DB dans la même instruction avec un mot clé DSN, ce pilote ignore le mot clé DSN. Par conséquent, le compactage d’une base de données et en spécifiant une source de données sont un processus en deux étapes.|  
|CREATE_DB|Crée un fichier de base de données. A le format suivant : CREATE_DB = < nom_chemin >\<optional_sort-ordre >< optional_ENCRYPT mot clé >, où le nom de chemin d’accès est le chemin d’accès complet à une base de données Microsoft Access. Une erreur est renvoyée si le nom de chemin d’accès spécifie une base de données existante. L’ordre de tri sera en tant que jeu de configuration dans la boîte de dialogue Nouvelle base de données affichée lorsque le bouton Créer est activé dans la boîte de dialogue d’installation de Microsoft Access. Si aucun ordre de tri n’est spécifié, général est utilisé.<br /><br /> Lorsque vous utilisez le mot clé CREATE_DB dans la même instruction avec un mot clé DSN, ce pilote ignore le mot clé DSN. Par conséquent, la création d’une base de données et en spécifiant une source de données sont un processus en deux étapes. Lorsque vous utilisez le mot clé CREATE_DB, si le chemin d’accès de la base de données Microsoft Access doit être créé contient un ou plusieurs espaces, le chemin d’accès entier doit être placé par des guillemets doubles, comme indiqué dans les exemples suivants :<br /><br /> « C:\PROGRAM FILES\COMMON Files\Microsoft MyAccess.mdb »<br /><br /> « C:\PROGRAM FILES\Access2.mdb »<br /><br /> CREATE_DB=C:\TEMP\test.mdb (sans les guillemets si nécessaires)|  
|CREATE_SYSDB|Crée un fichier de base de données système. A le format suivant : CREATE_SYSDB =\<nom de chemin d’accès >\<ordre de tri facultatif >, où le nom de chemin d’accès est le chemin d’accès complet à une base de données Microsoft Access. Une erreur est renvoyée si le nom de chemin d’accès spécifie une base de données existante. L’ordre de tri sera en tant que jeu de configuration dans le **nouvelle base de données** boîte de dialogue affichée lorsque le **créer** clic est effectué dans le **ODBC Microsoft Access Setup** boîte de dialogue. Si aucun ordre de tri n’est spécifié, général est utilisé.|  
|CREATE_V2DB|Crée un fichier de base de données qui est compatible avec Microsoft Access 2.0. A le format suivant : CREATE_V2DB =\<nom de chemin d’accès >\<ordre de tri facultatif >, où le nom de chemin d’accès est le chemin d’accès complet à une base de données Microsoft Access. Une erreur est renvoyée si le nom de chemin d’accès spécifie une base de données existante. L’ordre de tri sera en tant que jeu de configuration dans la boîte de dialogue Nouvelle base de données affichée lorsque le bouton Créer est activé dans la boîte de dialogue d’installation de Microsoft Access. Si aucun ordre de tri n’est spécifié, général est utilisé.<br /><br /> Lorsque vous utilisez le mot clé CREATE_V2DB dans la même instruction avec un mot clé DSN, ce pilote ignore le mot clé DSN. Par conséquent, la création d’une base de données et en spécifiant une source de données sont un processus en deux étapes.<br /><br /> Lorsque vous utilisez le mot clé CREATE_V2DB, si le chemin d’accès de la base de données Microsoft Access doit être créé contient un ou plusieurs espaces, le chemin d’accès entier doit être placé par des guillemets doubles, comme indiqué dans les exemples suivants :<br /><br /> « C:\PROGRAM FILES\COMMON Files\Microsoft MyAccess.mdb »<br /><br /> « C:\PROGRAM FILES\Access2.mdb »<br /><br /> CREATE_V2DB=C:\TEMP\test.mdb (sans les guillemets si nécessaires)|  
|DBQ|Le nom du fichier de base de données.<br /><br /> L’option est définie en tant que même **base de données** dans la boîte de dialogue d’installation.|  
|DEFAULTDIR|La spécification de chemin d’accès au fichier de base de données.|  
|DESCRIPTION|Une description des données dans la source de données.<br /><br /> L’option est définie en tant que même **Description** dans la boîte de dialogue d’installation.|  
|DRIVER|La spécification de chemin d’accès à la DLL du pilote.|  
|DRIVERID|Un ID d’entier pour le pilote.  25 (Microsoft Access)|  
|FIL|MS Access de Microsoft Access de type de fichier|  
|IMPLICITCOMMITSYNC|Détermine si le pilote Microsoft Access effectue les validations internes ou implicites en mode asynchrone. Cette valeur est initialement définie sur « Oui », ce qui signifie que le pilote Microsoft Access attendra les validations dans une transaction interne/implicite doit être terminé.<br /><br /> La valeur de cette option ne doit pas être modifiée sans tenez compte des conséquences. Pour plus d’informations sur l’option, consultez la *Guide du programmeur moteur Microsoft Jet de base de données*.<br /><br /> L’option est définie en tant que même **ImplicitCommitSync** dans la boîte de dialogue d’installation.|  
|MAXBUFFERSIZE|La taille de la mémoire tampon interne, en kilo-octets, qui est utilisé par Microsoft Access pour transférer des données vers et depuis le disque. La taille de mémoire tampon par défaut est 2048 Ko (affiché comme 2048). Toute valeur entière divisible par 256 peut être utilisé. L’option est définie en tant que même **taille de mémoire tampon** dans la boîte de dialogue d’installation.|  
|MAXSCANROWS|Le nombre de lignes à analyser lors de la définition du type de données d’une colonne en fonction des données existantes.<br /><br /> Nombre compris entre 1 et 16 peut être entré pour les lignes à analyser. La valeur par défaut est 8 ; Si elle est définie sur 0, toutes les lignes sont analysées. (Un nombre en dehors de la limite retournera une erreur).<br /><br /> L’option est définie en tant que même **lignes à analyser** dans la boîte de dialogue d’installation.|  
|PAGETIMEOUT|Spécifie la période de temps, en millisecondes, d’une page (si non utilisé) reste dans la mémoire tampon avant d’être supprimée. La valeur par défaut est cinq dixièmes de seconde (0,5 secondes). Notez que cette option s’applique à toutes les sources de données qui utilisent le pilote ODBC.<br /><br /> L’option est définie en tant que même **Page Timeout** dans la boîte de dialogue d’installation.|  
|PWD|Mot de passe.|  
|READONLY|TRUE pour rendre le fichier en lecture seule ; Valeur FALSE à rendre le fichier pas en lecture seule.<br /><br /> L’option est définie en tant que même **en lecture seule** dans la boîte de dialogue d’installation.|  
|REPAIR_DB|Répare une base de données endommagée par une défaillance se produit pendant le processus de validation.<br /><br /> Lorsque vous utilisez le mot clé REPAIR_DB dans la même instruction avec un mot clé DSN, ce pilote ignore le mot clé DSN. Par conséquent, la réparation d’une base de données et en spécifiant une source de données sont un processus en deux étapes.|  
|SYSTEMDB|Pour le pilote Microsoft Access, la spécification de chemin d’accès au fichier de base de données système.<br /><br /> L’option est définie en tant que même **base de données système** dans la boîte de dialogue d’installation.|  
|THREADS|Le nombre de threads d’arrière-plan pour le moteur à utiliser. Cette valeur par défaut est 3, mais peut être modifiée.<br /><br /> L’option est définie en tant que même **Threads** dans la boîte de dialogue d’installation.|  
|UID|Pour le pilote Microsoft Access, le nom d’ID utilisateur utilisé pour la connexion.|  
|USERCOMMITSYNC|Détermine si le pilote Microsoft Access effectuera les transactions définies par l’utilisateur en mode asynchrone. Cette valeur est initialement définie sur « Oui », ce qui signifie que le pilote Microsoft Access attendra les validations dans une transaction définie par l’utilisateur doit être terminé.<br /><br /> La valeur de cette option ne doit pas être modifiée sans tenez compte des conséquences. Pour plus d’informations sur l’option, consultez la *Guide du programmeur moteur Microsoft Jet de base de données*.<br /><br /> L’option est définie en tant que même **UserCommitSync** dans la boîte de dialogue d’installation.|
