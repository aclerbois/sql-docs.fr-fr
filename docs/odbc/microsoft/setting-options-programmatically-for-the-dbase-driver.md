---
title: Paramètre Options par programmation pour le pilote dBASE | Documents Microsoft
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
- Jet-based ODBC drivers [ODBC], DBasedriver
- desktop database drivers [ODBC], DBasedriver
- DBase driver [ODBC], setting options programmatically
- ODBC desktop database drivers [ODBC], DBasedriver
ms.assetid: 336d0fd4-5448-4d8c-b7d9-49e857228e36
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fcff8e5aefcd3ea810aae31d933cad14bc7541ba
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setting-options-programmatically-for-the-dbase-driver"></a>Paramètre Options par programmation pour le pilote dBASE
|Option| Description|Méthode|  
|------------|-----------------|------------|  
|Nombre de lignes approximatif|Détermine si les statistiques de taille de la table sont arrondies. Cette option s’applique à toutes les sources de données qui utilisent le pilote ODBC.|Pour définir cette option dynamiquement, utilisez le **statistiques** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-dbase-driver.md).|  
|Séquence de classement|La séquence dans laquelle les champs sont triés.<br /><br /> La séquence peut être : ASCII (la valeur par défaut) ou International.|Pour définir cette option dynamiquement, utilisez le **COLLATINGSEQUENCE** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-dbase-driver.md).|  
|Nom de la source de données|Un nom qui identifie la source de données, tel que paie ou Personnel.|Pour définir cette option dynamiquement, utilisez le **DSN** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-dbase-driver.md).|  
|Base de données|Une source de données Microsoft Access peut être configurée sans sélectionné ou créé une base de données. Si aucune base de données n’est fourni lors de l’installation, les utilisateurs seront invités à sélectionner un fichier de base de données lorsqu’ils se connectent à la source de données.|Pour définir cette option dynamiquement, utilisez le **DBQ** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-dbase-driver.md).|  
| Description|Une description facultative des données dans la source de données ; par exemple, « date d’embauche, historique de salaire et examen actuel de tous les employés. »|Pour définir cette option dynamiquement, utilisez le **DESCRIPTION** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-dbase-driver.md).|  
|Exclusive|Si le **exclusif** est activée, la base de données s’ouvre en mode exclusif et est accessible par un seul utilisateur à la fois. Les performances sont améliorées lorsqu’il s’exécute en mode exclusif.|Pour définir cette option dynamiquement, utilisez le **exclusif** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-dbase-driver.md).|  
|Délai d’expiration de la page|Spécifie la période de temps, en dixièmes de seconde, qui reste d’une page (si non utilisé) dans la mémoire tampon avant d’être supprimé. La valeur par défaut est 600 dixièmes de seconde (60 secondes). Cette option s’applique à toutes les sources de données qui utilisent le pilote ODBC.<br /><br /> Le délai d’expiration de la page ne peut pas être 0 en raison d’un délai inhérent. Le délai d’expiration de la page ne peut pas être inférieur au délai inhérent, même si l’option de délai d’attente de page a la valeur inférieure à cette valeur.|Pour définir cette option dynamiquement, utilisez le **PAGETIMEOUT** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-dbase-driver.md).|  
|Lecture seule|Désigne la base de données en lecture seule.|Pour définir cette option dynamiquement, utilisez le **READONLY** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-dbase-driver.md).|  
|Sélectionnez le répertoire|Affiche une boîte de dialogue où vous pouvez sélectionner un répertoire qui contient les fichiers que vous souhaitez accéder.<br /><br /> Lorsque vous définissez un répertoire de source de données, spécifiez le répertoire où se trouvent vos fichiers fréquemment utilisés. Le pilote ODBC utilise ce répertoire comme répertoire par défaut. Copier d’autres fichiers dans ce répertoire s’ils sont utilisés fréquemment. Ou bien, vous pouvez qualifier les noms de fichiers dans une instruction SELECT avec le nom du répertoire :<br /><br /> SÉLECTIONNEZ \* DE C:\MYDIR\EMP<br /><br /> Ou bien, vous pouvez spécifier un nouveau répertoire par défaut à l’aide de la **SQLSetConnectOption** fonction avec l’option SQL_CURRENT_QUALIFIER.|Pour définir cette option dynamiquement, utilisez le **DEFAULTDIR** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-dbase-driver.md).|  
|Afficher les lignes supprimées|Spécifie si les lignes qui ont été marquées comme supprimées peuvent être récupérées ou positionnés sur. Si désactivée, les lignes supprimées ne sont pas affichées. Si sélectionné, les lignes supprimées sont traitées le même que les lignes non supprimées. Par défaut, cette option est désactivée.|Pour définir cette option dynamiquement, utilisez le **DELETED** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-dbase-driver.md).|
