---
title: L’installation de SSMA pour SAP ASE (SybaseToSQL) | Documents Microsoft
ms.custom: ''
ms.date: 11/29/2017
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
ms.assetid: 8d5a4ce6-b747-46e3-9184-645d56e8b35c
caps.latest.revision: 6
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 336012819c23b02ac0527a70da930c5b74686e7a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="installing-ssma-for-sap-ase-sybasetosql"></a>L’installation de SSMA pour SAP ASE (SybaseToSQL)
[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Migration Assistant (SSMA) pour SAP Adaptive Server Enterprise (ASE) se compose d’une application cliente qui vous permet d’effectuer une migration à partir de SAP ASE à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou base de données SQL Azure. Il contient également un Pack d’extension qui prend en charge la migration des données et l’utilisation des fonctions de système ASE dans vos bases de données migrées.  
  
Installer l’application cliente sur l’ordinateur à partir de laquelle vous envisagez d’effectuer les étapes de migration. Installer les fichiers de pack d’extension sur l’ordinateur qui est en cours d’exécution [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] sur lequel les bases de données migrées doivent être hébergés.  
  
## <a name="upgrading-ssma-for-sap-ase"></a>La mise à niveau de SSMA pour SAP ASE  
Si vous souhaitez mettre à niveau vers une version ultérieure de SSMA pour SAP ASE, vous devez d’abord désinstaller le client et le Pack d’extension de serveur. Ensuite, installez une version plus récente.  
  
Si vous ouvrez un projet à partir d’une version antérieure de SSMA pour SAP ASE, SSMA vous demande si vous souhaitez convertir le projet vers la version la plus récente. Cliquez sur **Oui** pour travailler avec le projet dans la version la plus récente de SSMA.  
  
## <a name="contents"></a>Sommaire  
  
|Article| Description|  
|---------|---------------|  
|[L’installation de SSMA pour SAP ASE Client &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)|Fournit des informations et des instructions pour l’installation de SSMA pour les clients SAP ASE.|  
|[Installation des composants SSMA sur SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)|Fournit des informations et des instructions pour installer le Pack d’extension sur les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].|  
|[Suppression de SSMA pour les composants SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/removing-ssma-for-sybase-components-sybasetosql.md)|Fournit des instructions pour désinstaller le client pack programme et l’extension.|  
  
## <a name="see-also"></a>Voir aussi  
[Bases de données de migration SAP ASE à SQL Server - base de données SQL Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
