---
title: Invoke-ProcessASDatabase | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 44a5654207e84f3488c66b3bc9cfab4778b51acb
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="invoke-processasdatabase"></a>Invoke-ProcessASDatabase
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Effectue l’opération **Process** sur un **Database** spécifié avec un **ProcessType** ou **RefreshType** spécifique, selon le type de métadonnées sous-jacentes.  
  
 Utilisez **ProcessType** pour la base de données avec des métadonnées multidimensionnelles (y compris les bases de données tabulaires au niveau de compatibilité 1050, 1100 ou 1103).  
  
 Utilisez **RefreshType** pour les bases de données tabulaires au niveau de compatibilité 1200 ou supérieur.  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
## <a name="syntax"></a>Syntaxe  
 `Invoke-ProcessASDatabase [-DatabaseName] <string> [-RefreshType] <RefreshType> {Full | ClearValues | Calculate |     DataOnly | Automatic | Add | Defragment} [-Server <string>] [-Credential <pscredential>] [-WhatIf] [-Confirm]     [<CommonParameters>]`  
  
 `Invoke-ProcessASDatabase [-DatabaseName] <string> [-ProcessType] <ProcessType> {ProcessFull | ProcessAdd |     ProcessUpdate | ProcessIndexes | ProcessData | ProcessDefault | ProcessClear | ProcessStructure |     ProcessClearStructureOnly | ProcessScriptCache | ProcessRecalc | ProcessDefrag} [-Server <string>] [-Credential     <pscredential>] [-WhatIf] [-Confirm]  [<CommonParameters>]`  
  
## <a name="description"></a> Description  
 L’applet de commande **Invoke-ProcessASDatabase** traite une base de données au niveau que vous spécifiez. Par exemple, pour les bases de données tabulaires au niveau de compatibilité 1200, le réglage de **RefreshType** sur **Full** remplace les données existantes par des nouvelles.  
  
 Le type de traitement (multidimensionnel) ou le type d’actualisation (tabulaire) est nécessaire et peut être spécifié avant ou après les paramètres de la base de données et du serveur :  
  
-   Pour le traitement multidimensionnel, consultez [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md).  
  
-   Pour le traitement tabulaire, consultez [Traiter une base de données, une table ou une partition &#40;Analysis Services&#41;](../../analysis-services/tabular-models/process-database-table-or-partition-analysis-services.md).  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-databasename-string"></a>-DatabaseName \<chaîne >  
 Spécifie la base de données tabulaire ou multidimensionnelle à traiter.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|0|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-servermicrosoftanalysissevicesserver"></a>-Server\<Microsoft.AnalysisSevices.Server >  
 Spécifie éventuellement l’instance de serveur à laquelle se connecter si vous n’utilisez pas le répertoire de fournisseur **SQLAS** correspondant au contexte.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-refreshtype-microsoftanalysisservicesrefreshtype"></a>-RefreshType \<Microsoft.AnalysisServices.RefreshType >  
 Spécifie le type de processus pour une base de données tabulaire.  Les valeurs valides sont Full, ClearValues, Calculate, DataOnly, Automatic, Add et Defragment. Consultez [Traiter une base de données, une table ou une partition &#40;Analysis Services&#41;](../../analysis-services/tabular-models/process-database-table-or-partition-analysis-services.md) pour des descriptions et des conseils.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|1|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-processtype-microsoftanalysisservicesprocesstype"></a>-ProcessType \<Microsoft.AnalysisServices.ProcessType >  
 Spécifie le type de processus pour une base de données multidimensionnelle ou tabulaire aux niveaux de compatibilité 1050-1103. Les valeurs valides sont ProcessFull, ProcessAdd, ProcessUpdate, ProcessIndexes, ProcessData, ProcessDefault, ProcessClear, ProcessStructure, ProcessCelarStructureOnly, ProcessScriptCache et ProcessRecalc. Consultez [Options et paramètres de traitement &#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/processing-options-and-settings-analysis-services.md) pour des descriptions et des conseils.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|1|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-credential"></a>-Credential  
 Si ce paramètre est déclaré, le nom d’utilisateur et le mot de passe transmis seront utilisés pour se connecter à l’instance d’Analysis Services. Si aucune information d’identification n’est indiquée, le compte Windows par défaut de l’utilisateur qui exécute le script est utilisé.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
## <a name="-whatif"></a>-Whatif  
 Incluez ce paramètre pour obtenir des informations sur l’impact de l’opération avant son exécution.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
## <a name="-confirm"></a>-Confirm  
 Incluez ce paramètre pour confirmer interactivement l’opération par une réponse (oui ou non) avant son exécution.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?||  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?||  
|Accepter les caractères génériques ?|false|  
  
## <a name="example-1-sqlas-provider"></a>Exemple 1 (fournisseur SQLAS)  
 Cet exemple utilise le fournisseur **SQLAS** pour définir comme contexte la liste des bases de données sur une instance par défaut.  Si vous répertoriez le contenu du répertoire de bases de données, vous verrez toutes les bases de données ainsi que leur état de processus et le mode lecture/écriture.  
  
 À partir du dossier de base de données, vous pouvez exécuter **Invoke-ProcessASDatabase** en spécifiant simplement le nom de la base de données.  
  
```  
PS > import-module SQLPS -DisableNameChecking  
PS  SQL Server > cd sqlas\ssas-srv-01\default\databases  
PS SQLSERVER:\sqlas\ssas-srv-01\default\databases> dir  
. . . .  
PS SQLSERVER:\sqlas\ssas-srv-01\default\databases> Invoke-ProcessASDatabase "adventureworks"  
  
```  
  
 Selon le type de base de données, vous devrez spécifier **RefreshType** ou **ProcessType**.  
  
 Le traitement est attesté par la présence d’un objet d’impact dans l’instruction de retour : Microsoft.AnalysisServices.Tabular.ObjectImpact.  
  
 Notez que les informations sur l’état de l’objet sont parfois mises en cache. Par conséquent, si vous affichez le contenu d’un répertoire à l’issue du traitement, l’état de la base de données conserve son descripteur « non traité » d’origine. Ceci est source de confusion, car tant que l’objet d’impact est renvoyé, la base de données est traitée.  
  
 Vous pouvez vérifier que le traitement a abouti en consultant la page des propriétés de la base de données dans Management Studio, en démarrant une nouvelle session ou en retournant l’état de traitement d’un objet de base de données (via [Microsoft.AnalysisServices.ProcessableMajorObject.LastProcessed](https://msdn.microsoft.com/library/microsoft.analysisservices.processablemajorobject.lastprocessed.aspx)).  
  
## <a name="example-2"></a>Exemple 2  
 Cet exemple montre la même opération en n’utilisant que l’applet de commande sans le fournisseur. Des paramètres supplémentaires sont requis pour spécifier le type de serveur et de processus.  
  
```  
PS > import-module SQLPS -DisableNameChecking  
PS  SQL Server >  Invoke-ProcessASDatabase -databasename "AdventureWorks" -server '\\server-name\instancename' –ProcessType "ProcessFull"  
  
```  
  
  
  
