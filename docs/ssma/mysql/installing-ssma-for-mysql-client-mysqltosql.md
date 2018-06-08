---
title: L’installation de SSMA pour MySQL Client (MySQLToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-mysql
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
helpviewer_keywords:
- Installing client,Licensing
ms.assetid: ede3128c-370d-45a5-a815-3d94eecaea30
caps.latest.revision: 22
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: a0df85f3b44d7956ed9382213bc380681d47e48b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-ssma-for-mysql-client-mysqltosql"></a>L’installation de SSMA pour MySQL Client (MySQLToSQL)
SSMA pour MySQL client se compose des fichiers programme qui effectuent les tâches suivantes :  
  
-   Se connecter à une base de données MySQL.  
  
-   Se connecter à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
-   Convertir les objets de base de données MySQL à le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou des objets de SQL Azure.  
  
-   Chargez les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
-   Migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou SQL Azure.  
  
Cette rubrique fournit les conditions préalables d’installation et les instructions pour l’installation de SSMA pour MySQL client.  
  
## <a name="prerequisites"></a>Configuration requise  
SSMA pour MySQL est conçu pour fonctionner avec MySQL 4.1 ou ultérieur et toutes les éditions de SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014, SQL Server 2016, SQL Server 2017 et base de données SQL Azure.  
  
Avant d’installer SSMA, assurez-vous que l’ordinateur répond aux exigences suivantes :  
  
-   Windows 7 ou versions ultérieures, ou Windows Server 2008 ou versions ultérieures.  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 ou une version ultérieure.  
  
-   Le [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.0 ou version ultérieure. Le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.0 est disponible sur le support du produit SQL Server. Vous pouvez également obtenir de le [.NET Framework Developer Center](http://go.microsoft.com/fwlink/?LinkId=48882).  
  
-   Pilote de MySQL ODBC 5.1 et la connectivité aux bases de données MySQL que vous souhaitez migrer. Vous pouvez installer MySQL à partir du site Web de MySQL. Pour plus d’informations sur la connectivité, consultez [se connecter à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
-   Accès et des autorisations suffisantes sur l’ordinateur qui héberge l’instance cible de SQL Server où vous migrerez des objets de base de données et les données. Pour plus d’informations, consultez [connexion à SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
-   En cas de projets de SQL Azure, à l’accès et des autorisations suffisantes pour l’instance de base de données SQL Azure où vous migrerez de base de données objets et données. Pour plus d’informations, consultez [la connexion à la base de données SQL Azure &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md).  
  
-   4 Go de RAM est recommandé.  
  
## <a name="installing-ssma-for-mysql-client"></a>L’installation de SSMA pour MySQL Client  
SSMA est téléchargeable sur le Web. Pour télécharger la version la plus récente, consultez le [page de téléchargement de l’Assistant Migration SQL Server](http://aka.ms/ssmaformysql).  
  
Après avoir téléchargé la dernière version, vous devez extraire les fichiers d’installation avant de pouvoir installer SSMA.  
  
**Pour installer le client SSMA**  
  
1.  Double-cliquez sur SSMA pour MySQL *n*. Install.exe, où *n* est le numéro de build.  
  
2.  Dans la page d’accueil, cliquez sur **suivant**.  
  
    Si vous n’avez pas les composants requis installés, un message s’affiche, indiquant que vous devez d’abord installer les composants requis. Assurez-vous que vous avez installé tous les composants requis avant de réexécuter le programme d’installation.  
  
3.  Lisez le contrat de licence utilisateur final. Si vous acceptez, cochez **J’accepte les termes du contrat de licence**, puis cliquez sur **suivant**.  
  
4.  Dans la page Choisir un Type d’installation, cliquez sur **standard**.  
  
5.  Cliquez sur **Installer**.  
  
> [!IMPORTANT]  
> 1.  Désinstallez toutes les versions précédentes de SSMA pour MySQL avant d’installer la nouvelle version.  
  
L’emplacement d’installation par défaut est C:\Program Files\Microsoft SQL Server Migration Assistant pour MySQL.  
  
Sur l’ordinateur de Windows 64 bits, le produit est installé dans C:\Microsoft SQL Server Migration Assistant pour MySQL.  
  
## <a name="see-also"></a>Voir aussi  
[Bases de données de migration de MySQL vers SQL Server - base de données SQL Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
