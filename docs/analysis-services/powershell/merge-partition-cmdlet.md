---
title: Applet de commande Merge-Partition | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: powershell
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f78b946367c6bff01ab233cf5f4382ee3b9057e0
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="merge-partition-cmdlet"></a>Applet de commande Merge-Partition
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Fusionne les données d'une ou de plusieurs partitions sources dans une partition cible, puis supprime les partitions sources.  

>[!NOTE] 
>Cet article peut contenir des exemples et des informations obsolètes. Utilisez l’applet de commande Get-Help pour la dernière version.
  
## <a name="syntax"></a>Syntaxe  
 `Merge-ASDatabase [-Name] <string> [-SourcePartitions] <System.String[]> -Database <string> -Cube <string> -MeasureGroup <string> [-Server <string>] [-Credentials <PSCredential>] [<CommonParameters>]`  
  
 `Merge-ASDatabase -TargetPartition <Microsoft.AnalysisServices.Partition> [-SourcePartitions] <System.String[]> -Database <string> -Cube <string> -MeasureGroup <string> [-Server <string>] [-Credentials <PSCredential>] [<CommonParameters>]`  
  
## <a name="description"></a> Description  
 L'applet de commande Merge-Partition fusionne les données d'une ou de plusieurs partitions sources dans une partition cible, puis supprime les partitions sources. Les partitions ne peuvent être fusionnées que si elles répondent à tous les critères suivants :  
  
-   les partitions se trouvent dans le même groupe de mesures ;  
  
-   les partitions se trouvent sur le même ordinateur ;  
  
-   les partitions partagent le même mode de stockage (MOLAP, HOLAP et ROLAP pour les bases de données multidimensionnelles).  
  
## <a name="parameters"></a>Paramètres  
  
### <a name="-name-string"></a>-Name \<chaîne >  
 Spécifie la partition cible dans laquelle les données de partition source sont fusionnées. Cette partition doit déjà exister.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|0|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-sourcepartition-string"></a>-SourcePartition \<chaîne >  
 Spécifie la partition source qui sera fusionnée dans la partition cible. Vous pouvez créer une liste séparée par des virgules des partitions que vous voulez fusionner. Utilisez une variable pour stocker la liste. Par exemple $Sources=”Sales_2008”, “Sales_2009”, “Sales_2010”.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|1|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-database-string"></a>-De base de données \<chaîne >  
 Spécifie la base de données à laquelle les partitions appartiennent.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-cube-string"></a>-Cube \<chaîne >  
 Spécifie le cube auquel les partitions appartiennent.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-measuregroup-string"></a>Le groupe de mesures - \<chaîne >  
 Spécifie le groupe de mesures auquel la partition appartient.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-server-string"></a>-Serveur \<chaîne >  
 Spécifie l'instance Analysis Services à laquelle l'applet de commande se connectera et qu'il exécutera. Si aucun nom de serveur n'est fourni, une connexion est établie à localhost. Pour les instances par défaut, spécifiez simplement le nom du serveur. Pour les instances nommées, utilisez le format nom_serveur\nom_instance. Pour les connexions HTTP, utilisez le format http[s]://serveur[:port]/répertoirevirtuel/msmdpump.dll.  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut|localhost|  
|Accepter l'entrée de pipeline ?|false|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-credential-pscredential"></a>-Credential \<PSCredential >  
 Ce paramètre est utilisé pour transmettre un nom d'utilisateur et un mot de passe lors de l'utilisation d'une connexion HTTP à une instance Analysis Services, pour une instance que vous avez configurée pour l'accès HTTP. Pour plus d’informations, consultez [configurer l’accès HTTP à Analysis Services sur Internet Information Services &#40;IIS&#41; 8.0](../../analysis-services/instances/configure-http-access-to-analysis-services-on-iis-8-0.md) pour les connexions HTTP.  
  
 Si ce paramètre est indiqué, le nom d'utilisateur et le mot de passe seront utilisés pour la connexion à l'instance du serveur d'analyse spécifiée. Si aucune information d'identification n'est indiquée, le compte Windows par défaut de l'utilisateur qui exécute l'outil sera utilisé.  
  
 Pour utiliser ce paramètre, créez d’abord un objet PSCredential à l’aide de Get-Credential pour spécifier le nom d’utilisateur et le mot de passe (par exemple, `$Cred=Get-Credential “adventure-works\bobh”`. Vous pouvez ensuite canaliser cet objet vers le paramètre –Credential `(-Credential:$Cred`).  
  
|||  
|-|-|  
|Requis ?|false|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|True (ByValue)|  
|Accepter les caractères génériques ?|false|  
  
### <a name="-targetpartition-microsoftanalysisservicespartition"></a>-TargetPartition \<objet Microsoft.AnalysisServices.Partition >  
 Spécifie la partition cible dans laquelle les partitions sources sont fusionnées.  
  
|||  
|-|-|  
|Requis ?|true|  
|Position ?|nommée|  
|Valeur par défaut||  
|Accepter l'entrée de pipeline ?|true|  
|Accepter les caractères génériques ?|false|  
  
### <a name="commonparameters"></a>\<Paramètres_courants >  
 La commande cmdlet prend en charge les paramètres communs : -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer et -OutVariable. Pour plus d’informations, consultez [About_CommonParameters](http://go.microsoft.com/fwlink/?linkID=227825).  
  
## <a name="inputs-and-outputs"></a>Entrées et sorties  
 Le type d'entrée correspond au type des objets que vous pouvez canaliser vers l'applet de commande. Le type de retour correspond au type des objets retournés par l'applet de commande.  
  
|||  
|-|-|  
|Entrées|System.string|  
|Sorties|Aucun|  
  
## <a name="example-1"></a>Exemple 1  
 `PS SQL SERVER:\sqlas\locahost\default\Databases\AWTEST\Cubes\Adventure Works\MeasureGroups\sales orders\partitions> $Source=”Total_Orders_2001”, “Total_Orders_2002”, “Total_Orders_2003”` `PS SQL SERVER:\sqlas\locahost\default\Databases\AWTEST\Cubes\Adventure Works\MeasureGroups\sales orders\partitions> Merge-Partition –Name “Total_Orders_2004” –SourcePartitions:$Source –database “AWTEST” –cube “Adventure Works” –MeasureGroup “Sales Orders”`  
  
 Cette commande fusionne les partitions à partir de 2001, 2002 et 2003 dans la partition pour 2004, puis supprime les partitions des années précédentes.  
  

  
