---
title: ODBC Jet SQLConfigDataSource (pilote Excel) | Documents Microsoft
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
- SQLConfigDataSource function [ODBC], Excel Driver
- Excel driver [ODBC], SqlConfigDataSource
ms.assetid: 885b3bea-f4b6-4902-b994-f78a912b612f
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 00b2295ead09d0196a14248b03ee5f6a96936df6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odbc-jet-sqlconfigdatasource-excel-driver"></a>ODBC Jet SQLConfigDataSource (pilote Excel)
> [!NOTE]  
>  Cette rubrique fournit des informations spécifiques au pilote Excel. Pour obtenir des informations générales sur cette fonction, consultez la rubrique appropriée sous [référence de l’API ODBC](../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Le **SQLConfigDataSource** fonction qui est utilisée pour ajouter, modifier ou supprimer une source de données utilise dynamiquement les mots clés suivants.  
  
|Mot clé| Description|  
|-------------|-----------------|  
|DBQ|Pour le pilote Microsoft Excel pour accéder à Microsoft Excel 5.0 ou ultérieurement les fichiers, le nom du fichier de classeur.<br /><br /> L’option est définie en tant que même **base de données** dans la boîte de dialogue d’installation.|  
|DEFAULTDIR|La spécification de chemin d’accès au répertoire.<br /><br /> L’option est définie en tant que même **sélectionner un répertoire** ou **Sélectionner classeur** dans la boîte de dialogue d’installation.|  
|DESCRIPTION|Une description des données dans la source de données.<br /><br /> L’option est définie en tant que même **Description** dans la boîte de dialogue d’installation.|  
|DRIVER|La spécification de chemin d’accès à la DLL du pilote.|  
|DRIVERID|Un ID d’entier pour le pilote.<br /><br /> 534 (Microsoft Excel 3.0)<br /><br /> 278 (Microsoft Excel 4.0)<br /><br /> 22 (Microsoft Excel 5.0/7.0)<br /><br /> 790 (Microsoft Excel 97-2003)|  
|FIL|Fichier de type, par exemple, Excel 3.0, Excel 4.0, 5.0 d’Excel, Excel 7.0, Excel 97, Excel 2000 ou Excel 2003.|  
|FIRSTROWHASNAMES|Indique si les cellules de la première ligne de la plage contiennent les noms de colonnes pour la table (1) ou non (0).|  
|MAXSCANROWS|Le nombre de lignes à analyser lors de la définition du type de données d’une colonne en fonction des données existantes.<br /><br /> Nombre compris entre 1 et 16 peut être entré pour les lignes à analyser. La valeur par défaut est 8 ; Si elle est définie sur 0, toutes les lignes sont analysées. (Un nombre en dehors de la limite retournera une erreur).<br /><br /> L’option est définie en tant que même **lignes à analyser** dans la boîte de dialogue d’installation.|  
|READONLY|TRUE pour rendre le fichier en lecture seule ; Valeur FALSE à rendre le fichier pas en lecture seule.<br /><br /> L’option est définie en tant que même **en lecture seule** dans la boîte de dialogue d’installation.|  
|THREADS|Le nombre de threads d’arrière-plan pour le moteur à utiliser. Pour le pilote Microsoft Access, cette valeur par défaut est 3, mais peut être modifiée. Pour dBASE, MicrosoftExceldriver cette valeur est 3 et ne peut pas être modifié.<br /><br /> L’option est définie en tant que même **Threads** dans la boîte de dialogue d’installation.|
