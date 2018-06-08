---
title: Exécuter à partir de la ligne de commande (données Assistant Migration SQL Server) | Documents Microsoft
ms.custom: ''
ms.date: 09/01/2017
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.suite: sql
ms.technology: dma
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Command Line
ms.assetid: ''
caps.latest.revision: ''
author: HJToland3
ms.author: jtoland
manager: craigg
ms.openlocfilehash: df58c273c67868e894b7cba38344dc43628962ac
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="run-data-migration-assistant-from-the-command-line"></a>Exécutez l’Assistant Migration de données à partir de la ligne de commande
Avec la version 2.1 et versions ultérieures, lorsque vous installez l’Assistant Migration de données, il installe également dmacmd.exe dans *% ProgramFiles%\\l’Assistant Migration de données Microsoft\\*. Utilisez dmacmd.exe pour évaluer vos bases de données en mode sans assistance et produire le résultat dans le fichier JSON ou CSV. Ceci est particulièrement utile lors de l’évaluation de plusieurs bases de données ou les bases de données volumineux. 

> [!NOTE]
> Dmacmd.exe prend en charge uniquement les évaluations en cours d’exécution. Les migrations ne sont pas pris en charge pour l’instant.


## <a name="command-line-arguments"></a>Arguments de ligne de commande

```
DmaCmd.exe /AssessmentName="string"
/AssessmentDatabases="connectionString1" \["connectionString2"\]
\[/AssessmentTargetPlatform="TargetPlatform"\]
/AssessmentEvaluateRecommendations|/AssessmentEvaluateCompatibilityIssues
\[/AssessmentOverwriteResult\]
/AssessmentResultJson="file"|/AssessmentResultCsv="file"
```


|Argument  | Description  | Requis (Y/N)
|---------|---------|---------------|
| `/help or /?`     | Comment utiliser le texte d’aide dmacmd.exe        | N
|`/AssessmentName`     |   Nom du projet d’évaluation   | O
|`/AssessmentDatabases`     | Liste délimitée par des espaces de chaînes de connexion. Nom de la base de données (catalogue Initial) respecte la casse. | O
|`/AssessmentTargetPlatform`     | Plateforme cible pour l’évaluation, les valeurs prises en charge : SqlServer2012, SqlServer2014, SqlServer2016 et AzureSqlDatabaseV12. Valeur par défaut est SqlServer2016   | N
|`/AssessmentEvaluateFeatureParity`  | Exécuter les règles de parité de fonctionnalités  | N
|`/AssessmentEvaluateCompatibilityIssues`     | Exécuter les règles de compatibilité  | O <br> (AssessmentEvaluateCompatibilityIssues ou AssessmentEvaluateRecommendations est nécessaire.)
|`/AssessmentEvaluateRecommendations`     | Exécuter les recommandations de fonctionnalité        | O <br> (AssessmentEvaluateCompatibilityIssues ou AssessmentEvaluateRecommendationsis requis)
|`/AssessmentOverwriteResult`     | Remplacer le fichier de résultats    | N
|`/AssessmentResultJson`     | Chemin d’accès complet au fichier JSON     | O <br> (AssessmentResultJson ou AssessmentResultCsv est requis)
|`/AssessmentResultCsv`    | Chemin d’accès complet au fichier CSV   | O <br>(AssessmentResultJson ou AssessmentResultCsv est requis)




## <a name="examples"></a>Exemples

**Dmacmd.exe**

  `Dmacmd.exe /? or DmaCmd.exe /help`

**Évaluation de base de données unique à l’aide de règles de compatibilité en cours d’exécution et de l’authentification Windows**

```
DmaCmd.exe /AssessmentName="TestAssessment"
/AssessmentDatabases="Server=SQLServerInstanceName;Initial
Catalog=DatabaseName;***Integrated Security=true*"**
***/AssessmentEvaluateCompatibilityIssues*** /AssessmentOverwriteResult
/AssessmentResultJson="C:\\temp\\Results\\AssessmentReport.json"
```



**Évaluation de base de données unique à l’aide de la recommandation de fonctionnalité d’authentification et en cours d’exécution de SQL Server**

```
DmaCmd.exe /AssessmentName="TestAssessment"
/AssessmentDatabases="Server=SQLServerInstanceName;Initial
Catalog=DatabaseName;***User Id=myUsername;Password=myPassword;***"
***/AssessmentEvaluateRecommendations*** /AssessmentOverwriteResult
/AssessmentResultCsv="C:\\temp\\Results\\AssessmentReport.csv"
```


**Évaluation de la base de données unique pour la plateforme de la cible SQL Server 2012 et enregistrer les résultats dans un fichier .json et .csv**

```
DmaCmd.exe /AssessmentName="TestAssessment"
/AssessmentDatabases="Server=SQLServerInstanceName;Initial
Catalog=DatabaseName;Integrated Security=true"
***/AssessmentTargetPlatform="SqlServer2012"***
/AssessmentEvaluateRecommendations /AssessmentOverwriteResult
***/AssessmentResultJson***="C:\\temp\\Results\\AssessmentReport.json"
***/AssessmentResultCsv***="C:\\temp\\Results\\AssessmentReport.csv"
```


**Évaluation de base de données unique pour la plateforme cible base de données SQL Azure enregistrer les résultats dans un fichier .json et .csv**

```
DmaCmd.exe /AssessmentName="TestAssessment" 
/AssessmentDatabases="Server=SQLServerInstanceName;Initial
Catalog=DatabaseName;Integrated Security=true"
/AssessmentTargetPlatform="AzureSqlDatabaseV12"
/AssessmentEvaluateCompatibilityIssues /AssessmentEvaluateFeatureParity
/AssessmentOverwriteResult 
/AssessmentResultCsv="C:\\temp\\AssessmentReport.csv" 
/AssessmentResultJson="C:\\temp\\AssessmentReport.json"
```


**Évaluation de plusieurs bases de données**

```
DmaCmd.exe /AssessmentName="TestAssessment"
***/AssessmentDatabases="Server=SQLServerInstanceName1;Initial
Catalog=DatabaseName1;Integrated Security=true"
"Server=SQLServerInstanceName1;Initial Catalog=DatabaseName2;Integrated
Security=true" "Server=SQLServerInstanceName2;Initial
Catalog=DatabaseName3;Integrated Security=true"***
/AssessmentTargetPlatform="SqlServer2016"
/AssessmentEvaluateCompatibilityIssues /AssessmentOverwriteResult
/AssessmentResultCsv="C:\\temp\\Results\\AssessmentReport.csv"
/AssessmentResultJson="C:\\Results\\test2016.json"
```



## <a name="see-also"></a>Voir aussi

[Télécharger l’Assistant Migration de données](https://www.microsoft.com/download/details.aspx?id=53595)
