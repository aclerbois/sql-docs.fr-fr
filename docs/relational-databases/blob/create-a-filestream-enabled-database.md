---
title: Créer une base de données compatible FILESTREAM | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: blob
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-blob
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], FILESTREAM-enabled databases
ms.assetid: 0fc16356-76f7-44b8-a58b-f0b7c43694ec
caps.latest.revision: 16
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e9da6625b53d1640b5568b506734918cbaae51a1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-filestream-enabled-database"></a>Créer une base de données compatible FILESTREAM
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique montre comment créer une base de données qui prend en charge FILESTREAM. Étant donné que FILESTREAM utilise un type de groupe de fichiers spécial, lors de la création de la base de données vous devez spécifier la clause CONTAINS FILESTREAM pour au moins un groupe de fichiers.  
  
 Un groupe de fichiers FILESTREAM peut contenir plusieurs fichiers. Pour obtenir un exemple de code qui illustre la création d’un groupe de fichiers FILESTREAM contenant plusieurs fichiers, consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md).  
  
### <a name="to-create-a-filestream-enabled-database"></a>Pour créer une base de données compatible FILESTREAM  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cliquez sur **Nouvelle requête** pour afficher l'Éditeur de requête.  
  
2.  Copiez le code [!INCLUDE[tsql](../../includes/tsql-md.md)] de l'exemple suivant dans l'éditeur de requête. Ce code [!INCLUDE[tsql](../../includes/tsql-md.md)] crée une base de données prenant en charge FILESTREAM appelée Archive.  
  
    > [!NOTE]  
    >  Pour ce script, le répertoire C:\Data doit exister.  
  
3.  Pour générer la base de données, cliquez sur **Exécuter**.  
  
## <a name="example"></a> Exemple  
 L'exemple de code suivant crée une base de données nommée `Archive`. La base de données contient trois groupes de fichiers : `PRIMARY`, `Arch1`et `FileStreamGroup1`. `PRIMARY` et `Arch1` sont des groupes de fichiers ordinaires qui ne peuvent pas contenir de données FILESTREAM. `FileStreamGroup1` est le groupe de fichiers `FILESTREAM` .  
  
 [!code-sql[FILESTREAM#FS_CreateDB](../../relational-databases/blob/codesnippet/tsql/create-a-filestream-enab_1.sql)]  
  
 Pour un groupe de fichiers `FILESTREAM` , `FILENAME` fait référence à un chemin d'accès. Le chemin d'accès jusqu'au dernier dossier doit exister, et le dernier dossier ne doit pas exister. Dans cet exemple, `c:\data` doit exister. Toutefois, le sous-dossier `filestream1` ne peut pas exister lorsque vous exécutez l'instruction `CREATE DATABASE` . Pour plus d’informations sur la syntaxe, consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md).  
  
 Après l'exécution de l'exemple précédent, un fichier filestream.hdr et un dossier $FSLOG apparaissent dans le dossier c:\Data\filestream1. Le fichier filestream.hdr est un fichier d'en-tête pour le conteneur FILESTREAM.  
  
> [!IMPORTANT]  
>  Le fichier filestream.hdr est un fichier système important. Il contient des informations d'en-tête FILESTREAM. Vous ne devez ni le supprimer, ni le modifier.  
  
 Pour les bases de données existantes, vous pouvez utiliser l'instruction [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) pour ajouter un groupe de fichiers FILESTREAM.  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
  
  
