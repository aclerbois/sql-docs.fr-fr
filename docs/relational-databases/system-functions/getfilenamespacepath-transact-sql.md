---
title: GetFileNamespacePath (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-functions
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- GetFileNamespacePath
- GetFileNamespacePath_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GetFileNamespacePath function
ms.assetid: b393ecef-baa8-4d05-a268-b2f309fce89a
caps.latest.revision: 16
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: a31ca80ae50906f0789fdfef20fbf4fede9beea8
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="getfilenamespacepath-transact-sql"></a>GetFileNamespacePath (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne le chemin d'accès UNC d'un fichier ou d'un répertoire dans un FileTable.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<column-name>.GetFileNamespacePath(is_full_path, @option)  
```  
  
## <a name="arguments"></a>Arguments  
 *column-name*  
 Nom de colonne de la varbinary (max) **file_stream** colonne dans un FileTable.  
  
 Le *-nom de la colonne* valeur doit être un nom de colonne valide. Il ne peut pas s'agir d'une expression ni d'une valeur convertie à partir d'une colonne présentant un autre type de données.  
  
 *is_full_path*  
 Expression entière qui spécifie s'il faut retourner un chemin d'accès absolu ou relatif. *is_full_path* peut avoir l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Retourne le chemin d'accès relatif dans le répertoire au niveau de la base de données.<br /><br /> Il s'agit de la valeur par défaut|  
|**1**|Retourne le chemin d'accès UNC complet, en commençant par `\\computer_name`.|  
  
 *@option*  
 Expression entière qui définit comment le composant serveur du chemin d'accès doit être mis en forme. *@option* Peut avoir l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Retourne le nom de serveur converti au format NetBIOS, par exemple :<br /><br /> `\\SERVERNAME\MSSQLSERVER\MyDocumentDatabase`<br /><br /> Ceci est la valeur par défaut.|  
|**1**|Retourne le nom de serveur non converti, par exemple :<br /><br /> `\\ServerName\MSSQLSERVER\MyDocumentDatabase`|  
|**2**|Retourne le chemin d'accès complet du serveur, par exemple :<br /><br /> `\\ServerName.MyDomain.com\MSSQLSERVER\MyDocumentDatabase`|  
  
## <a name="return-type"></a>Type de retour  
 **nvarchar(max)**  
  
 Si l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est groupée dans un cluster de basculement, le nom de l'ordinateur qui est retourné dans le cadre de ce chemin d'accès représente le nom d'hôte virtuel de l'instance cluster.  
  
 Lorsque la base de données appartient à un groupe de disponibilité Always On, le **FileTableRootPath** fonction retourne le nom de réseau virtuel (VNN) au lieu du nom de l’ordinateur.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Le chemin d’accès qui le **GetFileNamespacePath** fonction retourne un chemin de répertoire ou fichier logique dans le format suivant :  
  
 `\\<machine>\<instance-level FILESTREAM share>\<database-level directory>\<FileTable directory>\...`  
  
 Ce chemin logique ne correspond pas directement à un chemin d'accès NTFS physique. Il est converti en chemin d'accès physique par le pilote de filtre du système de fichiers FILESTREAM et de l'agent FILESTREAM. Cette séparation entre le chemin d'accès logique et le chemin d'accès physique permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de réorganiser des données en interne sans affecter la validité du chemin d'accès.  
  
## <a name="best-practices"></a>Bonnes pratiques  
 Pour garder le code et les applications indépendantes de l'ordinateur actuel et de la base de données, évitez d'écrire du code qui contient des chemins d'accès de fichier absolus. Au lieu de cela, obtenez le chemin d’accès complet à un fichier en cours d’exécution à l’aide de la **FileTableRootPath** et **GetFileNamespacePath** fonctions, comme indiqué dans l’exemple suivant. Par défaut, la fonction **GetFileNamespacePath** retourne le chemin relatif du fichier sous le chemin racine de la base de données.  
  
```sql  
USE MyDocumentDatabase;  
@root varchar(100)  
SELECT @root = FileTableRootPath();  
  
@fullPath = varchar(1000);  
SELECT @fullPath = @root + file_stream.GetFileNamespacePath() FROM DocumentStore  
WHERE Name = N’document.docx’;  
```  
  
## <a name="remarks"></a>Notes  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants montrent comment appeler le **GetFileNamespacePath** fonction permettant d’obtenir le chemin d’accès UNC d’un fichier ou un répertoire dans un FileTable.  
  
```  
-- returns the relative path of the form “\MyFileTable\MyDocDirectory\document.docx”  
SELECT file_stream.GetFileNamespacePath() AS FilePath FROM DocumentStore  
WHERE Name = N’document.docx’;  
  
-- returns “\\MyServer\MSSQLSERVER\MyDocumentDatabase\MyFileTable\MyDocDirectory\document.docx”  
SELECT file_stream.GetFileNamespacePath(1, Null) AS FilePath FROM DocumentStore  
WHERE Name = N’document.docx’;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Travailler avec des répertoires et des chemins d'accès dans FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  
