---
title: Préparation des bases de données Access de Migration (AccessToSQL) | Documents Microsoft
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
- Access databases, versions
- Access databases, when to migrate
- Access databases, workgroup security
- backing up databases
- documenting databases
- files, preparing
- migrating databases, versions
- migrating databases, when to migrate
- versions of Access
- workgroup security
ms.assetid: 9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114
caps.latest.revision: 20
author: Shamikg
ms.author: Shamikg
manager: murato
ms.openlocfilehash: 1862e57551ccbc4a41c14c58b1fb0f9de43998d9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="preparing-access-databases-for-migration-accesstosql"></a>Préparation des bases de données Access de migration (AccessToSQL)
Avant de migrer des bases de données Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous devez déterminer les bases de données à migrer et vous assurer que ces bases de données sont prêts pour la migration.  
  
## <a name="determining-when-to-migrate-to-sql-server"></a>Déterminer le moment auquel effectuer la migration vers SQL Server  
Le moteur de base de données Jet, qui est utilisé en tant que le moteur de base de données pour l’accès, est une solution flexible et facile à utiliser pour la gestion des données. Toutefois, comme les bases de données plus volumineux et plus critique, de nombreux utilisateurs trouvent dont ils ont besoin d’améliorer les performances, de sécurité ou de disponibilité. Pour les applications qui nécessitent une plateforme de données plus fiable, envisagez de déplacer les bases de données sous-jacent pour les applications à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour plus d’informations sur le choix du moment auquel la migration, consultez la [page des informations de migration](http://go.microsoft.com/fwlink/?LinkId=68571) sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] site Web.  
  
Après la migration des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous pouvez continuer à utiliser l’accès à l’aide de tables liées, ou vous pouvez migrer manuellement vos applications pour [!INCLUDE[msCoName](../../includes/msconame_md.md)] code basé sur le .NET Framework qui interagit directement avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
## <a name="determining-which-databases-to-migrate"></a>Déterminer les bases de données à migrer  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant (SSMA) pour l’accès peut trouver des bases de données Access pour vous. Vous pouvez ensuite exporter les métadonnées relatives à ces bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour plus d’informations sur l’exportation et d’interroger les métadonnées, consultez [exportation d’un inventaire accès](http://msdn.microsoft.com/7e1941fb-3d14-4265-aff6-c77a4026d0ed).  

   > [!NOTE]
   > Pas toutes les fonctionnalités d’accès et les paramètres sont pris en charge par, ou peuvent être facilement converties [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Avant de commencer la migration des bases de données, consultez [fonctionnalités accès incompatibles](http://msdn.microsoft.com/99d45b9c-e3b9-4d56-8c25-b594b887ace1).
  
## <a name="preparing-for-migration"></a>Préparation de migration  
Suivez les indications suivantes pour vous aider à préparer vos bases de données Access pour la migration vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
### <a name="upgrading-older-access-databases"></a>La mise à niveau des bases de données Access  
SSMA pour Access prend en charge Access 97 et versions ultérieures. Si vous avez des bases de données à partir de versions antérieures d’Access, ouvrir et enregistrer des bases de données dans Access 97 ou version ultérieure.  
  
### <a name="removing-workgroup-protection"></a>Suppression de la protection du groupe de travail  
SSMA ne peut pas migrer des bases de données qui utilisent la protection du groupe de travail. Pour supprimer la protection du groupe de travail à partir d’une base de données Access, procédez comme suit :  
  
1.  Copiez le fichier de base de données Access vers un autre emplacement.  
  
2.  Ouvrez la base de données copiée.  
  
3.  Sur le **outils** menu, pointez sur **sécurité**, puis sélectionnez **autorisations d’accès**.  
  
4.  Sélectionnez le **utilisateurs** option, sélectionnez le **Admin** utilisateur, puis assurez-vous que le **administrer** est autorisé.  
  
5.  Sélectionnez le **groupes** option, sélectionnez le **utilisateurs** de groupe et vérifiez que le **administrer** est autorisé.  
  
6.  Cliquez sur **OK**, puis, dans le **fichier** menu, cliquez sur **Exit**.  
  
Vous pouvez maintenant utiliser SSMA pour migrer la base de données copiée. Après avoir chargé le schéma [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous pouvez sécuriser manuellement la base de données sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
### <a name="backing-up-databases"></a>Sauvegarde des bases de données  
Avant de migrer vos bases de données Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous devez sauvegarder les accès aux bases de données que vous migrerez, ainsi que les [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] dans lequel vous effectuez la migration des bases de données accèdent aux objets et des données.  
  
Pour sauvegarder une base de données Access, sur le **outils** menu, pointez sur **utilitaires de base de données**, puis sélectionnez **sauvegarder la base de données**.  
  
Pour plus d’informations sur la façon de sauvegarder [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] bases de données, consultez « sauvegarde et restauration des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]» dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
### <a name="documenting-databases"></a>Documentation des bases de données  
Vous souhaiterez également les propriétés, telles que les listes d’objets de base de données, les tailles des fichiers et les autorisations, de vos bases de données Access de document. Pour générer cette documentation dans Access, sur le **outils** menu, pointez sur **analyser**, puis cliquez sur **documentées**.  
  
## <a name="see-also"></a>Voir aussi  
[Migration des bases de données de l’accès à SQL Server](http://msdn.microsoft.com/76a3abcf-2998-4712-9490-fe8d872c89ca)  
[Liaison des Applications d’accès à SQL Server](http://msdn.microsoft.com/82374ad2-7737-4164-a489-13261ba393d4)
