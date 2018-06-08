---
title: Applet de commande Invoke-ProcessTable | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8fde6817c48df9aa5c3e663a5b76223be84f1a69
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="invoke-processtable-cmdlet"></a>Applet de commande Invoke-ProcessTable
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Exécute l’opération **Process** sur une **Table** avec un **RefreshType**spécifique.  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
## <a name="syntax"></a>Syntaxe  
 `Invoke-ProcessTable [-DatabaseName] <string> [-TableName] <string> [-RefreshType] <RefreshType> {Full |     ClearValues | Calculate | DataOnly | Automatic | Add | Defragment} [-Server <string>] [-Credential <pscredential>     [-WhatIf] [-Confirm]  [<CommonParameters>]`  
  
 `Invoke-ProcessTable -RefreshType <RefreshType> {Full | ClearValues | Calculate | DataOnly | Automatic | Add |     Defragment} -Table <Table> [-Server <string>] [-Credential <pscredential>] [-WhatIf] [-Confirm]     [<CommonParameters>]`  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-tablename-string"></a>-TableName \<chaîne >  
 Nom de la table à laquelle appartient la partition à traiter.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|0|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-databasename-string"></a>-DatabaseName \<chaîne >  
 Spécifie la base de données à laquelle appartient la table.  
  
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
  
### <a name="-credential"></a>-Credential  
 Si ce paramètre est déclaré, le nom d’utilisateur et le mot de passe transmis seront utilisés pour se connecter à l’instance d’Analysis Services. Si aucune information d’identification n’est indiquée, le compte Windows par défaut de l’utilisateur qui exécute le script est utilisé.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-whatif"></a>-Whatif  
 Incluez ce paramètre pour obtenir des informations sur l’impact de l’opération avant son exécution.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-confirm"></a>-Confirm  
 Incluez ce paramètre pour confirmer interactivement l’opération par une réponse (oui ou non) avant son exécution.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?||  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?||  
|Accepter les caractères génériques ?|false|  
  
## <a name="example-1"></a>Exemple 1  
 `PS SQLSERVER:\SQLAS\MachineName\Instance\Databases\DB1\> Invoke-ProcessTable -TableName "myTable" -Database "DB1"  -RefreshType "Full"`  
  
 Cette commande fournit l’identité de la table à traiter.  
  
## <a name="example-2"></a>Example 2  
 `PS SQLSERVER:\SQLAS\MachineName\Instance\Databases\DB1\> Invoke-ProcessTable -TableName "myTable" -Database "DB1"  -RefreshType [Microsoft.AnalysisServices.Tabular.RefreshType]::Full`  
  
 Cette commande traite une table de métadonnées tabulaires avec un type d’actualisation **enum** .  
  
  
