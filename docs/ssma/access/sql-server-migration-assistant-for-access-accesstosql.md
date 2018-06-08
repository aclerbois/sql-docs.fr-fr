---
title: SQL Server Migration Assistant pour Access (AccessToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-access
ms.custom: ''
ms.date: 08/14/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 40c1eb02-26b2-44ba-969d-6c430c61c281
caps.latest.revision: 22
author: Shamikg
ms.author: Shamikg
manager: murato
ms.openlocfilehash: 43b725e800fba4bdb085b03cd0ea183624249b9b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-migration-assistant-for-access-accesstosql"></a>SQL Server Migration Assistant pour Access (AccessToSQL)
[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant (SSMA) pour l’accès est un outil de migration de bases de données à partir de [!INCLUDE[msCoName](../../includes/msconame_md.md)] accéder aux versions 97 à 2010 pour [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2005 / [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2008 / [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2012 / [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2014 / [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2016 / [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] 2017 sur Windows et Linux (version préliminaire) / [!INCLUDE[msCoName](../../includes/msconame_md.md)] base de données SQL Azure. SSMA pour Access convertit les objets de base de données Access à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des objets de base de données SQL Azure, ces objets dans des charges [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure, et puis migre les données à partir de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
Cette documentation vous présente SSMA pour Access et fournit des instructions détaillées pour la migration des bases de données de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure et des informations sur les problèmes qui peuvent se produire après la migration.  
  
## <a name="contents"></a>Sommaire  
  
|Section| Description|  
|-----------|---------------|  
|[Nouveautés de SSMA pour Access](http://msdn.microsoft.com/en-us/a24d3fc0-6911-4bfa-828a-197abf222e02)|Répertorie les modifications apportées aux versions SSMA.|  
|[L’installation de SQL Server Migration Assistant pour Access](http://msdn.microsoft.com/en-us/dd50eebd-75df-4e0d-8c4d-88b511aae4c7)|Répertorie les conditions préalables pour l’installation de SSMA, la procédure d’installation et la licence SSMA et un lien vers la version la plus récente.|  
|[Prise en main de SQL Server Migration Assistant pour Access &#40;AccessToSQL&#41;](../../ssma/access/getting-started-with-sql-server-migration-assistant-for-access-accesstosql.md)|Introduit SSMA et son interface utilisateur.|  
|[Préparation pour la Migration des bases de données Access](http://msdn.microsoft.com/en-us/9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114)|Décrit comment préparer vos bases de données de l’accès à la conversion en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] SQL Azure.|  
|[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/en-us/76a3abcf-2998-4712-9490-fe8d872c89ca)|Fournit une vue d’ensemble du processus de conversion et des informations détaillées sur chaque étape du processus.|  
|[Liaison des Applications d’accès à SQL Server](http://msdn.microsoft.com/en-us/82374ad2-7737-4164-a489-13261ba393d4)|Décrit l’utilisation de vos applications Access existantes avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].|  
|[Guide de référence de l’interface utilisateur](http://msdn.microsoft.com/en-us/af24c303-4a41-449b-9c86-d6558a97e839)|Contient la documentation pour les boîtes de dialogue SSMA.|  
|[Utilisation de la console SSMA pour Access](http://msdn.microsoft.com/en-us/ef94e843-9f88-45a2-86c4-a0af268738c4)|Contient la documentation sur l’application Console de SSMA|  
|[Obtention d’aide de SSMA pour Access](http://go.microsoft.com/fwlink/?LinkID=708538&clcid=0x409)|Fournit des informations sur l’obtention d’une assistance supplémentaire.|  
  
