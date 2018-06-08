---
title: Read (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 7/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- Read_TSQL
- Read
dev_langs:
- TSQL
helpviewer_keywords:
- Read [Database Engine]
ms.assetid: f2b8207c-b69f-4327-a874-100b3a1f27d8
caps.latest.revision: 13
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 60575f2c5c5f1de4a7fa5b2531fb8431f13b4441
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="read-database-engine"></a>Read (moteur de base de données)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Read lit la représentation binaire de **SqlHierarchyId** à partir du **BinaryReader** passé et définit l’objet **SqlHierarchyId** sur cette valeur. Read ne peut pas être appelée au moyen de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Utilisez plutôt CAST ou CONVERT.
  
## <a name="syntax"></a>Syntaxe  
  
```sql
void Read( BinaryReader r )   
```  
  
## <a name="arguments"></a>Arguments  
*r*  
 Objet **BinaryReader** qui produit un flux binaire correspondant à une représentation binaire d’un nœud **hierarchyid**.  
  
## <a name="return-types"></a>Types de retour
 **Type de retour CLR : void**  
  
## <a name="remarks"></a>Notes   
 Read ne valide pas son entrée. Si une entrée binaire non valide est fournie, Read peut lever une exception. Elle peut aussi réussir et produire un objet **SqlHierarchyId** non valide dont les méthodes peuvent donner des résultats imprévisibles ou lever une exception.  
  
 Read peut être appelée uniquement sur un objet **SqlHierarchyId** créé récemment.  
  
 Read est utilisée en interne par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cas de nécessité, par exemple lors de l’écriture de données dans une colonne **hierarchyid**. Read est également appelée en interne quand une conversion est effectuée entre **varbinary** et **hierarchyid**.  
  
## <a name="examples"></a>Exemples  
  
```sql
Byte[] encoding = new byte[] { 0x58 };  
MemoryStream stream = new MemoryStream(encoding, false /*not writable*/);  
BinaryReader br = new BinaryReader(stream);  
SqlHierarchyId hid = new SqlHierarchyId();  
hid.Read(br);   
```  
  
## <a name="see-also"></a> Voir aussi  
[Write &#40;moteur de base de données&#41;](../../t-sql/data-types/write-database-engine.md)  
[ToString &#40;moteur de base de données&#41;](../../t-sql/data-types/tostring-database-engine.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Référence de méthodes de type de données hierarchyid](http://msdn.microsoft.com/library/01a050f5-7580-4d5f-807c-7f11423cbb06)
  
  
