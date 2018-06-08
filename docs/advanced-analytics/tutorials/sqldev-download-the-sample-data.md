---
title: Leçon 1 Télécharger les exemples de données | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: tutorial
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: a87d307dafa733e449c6ec893ece21645fe65640
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="lesson-1-download-the-sample-data"></a>Leçon 1 : Télécharger les exemples de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cet article fait partie d’un didacticiel pour les développeurs SQL sur la façon d’utiliser R dans SQL Server.

Dans cette étape, vous allez télécharger l’exemple de dataset et le [!INCLUDE[tsql](../../includes/tsql-md.md)] script des fichiers qui sont utilisés dans ce didacticiel. Les données et les fichiers de script sont partagés sur GitHub, mais le script PowerShell va télécharger les fichiers de script et de données dans un répertoire local de votre choix.

## <a name="download-the-data-and-scripts"></a>Télécharger les données et les scripts

1.  Ouvrez une console de commande Windows PowerShell.
  
    Utilisez l’option **Exécuter en tant qu’administrateur**, si des privilèges d’administrateur sont nécessaires pour créer le répertoire de destination ou pour écrire des fichiers dans la destination spécifiée.
  
2.  Exécutez les commandes PowerShell suivantes, en définissant la valeur du paramètre *DestDir* sur un répertoire local.  La valeur par défaut que nous avons utilisée ici est **TempRSQL**.
  
    ```ps
    $source = ‘https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/RSQL/Download_Scripts_SQL_Walkthrough.ps1’  
    $ps1_dest = “$pwd\Download_Scripts_SQL_Walkthrough.ps1”
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQL_Walkthrough.ps1 –DestDir ‘C:\tempRSQL’
    ```
  
    Si le dossier que vous spécifiez dans *DestDir* n’existe pas, il est créé par le script PowerShell.
  
    > [!TIP]
    > Si vous obtenez une erreur, vous pouvez temporairement définir la stratégie d’exécution des scripts PowerShell sur **illimitée** uniquement pour cette procédure pas à pas, en utilisant l’argument Bypass et en limitant l’étendue des modifications à la session actuelle.
    >   
    >````
    > Set\-ExecutionPolicy Bypass \-Scope Process
    >````
    > L’exécution de cette commande n’a pas d’incidence sur la configuration.
  
    Selon votre connexion Internet, le téléchargement peut prendre un certain temps.
  
3.  Une fois tous les fichiers téléchargés, le script PowerShell s’ouvre sur le dossier spécifié par  *DestDir*. Dans l’invite de commandes PowerShell, exécutez la commande suivante et examinez les fichiers qui ont été téléchargés.
  
    ```
    ls
    ```
  
    **Résultats :**
  
    ![liste des fichiers téléchargés par le script PowerShell](media/rsql-devtut-filelist.png "liste des fichiers téléchargés par le script PowerShell")
  
## <a name="next-lesson"></a>Leçon suivante

[Leçon 2 : Importer des données vers SQL Server à l’aide de PowerShell](../r/sqldev-import-data-to-sql-server-using-powershell.md)

## <a name="previous-lesson"></a>Leçon précédente

[Analytique de R dans base de données pour les développeurs SQL](../tutorials/sqldev-in-database-r-for-sql-developers.md)
