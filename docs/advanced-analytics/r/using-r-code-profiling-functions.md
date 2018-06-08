---
title: À l’aide de fonctions (SQL Server Machine Learning) du profilage du Code R | Documents Microsoft
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/15/2018
ms.topic: conceptual
author: HeidiSteen
ms.author: heidist
manager: cgronlun
ms.openlocfilehash: 05689ae356d415f9655b8709c619e40e6d8fa817
ms.sourcegitcommit: 7a6df3fd5bea9282ecdeffa94d13ea1da6def80a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2018
---
# <a name="using-r-code-profiling-functions"></a>À l’aide des fonctions du profilage du code R
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

En plus des ressources et des outils SQL Server permettant de surveiller l’exécution de script R, vous pouvez utiliser les outils de performances fournis par d’autres packages R pour obtenir des informations complémentaires sur les appels de fonctions internes. Cette rubrique vous propose une liste de ressources de base comme point de départ. Pour obtenir des conseils d’expert, nous vous recommandons de lire le chapitre [Performance](http://adv-r.had.co.nz/Performance.html) de l’ouvrage de Hadley Wickham, « Advanced R ».

## <a name="using-rprof"></a>Utilisation de RPROF

*rprof* est une fonction intégrée au package de base **utils**, qui est chargé par défaut. L’un des avantages de la fonction *rprof* est qu’elle effectue un échantillonnage, ce qui atténue la charge de performances de la surveillance.

Pour utiliser le profilage R dans votre code, vous devez appeler cette fonction et spécifier ses paramètres, notamment le nom de l’emplacement du fichier journal qui sera écrit. Pour plus d’informations, consultez l’aide de *rprof*.

En général, la fonction *rprof* opère en écrivant la pile d’appels dans un fichier selon des intervalles définis. Vous pouvez ensuite utiliser la fonction *summaryRprof* pour traiter le fichier de sortie. 

Le profilage peut être activé et désactivé dans le code. Pour l’activer, le mettre en suspens et le redémarrer, vous devez utiliser une séquence d’appels à *rprof*, à savoir :

1. Spécifiez le fichier de sortie du profilage.

    ```R
    varOutputFile <- "C:/TEMP/run001.log")
    Rprof(varOutputFile)
    ```
2. Désactivez le profilage.
    ```R
    Rprof(NULL)
    ```
    
3. Redémarrez le profilage.
    ```R
    Rprof(append=TRUE)
    ```


> [!NOTE]
> Pour pouvoir utiliser cette fonction, vous devez installer Windows Perl sur l’ordinateur appelé à exécuter le code. Par conséquent, nous recommandons de profiler le code pendant la phase de développement dans un environnement R, puis de déployer le code débogué sur SQL Server.  


## <a name="r-system-functions"></a>Fonctions du système R

Le langage R intègre la plupart des fonctions du package de base qui retournent le contenu des variables système. 

Par exemple, dans votre code R, vous pouvez utiliser `Sys.timezone` pour obtenir le fuseau horaire actuel ou `Sys.Time` pour obtenir l’heure système à partir de R. 

Pour obtenir des informations sur les différentes fonctions du système R, tapez le nom de la fonction comme argument de la fonction `help()` R à partir d’une invite de commandes R.

```R
help("Sys.time")
```

## <a name="debugging-and-profiling-in-r"></a>Débogage et profilage dans R

Dans la documentation de Microsoft R Open, qui est installée par défaut, vous trouverez un manuel sur le développement d’extensions pour le langage R qui traite en détail le profilage et le débogage.

Le chapitre est également disponible en ligne : [https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Debugging](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Debugging)

### <a name="location-of-r-help-files"></a>Emplacement des fichiers d’aide de R

C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\doc\manual



