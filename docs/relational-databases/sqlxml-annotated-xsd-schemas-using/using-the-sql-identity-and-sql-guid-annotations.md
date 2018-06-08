---
title: 'À l’aide des Annotations SQL : Identity et SQL : GUID | Documents Microsoft'
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
- sql:guid
- annotated XSD schemas, IDENTITY-type columns
- annotated XSD schemas, GUID values
- DiffGrams [SQLXML], annotations
- identity annotation
- XSD schemas [SQLXML], GUID values
- GUIDs [SQLXML]
- guid annotation
- sql:identity
- IDENTITY-type column
- XSD schemas [SQLXML], IDENTITY-type columns
- updategrams [SQLXML], GUID values
ms.assetid: 7661dfd0-6573-4692-a8f1-3597adcd33c4
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: da967220c77451d807a79d2e10701fd1f76791d2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-the-sqlidentity-and-sqlguid-annotations"></a>Utilisation des annotations sql:identity et sql:guid
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Vous pouvez spécifier le **: Identity** et **SQL : GUID** annotations dans un schéma XSD pour n’importe quel nœud mappé à une colonne de base de données dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Alors que le format de la mise à jour prend en charge la **updg : à l’identité** et **updg : GUID** attributs, le format DiffGram est pas le cas. Le **updg : à l’identité** attribut définit le comportement de mise à jour d’une colonne de type IDENTITY. Le **updg : GUID** attribut vous permet d’obtenir une valeur GUID à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et l’utiliser dans la mise à jour. Pour plus d’informations et des exemples fonctionnels, consultez [insertion de données à l’aide de programmes &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md).  
  
 Le **: Identity** et **SQL : GUID** annotations étendent cette fonctionnalité aux DiffGrams.  
  
 Lorsque vous exécutez un DiffGram, il est d'abord converti en un code de mise à jour (updategram), puis le code de mise à jour (updategram) est exécuté. En spécifiant le **: Identity** et **SQL : GUID** annotations dans le schéma XSD, sont en fait définissant le comportement d’une mise à jour. Par conséquent, toutes les annotations sont décrites dans le contexte d'un code de mise à jour (updategram). Les annotations peuvent être utilisées à la fois pour les DiffGrams et les codes de mise à jour (updategram) ; toutefois, les codes de mise à jour (updategram) offrent déjà un moyen plus puissant pour gérer l'identité et les valeurs GUID.  
  
 Le **: Identity** et **SQL : GUID** annotations peuvent être définies sur un élément de contenu complexe.  
  
## <a name="sqlidentity-annotation"></a>Annotation sql:identity  
 Vous pouvez spécifier le **: Identity** annotation dans le schéma XSD sur n’importe quel nœud mappé à une colonne de base de données de type IDENTITY. La valeur spécifiée pour cette annotation définit comment la colonne de type IDENTITY est mise à jour (en utilisant la valeur fournie dans le code de mise à jour (updategram) pour modifier la colonne ou en ignorant la valeur, auquel cas une valeur générée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est utilisée pour cette colonne).  
  
 Le **: Identity** annotation peut avoir deux valeurs :  
  
 ignore  
 Ordonne au code de mise à jour (updategram) d'ignorer toute valeur fournie dans le code de mise à jour (updategram) pour cette colonne et de compter sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour générer la valeur d'identité.  
  
 useValue  
 Ordonne au code de mise à jour (updategram) d'utiliser la valeur fournie dans le code de mise à jour (updategram) pour mettre à jour la colonne de type IDENTITY. Un code de mise à jour (updategram) ne vérifie pas si la colonne est une valeur d'identité ou pas.  
  
 Si la mise à jour Spécifie une valeur pour la colonne de type IDENTITY, le **: Identity = « useValue »** doit être spécifié dans le schéma.  
  
## <a name="sqlguid-annotation"></a>Annotation sql:guid  
 Un code de mise à jour (updategram) peut faire en sorte que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une valeur GUID, puis utiliser cette valeur dans le code de mise à jour (updategram). Dans le contexte de DiffGrams, vous pouvez utiliser la **SQL : GUID** annotation pour spécifier s’il faut utiliser une valeur GUID générée par SQL Server ou la valeur est fournie dans la mise à jour pour cette colonne.  
  
 Le **SQL : GUID** annotation peut avoir deux valeurs :  
  
 generate  
 Spécifie que le GUID généré par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être utilisé pour cette colonne de l'opération de mise à jour.  
  
 useValue  
 Spécifie que la valeur spécifiée dans le code de mise à jour (updategram) doit être utilisée pour la colonne. Ceci est la valeur par défaut.  
  
  
