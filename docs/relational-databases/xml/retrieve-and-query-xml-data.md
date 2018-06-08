---
title: Récupérer et interroger des données XML | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- XML data [SQL Server], retrieving
- XML instance retrieval
ms.assetid: 24a28760-1225-42b3-9c89-c9c0332d9c51
caps.latest.revision: 15
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 94717703d4601b6c8ddd29cfbf2d7849b73528be
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="retrieve-and-query-xml-data"></a>Récupérer et interroger des données XML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit les options de requête que vous devez spécifier pour interroger les données XML. Elle décrit aussi les parties des instances XML qui ne sont pas conservées lorsqu'elles sont stockées dans des bases de données.  
  
##  <a name="features"></a> Fonctionnalités d'une instance XML qui ne sont pas conservées  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve le contenu de l’instance XML, mais ne conserve pas les aspects de l’instance XML qui ne sont pas considérés significatifs dans le modèle de données XML. Cela signifie qu'une instance XML extraite peut ne pas être identique à l'instance stockée sur le serveur, mais contiendra les mêmes informations.  
  
### <a name="xml-declaration"></a>Déclaration XML  
 La déclaration XML d'une instance n'est pas conservée lors du stockage de l'instance dans la base de données. Exemple :  
  
```  
CREATE TABLE T1 (Col1 int primary key, Col2 xml)  
GO  
INSERT INTO T1 values (1, '<?xml version="1.0" encoding="windows-1252" ?><doc></doc>')  
GO  
SELECT Col2  
FROM T1  
```  
  
 Le résultat est `<doc/>`.  
  
 La déclaration XML, telle que `<?xml version='1.0'?>`, n’est pas conservée en cas de stockage des données XML dans une instance de type de données **xml** . C'est la procédure normale. La déclaration XML () et ses attributs (version/encoding/stand-alone) sont perdus une fois les données converties au type **xml**. La déclaration XML est traitée comme une directive de l'analyseur XML. Les données XML sont stockées en interne au format ucs-2. Toutes les autres instructions de traitement dans l'instance XML sont conservées.  
  
  
### <a name="order-of-attributes"></a>Ordre des attributs  
 L'ordre des attributs d'une instance XML n'est pas conservé. Quand vous interrogez l’instance XML stockée dans la colonne de type **xml** , l’ordre des attributs du code XML résultant peut différer de l’ordre utilisé dans l’instance XML d’origine.  
  
  
### <a name="quotation-marks-around-attribute-values"></a>Guillemets autour des valeurs d'attributs  
 Les guillemets et les guillemets simples qui encadrent les valeurs des attributs ne sont pas conservés. Les valeurs des attributs sont stockées sous forme de paire nom et valeur. Les guillemets ne sont pas stockés. En cas d'exécution d'une requête XQuery sur une instance XML, le code XML résultant est sérialisé, et les valeurs des attributs figurent entre guillemets.  
  
```  
DECLARE @x xml  
-- Use double quotation marks.  
SET @x = '<root a="1" />'  
SELECT @x  
GO  
DECLARE @x xml  
-- Use single quotation marks.  
SET @x = '<root a=''1'' />'  
SELECT @x  
GO  
```  
  
 Les deux requêtes retournent = `<root a="1" />`.  
  
  
### <a name="namespace-prefixes"></a>Préfixes d'espace de noms  
 Les préfixes d'espace de noms ne sont pas conservés. Quand vous définissez une requête XQuery sur une colonne de type **xml** , le code XML sérialisé qui en découle peut retourner des préfixes d’espace de noms différents.  
  
```  
DECLARE @x xml  
SET @x = '<ns1:root xmlns:ns1="abc" xmlns:ns2="abc">  
            <ns2:SomeElement/>  
          </ns1:root>'  
SELECT @x  
SELECT @x.query('/*')  
GO  
```  
  
 Le préfixe d'espace de noms peut avoir une valeur différente dans le résultat. Exemple :  
  
```  
<p1:root xmlns:p1="abc"><p1:SomeElement/></p1:root>  
```  
  
  
##  <a name="query"></a> Définition des options requises de requête  
 Quand vous interrogez des variables ou colonnes de type **xml** à l’aide de méthodes de type de données **xml** , vous devez définir les options suivantes, comme indiqué.  
  
|Options SET|Valeurs requises|  
|-----------------|---------------------|  
|ANSI_NULLS|ON|  
|ANSI_PADDING|ON|  
|ANSI_WARNINGS|ON|  
|ARITHABORT|ON|  
|CONCAT_NULL_YIELDS_NULL|ON|  
|NUMERIC_ROUNDABORT|OFF|  
|QUOTED_IDENTIFIER|ON|  
  
 Si les options ne sont pas définies comme indiqué, les requêtes et les modifications portant sur les méthodes de type de données **xml** sont vouées à l’échec.  
  
  
## <a name="see-also"></a> Voir aussi  
 [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md)  
  
  
