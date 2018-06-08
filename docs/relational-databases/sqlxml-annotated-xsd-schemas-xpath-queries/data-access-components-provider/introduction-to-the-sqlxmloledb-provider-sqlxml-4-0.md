---
title: Introduction au fournisseur SQLXMLOLEDB (SQLXML 4.0) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQLXMLOLEDB Provider, properties
- adExecuteStream flag
- SQLXMLOLEDB Provider, about SQLXMLOLEDB Provider
ms.assetid: 2e3f3817-4209-4bf4-9f46-248c95bc6f1b
caps.latest.revision: 16
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 602175a93d845693414d56e3f79048ef8560b05c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="introduction-to-the-sqlxmloledb-provider-sqlxml-40"></a>Présentation du fournisseur SQLXMLOLEDB (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Le fournisseur SQLXMLOLEDB est un fournisseur OLE DB qui expose des fonctionnalités [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML par l'intermédiaire d'objets ADO (ActiveX Data Objects). Toutefois, le fournisseur peut uniquement exécuter des commandes dans le mode « écriture dans un flux de sortie » de l'objet ADO. Le fournisseur SQLXMLOLEDB n'est pas un fournisseur d'ensembles de lignes. Lorsque vous exécutez une commande, vous devez spécifier l’indicateur adExecuteStream, qui indique à ADO d’utiliser le flux de sortie que vous avez spécifié.  
  
 L’exemple suivant montre la syntaxe de la commande d’exécution dans lequel l’indicateur adExecuteStream est spécifiée :  
  
```  
Dim oTestCommand As New ADODB.Command  
...  
oTestCommand.Properties("Output Stream").Value = oTestStream  
oTestCommand.Execute , , adExecuteStream  
...  
```  
  
## <a name="sqlxmloledb-provider-specific-properties"></a>Propriétés spécifiques au fournisseur SQLXMLOLEDB  
 Le fournisseur SQLXMLOLEDB expose la propriété de connexion spécifique au fournisseur suivante.  
  
|Connexion<br /><br /> propriété|Par défaut<br /><br /> (le cas échéant)| Description|  
|-----------------------------|----------------------------|-----------------|  
|Fournisseur de données||Fournit l'identificateur PROGID du fournisseur OLE DB par l'intermédiaire duquel SQLXMLOLEDB exécute les commandes. À compter de SQLXML 4.0 et de [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], ce fournisseur est contenu dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ; par conséquent, cette valeur de propriété est limitée à « SQLNCLI11 ». Pour plus d’informations, consultez [Programmation de SQL Server Native Client](../../../relational-databases/native-client/sql-server-native-client-programming.md).|  
  
 Le fournisseur SQLXMLOLEDB expose les propriétés de commande spécifiques au fournisseur suivantes.  
  
|Command<br /><br /> propriété|Par défaut<br /><br /> (le cas échéant)| Description|  
|--------------------------|----------------------------|-----------------|  
|Chemin d’accès de base|""|Spécifie le chemin d'accès au fichier de base. Le chemin d'accès au fichier de base est utilisé pour spécifier l'emplacement des fichiers XSL (XML Stylesheet Language) ou de schéma de mappage. Le chemin d’accès du fichier de base est également utilisé pour résoudre les chemins d’accès relatifs de XSL ou le mappage de fichiers de schéma qui ont été spécifiées dans les propriétés XSL ou le schéma de mappage.<br /><br /> Pour obtenir un exemple dans lequel cette propriété est utilisée, consultez [l’exécution des requêtes XPath &#40;fournisseur SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-sqlxmloledb-provider.md).|  
|ClientSideXML|False|Attribuez la valeur True à cette propriété si vous souhaitez que le processus de conversion de l'ensemble de lignes en XML se produise sur le client et non sur le serveur. Cela s'avère utile si vous souhaitez déplacer la charge de performance vers le niveau intermédiaire.<br /><br /> Pour obtenir un exemple dans lequel cette propriété est utilisée, consultez [l’exécution des requêtes SQL &#40;fournisseur SQLXMLOLEDB&#41; ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-sql-queries-sqlxmloledb-provider.md) ou [l’exécution de modèles que contiennent des requêtes SQL &#40;fournisseur SQLXMLOLEDB&#41; ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-templates-that-contain-sql-queries-sqlxmloledb-provider.md).|  
|Type de contenu||Retourne le type de contenu de sortie. Il s'agit d'une propriété READ ONLY.<br /><br /> Cette propriété fournit des informations au navigateur sur le type de contenu (telles que TEXT/XML, TEXT/HTML, image/jpeg et ainsi de suite). La valeur de cette propriété devient le **type de contenu** champ qui est envoyé au navigateur dans le cadre de l’en-tête HTTP, qui contient le type MIME (Multipurpose Internet Mail Extensions) du document envoyé comme corps.|  
|Schéma de mappage|NULL|Si une application cliente exécute une requête XPath sur un schéma de mappage (XDR ou XSD), cette propriété est utilisée pour spécifier le nom du schéma de mappage.<br /><br /> Le chemin d'accès spécifié peut être relatif (xyz/abc/MonSchéma.xml) ou absolu (C:\MonDossier\abc\MonSchéma.xml).<br /><br /> Si un chemin d’accès relatif est spécifié, le chemin d’accès de base qui est spécifié par la propriété de chemin d’accès de Base permet de résoudre le chemin d’accès relatif. Si aucun chemin d’accès n’a été spécifié dans la propriété de chemin d’accès de Base, le chemin d’accès relatif est relatif au répertoire en cours.<br /><br /> Si vous spécifiez une valeur pour la propriété de schéma de mappage, vous pouvez spécifier un chemin d’accès du répertoire local ou une URL (http://...). Si vous spécifiez une URL, vous devez configurer WinHTTP de telle sorte qu'il puisse accéder aux serveurs HTTP et HTTPS par le biais d'un serveur proxy. Pour cela, vous pouvez exécuter l'utilitaire Proxycfg.exe. Pour plus d'informations, consultez l'article relatif à l'utilisation de l'utilitaire de configuration de l'utilitaire WinHTTP dans MSDN Library.<br /><br /> Pour obtenir un exemple dans lequel cette propriété est utilisée, consultez [l’exécution des requêtes XPath &#40;fournisseur SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-sqlxmloledb-provider.md).|  
|espaces de noms||Cette propriété active l'exécution de requêtes XPath qui utilisent des espaces de noms. Pour obtenir un exemple dans lequel cette propriété est utilisée, consultez [l’exécution de requêtes XPath avec des espaces de noms &#40;fournisseur SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-xpath-queries-with-namespaces-sqlxmloledb-provider.md).|  
|ss Stream Flags||Cette propriété est utilisée pour spécifier des types particuliers de restrictions de sécurité. Par exemple, vous souhaitez peut-être interdire les références URL à des fichiers ou les chemins d'accès absolus à des fichiers (tels que des sites externes). Ou vous souhaitez peut-être interdire les requêtes dans les modèles.<br /><br /> Ces valeurs peuvent être attribuées à la propriété :<br /><br /> 1 = STREAM_FLAGS_DISALLOW_URL 2 = STREAM_FLAGS_DISALLOW_ABSOLUTE_PATH 4 = STREAM_FLAGS_DISALLOW_QUERY 8 = STREAM_FLAGS_ DONTCACHEMAPPINGSCHEMA 16 = STREAM_FLAGS_DONTCACHETEMPLATE 32 = STREAM_FLAGS_DONTCACHEXSL<br /><br /> Des informations supplémentaires sur ces valeurs sont fournies dans le tableau suivant.|  
|xml root||Cette propriété est utilisée pour définir une balise racine pour le document XML résultant. Par exemple, si vous exécutez des requêtes SQL sur la base de données et que le document XML résultant ne comporte aucun élément racine unique, la valeur de la propriété est utilisée pour ajouter un élément racine unique au document.<br /><br /> Pour obtenir un exemple dans lequel cette propriété est utilisée, consultez [l’exécution des requêtes SQL &#40;fournisseur SQLXMLOLEDB&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/executing-sql-queries-sqlxmloledb-provider.md).|  
|xsl||Cette propriété est utilisée pour spécifier le nom du fichier XSL lorsque vous souhaitez appliquer la transformation XSL au document XML retourné par la requête.<br /><br /> Le chemin d'accès spécifié peut être relatif (xyz/abc/MonFichierXSL.xsl) ou absolu (C:\MonDossier\abc\MonFichierXSL.xsl).<br /><br /> Si un chemin d’accès relatif est spécifié, le chemin d’accès de base qui est spécifié par la propriété de chemin d’accès de Base permet de résoudre le chemin d’accès relatif. Si aucun chemin d’accès n’a été spécifié dans la propriété de chemin d’accès de Base, le chemin d’accès relatif est relatif au répertoire en cours.<br /><br /> Pour obtenir un exemple dans lequel cette propriété est utilisée, voir appliquer une Transformation XSL (fournisseur SQLXMLOLEDB).|  
  
 Le tableau suivant décrit le valeurs de propriété des indicateurs de flux ss.  
  
|Valeur de propriété| Description|  
|--------------------|-----------------|  
|STREAM_FLAGS_DISALLOW_URL|Les URL ne sont pas acceptées pour les fichiers de schéma de mappage ou XSL.|  
|STREAM_FLAGS_DISALLOW_ABSOLTE_PATH|Un chemin d'accès spécifié pour un fichier de schéma de mappage ou XSL doit être relatif au chemin d'accès de base du modèle lui-même.|  
|STREAM_FLAGS_DISALLOW_QUERY|Les requêtes ne sont pas autorisées dans un modèle.|  
|STREAM_FLAGS_DONTCACHEMAPPINGSCHEMA|Le schéma de mappage n'est pas mis en cache. Cette valeur de propriété est utile au cours de la phase de développement de la base de données, lorsque les schémas de base de données font l'objet de modifications.|  
|STREAM_FLAGS_DONTCACHETEMPLATE|Les modèles ne sont pas mis en cache.|  
|STREAM_FLAGS_DONTCACHEXSL|Le fichier XSL n'est pas mis en cache.|  
  
  
