---
title: Bases de données DB2 migration vers SQL Server (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 14d2e655-af7e-4aa5-ba28-0e3d0d025518
caps.latest.revision: 4
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: fbc4823fc61b3678b29ac5e0fee482384605abc8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="migrating-db2-databases-to-sql-server-db2tosql"></a>Bases de données DB2 migration vers SQL Server (DB2ToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant (SSMA) pour DB2 est un environnement complet qui vous permet de migrer rapidement des bases de données DB2 [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure. À l’aide de SSMA pour DB2, vous pouvez examiner les objets de base de données et les données, évaluer les bases de données pour la migration, migrer des objets de base de données à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure, puis migrer les données à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure. Notez que vous ne pouvez pas migrer les schémas SYS et système DB2.  
  
## <a name="recommended-migration-process"></a>Processus de Migration recommandé  
Pour pouvoir migrer des objets et des données à partir de bases de données DB2 [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou Azure SQL DB, procédez comme suit :  
  
1.  [Nouveau projet SSMA](http://msdn.microsoft.com/en-us/66437b45-4686-4fc7-a91b-ebde45e0f1b0).  
  
    Après avoir créé le projet, vous pouvez définir les options de mappage de type, de migration et de conversion de projet. Pour plus d’informations sur les paramètres de projet, consultez [les paramètres de projet &#40;Conversion&#41; &#40;DB2ToSQL&#41; ](../../ssma/db2/project-settings-conversion-db2tosql.md) et rubriques connexes. Pour plus d’informations sur la personnalisation des mappages de types de données, consultez [DB2 de mappage et les Types de données SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).  
  
2.  [Se connecter à la base de données DB2](http://msdn.microsoft.com/en-us/5eb5801d-f0c3-4127-97c0-0b1ef49f4844).  
  
3.  [Connexion à SQL Server](http://msdn.microsoft.com/en-us/b59803cb-3cc6-41cc-8553-faf90851410e).  
  
4.  [Mapper des schémas de DB2 aux schémas SQL Server](http://msdn.microsoft.com/en-us/05ff7bd4-e60b-4f48-a893-bc2346aa9a8a).  
  
5.  Si vous le souhaitez, [de rapports d’évaluation](http://msdn.microsoft.com/en-us/9e13eba0-e3cf-4205-974f-c00f982061de) pour évaluer des objets de base de données pour la conversion et estimer le temps de conversion.  
  
6.  [Convertir des schémas de DB2](http://msdn.microsoft.com/en-us/7947efc3-ca86-4ec5-87ce-7603059c75a0).  
  
7.  [Charger les objets de base de données convertie dans SQL Server](http://msdn.microsoft.com/en-us/f4ea1ced-9f9f-4a9d-88ab-81dbab64adc3).  
  
    Pour cela, dans une des manières suivantes :  
  
    -   Enregistrer un script et exécutez-le en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
    -   Synchroniser les objets de base de données.  
  
8.  [Migration de données DB2 dans SQL Server](http://msdn.microsoft.com/en-us/86cbd39f-6dac-409a-9ce1-7dd54403f84b).  
  
9. Si nécessaire, mettez à jour des applications de base de données.  
  
## <a name="see-also"></a>Voir aussi  
[L’installation de SSMA pour DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-for-db2-db2tosql.md)  
[Prise en main de SSMA pour DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/getting-started-with-ssma-for-db2-db2tosql.md)  
  
