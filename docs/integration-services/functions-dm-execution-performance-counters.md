---
title: dm_execution_performance_counters (base de données SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: language-reference
ms.assetid: 1b38e8e3-c560-4b6e-b60e-bfd7cfcd4fdf
caps.latest.revision: 6
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 026b01925fbcf0ec615da77793dff7f84221322f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="functions---dmexecutionperformancecounters"></a>Fonctions - dm_execution_performance_counters
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]

  Retourne les statistiques de performance pour une exécution en cours sur le serveur [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```sql  
dm_execution_performance_counters [ @execution_id = ] execution_id  
  
```  
  
## <a name="arguments"></a>Arguments  
 [ @execution_id = ] *execution_id*  
 Identificateur unique de l'exécution qui contient un ou plusieurs packages. Packages exécutés avec la tâche d'exécution du package dans la même exécution comme package parent.  
  
 Si un ID d'exécution n'est pas spécifié, les statistiques de performance de plusieurs exécutions sont retournées. Si vous êtes membre du rôle de base de données **ssis_admin** , les statistiques de performances de toutes les exécutions en cours sont retournées.  Si vous n’êtes pas membre du rôle de base de données **ssis_admin** , les statistiques de performances des exécutions en cours pour lesquelles vous disposez d’autorisations de lecture sont retournées. *execution_id* est un **BigInt**.  
  
## <a name="remarks"></a>Notes   
 Le tableau suivant répertorie les valeurs de nom de compteur retournées par la fonction dm_execution_performance_counter.  
  
|Nom du compteur|Description|  
|------------------|-----------------|  
|Octets BLOB lus|Nombre d'octets des données d'objet BLOB (Binary Large Object) que le moteur de flux de données lit à partir de toutes les sources.|  
|Octets BLOB écrits|Nombre d'octets des données BLOB que le moteur de flux de données écrit sur toutes les destinations.|  
|Fichiers BLOB utilisés|Nombre de fichiers BLOB que le moteur de flux de données utilise pour la mise en file d'attente.|  
|Mémoire tampon|Quantité de mémoire utilisée par les mémoires tampons Integration Services, y compris la mémoire physique et virtuelle.|  
|Tampons en cours d'utilisation|Nombre d'objets de mémoire tampon, de tous types, utilisés par tous les composants de flux de données et le moteur de flux de données.|  
|Mémoires tampon spoulées|Nombre de mémoires tampons écrites sur le disque.|  
|Mémoire tampon plate|Quantité de mémoire, en octets, utilisée par toutes les mémoires tampons plates. Les mémoires tampons plates sont des blocs de mémoire utilisés par un composant pour stocker des données.|  
|Mémoires tampons plates en cours d'utilisation|Nombre de mémoires tampons plates utilisées par le moteur de flux de données. Toutes les mémoires tampons plates sont des mémoires tampons privées.|  
|Mémoire tampon privée|Quantité de mémoire utilisée par toutes les mémoires tampons privées. Une mémoire tampon privée est une mémoire tampon qu'une transformation utilise pour un travail temporaire.<br /><br /> Une mémoire tampon n’est pas privée si le moteur de flux de données la crée pour prendre en charge le flux de données.|  
|Mémoires tampons privées en cours d'utilisation|Nombre de mémoires tampons utilisées par les transformations pour un travail temporaire.|  
|Lignes lues|Nombre total de lignes lues par l'exécution.|  
|Lignes écrites|Nombre total de lignes écrites par l'exécution.|  
  
## <a name="return"></a>Return  
 La fonction dm_execution_performance_counters retourne une table comportant les colonnes suivantes, pour une exécution en cours. Les informations retournées concernent tous les packages contenus dans l'exécution. Si aucune exécution n'est en cours, une table vide est retournée.  
  
|Nom de la colonne|Type de colonne|Description|Notes |  
|-----------------|-----------------|-----------------|-------------|  
|execution_id|**BigInt**<br /><br /> **NULL** n’est pas une valeur valide.|Identificateur unique de l'exécution qui contient le package.||  
|counter_name|**nvarchar(128)**|Nom du compteur.|Consultez la section **Notes** des valeurs.|  
|counter_value|**BigInt**|Valeur retournée par le compteur.||  
  
## <a name="example"></a> Exemple  
 Dans l'exemple suivant, la fonction retourne des statistiques pour une exécution en cours ayant l'ID 34.  
  
```sql
select * from [catalog].[dm_execution_performance_counters] (34)  
```  
  
## <a name="example"></a> Exemple  
 Dans l'exemple suivant, la fonction retourne des statistiques pour toutes les exécutions en cours sur le serveur [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], selon les autorisations dont vous disposez.  
  
```sql
select * from [catalog].[dm_execution_performance_counters] (NULL)  
  
```  
  
## <a name="permissions"></a>Autorisations  
 Cette fonction requiert l'une des autorisations suivantes :  
  
-   Autorisations READ et MODIFY sur l'instance d'exécution  
  
-   Appartenance au rôle de base de données **ssis_admin**  
  
-   Appartenance au rôle serveur **sysadmin**  
  
## <a name="errors-and-warnings"></a>Erreurs et avertissements  
 La liste suivante décrit les conditions provoquant l'échec de la fonction.  
  
-   L'utilisateur ne dispose pas des autorisations MODIFY pour l'exécution spécifiée.  
  
-   L’ID d’exécution spécifié n’est pas valide.  
  
  
