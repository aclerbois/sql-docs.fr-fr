---
title: Introduction aux DiffGrams dans SQLXML 4.0 | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- annotations [SQLXML]
- DiffGrams [SQLXML], about DiffGrams
ms.assetid: 1902d67f-baf3-46e6-a36c-b24b5ba6f8ea
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7c897d32c8373301f62e83ef35f868a6435c3dcf
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="introduction-to-diffgrams-in-sqlxml-40"></a>Introduction aux DiffGrams dans SQLXML 4.0
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Cette rubrique fournit une brève introduction aux DiffGrams.  
  
## <a name="diffgram-format"></a>Format DiffGram  
 Voici le format DiffGram général :  
  
```  
<?xml version="1.0"?>  
<diffgr:diffgram   
         xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"  
         xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1"  
         xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
   <DataInstance>  
      ...  
   </DataInstance>  
   [<diffgr:before>  
        ...  
   </diffgr:before>]  
  
   [<diffgr:errors>  
        ...  
   </diffgr:errors>]  
</diffgr:diffgram>  
```  
  
 Le format DiffGram est constitué des blocs suivants :  
  
 **\<DataInstance>**  
 Le nom de cet élément, **DataInstance**, est utilisé à des fins d’explication dans cette documentation. Par exemple, si le DiffGram était généré à partir d’un jeu de données dans le .NET Framework, la valeur de la **nom** propriété du jeu de données serait utilisée comme le nom de cet élément. Ce bloc contient toutes les données pertinentes après la modification, y compris éventuellement les données qui n'ont pas été modifiées. La logique de traitement du DiffGram ignore les éléments de ce bloc pour lesquels le **diffgr : HasChanges** attribut n’est pas spécifié.  
  
 **\<diffgr : avant >**  
 Ce bloc facultatif contient les instances (éléments) de l'enregistrement d'origine qui doivent être mises à jour ou supprimées. Tables de la base de données en cours de modification (mise à jour ou supprimées) par le DiffGram doivent apparaître en tant qu’éléments de niveau supérieur dans le  **\<avant >** bloc.  
  
 **\<diffgr : Errors >**  
 Ce bloc facultatif est ignoré par la logique de traitement DiffGram.  
  
## <a name="diffgram-annotations"></a>Annotations DiffGram  
 Ces annotations sont définies dans l’espace de noms DiffGram **« urn : schemas-microsoft-com-diffgram-01 »**:  
  
 **id**  
 Cet attribut est utilisé pour associer les éléments dans le  **\<avant >** et  **\<DataInstance >** blocs.  
  
 **hasChanges**  
 Pour une insertion ou une opération de mise à jour, le DiffGram doit spécifier cet attribut avec la valeur **inséré** ou **modifié**. Si cet attribut n’est pas présent, l’élément correspondant dans le  **\<DataInstance >** est ignoré par le traitement logique et aucune mise à jour est effectuées. Pour obtenir des exemples fonctionnels, consultez [exemples de DiffGram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).  
  
 **parentID**  
 Cet attribut est utilisé pour spécifier les relations parent-enfant parmi les éléments du DiffGram. Cet attribut apparaît uniquement dans le \<avant > bloc. Il est utilisé par SQLXML lors de l'application de mises à jour. La relation parent-enfant est utilisée pour déterminer l'ordre dans lequel les éléments du DiffGram sont traités.  
  
## <a name="understanding-the-diffgram-processing-logic"></a>Fonctionnement de la logique de traitement DiffGram  
 La logique de traitement DiffGram utilise certaines règles pour déterminer si une opération est une opération d'insertion, de mise à jour ou de suppression. Ces règles sont décrites dans le tableau suivant.  
  
|Opération| Description|  
|---------------|-----------------|  
|Insert|Un DiffGram indique une opération d’insertion lorsqu’un élément apparaît dans le  **\<DataInstance >** bloc mais ne pas dans le  **\<avant >** bloc et le **diffgr : HasChanges** attribut est spécifié (**diffgr : HasChanges = inséré**) sur l’élément. Dans ce cas, le DiffGram insère l’instance de l’enregistrement qui est spécifié dans le  **\<DataInstance >** bloc dans la base de données.<br /><br /> Si le **diffgr : HasChanges** attribut n’est pas spécifié, l’élément est ignoré par la logique de traitement et aucune insertion n’est effectuée. Pour obtenir des exemples fonctionnels, consultez [exemples de DiffGram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).|  
|Update|Le DiffGram indique une opération de mise à jour lorsqu’il existe un élément dans le \<avant > bloc pour lesquelles il existe un élément correspondant dans le  **\<DataInstance >** bloc (autrement dit, les deux éléments ont un **diffgr : ID** attribut avec la même valeur) et le **diffgr : HasChanges** attribut est spécifié avec la valeur **modifié** sur l’élément dans le  **\<DataInstance >** bloc.<br /><br /> Si le **diffgr : HasChanges** attribut n’est pas spécifié sur l’élément dans le  **\<DataInstance >** bloc, une erreur est retournée par la logique de traitement. Pour obtenir des exemples fonctionnels, consultez [exemples de DiffGram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).<br /><br /> Si **diffgr : parentId** est spécifié dans le  **\<avant >** bloquer, la relation parent-enfant des éléments qui sont spécifiées par **parentID** sont utilisées pour déterminer l’ordre dans lequel les enregistrements sont mis à jour.|  
|Supprimer|Un DiffGram indique une opération de suppression lorsqu’un élément apparaît dans le  **\<avant >** bloc mais ne pas dans le correspondant  **\<DataInstance >** bloc. Dans ce cas, le DiffGram supprime l’instance de l’enregistrement qui est spécifié dans le  **\<avant >** bloc à partir de la base de données. Pour obtenir des exemples fonctionnels, consultez [exemples de DiffGram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md).<br /><br /> Si **diffgr : parentId** est spécifié dans le  **\<avant >** bloquer, la relation parent-enfant des éléments qui sont spécifiées par **parentID** sont utilisées pour déterminer l’ordre dans lequel les enregistrements sont supprimés.|  
  
> [!NOTE]  
>  Les paramètres ne peuvent pas être passés aux DiffGrams.  
  
  
