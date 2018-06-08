---
title: Instructions RESTORE pour la restauration, la récupération et la gestion des sauvegardes (T-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- restoring files [SQL Server], RESTORE statement
- RESTORE statement, about restore operations
- database restores [SQL Server], RESTORE statement
- restoring databases [SQL Server], RESTORE statement
- database backups [SQL Server], RESTORE statement
- backing up databases [SQL Server], RESTORE statement
- file restores [SQL Server], RESTORE statement
- transaction log backups [SQL Server], RESTORE statement
ms.assetid: fb29a151-f312-4f85-b857-5deeca0de8ce
caps.latest.revision: 15
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 003f3adce58a228e4965f91c1104ff33b60b8398
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="restore-statements-for-restoring-recovering-and-managing-backups-transact-sql"></a>Instructions RESTORE pour la restauration, la récupération et la gestion des sauvegardes (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md )]

  Cette section détaille les instructions RESTORE pour les opérations de sauvegarde. Outre l'instruction principale RESTORE {DATABASE | LOG} destinée à la restauration et à la récupération des sauvegardes, plusieurs instructions RESTORE auxiliaires vous permettent de gérer des sauvegardes et de prévoir des séquences de restauration. Les commandes RESTORE auxiliaires sont les suivantes : RESTORE FILELISTONLY, RESTORE HEADERONLY, RESTORE LABELONLY, RESTORE REWINDONLY et RESTORE VERIFYONLY.  
  
[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]

> [!IMPORTANT]  
>  Dans les versions précédentes de SQL Server, tout utilisateur pouvait obtenir des informations sur les jeux de sauvegarde et les unités de sauvegarde en utilisant les instructions Transact-SQL RESTORE FILELISTONLY, RESTORE HEADERONLY, RESTORE LABELONLY et RESTORE VERIFYONLY. Comme elles révèlent des informations sur le contenu des fichiers de sauvegarde, dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures, ces instructions requièrent l'autorisation CREATE DATABASE. Cette nécessité sécurise vos fichiers de sauvegarde et protège vos informations de sauvegarde de façon plus complète que dans les versions précédentes. Pour plus d’informations sur cette autorisation, consultez [Autorisations de base de données GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
|.|Description|  
|---------------|-----------------|  
|[RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)|Décrit les instructions Transact-SQL, RESTORE DATABASE et RESTORE LOG, qui sont utilisées pour restaurer et récupérer une base de données à partir des sauvegardes réalisées via la commande BACKUP. RESTORE DATABASE s'utilise pour les bases de données avec tous les modes de récupération, tandis que RESTORE LOG s'utilise uniquement en mode de récupération complète utilisant les journaux de transactions. RESTORE DATABASE permet également de rétablir une base de données en instantané de base de données.|  
|[Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md)|Obtient des informations sur les arguments décrits dans les sections « Syntaxe » de l'instruction RESTORE et du jeu associé d'instructions auxiliaires : RESTORE FILELISTONLY, RESTORE HEADERONLY, RESTORE LABELONLY, RESTORE REWINDONLY et RESTORE VERIFYONLY. La plupart des arguments sont pris en charge seulement par un sous-ensemble de ces six instructions. Cette prise en charge est précisée dans la description de chacun des arguments.|  
|[RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)|Décrit l'instruction RESTORE FILELISTONLY de Transact-SQL, qui est utilisée pour renvoyer un ensemble de résultats contenant une liste des fichiers journaux et des fichiers de la base de données contenus dans le jeu de sauvegarde.|  
|[RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)|Décrit l'instruction RESTORE HEADERONLY de Transact-SQL, qui est utilisée pour renvoyer un ensemble de résultats contenant toutes les informations d'en-tête pour tous les jeux de sauvegarde sur une unité de sauvegarde particulière.|  
|[RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)|Décrit l'instruction RESTORE LABELONLY de Transact-SQL, qui est utilisée pour renvoyer un ensemble de résultats contenant des informations sur les supports de sauvegarde identifiés par l'unité de sauvegarde donnée.|  
|[RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)|Décrit l'instruction RESTORE REWINDONLY de Transact-SQL, qui est utilisée pour rembobiner et fermer les périphériques à bandes laissés ouverts par les instructions BACKUP ou RESTORE qui ont été exécutées par l'intermédiaire de l'option NOREWIND.|  
|[RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)|Décrit l'instruction RESTORE VERIFYONLY de Transact-SQL, qui est utilisée d'une part pour vérifier la sauvegarde sans toutefois la restaurer, et d'autre part pour vérifier que le jeu de sauvegarde est complet et que l'ensemble de la sauvegarde est lisible ; elle ne tente pas de vérifier la structure des données.|  
  
## <a name="see-also"></a> Voir aussi  
 [Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
  
  
