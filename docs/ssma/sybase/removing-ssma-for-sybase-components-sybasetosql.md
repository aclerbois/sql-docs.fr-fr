---
title: Suppression de SSMA pour Sybase composants (SybaseToSQL) | Documents Microsoft
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
ms.assetid: aec09593-17d9-4ec2-ac56-3cd8851406fd
caps.latest.revision: 6
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: d59d3d18b4eb5b5ec81cb12422cc16cf64592731
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="removing-ssma-for-sybase-components-sybasetosql"></a>Suppression de SSMA pour Sybase composants (SybaseToSQL)
Lorsque vous avez terminé bases de données de migration à partir de Sybase Adaptive Server Enterprise (ASE) à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], vous souhaiterez peut-être désinstaller les composants SSMA. Vous pouvez désinstaller les composants du client à tout moment, mais vous ne devez pas désinstaller le pack d’extension à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] , sauf si vous êtes sûr que vos bases de données migrés ne plus utilisent les fonctions dans le **ssma_syb** schéma de la **sysdb** base de données.  
  
## <a name="uninstalling-the-ssma-for-sybase-client"></a>Désinstallation de SSMA pour Sybase Client  
Vous pouvez désinstaller SSMA à l’aide de **Ajout / Suppression de programmes**.  
  
**Pour désinstaller SSMA**  
  
1.  Dans le panneau de configuration, ouvrez **Ajout / Suppression de programmes**.  
  
2.  Sélectionnez **Microsoft SQL Server Migration Assistant pour Sybase**, puis cliquez sur **supprimer**.  
  
3.  Pour confirmer que vous souhaitez désinstaller SSMA, cliquez sur **Oui**.  
  
## <a name="uninstalling-the-extension-pack"></a>Désinstallation du Pack d’Extension  
Si vous êtes sûr de vos bases de données migrées n’utilisent pas les objets dans le **sysdb.ssma_syb** schéma, vous pouvez supprimer le Pack d’extension à l’aide de **Ajout / Suppression de programmes**.  
  
Pour désinstaller le Pack d’extension  
  
1.  Dans le panneau de configuration, ouvrez **Ajout / Suppression de programmes**.  
  
2.  Sélectionnez **Microsoft SQL Server Migration Assistant pour Sybase Extension Pack**, puis cliquez sur **supprimer**.  
  
3.  Pour confirmer que vous souhaitez désinstaller le Pack d’extension, cliquez sur **Oui**.  
  
4.  Sur les Instances de la page de Scripts de base de données d’utilitaires, cliquez sur **suivant**.  
  
5.  Dans la page Paramètres de connexion, sélectionnez la méthode d’authentification, puis cliquez sur **suivant**.  
  
    L’authentification Windows utilise vos informations d’identification Windows pour tenter de se connecter à l’instance de SQL Server. Si vous sélectionnez l’authentification SQL Server, vous devez entrer un nom de connexion SQL Server et le mot de passe.  
  
6.  Dans la page Fin de l’opération, cliquez sur **OK**.  
  
7.  Dans la page Terminer, cliquez sur **Exit**.  
  
Après la désinstallation, vous pouvez confirmer que le **sysdb.ssma_syb** schéma, voire la totalité **sysdb** de base de données, a été supprimé à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)]. Toutefois, si vous utilisez d’autres produits SSMA, ils également utilisent le **sysdb** base de données. Si la base de données existe et que vous êtes sûr qu’aucune autre base de données ne référence des objets dans cette base de données, vous pouvez détacher la base de données.  
  
## <a name="see-also"></a>Voir aussi  
[L’installation de SSMA pour Sybase Client &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)  
[Installation des composants SSMA sur SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
  
