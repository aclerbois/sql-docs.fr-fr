---
title: Accéder aux données FILESTREAM avec Transact-SQL | Microsoft Docs
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
- FILESTREAM [SQL Server], Transact-SQL
ms.assetid: a6bf0ce7-7e5e-4a07-8917-ee526c9d0a05
caps.latest.revision: 16
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f2577047b97faf37e9905f2cb98be9121fbf1939
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="access-filestream-data-with-transact-sql"></a>Accéder aux données FILESTREAM avec Transact-SQL
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment utiliser les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] INSERT, UPDATE et DELETE pour gérer des données FILESTREAM.  
  
> [!NOTE]  
>  Les exemples de cette rubrique nécessitent la base de données compatible FILESTREAM et la table qui sont créées dans [Créer une base de données compatible FILESTREAM](../../relational-databases/blob/create-a-filestream-enabled-database.md) et [Créer une table pour le stockage de données FILESTREAM](../../relational-databases/blob/create-a-table-for-storing-filestream-data.md).  
  
##  <a name="ins"></a> Insertion d'une ligne qui contient des données FILESTREAM  
 Pour ajouter une ligne à une table prenant en charge les données FILESTREAM, utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] INSERT. Quand vous insérez des données dans une colonne FILESTREAM, vous pouvez insérer une valeur NULL ou **varbinary(max)** .  
  
### <a name="inserting-null"></a>Insertion de NULL  
 L'exemple suivant montre comment insérer `NULL`. Lorsque la valeur FILESTREAM est `NULL`, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne crée pas de fichier dans le système de fichiers.  
  
 [!code-sql[FILESTREAM#FS_InsertNULL](../../relational-databases/blob/codesnippet/tsql/access-filestream-data-w_1_1.sql)]  
  
### <a name="inserting-a-zero-length-record"></a>Insertion d'un enregistrement de longueur nulle  
 L'exemple suivant illustre l'utilisation de `INSERT` pour créer un enregistrement de longueur nulle. C'est utile lorsque vous souhaitez obtenir un descripteur de fichier, mais que vous manipulerez le fichier en utilisant des API Win32.  
  
 [!code-sql[FILESTREAM#FS_InsertZero](../../relational-databases/blob/codesnippet/tsql/access-filestream-data-w_1_2.sql)]  
  
### <a name="creating-a-data-file"></a>Création d'un fichier de données  
 L'exemple suivant illustre l'utilisation de `INSERT` pour créer un fichier contenant des données. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] convertit la chaîne `Seismic Data` en valeur `varbinary(max)` . FILESTREAM crée le fichier Windows s'il n'existe pas déjà. Les données sont ensuite ajoutées au fichier de données.  
  
 [!code-sql[FILESTREAM#FS_InsertData](../../relational-databases/blob/codesnippet/tsql/access-filestream-data-w_1_3.sql)]  
  
 Quand vous sélectionnez toutes les données de la table `Archive.dbo.Records`, les résultats sont semblables à ceux affichés dans le tableau suivant. Toutefois, la colonne `Id` contiendra des GUID différents.  
  
|Id|SerialNumber|Graphique|  
|--------|------------------|------------|  
|`C871B90F-D25E-47B3-A560-7CC0CA405DAC`|`1`|`NULL`|  
|`F8F5C314-0559-4927-8FA9-1535EE0BDF50`|`2`|`0x`|  
|`7F680840-B7A4-45D4-8CD5-527C44D35B3F`|`3`|`0x536569736D69632044617461`|  
  
  
##  <a name="upd"></a> Mise à jour de données FILESTREAM  
 Vous pouvez utiliser [!INCLUDE[tsql](../../includes/tsql-md.md)] pour mettre à jour les données dans le fichier de système de fichiers, bien que cela soit plutôt déconseillé quand vous devez transmettre en continu de gros volumes de données à un fichier.  
  
 L'exemple suivant remplace tout texte dans l'enregistrement de fichier par le texte `Xray 1`.  
  
 [!code-sql[FILESTREAM#FS_UpdateData](../../relational-databases/blob/codesnippet/tsql/access-filestream-data-w_1_4.sql)]  
  
  
##  <a name="del"></a> Suppression de données FILESTREAM  
 Lorsque vous supprimez une ligne qui contient un champ FILESTREAM, vous supprimez également ses fichiers de système de fichiers sous-jacents. La seule façon de supprimer une ligne, et par conséquent le fichier, consiste à utiliser l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] DELETE.  
  
 L'exemple suivant montre comment supprimer une ligne et les fichiers de système de fichiers qui lui sont associés.  
  
 [!code-sql[FILESTREAM#FS_DeleteData](../../relational-databases/blob/codesnippet/tsql/access-filestream-data-w_1_5.sql)]  
  
 Quand vous sélectionnez toutes les données de la table `Archive.dbo.Records`, la ligne disparaît et vous ne pouvez plus utiliser le fichier associé.  
  
> [!NOTE]  
>  Les fichiers sous-jacents sont supprimés par le garbage collector FILESTREAM.  
  
  
## <a name="see-also"></a> Voir aussi  
 [Activer et configurer FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md)   
 [Éviter les conflits avec les opérations de base de données dans les applications FILESTREAM](../../relational-databases/blob/avoid-conflicts-with-database-operations-in-filestream-applications.md)  
  
  
