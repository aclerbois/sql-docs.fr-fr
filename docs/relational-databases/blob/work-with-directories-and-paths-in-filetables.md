---
title: Travailler avec des répertoires et des chemins d’accès dans FileTables | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
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
- FileTables [SQL Server], directories
ms.assetid: f1e45900-bea0-4f6f-924e-c11e1f98ab62
caps.latest.revision: 25
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: a846fc9d702d9bdec0aa8cf583b1d5bbd52f4751
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="work-with-directories-and-paths-in-filetables"></a>Travailler avec des répertoires et des chemins d'accès dans FileTables
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Décrit la structure de répertoires dans laquelle les fichiers sont stockés dans FileTables.  
  
##  <a name="HowToDirectories"></a> Procédure : travailler avec des répertoires et des chemins d'accès dans FileTables  
 Vous pouvez utiliser les trois fonctions suivantes pour travailler avec des répertoires FileTable dans [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
|Pour obtenir ce résultat|Utilisez cette fonction|  
|------------------------|-----------------------|  
|Obtenir le chemin d'accès UNC au niveau racine pour un FileTable spécifique ou pour la base de données actuelle.|[FileTableRootPath &#40;Transact-SQL&#41;](../../relational-databases/system-functions/filetablerootpath-transact-sql.md)|  
|Obtenir un chemin d'accès UNC absolu ou relatif pour un fichier ou répertoire d'un FileTable.|[GetFileNamespacePath &#40;Transact-SQL&#41;](../../relational-databases/system-functions/getfilenamespacepath-transact-sql.md)|  
|Obtenir la valeur d'ID de localisateur de chemin d'accès pour le fichier ou le répertoire spécifié d'un FileTable, en spécifiant le chemin d'accès.|[GetPathLocator &#40;Transact-SQL&#41;](../../relational-databases/system-functions/getpathlocator-transact-sql.md)|  
  
##  <a name="BestPracticeRelativePaths"></a> Procédure : utiliser des chemins d'accès relatifs pour du code portable  
 Pour garder le code et les applications indépendantes de l'ordinateur actuel et de la base de données, évitez d'écrire du code qui contient des chemins d'accès de fichier absolus. Au lieu de cela, récupérez le chemin d’accès complet d’un fichier au moment de l’exécution en utilisant les fonctions [FileTableRootPath &#40;Transact-SQL&#41;](../../relational-databases/system-functions/filetablerootpath-transact-sql.md) et [GetFileNamespacePath &#40;Transact-SQL&#41;](../../relational-databases/system-functions/getfilenamespacepath-transact-sql.md)ensemble, comme illustré dans l’exemple suivant. Par défaut, la fonction **GetFileNamespacePath** retourne le chemin relatif du fichier sous le chemin racine de la base de données.  
  
```sql  
USE database_name;  
DECLARE @root nvarchar(100);  
DECLARE @fullpath nvarchar(1000);  
  
SELECT @root = FileTableRootPath();  
SELECT @fullpath = @root + file_stream.GetFileNamespacePath()  
    FROM filetable_name  
    WHERE name = N'document_name';  
  
PRINT @fullpath;  
GO  
```  
  
##  <a name="restrictions"></a> Restrictions importantes  
  
###  <a name="nesting"></a> Niveau d'imbrication  
  
> **IMPORTANT** Vous ne pouvez pas stocker plus de 15 niveaux de sous-répertoires dans le répertoire FileTable. Lorsque vous stockez 15 niveaux de sous-répertoires, le niveau le plus bas ne peut pas contenir de fichiers, étant donné que ces fichiers représenteraient un niveau supplémentaire.  
  
###  <a name="fqnlength"></a> Longueur du nom du chemin d'accès complet  
  
> **IMPORTANT** Le système de fichiers NTFS prend en charge les noms de chemin d'accès qui sont beaucoup plus longs que la limite de 260 caractères imposée par le shell Windows et de la plupart des API Windows. Par conséquent, il est possible de créer des fichiers dans la hiérarchie des fichiers d'un FileTable à l'aide de Transact-SQL que vous ne pouvez pas afficher ou ouvrir avec l'Explorateur Windows ou de nombreuses autres applications Windows, car le chemin d'accès complet dépasse 260 caractères. Toutefois vous pouvez continuer à accéder à ces fichiers à l'aide de Transact-SQL.  
  
##  <a name="fullpath"></a> Chemin complet à un élément stocké dans un FileTable  
 Le chemin d'accès complet à un fichier ou un répertoire stocké dans un FileTable commence par les éléments suivants :  
  
1.  Le partage activé pour l'accès d'E/S de fichier FILESTREAM au niveau de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
2.  Le **DIRECTORY_NAME** spécifié au niveau de la base de données.  
  
3.  Le **FILETABLE_DIRECTORY** spécifié au niveau du FileTable.  
  
 La hiérarchie résultante ressemble à celle-ci :  
  
 `\\<machine>\<instance-level FILESTREAM share>\<database-level directory>\<FileTable directory>\`  
  
 Cette hiérarchie de répertoires forme la racine de l'espace de noms de fichiers du FileTable. Sous cette hiérarchie de répertoires, les données FILESTREAM pour le FileTable sont stockées en tant que fichiers, et comme sous-répertoires qui peuvent également contenir des fichiers et des sous-répertoires.  
  
 Il est important de se souvenir que la hiérarchie de répertoires créée sous le partage FILESTREAM au niveau de l'instance est une hiérarchie de répertoires virtuels. Cette hiérarchie est stockée dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et n'est pas représentée physiquement dans le système de fichiers NTFS. Toutes les opérations qui accèdent à des fichiers et des répertoires sous le partage FILESTREAM et dans le FileTables qu'il contient sont interceptées et gérées par un composant de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incorporé dans le système de fichiers.  
  
##  <a name="roots"></a> Sémantique des répertoires racines aux niveaux de l'instance, de la base de données et de FileTable  
 Cette hiérarchie de répertoires observe la sémantique suivante :  
  
-   Le partage FILESTREAM au niveau de l'instance est configuré par un administrateur et stocké comme propriété du serveur. Vous pouvez renommer ce partage à l'aide du gestionnaire de configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Une opération de changement de nom n'entre en vigueur qu'une fois que le serveur a redémarré.  
  
-   Le **DIRECTORY_NAME** au niveau de la base de données est null par défaut quand vous créez une base de données. Un administrateur peut définir ou modifier ce nom à l'aide de l'instruction **ALTER DATABASE** . Le nom doit être unique (dans une comparaison ne respectant pas la casse) dans cette instance.  
  
-   On fournit en général le nom **FILETABLE_DIRECTORY** dans le cadre de l’instruction **CREATE TABLE** quand on crée un FileTable. Vous pouvez modifier ce nom à l'aide de la commande **ALTER TABLE** .  
  
-   Vous ne pouvez pas renommer ces répertoires racines par le biais d'opérations d'E/S de fichier.  
  
-   Vous ne pouvez pas ouvrir ces répertoires racines avec des descripteurs de fichiers exclusifs.  
  
##  <a name="is_directory"></a> Colonne is_directory dans le schéma de FileTable  
 Le tableau suivant décrit l’interaction entre la colonne **is_directory** et la colonne **file_stream** qui contient les données FILESTREAM dans un FileTable.  
  
||||  
|-|-|-|  
|*is_directory* **is_directory**|*file_stream* **is_directory**|**Comportement**|  
|FALSE|NULL|Il s'agit d'une combinaison non valide qui est interceptée par une contrainte définie par le système.|  
|FALSE|\<valeur>|L'élément représente un fichier.|  
|TRUE|NULL|L'élément représente un répertoire.|  
|TRUE|\<valeur>|Il s'agit d'une combinaison non valide qui est interceptée par une contrainte définie par le système.|  
  
##  <a name="alwayson"></a> Utilisation de noms de réseau virtuel (VNN) avec des groupes de disponibilité AlwaysOn  
 Lorsque la base de données qui contient des données FILESTREAM ou FileTable appartient à un groupe de disponibilité AlwaysOn :  
  
-   Les fonctions FILESTREAM et FileTable acceptent ou retournent des noms de réseau virtuel (VNN) à la place de noms d'ordinateur. Pour plus d’informations sur ces fonctions, consultez [Fonctions FileStream et FileTable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/filestream-and-filetable-functions-transact-sql.md).  
  
-   Tous les accès à FILESTREAM ou aux données FileTable via les API du système de fichiers doivent utiliser des VNN à la place des noms d'ordinateur. Pour plus d’informations, consultez [FILESTREAM et FileTable avec groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/filestream-and-filetable-with-always-on-availability-groups-sql-server.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Activer les conditions préalables pour les FileTables](../../relational-databases/blob/enable-the-prerequisites-for-filetable.md)   
 [Créer, modifier et supprimer des FileTables](../../relational-databases/blob/create-alter-and-drop-filetables.md)   
 [Accéder aux FileTables avec Transact-SQL](../../relational-databases/blob/access-filetables-with-transact-sql.md)   
 [Accéder aux FileTables avec des API d’entrée-sortie de fichier](../../relational-databases/blob/access-filetables-with-file-input-output-apis.md)  
  
  
