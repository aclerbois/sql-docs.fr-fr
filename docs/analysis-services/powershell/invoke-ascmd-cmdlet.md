---
title: Applet de commande Invoke-ASCmd | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cf570da0e7be70fda804a3f17d11f6c8498c1605
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="invoke-ascmd-cmdlet"></a>Applet de commande Invoke-ASCmd
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Permet à un administrateur de base de données d’exécuter un script XMLA, des instructions MDX (Multidimensional Expressions) ou DMX (Data Mining Extensions) ou un script TMSL (Tabular Model Scripting Language).  
  
 Ce langage est uniquement pris en charge pour le mode serveur tabulaire sur une instance SQL Server 2016 Analysis Services.  
  
 Si vous souhaitez créer des bases de données ou d’autres objets, vous utilisez l’applet de commande avec un fichier de script d’entrée.  
  
## <a name="syntax"></a>Syntaxe  
 `Invoke-ASCmd –Query <string> [-Server <string>] [-Database <string>] [-Credential <PSCredential>] [-ConnectionTimeout <int>] [-QueryTimeout <int>] [-Variable <string[]>] [-TraceFile <string>] [-TraceFileFormat <TraceFileFormatOption>] [-TraceFileDelimiter <string>] [-TraceTimeout <int>] [-TraceLevel <TraceLevelOption>] [<CommonParameters>]`  
  
 `Invoke-ASCmd –InputFile <string> [-Server <string>] [-Database <string>] [-Credential <PSCredential>] [-ConnectionTimeout <int>] [-QueryTimeout <int>] [-Variable <string[]>] [-TraceFile <string>] [-TraceFileFormat <TraceFileFormatOption>] [-TraceFileDelimiter <string>] [-TraceTimeout <int>] [-TraceLevel <TraceLevelOption>] [<CommonParameters>]`  
  
