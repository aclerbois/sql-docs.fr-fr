---
title: Paramètres (Type de mappage) du projet (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 2698fb3a-f9e6-4e04-94e0-dad289d7ed0a
caps.latest.revision: 6
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 93d23239f4dc60dc419dde62ab0a4923f286669d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settings-type-mapping-sybasetosql"></a>Paramètres (Type de mappage) du projet (SybaseToSQL)
La page mappage de Type de la **les paramètres de projet** boîte de dialogue contient des paramètres permettant de personnaliser comment SSMA convertit les types de données Sybase Adaptive Server Enterprise (ASE) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données.  
  
La page mappage de Type est disponible dans le **les paramètres de projet** et **les paramètres de projet par défaut** boîtes de dialogue.  
  
-   Pour spécifier les paramètres de mappage de type pour tous les futurs projets SSMA, sur le **outils** menu, sélectionnez **les paramètres de projet par défaut**, sélectionnez le type de projet de migration pour lequel les paramètres sont requis pour être affichées ou modifiées à partir de **Version cible de la Migration** liste déroulante, puis sélectionnez **mappage de Type** en bas du volet gauche.  
  
-   Pour spécifier les paramètres pour le projet actuel, sur le **outils** menu, sélectionnez **paramètres du projet**, puis sélectionnez **le mappage de Type** en bas du volet gauche.  
  
## <a name="options"></a>Options  
**Type de source**  
Le type de données ASE mappé.  
  
**Type de cible**  
La cible [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] type de données pour le type de données ASE spécifié.  
  
Consultez le tableau dans la section suivante pour la valeur par défaut SSMA pour Sybase mappage du type.  
  
**Ajouter**  
Cliquez pour ajouter un type de données à la liste de mappage.  
  
**Modifier**  
Cliquez pour modifier le type de données sélectionné dans la liste de mappage.  
  
**Supprimer**  
Cliquez pour supprimer le mappage de type de données sélectionnées à partir de la liste de mappage.  
  
**Réinitialiser les valeurs par défaut**  
Cliquez pour réinitialiser la liste de mappage de type pour les valeurs par défaut SSMA.  
  
## <a name="default-type-mapping"></a>Mappage de Type par défaut  
Le tableau suivant contient le mappage de type par défaut entre ASE et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] des types de données.  
  
|Type de données de ASE|Type de données de SQL Server|  
|-----------------|------------------------|  
|**bigint**|**bigint**|  
|**binaire**|**binaire**|  
|**binaire [\*... 8000]**|**binaire [\*]**|  
|**binaire [8001..\*]**|**varbinary(max)**|  
|**bit**|**bit**|  
|**char**|**char**|  
|**char varying**|**varchar**|  
|**char varying [\*... 8000]**|**varchar[\*]**|  
|**char varying [8001..\*]**|**varchar(max)**|  
|**Char [\*... 8000]**|**char[\*]**|  
|**Char [8001..\*;]**|**varchar(max)**|  
|**character**|**char**|  
|**variable de caractère**|**varchar**|  
|**variable de caractères [\*... 8000]**|**varchar[\*]**|  
|**variable de caractères [8001..\*]**|**varchar(max)**|  
|**caractère [\*... 8000]**|**char[\*]**|  
|**caractère [8001..\*]**|**varchar(max)**|  
|**date**|**date**|  
|**datetime**|**datetime2[3]**|  
|**dec**|**decimal**|  
|**dec[\*..\*]**|**decimal[\*]**|  
|**dec[\*..\*][\*..\*]**|**decimal[\*][\*]**|  
|**decimal**|**decimal**|  
|**Decimal [\*... \*]**|**decimal[\*]**|  
|**Decimal [\*... \*][\*.. \*]**|**decimal[\*][\*]**|  
|**double précision**|**float[53]**|  
|**float**|**float[53]**|  
|**float [\*... 15]**|**float[24]**|  
|**float [16..\*]**|**float[53]**|  
|**image**|**image**|  
|**int**|**int**|  
|**entier**|**int**|  
|**longsysname**|**nvarchar[255]**|  
|**money**|**money**|  
|**national char**|**nchar**|  
|**national char [\*... 4000]**|**nchar[\*]**|  
|**national char varying**|**nvarchar**|  
|**national char varying [\*... 4000]**|**nvarchar[\*]**|  
|**national char varying [4001..\*]**|**nvarchar(max)**|  
|**national char [4001..\*]**|**nvarchar(max)**|  
|**caractères nationaux**|**nchar**|  
|**les caractères nationaux [\*... 4000]**|**nchar[\*]**|  
|**les caractères nationaux [4001..\*]**|**nvarchar(max)**|  
|**variable de caractères nationaux**|**nvarchar**|  
|**variable de caractères nationaux [\*... 4000]**|**nvarchar[\*]**|  
|**variable de caractères nationaux [4001..\*]**|**nvarchar(max)**|  
|**national varchar**|**nvarchar**|  
|**national varchar [\*... 4000]**|**nvarchar[\*]**|  
|**national varchar [4001..\*]**|**nvarchar(max)**|  
|**nchar**|**nchar**|  
|**NCHAR varying**|**nvarchar**|  
|**NCHAR varying [\*... 4000]**|**nvarchar[\*]**|  
|**NCHAR varying [4001..\*]**|**nvarchar(max)**|  
|**NCHAR [\*... 4000]**|**nchar[\*]**|  
|**NCHAR [4001..\*]**|**nvarchar(max)**|  
|**numeric**|**numeric**|  
|**numérique [\*... \*]**|**numeric[\*]**|  
|**numérique [\*... \*][\*.. \*]**|**numeric[\*][\*]**|  
|**nvarchar**|**nvarchar**|  
|**nvarchar [\*... 4000]**|**nvarchar[\*]**|  
|**nvarchar [4001..\*]**|**nvarchar(max)**|  
|**real**|**float[24]**|  
|**smalldatetime**|**smalldatetime**|  
|**smallint**|**smallint**|  
|**smallmoney**|**smallmoney**|  
|**sysname**|**nvarchar[128]**|  
|**sysname [\*... \*]**|**nvarchar[255]**|  
|**texte**|**texte**|  
|**time**|**time[3]**|  
|**timestamp**|**rowversion**|  
|**tinyint**|**tinyint**|  
|**UNICHAR**|**nchar**|  
|**UNICHAR variable**|**nvarchar**|  
|**UNICHAR varying [\*... 4000]**|**nvarchar[\*]**|  
|**UNICHAR varying [4001..\*]**|**nvarchar(max)**|  
|**UNICHAR [\*... 4000]**|**nchar[\*]**|  
|**UNICHAR [4001..\*]**|**nvarchar(max)**|  
|**unitext**|**nvarchar(max)**|  
|**univarchar**|**nvarchar**|  
|**univarchar [\*... 4000]**|**nvarchar[\*]**|  
|**univarchar [4001..\*]**|**nvarchar(max)**|  
|**bigint non signé**|**numeric[20][0]**|  
|**entier non signé**|**bigint**|  
|**smallint non signé**|**int**|  
|**tinyint non signé**|**tinyint**|  
|**varbinary**|**varbinary**|  
|**varbinary [\*... 8000]**|**varbinary[\*]**|  
|**varbinary [8001..\*]**|**varbinary(max)**|  
|**varchar**|**varchar**|  
|**varchar [\*... 8000]**|**varchar[\*]**|  
|**varchar [8001..\*]**|**varchar(max)**|  
  
