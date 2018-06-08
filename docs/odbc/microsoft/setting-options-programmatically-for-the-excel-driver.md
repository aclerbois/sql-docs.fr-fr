---
title: Définition des Options par programmation pour le pilote Excel | Documents Microsoft
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
- Jet-based ODBC drivers [ODBC], Excel driver
- desktop database drivers [ODBC], Excel driver
- ODBC desktop database drivers [ODBC], Excel driver
- Excel driver [ODBC], setting options programmatically
ms.assetid: b5ee3636-4591-427a-a65a-a2d5926fcc1a
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 30c48b72009b687e85edafc383213ae048b942f2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setting-options-programmatically-for-the-excel-driver"></a>Définition des Options par programmation pour le pilote Excel
|Option| Description|Méthode|  
|------------|-----------------|------------|  
|Nom de la source de données|Un nom qui identifie la source de données, tel que paie ou Personnel.|Pour définir cette option dynamiquement, utilisez le **DSN** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/odbc-jet-sqlconfigdatasource-excel-driver.md).|  
|Base de données|Une source de données Microsoft Access peut être configurée sans sélectionné ou créé une base de données. Si aucune base de données n’est fourni lors de l’installation, l’utilisateur sera invité à choisir un fichier de base de données lors de la connexion à la source de données.|Pour définir cette option dynamiquement, utilisez le **DBQ** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/odbc-jet-sqlconfigdatasource-excel-driver.md).|  
| Description|Une description facultative des données dans la source de données ; par exemple, « date d’embauche, historique de salaire et examen actuel de tous les employés. »|Pour définir cette option dynamiquement, utilisez le **DESCRIPTION** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/odbc-jet-sqlconfigdatasource-excel-driver.md).|  
|Répertoire|Affiche le répertoire actuellement sélectionné.<br /><br /> Pour les fichiers Microsoft Excel 3.0/4.0, l’affichage du chemin d’accès est étiqueté « Directory », tandis que pour Microsoft Excel 5.0, 7.0, 97 ou les fichiers, l’affichage du chemin d’accès est étiqueté « Classeur ».|Pour définir cette option dynamiquement, utilisez le **DEFAULTDIR** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/odbc-jet-sqlconfigdatasource-excel-driver.md).|  
|Lecture seule|Désigne la base de données en lecture seule.|Pour définir cette option dynamiquement, utilisez le **READONLY** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/odbc-jet-sqlconfigdatasource-excel-driver.md).|  
|Lignes à analyser|Le nombre de lignes à analyser pour déterminer le type de données de chaque colonne. Le type de données est déterminé compte tenu du nombre maximal de types de données trouvées. Si les données sont rencontrées, qui ne correspond pas au type de données estimé pour la colonne, le type de données est retourné en une valeur NULL.<br /><br /> Pour le pilote Microsoft Excel, vous pouvez entrer un nombre compris entre 1 à 16 pour les lignes à analyser. La valeur par défaut est 8 ; Si elle est définie sur 0, toutes les lignes sont analysées. (Un nombre en dehors de la limite retournera une erreur).|Pour définir cette option dynamiquement, utilisez le **MAXSCANROWS** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/odbc-jet-sqlconfigdatasource-excel-driver.md).|  
|Sélectionnez le répertoire|Affiche une boîte de dialogue où vous pouvez sélectionner un répertoire contenant les fichiers que vous souhaitez accéder.<br /><br /> Lorsque vous définissez un répertoire de source de données (pour tous les pilotes à l’exception de Microsoft Access), spécifiez le répertoire où se trouvent vos fichiers couramment utilisées. Le pilote ODBC utilise ce répertoire comme répertoire par défaut. Copier d’autres fichiers dans ce répertoire s’ils sont utilisés fréquemment. Ou bien, vous pouvez qualifier les noms de fichiers dans une instruction SELECT avec le nom du répertoire :<br /><br /> SÉLECTIONNEZ \* DE C:\MYDIR\EMP<br /><br /> Ou bien, vous pouvez spécifier un nouveau répertoire par défaut à l’aide de la **SQLSetConnectOption** fonction avec l’option SQL_CURRENT_QUALIFIER.<br /><br /> Pour Microsoft Excel 3.0 ou 4.0 fichiers, l’affichage du chemin d’accès est intitulé « Directory », et le bouton de sélection du chemin d’accès est étiqueté « Sélectionner un répertoire ». Pour les fichiers Microsoft Excel 5.0, 7.0 ou 97, l’affichage du chemin d’accès est intitulé « Classeur », et le bouton de sélection du chemin d’accès est étiqueté « Sélectionner le classeur ». Lorsque vous définissez un répertoire de source de données, spécifiez le répertoire où vos fichiers Microsoft Excel plus couramment utilisés pour Microsoft Excel 3.0/4.0, ou le répertoire où se trouve le fichier de classeur pour Microsoft Excel 5.0, 7.0 ou 97. **Utiliser Active Directory** est désactivée pour Microsoft Excel 5.0, 7.0 et 97.|Pour définir cette option dynamiquement, utilisez le **DEFAULTDIR** mot clé dans un appel à [SQLConfigDataSource](../../odbc/microsoft/odbc-jet-sqlconfigdatasource-excel-driver.md).|
