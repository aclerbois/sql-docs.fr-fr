---
title: Définition des Options par programmation pour le pilote Paradox | Documents Microsoft
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
- ODBC desktop database drivers [ODBC], Paradox driver
- Paradox driver [ODBC], setting options programmatically
- desktop database drivers [ODBC], Paradox driver
- Jet-based ODBC drivers [ODBC], Paradox driver
ms.assetid: 7996d3f8-b5f5-4cac-8a66-fc96a42b603e
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5fa8084499f38a7152eb5eeab50010b919f4a06e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setting-options-programmatically-for-the-paradox-driver"></a>Définition des Options par programmation pour le pilote Paradox
|Option| Description|Méthode|  
|------------|-----------------|------------|  
|Répertoire|Définit le répertoire cible.|Pour définir cette option dynamiquement, utilisez le **DEFAULTDIR** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-paradox-driver.md).|  
|Séquence de classement|La séquence dans laquelle les champs sont triés.<br /><br /> La séquence peut être ASCII (la valeur par défaut), International, suédois-finnois ou Norvégien-Danois.|Pour définir cette option dynamiquement, utilisez le **COLLATINGSEQUENCE** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-paradox-driver.md).|  
| Description|Une description facultative des données dans la source de données ; par exemple, « date d’embauche, historique de salaire et examen actuel de tous les employés. »|Pour définir cette option dynamiquement, utilisez le **DESCRIPTION** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-paradox-driver.md).|  
|Exclusive|Si le **exclusif** est activée, la base de données s’ouvre en mode exclusif et est accessible par un seul utilisateur à la fois. Les performances sont améliorées lors de l’exécution en mode exclusif.|Pour définir cette option dynamiquement, utilisez le **exclusif** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-paradox-driver.md).|  
|Style de réseau|Le style d’accès au réseau à utiliser lors de l’accès aux données Paradox : soit « 3.*x*» pour Paradox 3. *x* ou « 4. *x*» pour Paradox 4. *x* ou 5. *x*. Peut avoir la valeur « 3. *x*» ou « 4. *x*» si la version est Paradox 4. *x* ou 5. *x*; si la version est Paradox 3. *x*, le style doit être « 3. *x*».|Pour définir cette option dynamiquement, utilisez le **PARADOXNETSTYLE** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-paradox-driver.md).|  
|Délai d’expiration de la page|Spécifie la période de temps, en dixièmes de seconde, une page (si non utilisé) reste dans la mémoire tampon avant d’être supprimée. La valeur par défaut est 600 dixièmes de seconde (60 secondes). Cette option s’applique à toutes les sources de données qui utilisent le pilote ODBC.<br /><br /> Le délai d’expiration de la page ne peut pas être 0 en raison d’un délai inhérent. Le délai d’expiration de la page ne peut pas être inférieur au délai inhérent, même si l’option de délai d’attente de page a la valeur inférieure à cette valeur.|Pour définir cette option dynamiquement, utilisez le **PAGETIMEOUT** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-paradox-driver.md).|  
|Lecture seule|Désigne la base de données en lecture seule.|Pour définir cette option dynamiquement, utilisez le **READONLY** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-paradox-driver.md).|  
|Sélectionnez le répertoire|Affiche une boîte de dialogue où vous pouvez sélectionner un répertoire contenant les fichiers que vous souhaitez accéder.<br /><br /> Lorsque la définition d’un répertoire de source de données spécifiez le répertoire où les plus couramment utilisées fichiers se trouvent. Le pilote ODBC utilise ce répertoire comme répertoire par défaut. Copier d’autres fichiers dans ce répertoire s’ils sont utilisés fréquemment. Ou bien, vous pouvez qualifier les noms de fichiers dans une instruction SELECT avec le nom du répertoire :<br /><br /> SÉLECTIONNEZ \* DE C:\MYDIR\EMP<br /><br /> Ou bien, vous pouvez spécifier un nouveau répertoire par défaut à l’aide de la **SQLSetConnectOption** fonction avec l’option SQL_CURRENT_QUALIFIER.|Pour définir cette option dynamiquement, utilisez le **DEFAULTDIR** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-paradox-driver.md).|  
|Sélectionnez le répertoire réseau|Le chemin d’accès complet du répertoire contenant une base de données de verrou Paradox, car il contient le fichier Pdoxusrs.net (dans Paradox 4. *x*) ou le fichier Paradox.net (dans Paradox 5. *x*). Si le répertoire ne contient pas un de ces fichiers, le pilote Paradox en crée un. Pour plus d’informations sur ces fichiers, consultez la documentation de Corel.<br /><br /> Avant de pouvoir sélectionner un répertoire réseau, vous devez entrer votre nom d’utilisateur Paradox dans le **nom d’utilisateur** zone de texte. Cliquez sur **sélectionner un répertoire réseau** pour sélectionner un répertoire réseau.|Pour définir cette option dynamiquement, utilisez le **PARADOXNETPATH** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-paradox-driver.md).|  
|Nom d'utilisateur|Le nom d’utilisateur de Corel. Il s’agit du nom affiché à d’autres utilisateurs des fichiers Paradox lorsqu’un verrou est rencontré.|Pour définir cette option dynamiquement, utilisez le **PARADOXUSERNAME** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/sqlconfigdatasource-paradox-driver.md).|
