---
title: 'Migrer des bases de données Access vers SQL Server : base de données SQL Azure | Documents Microsoft'
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
helpviewer_keywords:
- instructions, migration
- migrating databases, overview
- overview, migration process
- procedure, migration
- recommended migration process
ms.assetid: 76a3abcf-2998-4712-9490-fe8d872c89ca
caps.latest.revision: 23
author: Shamikg
ms.author: Shamikg
manager: murato
ms.openlocfilehash: 9b17a4e565c27d501d2e515df195c84fb5116321
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="migrating-access-databases-to-sql-server---azure-sql-db-accesstosql"></a>Migration des bases de données de Access vers SQL Server - base de données SQL Azure (AccessToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant (SSMA) est un outil qui fournit un environnement complet qui vous permet de migrer rapidement des bases de données Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure. À l’aide de SSMA, vous pouvez vérifier l’accès et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure les objets de base de données, évaluer la base de données Access pour la migration, convertir des objets de base de données Access, les charger dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, puis migrer les données.  
  
## <a name="recommended-migration-process"></a>Processus de migration recommandée  
Pour pouvoir migrer des objets et des données à partir de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, procédez comme suit :  
  
1.  [Créez un projet SSMA](http://msdn.microsoft.com/f2d1f0b0-5394-4adb-b3f3-abd71eb68ca7). Après avoir créé le projet, vous pouvez [définir les options de projet](http://msdn.microsoft.com/0a7304df-2f35-4453-96ef-7ac83dea1167), y compris les options de conversion, les options de migration et les mappages de types de données.  
  
2.  [Ajouter des fichiers de base de données Access](http://msdn.microsoft.com/e944c740-4c8a-4bc1-b0ed-be57bc06dced) au projet.  
  
    Vous pouvez ajouter des fichiers individuels, y compris les fichiers que vous trouvez sur l’ordinateur ou le réseau.  
  
3.  [Se connecter à l’instance cible de SQL Server](http://msdn.microsoft.com/f84cf007-ddf1-4396-a07c-3e0729abc769) ou [se connecter à l’instance cible de SQL Azure](http://msdn.microsoft.com/1ba0d113-dc05-4431-8689-e14a8821bafd).  
  
    Vous pouvez vous connecter à SQL Server ou SQL Azure.  
  
4.  Pour personnaliser le mappage entre une ou plusieurs bases de données Access et [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des schémas de SQL Azure, [mapper les bases de données source et cible](http://msdn.microsoft.com/69bee937-7b2c-49ee-8866-7518c683fad4).  
  
5.  Si vous le souhaitez, vous pouvez [créer un rapport d’évaluation](http://msdn.microsoft.com/8b9e23d6-da62-437a-8c05-8ad2628b9441) pour déterminer si les objets de base de données Access peuvent être convertis à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
6.  [Convertir les objets de base de données Access](http://msdn.microsoft.com/e0ef67bf-80a6-4e6c-a82d-5d46e0623c6c) à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou définitions d’objets SQL Azure.  
  
7.  [Charger les objets de base de données convertie dans SQL Server](http://msdn.microsoft.com/4e854eee-b10c-4f0b-9d9e-d92416e6f2ba).  
  
    Vous pouvez charger soit les objets de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure à l’aide de SSMA, ou vous pouvez enregistrer [!INCLUDE[tsql](../../includes/tsql_md.md)] des scripts.  
  
8.  [Migrer les données de l’accès à SQL Server](http://msdn.microsoft.com/f3b18af7-1af0-499d-a00d-a0af94895625).  
  
    > [!NOTE]  
    > Vous pouvez convertir, charger et migrer les schémas et les données en une seule étape. Pour effectuer la migration d’un seul clic, cliquez sur le **convertir, charger et migrer** bouton.  
  
9. Si vous souhaitez utiliser les données dans vos applications accès [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, utilisez [lier les tables de l’accès aux tables SQL Server](http://msdn.microsoft.com/82374ad2-7737-4164-a489-13261ba393d4).  
  
Vous pouvez également utiliser l’Assistant Migration pour vous guider tout au long de ce processus. Pour plus d’informations, consultez [Assistant Migration](http://msdn.microsoft.com/5bab5914-b2ae-4795-8cf5-83e42d64bef2).  
  
## <a name="see-also"></a>Voir aussi  
[Prise en main de SQL Server Migration Assistant pour Access](http://msdn.microsoft.com/462a731f-08f1-44e1-9eeb-4deac6d2f6c5)  
[Préparation pour la Migration des bases de données Access](http://msdn.microsoft.com/9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114)