## <a name="description"></a> Description  
 L'applet de commande Invoke-ASCmd peut exécuter des requêtes ou des scripts contenus dans les fichiers d'entrée.  
  
 Pour XMLA, les commandes suivantes sont prises en charge : Alter, Backup, Batch, BeginTransaction, Cancel, ClearCache, CommitTransaction, Create, Delete, DesignAggregations, Drop, Insert, Lock, MergePartitions, NotifyTableChange, Process, Restore, RollbackTransaction, Statement (utilisée pour l’exécution de requêtes MDX et d’instructions DMX), Subscribe, Synchronize, Unlock, Update et UpdateCells.  
  
 Pour TMSL, il s’agit des commandes suivantes : Alter, Create, Delete, MergePartitions, Process, Update.  
  
 Cette applet de commande prend en charge le paramètre –Credential, qui peut être utilisé si vous avez configuré l'instance Analysis Services pour l'accès HTTP. Le paramètre –Credential accepte un objet PSCredential qui fournit une identité d'utilisateur Windows. IIS emprunte l'identité de cet utilisateur lors de la connexion à Analysis Services. L'identité doit avoir des autorisations d'administrateur système sur l'instance Analysis Services pour exécuter le script.  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-query-string"></a>-Requête \<chaîne >  
 Spécifie le script, la requête ou l'instruction directement sur la ligne de commande plutôt que dans un fichier. Vous pouvez également spécifier une requête comme entrée de pipeline. Vous devez spécifier une valeur pour le paramètre **–InputFile** ou **–Query** quand vous utilisez **Invoke-AsCmd**.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|True (ByValue)|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-inputfile-string"></a>-InputFile \<chaîne >  
 Identifie le fichier qui contient le script XMLA, la requête MDX, l’instruction DMX ou le script TMSL (dans JSON). Vous devez spécifier une valeur pour le paramètre **–InputFile** ou **–Query** quand vous utilisez **Invoke-AsCmd**.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-server-string"></a>-Serveur \<chaîne >  
 Spécifie l'instance Analysis Services à laquelle l'applet de commande se connectera et qu'il exécutera. Si aucun nom de serveur n'est fourni, une connexion sera établie à localhost. Pour les instances par défaut, spécifiez simplement le nom du serveur. Pour les instances nommées, utilisez le format nom_serveur\nom_instance. Pour les connexions HTTP, utilisez le format http[s]://serveur[:port]/répertoirevirtuel/msmdpump.dll.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|localhost|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-database-string"></a>-De base de données \<chaîne >  
 Indique la base de données dans laquelle la requête MDX ou l'instruction DMX s'exécutera. Le paramètre de base de données est ignoré lorsque l'applet de commande exécute un script XMLA, car le nom de la base de données est incorporé dans le script XMLA.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-credential-pscredential"></a>-Credential \<PSCredential >  
 Spécifie un objet PSCredential qui fournit un nom et un mot de passe d'utilisateur Windows. Spécifiez ce paramètre uniquement si l'instance Analysis Services est configurée pour l'accès HTTP, à l'aide de l'authentification de base. Pour les connexions natives utilisant la sécurité intégrée, ce paramètre est ignoré.  
  
 Si ce paramètre est présent, les informations d'identification qu'il fournit sont ajoutées à la chaîne de connexion. IIS emprunte l'identité de cet utilisateur lors de la connexion à Analysis Services. Si aucune information d'identification n'est indiquée, le compte Windows par défaut de l'utilisateur qui exécute l'outil sera utilisé.  
  
 Pour utiliser ce paramètre, créez d’abord un objet PSCredential à l’aide de Get-Credential pour spécifier le nom d’utilisateur et le mot de passe, par exemple `$Cred=Get-Credential “adventure-works\admin”`). Vous pouvez ensuite canaliser cet objet vers le paramètre –Credential `(-Credential:$Cred`).  
  
   Pour plus d’informations sur l’accès HTTP, consultez [Configurer l’accès HTTP à Analysis Services sur Internet Information Services &#40;IIS&#41; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md).  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|True (ByValue)|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-connectiontimeout-int"></a>-ConnectionTimeout \<int >  
 Indique le nombre de secondes avant l'expiration de la connexion à l'instance Analysis Services. La valeur du délai d'attente doit être un entier compris entre 0 et 65534. Si la valeur 0 est spécifiée, les tentatives de connexion n'expirent pas.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|30|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-querytimeout-int"></a>-QueryTimeout \<int >  
 Spécifie le nombre de secondes avant l'expiration des requêtes. Si la valeur du délai d'attente n'est pas spécifiée, les requêtes n'expirent pas. La valeur du délai d'attente doit être un entier compris entre 1 et 65535.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|30|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-variable-string"></a>-Variable \<string [] >  
 Spécifie des variables de script supplémentaires. Chaque variable est une paire nom-valeur. Si la valeur contient des espaces ou des caractères de contrôle incorporés, elle doit figurer entre guillemets doubles. Utilisez un tableau PowerShell pour spécifier plusieurs variables et les valeurs correspondantes.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-tracefile-string"></a>Le fichier de trace - \<chaîne >  
 Identifie un fichier qui reçoit des événements de trace Analysis Services lors de l'exécution du script XMLA, de la requête MDX ou de l'instruction DMX. Si le fichier existe déjà, il est automatiquement remplacé (à l'exception des fichiers de trace créés à l'aide des paramètres -TraceLevel:Duration et –TraceLevel:DurationResult). Les noms de fichiers qui contiennent des espaces doivent être mis entre guillemets (" "). Si le nom de fichier n'est pas valide, un message d'erreur est généré.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-tracefileformat-string"></a>-TraceFileFormat \<chaîne >  
 Indique le format de fichier pour le paramètre –TraceFile (si ce paramètre est spécifié). Les options disponibles sont texte ou csv. La valeur par défaut est « csv ».  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|csv|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-tracefiledelimiter-string"></a>-TraceFileDelimiter \<chaîne >  
 Spécifie le caractère à utiliser comme délimiteur de fichier de trace lorsque vous spécifiez csv comme format de fichier de trace. Le délimiteur par défaut est | (barre verticale).  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-tracetimeout-int"></a>-TraceTimeout \<int >  
 Indique le nombre de secondes pendant lesquelles le moteur Analysis Services patiente avant d'achever la trace (si vous spécifiez le paramètre –TraceFile). Une trace est considérée terminée si aucun message de trace n'a été enregistré au cours de la période spécifiée. La valeur de délai d'attente de la trace par défaut est de 5 secondes.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|5|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-tracelevel-traceleveloption"></a>-TraceLevel \<TraceLevelOption >  
 Indique quelles données sont collectées et enregistrées dans le fichier de trace. Les valeurs possibles sont High, Medium, Low, Duration, DurationResult.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|Élevée|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="commonparameters"></a>\<Paramètres_courants >  
 La commande cmdlet prend en charge les paramètres communs : -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer et -OutVariable. Pour plus d’informations, consultez [About_CommonParameters](http://go.microsoft.com/fwlink/?linkID=227825).  
  
## <a name="inputs-and-outputs"></a>Entrées et sorties  
 Le type d'entrée correspond au type des objets que vous pouvez canaliser vers l'applet de commande. Le type de retour correspond au type des objets retournés par l'applet de commande.  
  
|||  
|-|-|  
|Entrées|PSObject|  
|Sorties|Chaîne|  
  
## <a name="example-1-xmla-input-file"></a>Exemple 1 (fichier d’entrée XMLA)  
  
```  
Invoke-ASCmd –InputFile:"C:\MyFolder\DiscoverConnections.xmla"  
```  
  
 Cette commande exécute un script XMLA qui retourne la liste des connexions actives sur le serveur. Le fichier DiscoverConnections.xmla contient le script XMLA suivant :  
  
```  
<Discover xmlns="urn:schemas-microsoft-com:xml-analysis">  
<RequestType>DISCOVER_CONNECTIONS</RequestType>  
<Restrictions />  
   <Properties>  
      <PropertyList>  
         <Content>Data</Content>  
      </PropertyList>  
   </Properties>  
</Discover>  
```  
  
## <a name="example-2-tmsl-input-file"></a>Exemple 2 (fichier d’entrée TMSL)  
 Cet exemple est identique au premier, sauf que le script est de type TMSL (JSON) et nécessite une instance tabulaire de SQL Server 2016. Vous pouvez générer un script TMSL dans SQL Server Management Studio.  
  
 Si vous disposez de plusieurs instances et que votre instance tabulaire est une instance nommée, n’oubliez pas de définir le nom du serveur :  
  
```  
Invoke-ASCmd –InputFile "C:\folder-name\T1200-NewDB.json" -Server "server-name\instance-name"  
```  
  
## <a name="example-3-query"></a>Exemple 3 (requête)  
  
```  
Invoke-ASCmd -Database:"Adventure Works DW" -Query:"<Discover xmlns='urn:schemas-microsoft-com:xml analysis'><RequestType>DISCOVER_DATASOURCES</RequestType><Restrictions></Restrictions><Properties></Properties></Discover>"  
```  
  
 La requête Discover XMLA retourne les sources de données disponibles pour le serveur d'analyse et les informations requises pour se connecter à celles-ci. Les résultats sont en XML. Pour une meilleure lisibilité, vous pouvez diriger la sortie vers un fichier XML (par exemple, ajoutez `| Out-file C:\Results\XMLAQueryOutput.xml` à la commande) et afficher les résultats dans un navigateur ou une autre application qui prend en charge le code XML structuré.  
  
  
  
